# Condições e Complexidade

O capítulo anterior discutiu os benefícios do pipeline funcional e da execução de código linear. Em aplicativos do mundo real, no entanto, nem sempre é óbvio como tornar o código linear.

Grandes aplicativos geralmente têm regras lógicas complexas com várias condições. As condições tornam o aplicativo útil: elas descrevem seu comportamento em diferentes situações. Mas eles também tornam o código mais complicado.

Este capítulo discutirá como desvendar condições complexas e quais padrões podem ajudar.

## Complexidade Ciclomática e Cognitiva

Para entender exatamente como as condições tornam o código mais complexo, vamos primeiro tentar definir “complexidade”.

Uma heurística para encontrar código complexo é prestar atenção ao seu aninhamento. Quanto maior o aninhamento, mais complexo o código. Podemos entender _o que_ nos espera apenas olhando para um snippet com código profundamente aninhado:

```
function doSomething() {
__while (...) {
____if (...) {
______for (...) {
________if (...) {
__________break;
________}
________for (...) {
__________if (...) {
____________continue
__________}
________}
______}
____}
__}
}
```

O espaço vazio no lado esquerdo do texto mostra quanta informação o snippet contém. Quanto mais espaço houver, mais detalhes devemos ter em mente e mais complicado será o código.

| A propósito 🫙                                                                                 |
| :-------------------------------------------------------------------------------------------- |
| No livro “Your Code as a Crime Scene”, Adam Tornhill chama esse espaço de “negativo”.[^scene] |

Ao ler o código, construímos um modelo de como ele funciona em nossa cabeça. Este modelo descreve o que acontece com os dados e como as instruções são executadas.

Cada condição e loop adiciona ao modelo mental um novo “caminho”. Esses caminhos mostram como ir do início ao fim do código. Quanto mais “caminhos” diferentes existem, mais difícil é acompanhar todos eles simultaneamente. O número de “caminhos” que podemos tomar desde o início de uma função até o final é chamado de _complexidade ciclomática_ da função.[^cyclomaticcomplexity]

Podemos visualizar o número de “caminhos” na função como um gráfico.[^controlflowgraph][^controlflowexample] Cada nova condição ou loop adiciona um novo “ramo”, o que torna o gráfico e a função mais complexos.

<figure>
  <img src="../images/10-cyclomatic-complexity.png" width="800">
  <figcaption><em>Gráfico de função com complexidade 3. Podemos calcular a complexidade como a diferença entre os nós e arestas do grafo ou o número de regiões nele</em><br><br></figcaption>
</figure>

Quanto mais “ramificações” a função tiver, mais difícil será trabalhar.

| A propósito 🧠                                                                                                                                                                                             |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Além da complexidade ciclomática, há também a _complexidade cognitiva_.[^cognitivecomplexity]                                                                                                              |
| A complexidade ciclomática conta o número de “caminhos diferentes” em uma função. A complexidade cognitiva conta as “quebras no fluxo linear” da função.                                                   |
| Por causa disso, costuma-se dizer que a complexidade ciclomática mostra o quão difícil é testar uma função (quantos ramos cobrir nos testes), e a complexidade cognitiva mostra o quão difícil é entender. |
| Não vamos nos concentrar nas diferenças entre os dois porque elas não serão significativas para este livro. Mais adiante no texto, usaremos o termo “complexidade” como sinônimo de ambas as propriedades. |

Uma característica importante de tais propriedades é que elas são _mensuráveis_. Podemos escolher _limites_ para propriedades mensuráveis e automatizar suas verificações. Por exemplo, podemos configurar o linter para nos dizer quando a complexidade do código excede o limite selecionado:

<figure>
  <img src="../images/10-complexity-linter.png" width="800">
  <figcaption><em>Erros de linter quando a complexidade excede o limite</em><br><br></figcaption>
</figure>

O número específico depende da propriedade, do projeto, da linguagem e da equipe. Para complexidade ciclomática, Mark Seemann em “Code That Fits in Your Head” sugere usar o número 7.[^codethatfits] Wikipedia oferece o número 10.[^cyclomaticcomplexity] Eu costumo usar 10 em meus projetos porque é um número “redondo”, mas tudo isso é altamente arbitrário.

## Plano melhor que Aninhado

Espaço negativo e aninhamento grande são consequência da alta complexidade do código. Para tornar o código mais simples, ao refatorar, podemos seguir esta heurística:

---

**❗️ Torne as condições planas. ...E simplifique ainda mais o código**

---

