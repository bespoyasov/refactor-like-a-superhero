# Nomes

Depois de “tirarmos a poeira do código”, estamos prontos para considerar problemas mais graves.

As seguintes melhorias e técnicas que estamos prestes a discutir são muito mais difíceis de classificar do simples ao complexo. Nem sempre é óbvio quando e qual técnica deve ser aplicada.

| Ordem do capítulo 🎯                                                                                                                                                                                                    |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sugiro pensar neste e nos capítulos seguintes não como um “manual passo a passo”, mas como uma lista desordenada de problemas e possíveis soluções.                                                                     |
| Os capítulos do livro são organizados de modo que o tamanho das alterações de código sugeridas cresça gradualmente. Mas não há garantia de que os problemas ocorrerão nesta ordem quando você refatorar o projeto real. |
| Tenha cuidado ao analisar seu código. Faça anotações e escreva os problemas da maneira que você se sentir mais confortável e use este livro como material de apoio.                                                     |

A atenção aos nomes de variáveis, funções, classes e módulos pode ser a caixa de Pandora no mundo da refatoração. Nomes “não claros” podem sinalizar alto acoplamento entre módulos, separação inadequada de interesses ou abstrações “vazadas”. Este capítulo discutirá como nomes obscuros podem nos ajudar a encontrar problemas em nosso código.

## Diretrizes Gerais

Um nome “obscuro” pode ocultar detalhes valiosos do projeto e a experiência de outros desenvolvedores. Precisamos preservá-los, então vale a pena mergulhar no significado de tal nome em vez de descartá-lo completamente. Se durante a refatoração, vemos um nome cujo significado não está claro para nós, podemos tentar:

- Encontrar alguém da equipe que saiba o significado desse nome;
- Procurar o significado na documentação do código;
- Como último recurso, faça uma série de experimentos, alterando a variável e vendo como isso afeta o código.

| A propósito 🔬                                                                                                                                                                                                                                                             |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Se o efeito de uma variável for visível na “costura”, os testes podem nos ajudar com esses experimentos. Os resultados do teste mostrarão como os diferentes valores de variáveis alteram a saída do código. Isso pode nos ajudar a concluir o propósito real da variável. |
| Em outros casos, provavelmente teremos que avaliar as alterações manualmente. Como opção no _debugger_, podemos observar como os diferentes valores afetam as outras variáveis e dados com os quais o código trabalha.                                                     |
| Tais experimentos não vão _garantir_ que entendemos o significado da variável corretamente,[^posthoc], mas podem nos dizer que conhecimento sobre o projeto nos falta.                                                                                                     |

É melhor manter o conhecimento adquirido diretamente no código renomeando uma variável. Se não pudermos fazer isso, vale a pena mantê-lo _o mais próximo possível do código_. Podemos usar um comentário, documentação, uma mensagem de confirmação ou uma descrição de ticket. O objetivo é permitir que os desenvolvedores encontrem e usem essas informações de forma _mais fácil_.

## Nomes Muito Curtos

Podemos considerar um nome “bom” se representar adequadamente o significado da variável ou informação sobre o domínio. Nomes muito curtos e abreviações incomuns não fazem isso. Mais cedo ou mais tarde, alguém interpretará mal esse nome porque todos os desenvolvedores “informados” deixarão de trabalhar no projeto.

```js
// Nomes `d`, `c`, and `p` são muito curtos,
// por isso é difícil entender sobre o seu significado:

let d = 0;
if (c === "HAPPY_FRIDAY") d = p * 0.2;

// Variáveis com nomes completos
// são mais fáceis de entender:

let discount = 0;
if (coupon === "HAPPY_FRIDAY") discount = price * 0.2;
```

Nomes de uma letra estão bem em pedaços de código concisos, como `for`-loops. Mas para lógica de negócios, é melhor usar palavras mais descritivas.

O mesmo vale para as abreviaturas. Prefiro não usar abreviações no código, exceto em 2 casos:

- Se a abreviação for bem conhecida (por exemplo, EUA, OOP, USD);
- Se for usado nesta forma exata no domínio (por exemplo, _Challenge Rate_ como CR na calculadora de encontros para D&D).

Em outros casos, é melhor escolher palavras completas para o nome:

```js
// Okay, USD é uma abreviação comum:
const usd = {};

// Pode ser okay se o domínio é relacionado a matemática
// e o programa trabalha com derivadas:
const dx = 0.42;

// Não é okay; Devemos usar o nome completo:
const ec = 0.6188;

// Muito melhor:
const elasticityCoefficient = 0.6188;
```

## Nomes muito longos

Nomes muito longos indicam que a entidade está fazendo muitas coisas diferentes. A palavra-chave aqui é _“diferente”_ porque a funcionalidade não coesa é a mais difícil de combinar em um único nome.

Quando a funcionalidade não é coesa, o nome tenta transmitir todos os detalhes do trabalho em uma única frase. Incha o nome e o torna barulhento. Devemos prestar atenção a nomes que contenham palavras como _`that`_, _`which`_, _`after`_, etc.

Na maioria das vezes, nomes longos são um sinal de uma função que faz muito. Essa função provavelmente usa termos muito primitivos ou muito abstratos e não consegue encontrar as palavras certas para expressar seu significado. A principal heurística para encontrar tais funções é criar um nome mais curto para a função. Se não pudermos fazer isso, a função provavelmente está fazendo demais.

```js
async function submitOrderCreationFormIfValid() {
  // ...
}
```

A função `submitOrderCreationFormIfValid` do exemplo a cima faz três coisas ao mesmo tempo:

- Ele trata do evento de envio de formulário;
- Valida os dados do formulário;
- Cria e envia um novo pedido.

Cada passo é importante o suficiente para ser refletido no nome, mas isso incha o nome. É melhor pensar em como dividir a tarefa em tarefas menores e separar a responsabilidade entre as funções individuais:

```js
// Serializa dados de formulário em um objeto:
function serializeForm() {}

// Valida os dados do objeto:
function validateFormData() {}

// Crie um novo objeto de pedido a partir dos dados:
function createOrder() {}

// Envia o pedido para o servidor:
async function sendOrder() {}

// Manipula o evento DOM chamando outras funções:
function handleOrderSubmit() {}
```

Então, em vez de uma grande função tentando fazer _tudo_, teríamos uma cadeia de funções diferentes e menores. As funções internas conteriam ações _agrupadas por significado_. Seus nomes podem ter alguns detalhes do nome da função raiz, o que facilitaria:

```js
async function handleOrderSubmit(event) {
  const formData = serializeForm(event.target);
  const validData = validateFormData(formData);
  const order = createOrder(validData);
  await sendOrder(order);
}
```

Parece que estamos tornando o nome da função raiz menos preciso porque estamos tirando os detalhes dele. Mas é tudo uma questão de que tipo de detalhes tiramos.

Uma boa tática é ver o nome da função da perspectiva do _chamador_. O formulário se importa _como exatamente_ seu envio é tratado? Provavelmente não; importa apenas que a submissão seja tratada em princípio. O manipulador pode querer expressar _qual formulário_ ele irá manipular em seu nome:

```
handle          +
submit event    +
on order form   =
-----------------
handleOrderSubmit
```

...Mas _como exatamente_ isso acontece são os detalhes de implementação de `handleOrderSubmit`. Esses detalhes não são realmente necessários no nome porque são importantes apenas dentro da função. E lá, eles podem ser expressos com nomes de funções internas.

| A Propósito 👀                                                                                                                                                        |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Você provavelmente reconheceu isso como um exemplo de separação de camadas de abstração.[^abstractionlayers] Falaremos mais sobre isso em um dos capítulos seguintes. |

Então, basicamente, quando vemos um nome de função longo, devemos descobrir precisamente o que ela está tentando nos dizer. Podemos tentar tirar todos os detalhes que o nome carrega e dividi-los em “importantes por fora” e “importantes por dentro”.

