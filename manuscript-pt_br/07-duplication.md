# Duplicação de Código

O principal objetivo da refatoração é tornar o código mais legível. Uma maneira de conseguir isso é reduzir a quantidade de ruído nele.

A duplicação de código torna o código ruidoso porque não contém informações úteis. No entanto, nem toda duplicação é ruim e pode até ser uma ferramenta de desenvolvimento. Portanto, ao refatorar, devemos entender com que tipo de duplicação estamos lidando.

Neste capítulo, discutiremos o que analizar ao procurar por duplicação e como saber quando é hora de eliminá-la.

## Nem Toda Duplicação é Má

Se duas partes de código tiverem a mesma finalidade, contiverem o mesmo conjunto de ações e processarem os mesmos dados, isso será duplicação _direta_. Podemos nos livrar da parte duplicada com segurança extraindo o código repetido em uma variável, função ou módulo.

Mas há casos em que dois pedaços de código parecem “semelhantes”, mas depois se tornam diferentes. Se os mesclarmos muito cedo, será mais difícil dividir esse código mais tarde. Em geral, é muito mais fácil combinar código idêntico do que quebrar módulos mesclados anteriormente.

Quando não temos certeza de que estamos diante de dois códigos _realmente_ idênticos, podemos marcar esses locais com rótulos exclusivos. Podemos escrever uma suposição sobre o que está duplicado nesses rótulos.

```js
/** @duplicate Aplica um cupom de desconto ao pedido. */
function applyCoupon(order, coupon) {}

/** @duplicate Aplica um cupom de desconto ao pedido. */
function applyDiscount(order, discount) {}
```

Esses rótulos só serão úteis se realizarmos suas revisões regulares. Durante a revisão, devemos verificar se aprendemos algo novo sobre possíveis duplicatas, o que confirma que são idênticas ou não, mostrando a diferença entre elas.

| A propósito ⏰                                                                                                                                                                                                                                                                                                                                                              |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Considero auditorias regulares em meus projetos como parte do pagamento da dívida técnica (_technical debt_). Faço listas de tarefas para essas tarefas periódicas. Dentro da lista, eu especifico o que precisa ser feito dentro da tarefa. A técnica de auditorias regulares e seus benefícios estão bem descritos em “Jedi Techniques” de Maxim Dorofeev.[^jeditechnics] |

Se ficar aparente durante uma auditoria de um rótulo que a duplicação descrita nele é _direta_, podemos prosseguir com a refatoração do código com esse rótulo. Se o código for diferente, podemos remover o rótulo. Essa abordagem ajuda a não apressar a generalização do código, mas também a não esquecer os locais onde existe uma possível duplicação.

## Variáveis para Dados

Dados duplicados ou resultados de cálculos são convenientes para colocar em variáveis. Por exemplo, ajuda a desvendar condições complexas ou destacar as etapas das transformações de dados.

```js
// Se as condições forem relativamente próximas umas das outras:

if (user.age < 18) toggleParentControl();
// ...
if (user.age < 18) askParents();

// ...Podemos extrair a expressão em uma variável:

const isChild = user.age < 18;

if (isChild) toggleParentControl();
// ...
if (isChild) askParents();
```

Às vezes, a duplicação de dados pode ser menos óbvia e mais difícil de detectar:

```js
// A segunda condição é virada “de dentro para fora”
// e usa a variável `years` em vez do atributo do objeto.

if (user.age < 18) askParents();
// ...
const { age: years } = user;
if (years >= 18) askDocuments();

// Mas ainda podemos nos livrar dele,
// extraindo a expressão em uma variável:

const isChild = user.age < 18;

if (isChild) askParents();
// ...
if (!isChild) askDocuments();
```

| Mais informações 🔬                                                                                         |
| :---------------------------------------------------------------------------------------------------------- |
| Falaremos sobre simplificar e desvendar condições complexas com mais detalhes em um dos capítulos a seguir. |

## Funções para Ações

Podemos extrair ações duplicadas e transformações de dados em funções e métodos. Para detectá-los, podemos usar a “verificação de semelhança”:

- As ações são duplicadas se tiverem o mesmo objetivo – o resultado desejado;
- Ter o mesmo escopo — a parte do aplicativo que eles afetam;
- Ter a mesma entrada direta – argumentos e parâmetros;
- Ter a mesma entrada indireta — dependências e módulos importados.

No exemplo abaixo, os trechos de código passam neste teste:

```js
// - Objetivo: adicionar um campo com o valor do desconto absoluto ao pedido;
// - Escopo: o objeto do pedido;
// - Entrada direta: objeto da ordem, valor de desconto relativo;
// - Entrada indireta: função que converte porcentagens em valor absoluto.

// a)
const fromPercent = (amount, percent) => (amount * percent) / 100;

const order = {};
order.discount = fromPercent(order.total, 50);

// b)
const order = {};
const discount = (order.total * percent) / 100;
const discounted = { ...order, discount };

// As ações em “a” e “b” são as mesmas,
// então podemos extraí-los em uma função:

function applyDiscount(order, percent) {
  const discount = (order.total * percent) / 100;
  return { ...order, discount };
}
```

No outro exemplo, o objetivo e a entrada direta são os mesmos, mas as dependências são diferentes:

```js
// O primeiro trecho de código calcula o desconto em porcentagem,
// a segunda aplica o “desconto do dia”
// usando a função `todayDiscount`.

// a)
const order = {};
const discount = (order.total * percent) / 100;
const discounted = { ...order, discount };

// b)
const todayDiscount = () => {
  // ...Associe o desconto à data de hoje.
};

const order = {};
const discount = todayDiscount();
const discounted = { ...order, discount };
```

No exemplo acima, o fragmento “b” possui a função `todayDiscount` entre suas dependências. Por causa disso, os conjuntos de ações diferem o suficiente para serem considerados “semelhantes”, mas não “iguais”.

Podemos usar rótulos `@duplicate` e esperar um pouco para obter mais informações sobre como eles devem funcionar. Quando sabemos exatamente como essas funções devem funcionar, podemos “generalizar” as ações:

```js
// A função generalizada `applyDiscount` levará
// o valor do desconto absoluto:

function applyDiscount(order, discount) {
  return {...order, discount}
}

// Diferenças no cálculo (porcentagens, “desconto do dia”, etc.)
// são coletadas como um conjunto separado de funções:

const discountOptions = {
  percent: (order, percent)  => order.total * percent / 100
  daily: daysDiscount()
}

// Como resultado, obtemos uma ação generalizada para aplicar um desconto
// e um dicionário com descontos de vários tipos.
// Então, a aplicação de qualquer desconto agora se tornará uniforme:

const a = applyDiscount(order, discountOptions.daily)
const b = applyDiscount(order, discountOptions.percent(order, 40))
```

| Mais informações 🔬                                                                      |
| :--------------------------------------------------------------------------------------- |
| Discutiremos algoritmos generalizados, seu uso e parametrização em um capítulo separado. |

[^jeditechnics]: “Jedi Technics” by Maxim Dorofeev, Translated summary, https://bespoyasov.me/blog/jedi-technics/
