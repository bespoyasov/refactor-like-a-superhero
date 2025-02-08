# Efeitos Colaterais

Qualquer interação entre um aplicativo e o mundo real é um efeito colateral. Salvar dados no servidor, renderizar componentes na interface do usuário ou acessar a API do navegador — todos esses são efeitos colaterais. Eles são necessários para um aplicativo útil, mas são difíceis de raciocinar e podem causar erros.

Neste capítulo, discutiremos como projetar programas e refatorar código para que os efeitos não tornem o código mais complicado e propenso a erros. Exploraremos os benefícios da programação funcional e das estruturas de dados imutáveis. Também discutiremos como organizar os efeitos de um aplicativo e a diferença entre CQS e CQRS.

## Funções Puras

O principal problema com os efeitos é que eles são _imprevisíveis_. Eles mudam o estado, então não podemos ter certeza de que o resultado do código será sempre o mesmo.

Por outro lado, _funções puras_ não produzem efeitos, não dependem do estado e sempre retornam o mesmo resultado quando recebem os mesmos argumentos. Isso torna seus resultados previsíveis e reprodutíveis.

Ao refatorar, tentaremos usar funções puras com mais frequência e expressar mais funcionalidade com elas. Mas para entender os benefícios dessa abordagem, vamos primeiro discutir a natureza das funções puras.

### Transparência Referencial

É mais fácil ver a vantagem de funções puras durante a depuração (_debugging_). Podemos usar a pesquisa binária para acelerar a pesquisa de bugs no trecho de código problemático. Usando-o, dividimos o snippet em duas metades, detectamos um erro em uma delas e depois procuramos bugs apenas nessa metade:

```
Digamos que esta é a parte problemática do código.
O erro nele é marcado com “X”:

[....................X...........]

1.
Para encontrar o erro neste snippet,
dividimos a funcionalidade pela metade
e verifcaremos cada metade:

[...............|....X...........]

2.
Se a metade esquerda funcionar bem,
o erro está na direita:

[               |....X...........]

3.
Nós dividimos a parte da direita ao meio
e verificaremos cada metade:

[               |....X..|........]

4.
A parte direita está funcionando,
então o erro está na esquerda:

[               |....X..|        ]

5.
Novamente dividimos a parte problemática ao meio e repetimos a busca.
Fazemos isso até encontrarmos o local exato com o erro:

[               |...|X..|        ]
[                   |X..|        ]
[                   |X|.|        ]
[                   |X|          ]

A cada iteração, a área de código problemática é reduzida pela metade.
Essa abordagem economiza muito tempo durante a depuração.

Em geral, após 3-6 iterações, fica claro,
qual é o problema e onde ele está.
```

| A propósito 🔪                                                                                                |
| :------------------------------------------------------------------------------------------------------------ |
| Às vezes, esse processo de busca binária é chamado de bissecção por analogia com a bissecção no git.[^bisect] |

Em uma sequência de funções puras, os dados fluem de uma função para outra, sendo passados como argumentos. A vantagem de tal sequência é que podemos “cortá-la” _em qualquer lugar_. As funções após o “corte” não se importam com quais funções foram chamadas antes delas. Eles funcionarão da mesma maneira se receberem os mesmos argumentos.

Podemos literalmente substituir as _chamadas_ de funções antes do “corte” por _seus resultados_, e o comportamento do programa não mudará. Essa propriedade é chamada de _transparência referencial_,[^referentialtransparency] e torna a pesquisa de erros e testes muito mais simples.

<figure>
  <img src="../images/11-referential-transparency.png" width="800">
  <figcaption><em>A transparência referencial nos permite substituir uma chamada de função pelo seu resultado</em><br><br></figcaption>
</figure>

Com efeitos colaterais, isso é muito mais difícil de conseguir. Então, ao refatorar, tentaremos reduzir o número de efeitos e usar funções puras com a maior frequência possível.

## Imutável por Padrão

Como dissemos anteriormente, o código com efeitos é menos previsível. Ao refatorar, devemos primeiro verificar se podemos reescrevê-lo sem efeitos. Na prática, isso geralmente significa substituir o “estado compartilhado” por uma cadeia de transformações de dados.

