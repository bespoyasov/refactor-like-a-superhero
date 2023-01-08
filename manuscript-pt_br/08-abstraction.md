# Abstração

Códigos excessivamente detalhados são “ruidosos” e difíceis de caber na sua cabeça. Ele está sobrecarregado de informações e faz tanto que é difícil entender o que realmente faz.

Geralmente, o código ruidoso reflete uma ação complexa com muitas etapas. Quanto mais complexa a ação, mais detalhes precisamos considerar e mais etapas precisam ser descritas. Juntos, esses detalhes sobrecarregam o leitor e tornam o código complicado.

Neste capítulo, discutiremos como usar a abstração para reduzir o ruído no código e torná-lo menos complicado.

## Intenção e Implementação

> A abstração é a eliminação do irrelevante e a ampliação do essencial[^abstractionquote]

Em um exemplo no capítulo “Nomes”, dividimos os detalhes do nome da função em dois grupos: “importante por fora” e “importante por dentro”. Dessa forma, simplificamos o nome e equilibramos a quantidade de informações no nome da função e seu corpo. Mantivemos detalhes essenciais no nome e ocultamos os menos importantes na implementação. Essa separação de detalhes “por níveis” é chamada de abstração.

A abstração ajuda a domar a complexidade separando _intenção_ e _implementação_. A intenção descreve _o que_ vamos fazer, e a implementação descreve _como_ vamos fazer. A intenção é essencial no nível “superior” ao descrever uma entidade e sua interação com o ambiente. Os detalhes de implementação são importantes no nível “abaixo”, quando focamos nos processos internos da entidade.

```js
// Nome e assinatura refletem a intenção...
function isChild(user) {
  // ...E o corpo da função reflete a implementação.
  return user.age < 18;
}

// Quando usamos a função com outras,
// nos preocupamos com seu propósito e objetivos,
// não sobre seus detalhes de implementação:
if (isChild(user)) toggleParentControl();
```

Nosso cérebro só pode trabalhar com uma quantidade limitada de informações por vez. A abstração nos ajuda a focar nos detalhes que são importantes _agora_. Esse foco é especialmente necessário ao trabalhar com código em que detalhes de “diferentes níveis” são misturados.

Considere um exemplo. Vamos imaginar que temos uma função, `subscribeToFeed`, que verifica a validade do email fornecido no início:

```js
function subscribeToFeed(email) {
  if (!email.includes("@") || !email.includes(".")) return false;

  const recipients = addRecipient(email);
  confirmFeedSubscription(recipients);
}
```

Se compararmos a validação de email com as outras ações (`addRecipient`, `confirmFeedSubscription`), veremos que ela fala em termos “muito primitivos” para esta tarefa.

Enquanto outras funções falam em termos de “e-mails”, “feeds” e “assinaturas”, a validação fala sobre os caracteres `"@"` e `".."`. Por isso, temos que “saltar” entre os detalhes da validação e seu propósito quando lemos a função.

A função `subscribeToFeed` quer saber se o endereço é válido. Mas não é crucial como exatamente verificamos o e-mail para essa função. Esses detalhes não são necessários aqui.

Podemos extrair (abstrair) a verificação de e-mail em uma função separada `isValidEmail`:

```js
function isValidEmail(email) {
  return email.includes("@") && email.includes(".");
}

function subscribeToFeed(email) {
  if (!isValidEmail(email)) return false;

  const recipients = addRecipient(email);
  confirmFeedSubscription(recipients);
}
```

O nome da função `isValidEmail` agora reflete todo o “conjunto de ações de validação” como uma única frase. O nome usa termos próximos aos usados pelos nomes das funções ao seu redor. Essa proximidade ajuda a focar nos objetivos das ações e não em seus processos internos.

| A propósito 👀                                                                                                             |
| :------------------------------------------------------------------------------------------------------------------------- |
| Isso é um pouco mais difícil de conseguir em código com efeitos colaterais, mas discutiremos isso em um capítulo separado. |