É melhor manter os detalhes do primeiro grupo no nome da função original e representar os detalhes do segundo grupo como nomes de funções internas. Isso ajuda a dividir a tarefa em tarefas mais simples e agrupar funcionalidades relacionadas.

| A Propósito 🛠                                                                                                                                       |
| :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| A lógica da função em si ainda pode ser complexa. Mas geralmente há menos problemas com a nomenclatura se os detalhes relacionados forem agrupados. |

### Opção para Nomear Funções

Em relação às funções, o padrão A/HC/LC ajuda a manobrar entre nomes “muito curtos” e “muito longos”.[^ahclc] Esse padrão sugere combinar a ação, seu sujeito e seu objeto:

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

Podemos usá-lo como referência ou ponto de partida para nomear funções. Nem sempre é possível seguir o padrão, então podemos nos desviar dele e mudar o nome, se necessário.

## Diferentes Entidades com Nomes Idênticos

Durante a refatoração, também devemos considerar a sensação de “confusão”. Pode ocorrer quando diferentes entidades no código têm nomes idênticos.

Ao ler, confiamos principalmente nos nomes de classes, funções e variáveis para entender o significado do código. Se o nome não se correlacionar com uma variável de 1 para 1, devemos descobrir seu significado durante a leitura todas as vezes. Para fazer isso, devemos nos lembrar de muitas “meta-informações” sobre variáveis. Isso torna o código mais difícil de ler.

| A Propósito 🎫                                                                                                                                                                                            |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Em alguns casos, o mesmo se aplica a nomes diferentes para a mesma variável. Por exemplo, é melhor evitar chamar entidades de domínio com nomes diferentes, mas falaremos mais sobre isso posteriormente. |

Para facilitar a leitura do código, é melhor usar nomes _diferentes_ para entidades _diferentes_:

```js
// Aqui, `user` é um objeto com informações do usuário:
function isOldEnough(user, minAge) {
  return user.age >= minAge;
}

// E aqui, `user` é o nome do usuário:
function findUser(user, users) {
  return users.find(({ name }) => name === user);
}

// À primeira vista, é difícil distingui-los,
// porque os nomes idênticos nos obrigam a pensar
// que as variáveis têm o mesmo significado.
// Isso pode ser enganoso.

// Para evitar isso, temos que lembrar
// a diferença entre como cada variável é usada,
// ou expressa o significado da variável
// mais precisamente diretamente através de seu nome:
function findUser(userName, users) {
  return users.find(({ name }) => name === userName);
}
```

Nomes idênticos para entidades diferentes são especialmente perigosos se estiverem próximos. A localização próxima cria um “contexto comum” para as variáveis, reforçando a sensação de “mesmice”.

| Entretando 🦦                                                                                                                                                                                   |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Há exceções a essa regra, é claro. Por exemplo, podemos inferir a distinção dos tipos ou do contexto de uso. Embora, _por padrão_, seja melhor usar nomes diferentes para entidades diferentes. |

## Linguagem Ubíqua

_Linguagem ubíqua_ pode nos ajudar a combater problemas de nomenclatura. É um conjunto de termos que descrevem o domínio, que toda a equipe usa. “Toda a equipe” inclui desenvolvedores e a equipe do produto, designers, proprietários do produto, partes interessadas, etc.

| A Propósito 🎙                                                                                                                          |
| :------------------------------------------------------------------------------------------------------------------------------------- |
| O termo vem da metodologia _Domain-Driven Design, DDD_.[^ddd] Euacho útil descrever a lógica de negócios, você também pode achar útil. |

Na prática, a linguagem onipresente nos diz que se “pessoas de negócios” usam o termo “Pedido” para descrever pedidos, devemos usar essa palavra em nosso código, testes, documentação e comunicação verbal.

O poder da linguagem onipresente é a _inambiguidade_. Se todos chamarem uma coisa pelo mesmo nome, perderemos menos na “tradução entre negócios e desenvolvimento”.

| Leia Mais 📚                                                                                                                |
| :-------------------------------------------------------------------------------------------------------------------------- |
| Mais sobre isso você pode encontrar em “Domain Modeling Made Functional” por Scott Wlaschin.[^dmmf] Eu recomendo a leitura. |