Por exemplo, veja a função `prepareExport`, que prepara os dados do pedido da loja para exportação. Ele calcula os totais e a última data de envio para cada item.

```js
function prepareExport(items) {
  let latestShipmentDate = 0;

  for (const item of items) {
    item.subtotal = item.price * item.count;

    if (item.shipmentDate >= latestShipmentDate) {
      latestShipmentDate = item.shipmentDate;
    }
  }

  for (const item of items) {
    item.shipmentDate = latestShipmentDate;
  }

  return items;
}
```

O cálculo do `subtotal` dentro da função altera o array `items`. No entanto, outros cálculos também dependem dessa matriz, e alterá-la também os afetará.

Isso significa que ao calcular o `subtotal` teremos que considerar como isso afetará o cálculo do `shipmentDate`. Quanto mais extensa a função, mais ações serão afetadas e mais detalhes devemos ter em mente.

Além disso, como `items` é um array, suas alterações serão visíveis _fora_ da função `prepareExport`. Eles podem afetar o código sobre o qual podemos não saber nada. Nesse caso, será impossível prever possíveis problemas.

Em vez de acompanhar os efeitos, podemos tentar evitá-los. Vamos reescrever o código não para alterar o estado compartilhado, mas para expressar o problema como uma sequência de etapas.

O resultado de cada etapa será um _novo_ conjunto de dados. As etapas em si não dependerão de nenhuma variável compartilhada para que não afetem o trabalho um do outro:

```js
function prepareExport(items) {
  // 1. Calcular os subtotais.
  // O resultado é um novo array.
  const withSubtotals = items.map((item) => ({
    ...item,
    subtotal: item.price * item.count,
  }));

  // 2. Calcular a data de envio.
  // O resultado da etapa anterior é a entrada.
  let latestShipmentDate = 0;
  for (const item of withSubtotals) {
    if (item.shipmentDate >= latestShipmentDate) {
      latestShipmentDate = item.shipmentDate;
    }
  }

  // 3. Anexe a data a cada posição.
  // O resultado é mais um novo array.
  const withShipment = items.map((item) => ({
    ...item,
    shipmentDate: latestShipmentDate,
  }));

  // 4. Retorna o resultado da etapa 3,
  // como resultado da função.
  return withShipment;
}
```

Também podemos extrair as etapas em funções separadas e, se necessário, refatorar cada uma separadamente:

```js
function calculateSubtotals(items) {
  return items.map((item) => ({ ...item, subtotal: item.price * item.count }));
}

function calculateLatestShipment(items) {
  const latestDate = Math.max(...items.map((item) => item.shipmentDate));
  return items.map((item) => ({ ...item, shipmentDate: latestDate }));
}
```

Em seguida, a função `prepareExport` se parecerá com o resultado de uma sequência de transformação de dados:

```js
function prepareExport(items) {
  const withSubtotals = calculateSubtotals(items);
  const withShipment = calculateLatestShipment(withSubtotals);
  return withShipment;
}

// items -> withSubtotals -> withShipment
```

Ou ainda assim, se usarmos o Hack Pipe Operator, que no momento da escrita está no Stage 2:[^hackpipe]

```js
const prepareExport =
  items |> calculateSubtotals(%) |> calculateLatestShipment(%);
```

Nem sempre é necessário extrair as etapas em funções separadas, mas é conveniente em vários casos:

- Se precisarmos testar cada etapa isoladamente das demais.
- Se quisermos usar transformações de dados em outro lugar do aplicativo.
- Se quisermos tornar cada estado de dados da sequência mais explícito.

Observe que na nova implementação, a função `prepareExport` _não altera_ os dados originais. Em vez disso, ele _cria uma cópia_ dos dados e os altera. A matriz `items` permanece intocada, o que evita erros no código fora da função.

As etapas dentro de `prepareExport` agora estão conectadas apenas por seus _dados de entrada e saída_. Eles não têm estado compartilhado que possa afetar sua operação. Isso torna mais fácil para nós construir um modelo mental de como a função `prepareExport` funciona. A função torna-se uma cadeia de transformações de dados, cada uma das quais é isolada das outras e não pode ser afetada de fora.