Quando _chamamos_ a função `isValidEmail`, focamos em seu nome e intenção. Neste ponto, nos preocupamos em como a função interage com as entidades ao seu redor, ou seja, “o que acontece se o email for inválido”.

Se nos preocupamos com as regras de validação, estudamos o corpo da função – a implementação. Nesse ponto, nos importamos em quando a função decide se deve retornar `true` ou `false`.

| A propósito 🤖                                                                                                                                             |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Em linguagens com tipagem estática, a assinatura da função também pode expressar a intenção. Falaremos mais sobre isso no capítulo sobre tipagem estática. |

A abstração nos ajuda a “mergulhar” gradualmente em conceitos e processos complexos. Ele nos dá informações sobre o sistema em pedaços. Em cada “nível de detalhe”, temos apenas as informações de que precisamos para entender o sistema naquele nível. Mark Seemann chama isso de arquitetura fractal, e acho essa metáfora muito útil.[^codethatfits]

## Arquitetura Fractal

Comparado a um computador, nossos cérebros são computacionalmente fracos. É difícil para nós multiplicar grandes números ou manter mais de dez conceitos em nossas cabeças simultaneamente.

Ao refatorar, devemos ficar de olho em quão difícil é “encaixar um pedaço de código em nossa cabeça”. Se for difícil lembrar de todos os detalhes, o código tem um problema.

Em um bom código, há exatamente tanta informação na tela quanto o leitor precisa a qualquer momento. Mark Seemann sugere escrever programas de modo que o número de partes constituintes em cada “nível de detalhe” não exceda algum limite. Podemos usar essa heurística para verificar se o código está muito detalhado.[^codethatfits]

| A propósito 🧠 |
| :------------- |

| Mark sugere o número 7 como limite. Ele se baseia na premissa de que podemos manter 7±2 objetos em nossas cabeças.
[^shorttermmemory][^thinkingfastandslow] Ele também diz que o número específico não é crucial. O principal objetivo é ter esse limite em princípio. |

Para visualizar “níveis”, ele sugere usar uma grade de hexágonos. Cada hexágono é uma parte do sistema, que pode ser detalhado mais profundamente. Em cada nível de detalhe, não veremos mais do que N partes importantes para esse nível. Se precisarmos saber como uma determinada peça funciona, podemos “ampliar” um hexágono e ver em que consiste.

<figure>
  <img src="../images/08-fractal-architecture.png" width="600">
  <figcaption><em>Então o programa se divide em pedaços, que se dividem em pedaços, que se dividem em pedaços...</em><br><br></figcaption>
</figure>

| Nota de direitos autorais ©                                                                 |
| :------------------------------------------------------------------------------------------ |
| A imagem acima foi gerada pela ferramenta do artigo sobre Fractal Hex Flowers.[^hexflowers] |

Vejamos um exemplo para entender como ele nos ajuda a refatorar o código. Considere um aplicativo onde mostramos um painel para usuários logados e uma página de login para outros.

O ponto de entrada para o aplicativo (o nível superior de detalhes) pode ser algo assim:

```js
const App = () => {
  const user = currentUser();
  const isManager = hasManagerRole(user);
  const isPromoAccount = checkPromoAccount(location);

  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const handleSubmit = () => {
    /*...*/
  };

  return isManager || isPromoAccount ? (
    <Dashboard />
  ) : (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}
        onChange={({ target }) => setEmail(target.value)}
      />
      <input
        type="password"
        password={password}
        onChange={({ target }) => setPassword(target.value)}
      />
      <button>Login</button>
    </form>
  );
};
```

É bem possível entender esse código, mas levará um tempo comparativamente mais longo devido ao número de detalhes. Isso porque a quantidade de informações nesse código está próxima dos limites da nossa memória de trabalho.

Se expressarmos esse código no diagrama hexagonal, veremos que algumas partes simplesmente não se encaixam:

<figure>
  <img src="../images/08-app-detalization.png" width="600">
  <figcaption><em>Objetos e funções refletidos em ladrilhos hexagonais; um deles não se encaixa</em><br><br></figcaption>