Quando usamos termos próximos da realidade, o modelo mental de um programa se aproxima _do que_ está modelando. Nesse caso, é muito mais fácil identificar o comportamento incorreto do programa.

## Nomes Mentirosos

Às vezes, durante a refatoração, podemos encontrar nomes que não são precisos. A imprecisão pode variar de uma ligeira discrepância a mentiras completas. Devemos corrigir nomes falsos o mais rápido possível.

Se não tivermos certeza de como chamar a variável corretamente, devemos escrever por que o nome atual não se encaixa. Por exemplo, se encontrarmos um código como este:

```js
const trend = currentValue - previousValue;

// “Trend” aqui é a diferença entre
// os valores “current” e “previous”.
```

...E percebemos nas conversas que a equipe usa o termo “Delta” (não “Trend”), então vale a pena mencionar essa imprecisão. Podemos dizer algo como:

> - O termo "Trend" provavelmente não descreve com precisão o valor;
> - A equipe usa com mais frequência a palavra “Delta”, incluindo o _product owner_;
> - As especificidades do projeto podem exigir tendências construídas a partir de mais de dois pontos.

Podemos levantar essas preocupações para a equipe e considerar renomeá-las. Se o nome obviamente mente, podemos pular a discussão. Mas em caso de dúvida, vale a pena discutir primeiro a preocupação com outros desenvolvedores e com o _product owner_.

## Tipos de Domínio

Em linguagens com tipagem estática, podemos usar tipos para transmitir detalhes sobre o domínio. Os tipos levam a carga do nome da entidade colocando alguns detalhes no tipo. O nome fica mais sucinto, mas dificilmente perde o significado por causa do tipo.

| A Propósito 💬                                                                                                                                                                                                                 |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Poderíamos argumentar que o tipo só é visível em assinaturas ou pop-ups IDE, enquanto o nome está sempre visível.                                                                                                              |
| Sim, mas vários nomes mais longos tornariam o código ruidoso e seu valor desapareceria. E em segundo lugar, eu costumo extrair detalhes que não são necessários _instantaneamente_ e podem ser facilmente pesquisados no tipo. |
| Para mim, parece um compromisso perfeitamente aceitável.                                                                                                                                                                       |

Por exemplo, é conveniente usar tipos para descrever vários estados de dados que aparecerão no aplicativo ao longo de seu ciclo de vida.

```ts
type CreatedOrder = {
  createdAt: TimeStamp;
  createdBy: UserId;
  products: ProductList;
};

type ProcessedOrder = {
  createdAt: TimeStamp;
  createdBy: UserId;
  products: ProductList;
  address: Delivery;
};
```

Esses tipos descrevem diferentes estados de dados (entidades essencialmente diferentes) com nomes diferentes. Mas também desautorize o uso do tipo “errado” onde não cabe. Por exemplo, podemos proibir tentativas de enviar pedidos não preparados:

```ts
function sendOrder(order: ProcessedOrder) {
  // ...
}

const order: CreatedOrder = {
  /*...*/
};

// A chamada de função abaixo não compila
// porque o tipo de argumento não satisfaz a assinatura.
// Pode ser traduzido como: “O pedido ainda não está pronto para ser enviado.”
sendOrder(order);
```

| _Boolean flags?_ 🚩                                                                                                                                 |
| :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| Discutiremos por que usar _boolean flags_ para denotar diferentes estados não é tão bom quanto tipos diferentes no capítulo sobre tipagem estática. |

[^abstractionlayers]: Abstraction layer, Wikipedia, https://en.wikipedia.org/wiki/Abstraction_layer
[^ahclc]: Naming functions, A/HC/LC Pattern, https://github.com/kettanaito/naming-cheatsheet#ahclc-pattern
[^ddd]: “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
[^dmmf]: “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
[^posthoc]: Post Hoc Ergo Propter Hoc, Wikipedia, https://en.wikipedia.org/wiki/Post_hoc_ergo_propter_hoc