| Abstração e encapsulamento 👀                                                                                                                                                                                                                                                                                           |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ao abstrair cada etapa em uma função separada com um nome claro, tornamos o significado de toda a cadeia mais explícito. Ajuda a se concentrar nos passos individuais sem se preocupar com os outros. O isolamento ajuda a garantir a validade dos dados em cada etapa, pois evita que a função seja afetada “de fora”. |

A imutabilidade pode exigir desempenho de memória e custo. Geralmente não é um problema no frontend, mas ainda vale a pena conhecer os possíveis problemas. Uma abordagem mutável pode ser mais adequada se estivermos perseguindo o desempenho.

| A propósito 🚜                                                                                                                                                  |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Em JavaScript, a imutabilidade real é difícil de alcançar. Para objetos verdadeiramente imutáveis, precisaríamos de `Object.freeze`, que raramente é usado.     |
| Mas a imutabilidade “real” nem sempre é necessária. Na maioria das vezes, basta _escrever e tratar_ o código como se estivesse trabalhando com dados imutáveis. |

## _Functional Core in Imperative Shell_

Imutabilidade e funções puras são boas, mas como mencionamos, não podemos criar um aplicativo útil sem efeitos. As interações com o mundo exterior – receber e salvar dados ou renderizá-los na interface do usuário – são sempre efeitos. Sem essas interações, o aplicativo é inútil.

Como o problema com os efeitos é que eles são imprevisíveis, nossa principal preocupação com eles é:

---

**❗️ Minimize o número de efeitos e isole-os de outros códigos**

---

Existe uma técnica para gerenciar efeitos chamada _functional core in an imperative shell_ ou _Impureim Sandwich_.[^fcis][^impureim] Usando essa abordagem, descrevemos a lógica do aplicativo como funções puras e “empurramos” toda a interação com o mundo externo _para as bordas do aplicativo_.

<figure>
  <img src="../images/11-impureim.png" width="600">
  <figcaption><em>Um _"impureim sandwich”_: efeito impuro para ler os dados, lógica pura, efeito impuro para salvar os dados</em><br><br></figcaption>
</figure>

Considere um exemplo. A função `updateUserInfo` mistura transformação de dados com salvamento e leitura no DOM:

```js
function updateUserInfo(event) {
  const { email, birthYear, password } = event.target;
  const root = document.querySelector(".userInfo");

  root.querySelector(".age").innerText = new Date().getFullYear() - birthYear;
  root.querySelector(".password").innerText = password.replace(/./g, "*");
  root.querySelector(".login").innerText = email.slice(
    0,
    email.lastIndexOf("@")
  );
}
```

Vamos tentar separar a lógica dos efeitos. Primeiro, podemos fazer isso dentro da função agrupando o código em “seções”:

```js
function updateUserInfo(event) {
  // Ler dados:
  const { email, birthYear, password } = event.target;

  // Transformar os dados:
  const age = new Date().getFullYear() - birthYear;
  const username = email.slice(0, email.lastIndexOf("@"));
  const hiddenPassword = password.replace(/./g, "*");

  // “Salvar” dados, no nosso caso, renderiza na UI:
  const root = document.querySelector(".userInfo");
  root.querySelector(".age").innerText = age;
  root.querySelector(".password").innerText = hiddenPassword;
  root.querySelector(".login").innerText = username;
}
```

Em seguida, podemos extrair a transformação de dados em uma função separada. Ela não saberia nada sobre ler e salvar dados e apenas lidaria com a lógica da transformação:

```js
function toPublicAccount({ email, birthYear, password, currentYear }) {
  return {
    age: currentYear - birthYear,
    username: email.slice(0, email.lastIndexOf("@")),
    hiddenPassword: password.replace(/./g, "*"),
  };
}
```

Então podemos usar a função `toPublicAccount` dentro de `updateUserInfo` assim:

```js
function updateUserInfo(event) {
  // Ler dados:
  const { email, birthYear, password } = event.target;
  const currentYear = new Date().getFullYear();

  // Transformar:
  const { age, username, hiddenPassword } = toPublicAccount({
    email,
    birthYear,
    password,
    currentYear,
  });

  // “Salvar”:
  const root = document.querySelector(".userInfo");
  root.querySelector(".age").innerText = age;
  root.querySelector(".password").innerText = hiddenPassword;
  root.querySelector(".login").innerText = username;
}
```