</figure>

O código seria muito mais fácil de explorar e entender se, no nível superior, "preparássemos" o leitor e dissessemos a ele _o que_ o componente `App` faz. Nesse caso, nomes de variáveis e subcomponentes expressariam a intenção e somariam uma “história”:

```js
const App = () => {
  const hasAccess = useHasAccess();
  return hasAccess ? <Dashboard /> : <Login />;
};

/**
 * Se um usuário tiver acesso (`hasAccess`) ao painel de controle,
 * o aplicativo mostrará a eles o componente do painel (`Dashboard`).
 * Caso contrário, eles serão solicitados a fazer login (`Login`).
 */
```

A implementação das funções e componentes correspondentes refletiria os detalhes da “história”. Por exemplo, poderíamos explicar como determinar se um usuário tem acesso ao painel de controle através da implementação do _hook_ `useHasAccess`:

```js
function useHasAccess() {
  const user = currentUser();
  const isManager = hasManagerRole(user);
  const isPromoAccount = checkPromoAccount(location);
  return isManager || isPromoAccount;
}

/**
 * Vamos verificar se o usuário atual (`currentUser`)
 * é um _manager_ (`hasManagerRole`).
 * Também verificaremos se o aplicativo está sendo executado em uma conta promocional (_promo_),
 * em que o painel de controle está disponível para todos (`checkPromoAccount`).
 */
```

Assim, no nível superior de detalhes, veríamos apenas três partes: `hasAccess`, `Dashboard` e `Login`. Esse código é muito mais fácil de “carregar” em nossas cabeças e focar nas relações entre suas partes.

<figure>
  <img src="../images/08-app-detalization-simpler.png" width="600">
  <figcaption><em>Camada superior de detalhes de aplicação na forma de ladrilhos hexagonais</em><br><br></figcaption>
</figure>

Se precisássemos detalhar uma parte da “história”, poderíamos “ampliar” uma das células e examinar sua estrutura.

Por exemplo, em `useHasAccess` podemos ver como suas quatro partes funcionam juntas. Nesse nível, não importa realmente o que acontece “nível acima” porque focamos na estrutura do `useHasAccess`.

<figure>
  <img src="../images/08-has-access-detalization.png" width="600">
  <figcaption><em>Detalhamento do ladrilho <code>useHasAccess</code> _hook_</em><br><br></figcaption>
</figure>

Chama-se arquitetura fractal porque podemos aninhar um nível de detalhe em outro:

<figure>
  <img src="../images/08-app-fractal.png" width="600">
  <figcaption><em>Níveis de detalhes estão aninhados em outro como uma boneca russa</em><br><br></figcaption>
</figure>

...E mudar nossa atenção entre os níveis a qualquer momento:

<figure>
  <img src="../images/08-fractal-levels.png" width="800">
  <figcaption><em>Mudando a atenção entre os níveis</em><br><br></figcaption>
</figure>

Cada uma das células pode ser “ampliada” para suas partes. E cada uma de suas partes também pode ser “ampliada” ainda mais. Dessa forma, podemos ir cada vez mais fundo no sistema, mas controlar quanta informação consumimos.

Em cada nível, há apenas uma quantidade _confortável_ limitada de informações esperando por nós. Esse código é muito mais fácil de “encaixar na cabeça”.

A abstração está no centro dessa abordagem. Podemos “diminuir o zoom” para observar as interações de um módulo com outros ou “aumentar o zoom” para estudar os detalhes de como ele funciona.

| A propósito 📚                                                                                                          |
| :---------------------------------------------------------------------------------------------------------------------- |
| A arquitetura fractal lembra um pouco o conceito Zoom World de “The Humane Interface” de Jeff Raskin.[^humaneinterface] |

## Separação de preocupações

A abstração nos força a dividir o código em partes, mas nem sempre é óbvio como fazer isso. Para facilitar essa tarefa, podemos usar o princípio _Separation of Concerns, SoC_.[^soc] Ele oferece a divisão do sistema em tais partes, cada uma das quais é responsável por apenas uma tarefa.