Condições planas “endireitam” o fluxo de execução. Eles diminuem o número de detalhes que precisamos acompanhar ao mesmo tempo. Eles nos ajudam a ver padrões no código e simplificá-lo ainda mais. Quanto mais plano o código, mais fácil é entender o significado de uma função ou módulo.

| A propósito 🐍                                                                                                                                                |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Assemelha-se a um princípio do Zen of Python: ”Plano é melhor que aninhado.”[^zenpython] Na minha opinião, isso apenas confirma que estamos no caminho certo. |

## Retorno Antecipado

A técnica mais fácil e comum que podemos usar para “achatar” o código é o retorno antecipado. Isso ajuda a filtrar as ramificações de condição uma a uma e não mais mantê-las em nossas cabeças.

Como exemplo, vejamos a função `usersController`. Ele cria um novo usuário com base em seus argumentos. As condições na função nos forçam a manter _todos os seus ramos_ em mente até o final porque todos eles contêm algum comportamento importante.

```js
function usersController(req, res) {
  if (req.body) {
    const { name, email } = req.body;
    if (name && email) {
      const user = createUser({ name, email });
      if (user) {
        res.json({ user });
      } else {
        res.status(500);
      }
    } else {
      res.status(400).json({ error: "Name and email required" });
    }
  } else {
    res.status(400).json({ error: "Invalid request payload." });
  }
}
```

Observe, no entanto, que o código dentro dos blocos `else` trata de “casos extremos”—erros e dados inválidos. Podemos simplificar a função “invertendo” a condição e tratando os casos extremos primeiro, deixando apenas o “caminho feliz” no final:

```js
function usersController(req, res) {
  if (!req.body) {
    return res.status(400).json({ error: "Invalid request payload." });
  }

  const { name, email } = req.body;
  if (!name || !email) {
    return res.status(400).json({ error: "Name and email required" });
  }

  const user = createUser({ name, email });
  if (!user) return res.status(500);

  res.json({ user });
}
```

A quantidade total de informações e casos que a função trata não mudou. Mas reduzimos o número de ramificações que precisamos _lembrar ao mesmo tempo_.

Verificamos os casos extremos um a um e _terminamos_ a função se nos depararmos com um deles. Se a função continuar a funcionar, significa que não ocorreram casos extremos.

Filtrar casos extremos um por um nos permite esquecer os verificados. Eles não ocupam mais nossa atenção e memória de trabalho.

<figure>
  <img src="../images/10-early-return.png" width="800">
  <figcaption><em>A condição fica mais fácil de entender porque não precisamos lembrar de tantos detalhes</em><br><br></figcaption>
</figure>

| Entretanto 🛡                                                                                                                                                                                           |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| O retorno antecipado pode não ser adequado para programação “defensiva”: quando lidamos explicitamente com cada ramificação de uma condição.                                                           |
| Na minha experiência, não houve muitos projetos que exigiram programação defensiva. Portanto, costumo usar o retorno antecipado por padrão e mudar para a programação defensiva em casos excepcionais. |

### Renderização de Componentes

O retorno antecipado pode ser útil para simplificar a renderização de componentes React. Por exemplo, o código do componente `Cover` abaixo é relativamente difícil de ler devido às condições:

```js
function Cover({ error, isLoading, data }) {
  if (!error && !isLoading) {
    const image = data.image ?? DEFAULT_COVER_IMAGE;
    return <Picture src={image} />;
  } else if (isLoading) {
    return <Loader />;
  } else {
    return <Error message={error} />;
  }
}
```

Podemos simplificá-lo “invertendo” a condição e lidando primeiro com os estados “loading” e “error”:

```js
function Cover({ error, isLoading, data }) {
  if (isLoading) return <Loader />;
  if (error) return <Error message={error} />;

  const image = data.image ?? DEFAULT_COVER_IMAGE;
  return <Picture src={image} />;
}
```

Lembramos que o _número de ramificações_ na condição permanece o mesmo após a aplicação do retorno antecipado. Isso significa que a complexidade do código ainda pode ser alta. Devemos olhar para as métricas e ver se o código ficou mais simples.

Se a complexidade da função ainda for alta, devemos verificar se há problemas com abstração ou separação de interesses. Provavelmente notaremos que a função pode ser dividida em várias:

```js
// Neste caso, podemos separar a “renderização da imagem”
// e a “decisão de qual componente renderizar.”
//
// Vamos extrair a renderização da imagem no componente `CoverPicture`
// e usar isso quando os dados forem carregados sem erros.

function CoverPicture(image) {
  const source = image ?? DEFAULT_COVER_IMAGE;
  return <Picture src={source} />;
}

// O componente `Cover` se tornará um “ponto de entrada”
// e decidirá o que mostrar:
// `Loader`, `Error` ou `CoverPicture`.

function Cover({ error, isLoading, data }) {
  if (isLoading) return <Loader />;
  if (error) return <Error message={error} />;
  return <CoverPicture image={data.image} />;
}
```

Após tal refatoração, devemos considerar os nomes das funções e componentes alterados. Os nomes antigos podem se tornar imprecisos depois que melhorarmos a separação de suas responsabilidades. No exemplo acima, devemos pensar se os nomes `Cover` e `CoverPicture` refletem adequadamente o significado desses componentes.

| A propósito 🤖                                                                                                                                                                                                                            |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Em geral, máquinas de estado finito são melhores para trabalhar com UI do que o retorno antecipado.[^fsmfrontend] Mas se não pudermos implementá-las em nosso projeto, o retorno antecipado pode simplificar significativamente o código. |

Conceitualmente, o retorno antecipado dentro de uma função de renderização é semelhante à validação de dados no início de um fluxo de trabalho de negócios. Apenas filtramos estados de interface do usuário “errados” em vez de dados inválidos.

## Variáveis, Predicados e Álgebra Booleana

Não podemos “inverter” todas as condições. Se os ramos de condição estiverem fortemente entrelaçados, podemos não encontrar por onde começar a desembaraçá-los. Para simplificar tais condições, precisamos encontrar padrões nelas.

A primeira coisa que devemos fazer é extrair partes de condição repetitivas em variáveis. Os nomes das variáveis abstrairão os detalhes e ajudarão a focar no significado da condição. Isso nos permitirá ver se podemos simplificar a condição com lógica booleana.

### Leis de De Morgan

As _leis de De Morgan_ são um conjunto de regras de transformação que ligam operações lógicas por meio da negação.[^demorganlaws] Elas podem nos ajudar a “desdobrar” parênteses em condições:

```
!(A && B) === !A || !B;
!(A || B) === !A && !B;
```

Podemos usar as leis de De Morgan para tornar a condição menos barulhenta. Por exemplo, a condição abaixo tem muitos detalhes e é difícil ver seu significado “através dos caracteres”:

```js
if (user.score >= 50) {
  if (user.armor < 50 || user.resistance !== 1) {
  }
} else if (user.score < 50) {
  if (user.resistance === 1 || user.armor >= 50) {
  }
}
```

Como primeiro passo, podemos extrair expressões repetitivas em variáveis:

```js
const hasHighScore = user.score >= 50;
const hasHeavyArmor = user.armor >= 50;
const hasResistance = user.resistance === 1;
```

...Então a condição se tornará uma combinação dessas variáveis:

```js
if (hasHighScore) {
  if (!hasHeavyArmor || !hasResistance) {
  }
} else if (!hasHighScore) {
  if (hasResistance || hasHeavyArmor) {
  }
}
```

Em tal condição, é muito mais fácil notar padrões dentro de blocos `if` e simplificá-los. Em particular, podemos aplicar a primeira lei de De Morgan para simplificar as combinações das variáveis `hasHeavyArmor` e `hasResistance`:

```js
// Extraia a 2ª condição aninhada em uma variável:
const hasAdvantage = hasHeavyArmor || hasResistance;

// Observe que a 1ª condição aninhada
// pode se transformar em `!hasAdvantage`
// de acordo com a primeira lei de De Morgan.
//
// !A || !B === !(A && B)
//
// A -> hasHeavyArmor
// B -> hasResistance
//
// !hasHeavyArmor || !hasResistance
//    === !(hasHeavyArmor && hasResistance)
//    === !hasAdvantage

// Então toda a condição se torna:
if (hasHighScore && !hasAdvantage) {
} else if (!hasHighScore && hasAdvantage) {
}
```

### Predicados

Nem todas as condições podem ser extraídas para uma variável. Por exemplo, extrair uma condição é mais difícil se depender de outras variáveis ou dados dinâmicos.

Para esses casos, podemos usar _predicates_ — funções que recebem um número arbitrário de argumentos e retornam `true` ou `false`. A função `isAdult` no snippet abaixo é um exemplo de predicado:

```js
const isAdult = (user) => user.age >= 21;

isAdult({ age: 25 }); // true
isAdult({ age: 15 }); // false
```

Podemos armazenar a condição “blueprint” em predicados. Ou seja, descreva o princípio de comparação entre diferentes valores na função:

```js
// A condição no exemplo abaixo compara variáveis
// com o mesmo valor de referência—21:
if (user1.age >= 21) {
} else if (user2.age < 21) {
}

// Podemos colocar o “blueprint” desta comparação
// no predicado `isAdult` e usar isso ao invés:
if (isAdult(user1)) {
} else if (!isAdult(user2)) {
}
```

Os nomes dos predicados refletem o significado da verificação realizada como um todo. Eles ajudam a corrigir abstrações “vazadas” e tornam o código menos barulhento. Por exemplo, no snippet abaixo, é difícil dizer à primeira vista o significado da verificação de condição:

```js
if (user.account < totalPrice(cart.products) - cart.discount) {
  throw new Error("Not enough money.");
}
```

Se extrairmos a comparação no predicado _`hasEnoughMoney`_, será mais fácil entender seu significado a partir do nome da função:

```js
function hasEnoughMoney(user, cart) {
  return user.account >= totalPrice(cart.products) - cart.discount;
}

if (!hasEnoughMoney(user, cart)) {
  throw new Error("Not enough money.");
}
```

Além disso, predicados, como variáveis, ajudam a perceber padrões em condições complexas e simplificá-los com lógica booleana.

## Reconhecimento de Padrões Primitivos

Ao refatorar, também devemos considerar várias instruções `else-if`. Se tais condições selecionam um valor, construções mais declarativas podem substituí-las.

Por exemplo, vejamos a função `showErrorMessage`, que mapeia um erro de validação para uma mensagem:

```ts
type ValidationMessage = string;
type ValidationError = "MissingEmail" | "MissingPassword" | "TooShortPassword";

function showErrorMessage(errorType: ValidationError): ValidationMessage {
  let message = "";

  if (errorType === "MissingEmail") {
    message = "Email is required.";
  } else if (errorType === "MissingPassword") {
    message = "Password is required.";
  }

  return message;
}
```

A função deve verificar todas as variantes possíveis de `ValidationError` e escolher uma mensagem, mas perde uma mensagem para `TooShortPassword`. Sem verificações especiais (por exemplo, verificação de exaustividade [^exhaustivecheck]), o compilador TypeScript não pode nos ajudar aqui. Portanto, é relativamente fácil perder alguma coisa e nem perceber.

Podemos substituir várias condições por construções mais declarativas. Por exemplo, podemos coletar todas as mensagens de erro em um dicionário com tipos de erro como chaves e mensagens como valores. A seleção de erros de tal dicionário pode ser mais confiável porque o compilador pode detectar chaves ausentes:

```ts
function showErrorMessage(errorType: ValidationError): ValidationMessage {
  // No tipo de variável `messages`, especificamos explicitamente,
  // que todos os possíveis erros devem ser listados nele:
  const messages: Record<ValidationError, ValidationMessage> = {
    MissingEmail: "Email is required.",
    MissingPassword: "Password is required.",

    // Se alguma chave estiver faltando, o compilador nos informará sobre isso:
    // “A propriedade 'TooShortPassword' está faltando no tipo...”
  };

  return messages[errorType];
}
```

Conceitualmente, isso é semelhante ao reconhecimento de padrões.[^patternmatching] Nós combinamos `errorType` com as chaves do objeto `messages` e selecionamos o valor apropriado por ele.

Esta seleção é um pouco semelhante a como `switch` funciona, só que, neste caso, a verificação de tipo não requer ações adicionais (como uma verificação de exaustividade). No TypeScript, essa é provavelmente a maneira mais barata de simular reconhecimento de padrões” com segurança de tipo sem bibliotecas de terceiros.

| A propósito 🔍                                                                                                                                                                                                                             |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Vale a pena notar que esta técnica é mais adequada para casos simples e não parece tão legal quanto a reconheciento de padrões real em linguagens funcionais.[^patternmatchinghaskell]                                                     |
| A sintaxe nativa para reconhcimento de padrões em JS ainda está no Estágio 1 no momento da escrita.[^patternmatchingjs] Podemos usar bibliotecas de terceiros como `ts-pattern` para casos mais complexos.[^tspattern][^patternmatchingts] |

No código JavaScript, não há verificação de tipo, é claro. Mas o estilo declarativo dessa abordagem também facilita a busca manual de erros. Quanto mais explicitamente combinarmos chaves e valores, mais fácil será identificar incompatibilidades.