Para verificar se o código melhorou, podemos tentar escrever testes para lógica de transformação de dados. Na primeira versão do código, o teste ficaria assim:

```js
// 1. No caso de `updateUserInfo`,
// precisamos criar mocks para DOM e Event:
const dom = jsdom(/*...*/);
const event = {
  target: {
    email: "test@test.com",
    password: "strong-password-1234",
    birthYear: 1994,
  },
};

// Também precisamos simular a data atual,
// para que os resultados do teste sejam reproduzíveis:
jest.useFakeTimers().setSystemTime(new Date("2022-01-01"));

// Mocks e timers precisam ser resetados após cada teste,
// para não afetar outros testes:
afterAll(() => jest.useRealTimers());

describe("quando recebe um objeto de informações do usuário", () => {
  it("deve calcular a idade do usuário", () => {
    updateUserInfo(event);

    // O resultado é verificado pelo conteúdo do nó DOM:
    const node = dom.document.querySelector(".userInfo .age");

    // Isso perde o tipo de dado
    // porque os nós DOM contêm apenas strings:
    expect(node.innerText).toEqual("28");
  });
});
```

Agora vamos escrever um teste para a função `toPublicAccount` e compará-lo com o anterior:

```js
// 2. No caso de `toPublicAccount`, não precisamos de mocks.
// Para testá-lo, precisamos apenas de dados de entrada
// e o resultado esperado.

describe("quando recebe um objeto de informações do usuário", () => {
  it("deve calcular a idade do usuário", () => {
    const { age } = toPublicAccount({
      email: "test@test.com",
      password: "strong-password-1234",
      birthYear: 1994,
      currentYear: 2022,
    });

    // Podemos testar a função por comparação direta,
    // nenhuma informação é perdida, incluindo os tipos de dados:
    expect(age).toEqual(28);
  });
});
```

No primeiro caso, a função `updateUserInfo` lida com diferentes tarefas: transformar dados e interagir com a interface do usuário. Seus testes confirmam isso - eles verificam como os dados foram alterados e usam simulações do DOM.

Se outra função semelhante aparecer em seus testes, teremos que simular o DOM _novamente_ para verificar as alterações nos dados. Isto deve se tornar uma preocupação porque há uma duplicação óbvia sem benefícios adicionais.

O teste é muito mais fácil no segundo caso porque não precisamos criar mocks. Precisamos apenas dos dados de entrada e do resultado esperado para testar funções puras. (É por isso que muitas vezes se diz que funções puras são intrinsecamente testáveis.)

Interagir com o DOM torna-se uma tarefa separada. Os mocks do DOM aparecerão nos testes do módulo que trata das interações da interface do usuário e em nenhum outro lugar.

Dessa forma, simplificamos o código da função e melhoramos a separação de interesses entre as diferentes partes do aplicativo.

| A propósito 🔌                                                                                                                                  |
| :---------------------------------------------------------------------------------------------------------------------------------------------- |
| Aqui, não queremos dizer que “mocks são sempre ruins”. Às vezes, os mocks são a única maneira de testar o efeito desejado, como em adaptadores. |
| O ponto é que, se tivermos que escrever um mock para testar _lógica_, provavelmente haverá uma maneira melhor de organizar o código.            |

Após a refatoração, podemos ver que a tarefa da função `updateUserInfo` se transformou em uma “composição” da funcionalidade de outras funções. Agora ele reúne dados de leitura, transformando-os e salvando-os no armazenamento.

A estrutura da função começou a se assemelhar ao sanduíche com efeitos e lógica. Com uma adequada separação de responsabilidades, as “camadas” do sanduíche tornam-se completamente independentes. Isso torna as transformações de dados previsíveis e isoladas dos efeitos.

### Adaptadores para Efeitos

Separar lógica e efeitos ajuda a detectar duplicação no código que lê e salva os dados. Pode ser perceptível por simulações idênticas em testes ou código semelhante nos próprios efeitos.

Se diferentes partes de um aplicativo interagem com o mundo exterior de forma semelhante, podemos extrair essa interação para uma entidade separada, o _adapter_. Os adaptadores reduzem a duplicação, desacoplam o código do aplicativo do mundo externo e facilitam o teste dos efeitos:

```js
// Se notarmos a mesma funcionalidade
// ao ler ou salvar dados:

function updateUserInfo(user) {
  // ...

  if (window?.localStorage) {
    window.localStorage.setItem("user", JSON.stringify(user));
  }
}

function updateOrder(order) {
  // ...

  if (window?.localStorage) {
    window.localStorage.setItem("order", JSON.stringify(order));
  }
}

// Podemos extraí-lo em um adaptador:

const storageAdapter = {
  update(key, value) {
    if (window?.localStorage) {
      window.localStorage.setItem(key, JSON.stringify(value));
    }
  },
};

// E então use apenas o adaptador
// no outro código do aplicativo:

function updateUserInfo(user) {
  // ...
  storageAdapter.update("user", user);
}

function updateOrder(order) {
  // ...
  storageAdapter.update("order", order);
}

// Desta forma, toda a lógica de interação
// com o armazenamento é descrito no `storageAdapter`.
//
// Se precisarmos testar essa interação, precisamos apenas testar os métodos do adaptador,
// sem executar verificações adicionais em funções como `updateUserInfo`.
```

| Em detalhes 👀                                                                           |
| :--------------------------------------------------------------------------------------- |
| Discutiremos os adaptadores com mais detalhes em um capítulo separado sobre arquitetura. |

## Comandos e Consultas

Quando empurramos a interação com o mundo exterior para as bordas do aplicativo, podemos pensar em como exatamente eles afetam o mundo. Efeitos diferentes têm consequências diferentes:

- Alguns efeitos apenas _lêem_ informações e não alteram o estado do mundo.
- Outros efeitos _alteram_ (adicionar, atualizar e excluir) o estado.

O princípio que divide o código responsável por essas tarefas é chamado de _Command-Query Separation (CQS)_.[^cqs]

De acordo com o CQS, _consultas (queries)_ apenas retornam dados e não alteram o estado, enquanto _comandos (commands)_ alteram o estado e não retornam nada. O objetivo do CQS é:

---

**❗️ Continue lendo e alterando os dados separadamente**

---

Misturar comandos e consultas torna o código complicado e inseguro. É difícil prever o resultado de uma função se ela puder alterar os dados durante sua chamada. Por esta razão, ao refatorar, devemos prestar atenção aos efeitos que violam o CQS.

Por exemplo, vamos ver a assinatura da função `getLogEntry`:

```ts
function getLogEntry(id: Id<LogEntry>): LogEntry {}
```

A partir dos tipos, podemos supor que esta função de alguma forma _obtém_ dados dos logs. Pode se tornar uma surpresa se, na implementação, vemos:

```ts
function getLogEntry(id: Id<LogEntry>): LogEntry {
  const entry =
    logger.getById(id) ?? logger.createEntry(id, Date.now(), "Access");

  return entry;
}
```

O problema com a função é sua imprevisibilidade. Não podemos saber antecipadamente qual resultado obteremos após a chamada da função. Podemos tentar resolver esse problema adicionando detalhes ao nome da função:

```ts
function getOrCreateLogEntry(id: Id<LogEntry>): LogEntry {}
```

Há mais informações agora, mas ainda não temos ideia do que a função fará. Ele ainda pode ler uma entrada de log existente ou criar uma nova.

Quanto menos previsível for uma função, mais problemas teremos ao depurá-la. A depuração é mais rápida quando temos que verificar menos suposições. Quando não podemos prever o comportamento de uma função, precisamos _testar mais suposições_. A depuração de tal função levará mais tempo.

Além disso, efeitos imprevisíveis são muito mais difíceis de testar. Por exemplo, para testar a função `getOrCreateLogEntry`, teríamos que escrever um teste mais ou menos como este:

```ts
afterEach(() => jest.clearAllMocks());
afterAll(() => jest.restoreAllMocks());

describe("quando recebe um ID que existe no serviço", () => {
  it("deve retornar a entrada com esse ID", () => {
    jest.spyOn(logger, "getById").mockImplementation(() => testEntry);
    const result = getOrCreateLogEntry("test-entry-id");
    expect(result).toEqual(testEntry);
  });
});

describe("quando recebe um ID de uma entrada que não existe", () => {
  it("deve criar uma nova entrada com o ID fornecido", () => {
    jest.spyOn(logger, "getById").mockImplementation(() => null);
    const spy = jest.spyOn(logger, "createEntry");

    const result = getOrCreateLogEntry("test-entry-id");
    expect(spy).toHaveBeenCalledWith("test-entry-id", timeStub, "Access");

    expect(result).toEqual({
      createdAt: timeStub,
      id: "test-entry-id",
      type: "Access",
    });
  });
});
```

A partir do número de mocks, podemos supor que a função faz “demais”. Se não forem escritos com cuidado, os próprios mocks podem afetar a funcionalidade uns dos outros, tornando os testes pouco confiáveis e frágeis.

E, finalmente, criação e leitura de dados são operações conceitualmente diferentes. Suas razões para mudar são diferentes, então é melhor mantê-los separados.

Vamos dividir esta função em duas:

```ts
function readLogEntry(id: Id<LogEntry>): MaybeNull<LogEntry> {}
function createLogEntry(id: Id<LogEntry>): void {}
```

De acordo com as assinaturas, vemos agora que a primeira função retorna um resultado, enquanto a segunda função _faz algo e não retorna nada_. A assinatura já implica que a segunda função _muda_ o estado, então é um efeito.

```ts
function readLogEntry(id: Id<LogEntry>): MaybeNull<LogEntry> {
  return logger.getById(id) ?? null;
}

function createLogEntry(id: Id<LogEntry>): void {
  logger.createEntry(id, Date.now(), "Access");
}
```

O código tornou-se mais previsível porque a assinatura da função não nos engana mais. Pelo contrário, agora nos ajuda a prever o comportamento antes de considerar a implementação.

Os testes para ambas as funções agora são independentes. Não precisamos mais criar _mock_ da funcionalidade interna do serviço `logger` para testar os detalhes de cada efeito. Basta verificar se as funções chamam os métodos corretos com os dados necessários:

```ts
// readLogEntry.test.ts

describe("quando recebe um ID", () => {
  it("deve chamar o serviço de logger com esse ID", () => {
    const spy = jest.spyOn(logger, "getById");
    readLogEntry("test-entry-id");
    expect(spy).toHaveBeenCalledWith("test-entry-id");
  });
});

// createLogEntry.test.ts

describe("quando recebe um ID", () => {
  it("deve chamar o serviço de logger createEntry com esse ID e dados de entrada padrão", () => {
    const spy = jest.spyOn(logger, "createEntry");
    createLogEntry("test-entry-id");
    expect(spy).toHaveBeenCalledWith("test-entry-id", timeStub, "Access");
  });
});
```

| Simplificação 🚧                                                                                                                                                                                                                                                      |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Muitas vezes, para adaptadores, também precisamos verificar se eles convertem corretamente os dados entre o serviço e nosso aplicativo (também conhecido como “adaptar a interface”). Mas neste exemplo em particular, não achei necessário e não me concentrei nele. |

### CQS e IDs gerados

No desenvolvimento de back-end, há um padrão típico que contradiz o CQS. Imagine um aplicativo em que o módulo de banco de dados retorna um ID gerado em resposta à criação de uma entidade. A ação “create” é um comando e não deve retornar nada, mas retorna um ID, portanto, viola o CQS.

Em geral, não acho essa violação fatal. Afinal, CQS é uma recomendação. Sua aplicabilidade deve ser avaliada caso a caso. Se retornar o ID for um padrão comum no projeto, não há nada de errado em usá-lo. Devemos apenas ser consistentes com isso e documentar as razões por trás disso.

Mas se não quisermos desviar do CQS, podemos passar o ID junto com os dados que queremos salvar. Não se encaixa em todos os projetos, mas pode ser uma opção valiosa a ser considerada.

| Leia mais 👀                                                                                                                                                                                          |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Essa solução está bem descrita no artigo “CQS versus IDs gerados pelo servidor” de Mark Seemann.[^cqsvsid] Ele explica detalhadamente a solução em si, suas variações, aplicabilidade e desvantagens. |

### CQRS

Falando do backend, vale a pena mencionar as operações CRUD e CQRS.[^crud][^cqrs] Ao projetar uma API, podemos querer usar os mesmos tipos de dados para ler e gravar dados:

```ts
type UserModel = {
  id: Id<User>;
  name: FullName;
  birthDate: DateTime;
  role: UserRole;
};

function readUser(id: Id<User>): UserModel {}
function updateUser(user: UserModel): void {}
```

Na maioria dos casos, essa solução é suficiente e não causará problemas. No entanto, pode se tornar um problema se lermos e gravarmos dados de maneira diferente. Por exemplo, se quisermos atualizar os dados do usuário apenas parcialmente.

A função `updateUser` requer todo o objeto `UserModel` como entrada, então não podemos atualizar campos individuais. Temos que passar todo o objeto atualizado para a função.

Se um projeto encontrar tal problema, o _Command-Query Responsibility Segregation, CQRS_ pode ajudar.[^cqrs] Este princípio estende a ideia do CQS sugerindo o uso de diferentes tipos (também chamados de “modelos”) para leitura e gravação de dados.

Continuando com o exemplo `UserModel`, podemos expressar a essência do CQRS desta forma:

```ts
// Para leitura usamos um tipo, `ReadUserModel`:
function readUser(id: Id<User>): ReadUserModel {}

// Para escrever usamos outro tipo, `UpdateUserModel`
function updateUser(user: UpdateUserModel): void {}
```

Modelos independentes tornam explícito quais dados fornecer durante a gravação e quais dados esperar durante a leitura. Por exemplo, podemos descrever o tipo `ReadUserModel` como um conjunto de campos obrigatórios que são garantidos nos dados durante a leitura:

```ts
type ReadUserModel = {
  id: Id<User>;
  name: FullName;
  birthDate: DateTime;
  role: UserRole;
};
```

Para atualizações, podemos usar um tipo diferente:

```ts
type UpdateUserModel = {
  // ID é obrigatório para deixar claro,
  // quais dados do usuário atualizar:
  id: Id<User>;

  // Todo o resto é opcional,
  // para atualizar apenas o que precisamos:
  name?: FullName;
  birthDate?: DateTime;

  // O campo _role_, por exemplo, não pode ser atualizada,
  // é por isso que este campo não existe aqui.
};
```

Dessa forma, nenhum dos tipos nos impede de declarar expectativas _diferentes_ para leituras e gravações.

| Tome cuidado 🚧                                                                                                                                                                                                                         |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CQRS aumenta a quantidade de código (modelos, objetos, tipos) no projeto. Vale a pena discutir a abordagem com a equipe antes de usá-la e garantir que não haja argumentos contra isso.                                                 |
| As principais razões para usar o CQRS são as diferenças nos tipos de leitura e gravação e a diferença na carga de backend para operações de leitura e gravação que às vezes leva a um dimensionamento separado de leituras e gravações. |
| Em outros casos, usar um modelo genérico provavelmente será mais fácil e barato.                                                                                                                                                        |

[^referentialtransparency]: Referential Transparency, Haskell Wiki, https://wiki.haskell.org/Referential_transparency
[^hackpipe]: “A pipe operator for JavaScript” by Axel Rauschmayer, https://2ality.com/2022/01/pipe-operator.html
[^fcis]: “Functional Core in Imperative Shell” by Gary Bernhardt, https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell
[^impureim]: “Impureim Sandwich” by Mark Seemann, https://blog.ploeh.dk/2020/03/02/impureim-sandwich/
[^cqs]: “Command-Query Separation” by Martin Fowler, https://martinfowler.com/bliki/CommandQuerySeparation.html
[^cqsvsid]: “CQS versus server generated IDs” by Mark Seemann, https://blog.ploeh.dk/2014/08/11/cqs-versus-server-generated-ids/
[^cqrs]: “Command-Query Responsibility Segregation” by Martin Fowler, https://martinfowler.com/bliki/CQRS.html
[^crud]: Create, Read, Update, and Delete, Wikipedia, https://en.wikipedia.org/wiki/Create,_read,_update_and_delete
[^bisect]: git-bisect, Use binary search to find the commit that introduced a bug, https://git-scm.com/docs/git-bisect