“Responsabilidade” e “tarefa” são termos um tanto vagos. Assim, podemos entendê-los como um conjunto limitado de dados e ações que são _relacionados uns aos outros mais fortemente do que outros_ dados e ações.

| A propósito 🔗                                                                                                                       |
| :----------------------------------------------------------------------------------------------------------------------------------- |
| Você pode se lembrar do termo coesão dessa descrição.[^cohesion] Discutiremos um pouco mais no capítulo sobre integração de módulos. |

Partes de um código bem dividido não se sobrepõem e não duplicam a funcionalidade umas das outras. O trabalho de tal sistema é então baseado na composição de suas partes. Durante o projeto e a refatoração, essa separação nos força a decompor tarefas complexas em tarefas mais simples.

### Decomposição de Tarefas

Quando vemos um grande pedaço de código, devemos primeiro considerar quantas tarefas diferentes existem. Para fazer isso, podemos contar quantos conjuntos de dados e ações estão nesse código.

Por exemplo, vejamos a função de envio do formulário de login:

```js
async function submitLoginForm(event) {
  const form = event.target;
  const data = {};

  if (!form.email.value || !form.password.value) return;
  data.email = form.email.value;
  data.password = form.password.value;

  const response = await fetch("/api/login", {
    method: "POST",
    body: JSON.stringify(data),
  });
  return response.json();
}
```

É bem compacto, apenas 13 linhas, mas podemos contar três tarefas nele:

- Serialização de formulários;
- Validação de dados;
- Requisições de rede.

Também podemos contá-los alterando as condições de uma tarefa e ver _qual código mudará por causa disso_. Por exemplo, se adicionarmos um _checkbox_ ao formulário, o código de serialização será alterado:

```js
async function submitLoginForm(event) {
  // ...

  data.email = form.email.value;
  data.password = form.password.value;

  // Novo campo aparecerá no objeto:
  data.rememberMe = form.rememberMe.checked;

  // ...
}
```

E se mudarmos o esquema da API, apenas a parte de requisiçao de rede mudará:

```js
async function submitLoginForm(event) {
  // ...

  // Argumento para `fetch` irá mudar:
  const response = await fetch("/api/v2/login", {
    method: "POST",
    body: JSON.stringify(data),
  });

  // ...
}
```

Essa verificação ajuda a contar _motivos para mudança_ no código e correlaciona fragmentos a eles. O número de razões diferentes nos dirá quantas tarefas o código resolve.

| A propósito 🧶                                                                                                                                                                                                                                               |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nem sempre um grande pedaço de código tem muitas tarefas. Uma implementação de algoritmo complexo pode ser extensa, mas resolve apenas uma tarefa. Também podemos verificar isso alterando as condições da tarefa e vendo quais são as alterações de código. |

### Princípio da Responsabilidade Única

O código que muda por motivos diferentes é melhor mantido separado, e o código que muda pelo mesmo motivo é melhor mantido junto. É conhecido como o _Princípio da Responsabilidade Única_ (_Single Responsibility Principle, SRP_).[^srp][^singleresponsibility]

Podemos aplicar este princípio para refatorar a função `submitLoginForm` do exemplo acima. Vamos extrair cada tarefa em uma função separada e ver como o código `submitLoginForm` muda. Vamos começar com a serialização:

```js
// Extraia a serialização em uma função separada.
// Agora tudo está reunido aqui, e sabemos exatamente
// onde procurar se precisarmos saber seus detalhes.
function serializeForm(form) {
  const data = {};

  data.email = form.email.value;
  data.password = form.password.value;

  return data;
}

async function submitLoginForm(event) {
  const form = event.target;

  // Dentro de `submitLoginForm` agora focamos
  // somente ao usar os dados serializados.
  const data = serializeForm(form);

  if (!form.email.value || !form.password.value) return;

  const response = await fetch("/api/login", {
    method: "POST",
    body: JSON.stringify(data),
  });
  return response.json();
}
```