| Leia mais 👓                                                                                     |
| :----------------------------------------------------------------------------------------------- |
| Falaremos sobre o estilo declarativo e seus benefícios em detalhes em um dos capítulos a seguir. |

## Estratégia

Múltiplas instruções `else-if` podem escolher não apenas um valor, mas o comportamento do programa. A escolha do comportamento entre variantes semelhantes pode indicar separação insuficiente de interesses ou polimorfismo pobre.[^polymorphism] Uma solução possível para este problema é idêntica às técnicas de refatoração da seção anterior.

Vejamos a função `notifyUser`, por exemplo. Ele envia uma mensagem para um usuário em uma das três maneiras possíveis. A escolha da opção de notificação específica depende das condições desta função:

```ts
function notifyUser(message) {
  if (method === methods.popup) {
    const popup = document.querySelector(".popup");
    popup.innerText = message;
    popup.style.display = "block";
  } else if (method === methods.remote) {
    notificationService.setup(TOKEN);
    notificationService.sendMessage(message);
  } else if (method === methods.hidden) {
    console.log(message);
  }
}
```

O problema com a função é que ela mistura tarefas diferentes. Ele escolhe como enviar uma mensagem _e_ envia a mensagem. Adicionar uma nova opção de notificação ou alterar uma existente afetará toda a função. Talvez até as partes que _não estão relacionadas a essa mudança_.

Por isso, não podemos usar uma das opções de notificação separadamente no restante do código. Também não podemos testar diferentes opções isoladas umas das outras. Além disso, se adicionarmos novas opções, a função `notifyUser` se tornará muito maior e mais complexa.

Em vez disso, podemos separar a _escolha_ do comportamento do próprio _comportamento_:

```ts
// Vamos extrair cada opção de notificação em uma função separada:
function showPopupMessage(message) {
  const popup = document.querySelector(".popup");
  popup.innerText = message;
  popup.style.display = "block";
}

// Se a assinatura da função for incompatível com as demais:
function notifyUser(token, message) {
  notificationService.setup(token);
  notificationService.sendMessage(message);
}

// ...Podemos adaptar:
const showNotificationMessage = (message) => notifyUser(TOKEN, message);

// Em seguida, preparamos um conjunto de tais funções,
// um para cada valor de `methods`:
const notifiers = {
  [methods.popup]: showPopupMessage,
  [methods.hidden]: console.log,
  [methods.remote]: showNotificationMessage,
};

// Ao usá-lo, escolhemos o comportamento
// dependendo do parâmetro `method`:
function notifyUser(message) {
  const notifier = notifiers[method] ?? defaultNotifier;
  notifier(message);
}

// Também é possível selecionar a função de antemão,
// se `method` for conhecido antes de `notifyUser` ser executado.
```

Essa implementação facilita a adição e exclusão de opções de notificação:

```ts
// Para adicionar uma nova opção, adicionamos uma nova função...
function showAlertMessage(message) {
  window.alert(message);
}

const notifiers = {
  // ...
  // ...E “registre“ no objeto:
  [methods.browser]: showAlertMessage,
};

// O outro código permanece inalterado.
```

As alterações em uma função individual não irão além dela e não afetarão `notifyUser` ou outras funções. Testar e usar essas funções de forma independente é muito mais fácil.

A separação de escolha e comportamento é essencialmente o padrão “Estratégia”.[^strategy] Pode ser difícil reconhecê-la em código sem classes porque exemplos desse padrão são mais frequentemente mostrados no paradigma POO, mas é. A única diferença é que enquanto em POO, toda estratégia de comportamento é uma classe, em nosso exemplo, é uma função.

## Objeto Nulo

A funcionalidade “a mesma, mas ligeiramente diferente” também pode causar verificações e condições desnecessárias.

Por exemplo, digamos que criamos um aplicativo móvel para iOS, Android e a Web. Na versão móvel, queremos adicionar widgets de tela de bloqueio. Tanto o iOS quanto o Android têm APIs nativas para criar esses widgets. Vamos fingir que temos adaptadores JS para essas APIs.

A interface `Device` descreve a funcionalidade dos adaptadores. Os adaptadores contêm um identificador de plataforma e um método para atualizar o widget:

```ts
interface Device {
  platform: Platform;
  updateWidget(data: WidgetData);
}
```

Não há widgets na web, então decidimos desabilitar essa funcionalidade para o aplicativo web. Uma maneira de fazer isso é detectar a plataforma e habilitar o recurso por condição:

```ts
const iosDevice: Device = {
  platform: Platform.Ios,
  updateWidget: (data) => {}, // Ponte para a API nativa do iOS...
};

const androidDevice: Device = {
  platform: Platform.Android,
  updateWidget: (data) => {}, // Ponte para a API nativa do Android...
};

function update(device: Device, data: WidgetData) {
  if (
    device.platform === Platform.Android ||
    device.platform === Platform.Ios
  ) {
    // Chamando `updateWidget` apenas em dispositivos iOS e Android:
    device.updateWidget(data);
  }
}
```

Não é um problema se tivermos uma ou duas dessas verificações. Mas quanto mais recursos detectarmos dessa maneira, mais condições aparecerão em nosso código. Se acharmos que essas verificações são usadas com muita frequência, podemos usar a segunda opção para resolver o problema.

Vamos adicionar um “objeto de dispositivo fictício” para o aplicativo da web que implementará a interface `Device`, mas não fará nada em resposta a uma chamada `updateWidget`:

```ts
const webDevice: Device = {
  platform: Platform.Web,
  updateWidget() {},
};
```

Esses “objetos fictícios” são chamados \_ objetos nulos.[^nullobject] Eles geralmente são usados onde precisamos de uma chamada de método, mas não precisamos da implementação dessa chamada.

Ao usar um objeto nulo, não precisamos mais verificar o tipo do dispositivo – só precisamos chamar o método. Se o dispositivo atual for um navegador da Web, a chamada do método passará sem fazer nada:

```ts
function update(device: Device, data: WidgetData) {
  device.updateWidget(data);

  // webDevice.updateWidget();
  // void;

  // A condição não é mais necessária.
}
```

O objeto nulo às vezes é considerado **antipadrão**.[^nullobjectcriticism] É melhor decidir se é apropriado usá-lo dependendo da tarefa, formas de uso e preferências da equipe. É melhor consultar outros desenvolvedores antes de usá-lo e garantir que ninguém tenha motivos contra isso.

[^scene]: “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene
[^cyclomaticcomplexity]: Cyclomatic Complexity, Wikipedia, https://en.wikipedia.org/wiki/Cyclomatic_complexity
[^controlflowgraph]: Control-Flow Graph, Wikipedia, https://en.wikipedia.org/wiki/Control-flow_graph
[^controlflowexample]: Control flow graph & cyclomatic complexity for following procedure, Stackoverflow, https://stackoverflow.com/a/2670135/3141337
[^codethatfits]: “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
[^demorganlaws]: De Morgan's Laws, Wikipedia, https://en.wikipedia.org/wiki/De_Morgan%27s_laws
[^strategy]: Strategy Pattern, Refactoring Guru, https://refactoring.guru/design-patterns/strategy
[^exhaustivecheck]: `switch-exhaustiveness-check`, ES Lint TypeScript, https://typescript-eslint.io/rules/switch-exhaustiveness-check/
[^patternmatching]: Pattern Matching, Wikipedia, https://en.wikipedia.org/wiki/Pattern_matching
[^patternmatchinghaskell]: Pattern matching in Haskell, Learn You Haskell, http://learnyouahaskell.com/syntax-in-functions#pattern-matching
[^patternmatchingjs]: ECMAScript Pattern Matching Proposal, https://github.com/tc39/proposal-pattern-matching
[^tspattern]: `ts-pattern`, Library for Pattern Matching in TypeScript, https://github.com/gvergnaud/ts-pattern
[^patternmatchingts]: “Bringing Pattern Matching to TypeScript” by Gabriel Vergnaud, https://dev.to/gvergnaud/bringing-pattern-matching-to-typescript-introducing-ts-pattern-v3-0-o1k
[^nullobject]: Introduce Null Object, Refactoring Guru, https://refactoring.guru/introduce-null-object
[^nullobjectcriticism]: Null object pattern, Criticism, Wikipedia, https://en.wikipedia.org/wiki/Null_object_pattern#Criticism
[^fsmfrontend]: “Application State Management with Finite State Machines” by Alex Bespoyasov, https://bespoyasov.me/blog/fsm-to-the-rescue/
[^cognitivecomplexity]: “Cognitive Complexity. A new way of measuring understandability” by G. Ann Campbell, SonarSource SA, https://www.sonarsource.com/docs/CognitiveComplexity.pdf
[^zenpython]: The Zen of Python, https://peps.python.org/pep-0020/#the-zen-of-python
[^polymorphism]: Polymorphism, Wikipedia, https://en.wikipedia.org/wiki/Polymorphism_(computer_science)