Em seguida, vamos pensar na validação. Agora vemos que validar o objeto DOM como antes não faz sentido. Precisamos validar os dados, mas não importa de onde os obtemos. Ao separar a responsabilidade dessa maneira, podemos nos livrar do acoplamento indesejado entre as tarefas.

```js
// Toda a validação agora está reunida na função `isValidLogin`.
// Dentro dela, verificamos os dados, não as propriedades do objeto DOM:
function isValidLogin({ email, password }) {
  return !!email && !!password;
}

async function submitLoginForm(event) {
  const form = event.target;
  const data = serializeForm(form);
  if (!isValidLogin(data)) return;

  const response = await fetch("/api/login", {
    method: "POST",
    body: JSON.stringify(data),
  });
  return response.json();
}
```

A chamada da API também pode ser uma função separada:

```js
// Toda a camada de rede agora está na função `loginUser`.
async function loginUser(data) {
  const method = "POST";
  const body = JSON.stringify(data);

  const response = await fetch("/api/login", { method, body });
  return await response.json();
}

async function submitLoginForm(event) {
  const form = event.target;
  const data = serializeForm(form);
  if (!isValidLogin(data)) return;

  return await loginUser();
}
```

O código resultante é mais fácil de modificar porque as funções extraídas limitam os “escopos de responsabilidade” entre as tarefas. Alterações em uma das funções são menos propensas a causar alterações em outras funções. Por exemplo, ao atualizar as regras de validação, apenas o código da função `isValidLogin` mudará:

```js
function isValidLogin({ email, password }) {
  // Agora verificando se o email contém o caractere `@`:
  return email.includes("@") && !!password;
}

// Funções `serializeForm`, `loginUser`, e `submitLoginForm`
// não são modificadas.
```

Essa separação facilita o teste e o desenvolvimento de funções isoladas umas das outras. E quanto maior o isolamento, menor a chance de cometer um erro acidental ao atualizar o código.

## Encapsulamento

O princípio de responsabilidade única ajuda a pensar em partes de código (funções, módulos, objetos) como partes independentes de um aplicativo.

As partes se comunicam por meio de APIs (protocolos, contratos, interfaces) e não interferem nos detalhes internos umas das outras. Podemos chamar esse relacionamento entre entidades de _encapsulamento_.[^encapsulation]

O encapsulamento é frequentemente descrito como apenas ocultando dados ou restringindo o acesso a eles, mas:

> A noção mais importante [de encapsulamento] é que um objeto deve garantir que nunca estará em um estado inválido... O objeto [encapsulado] sabe melhor o que significa “válido” e como fazer essa garantia[^codethatfits]

O encapsulamento deficiente leva a verificações repetidas no código e erros devido a dados inválidos. Ele pode ser detectado por abstrações com vazamento e alto acoplamento entre os módulos. Em um dos capítulos a seguir, discutiremos o acoplamento com mais detalhes, mas agora vamos nos concentrar em abstrações com vazamento.

Vamos tentar determinar o que há de errado com a função `makePurchase` no exemplo abaixo:

```js
// purchase.js
import { createOrder } from "./order";

async function makePurchase(user, cart, coupon) {
  if (!cart.products.length) throw new Error("Cart is empty!");

  const order = createOrder(user, cart);
  order.discount = coupon === "HAPPY_FRIDAY" ? order.total * 0.2 : 0;

  await sendOrder(order);
}
```

O principal problema com este código é que não há garantia de que a função `sendOrder` obterá um pedido _válido_. A função `makePurchase` _altera_ o estado do objeto `order` criado por _outro_ módulo. Ele se comporta como se soubesse qual estado para o objeto `order` é válido e qual não é. Mas:

---

**❗️ Certificar-se de que os dados são válidos é uma tarefa interna de um determinado módulo**

---

Simplificando, apenas o módulo que sabe aplicar um desconto no pedido pode fazê-lo corretamente. No nosso caso é `order.js`—o pedido foi criado por ele, então o desconto deve ser aplicado por ele também:

```js
// order.js
export function createOrder() {
  /*...*/
}

export function applyDiscount(order) {
  const discount = coupon === "HAPPY_FRIDAY" ? order.total * 0.2 : 0;
  return { ...order, discount };
}

// purchase.js
import { createOrder, applyDiscount } from "./order";

async function makePurchase(user, cart, coupon) {
  if (!cart.products.length) throw new Error("Cart is empty!");

  const order = createOrder(user, cart);
  const discounted = applyDiscount(order, coupon);
  await sendOrder(discounted);
}

// Agora certificando-se de que os dados do pedido são válidos
// é uma tarefa interna do módulo `order`,
// não é responsabilidade do código que o chama.
```

| A propósito 🥶                                                                                                                                                                            |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Em algumas linguagens, podemos proibir a alteração de dados após a criação usando estruturas imutáveis. Eles tornam impossível trazer os dados para um estado inválido de fora.           |
| Em JavaScript, podemos tornar os objetos imutáveis com `Object.freeze`,[^objectfreeze], mas isso geralmente é uma sobrecarga. Normalmente, é suficiente _tratar_ os dados como imutáveis. |

Os mesmos problemas se aplicam à verificação de vazio do carrinho. Embora a função `makePurchase` não altere os dados do carrinho, ela ainda se comporta como se soubesse qual estado do carrinho é válido e qual não é.

É melhor deixar a verificação de vazio para o módulo que cria o carrinho e sabe como mantê-lo válido:

```js
// cart.js
export function isEmpty(cart) {
  return !cart.products.length;
}

// purchase.js
import { isEmpty } from "./cart";
import { createOrder, applyDiscount } from "./order";

async function makePurchase(user, cart, coupon) {
  if (isEmpty(cart)) throw new Error("Cart is empty!");

  const order = createOrder(user, cart);
  const discounted = applyDiscount(order, coupon);
  await sendOrder(discounted);
}
```

No código atualizado, a função `makePurchase` não altera o estado do pedido diretamente e não decide se o carrinho é válido ou não. Em vez disso, ele chama a _API pública_ de outros módulos.

Isso não significa que todos os erros desaparecerão automaticamente após essa alteração - afinal, outros módulos podem conter erros. Mas separamos as responsabilidades entre os módulos, para sabermos o que corrigir se houver um erro nos dados do pedido ou na validação do carrinho.

Também limitamos o escopo da mudança no código. Enquanto a API pública dos módulos não for alterada, as atualizações e correções desses módulos _serão limitadas_ aos seus limites e não sairão deles.

Entre outras coisas, esse código é mais fácil de cobrir com testes e verificar os requisitos do projeto.

[^abstractionquote]: “Agile Principles, Patterns, and Practices in C#” by Robert C. Martin, https://www.goodreads.com/quotes/8806618-abstraction-is-the-elimination-of-the-irrelevant-and-the-amplification
[^codethatfits]: “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
[^shorttermmemory]: Working memory, Capacity, Wikipedia, https://en.wikipedia.org/wiki/Working_memory#Capacity
[^thinkingfastandslow]: “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow
[^hexflowers]: “Fractal hex flowers” by Mark Seemann, https://observablehq.com/@ploeh/fractal-hex-flowers
[^humaneinterface]: “The Humane Interface” by Jef Raskin, https://www.goodreads.com/book/show/344726.The_Humane_Interface
[^soc]: Separation of Concerns, Wikipedia, https://en.wikipedia.org/wiki/Separation_of_concerns
[^cohesion]: Cohesion in Computer Science, Wikipedia, https://en.wikipedia.org/wiki/Cohesion_(computer_science)
[^singleresponsibility]: The Single Responsibility Principle by Robert C. Martin, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_76/
[^srp]: Single Responsibility Principle, Principles of OOD, http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod
[^encapsulation]: Encapsulation, Wikipedia, https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)
[^objectfreeze]: `Object.freeze()`, MDN, https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze
