# Pipeline Funcional

Como vimos no capítulo anterior, a abstração ajuda a dividir o programa por “níveis de detalhe”. Prestamos atenção aos detalhes mais importantes em cada nível e usamos os termos mais adequados.

Em aplicativos de usuário, um desses níveis descreve a _lógica de negócios_ — os processos de domínio que tornam o aplicativo único e lucrativo. Esses, em outras palavras, são os problemas que os negócios querem que os desenvolvedores resolvam.

| Por exemplo 👀                                                                                                                                        |
| :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Para uma loja online, a lógica de negócios seria criar pedidos e fazer check-out. Para empresa de transporte - otimização de rota e carga de tráfego. |

A lógica de negócios fala na linguagem do domínio. Ele descreve os fluxos de trabalho como sequências de eventos e consequências: “Quando um usuário insere um cupom de desconto, o aplicativo verifica sua validade e reduz o preço do pedido”.

Tal linguagem está “longe do código”. Devido ao alto nível de abstração, traduzir fluxos de trabalho de negócios em código pode ser difícil. O pipeline funcional, que discutiremos neste capítulo, ajuda a descrever fluxos de trabalho de negócios em código com mais precisão e mais próximo da realidade.

## Transformações de Dados

Os fluxos de trabalho de negócios são transformações de dados. Por exemplo, aplicar um desconto a um pedido pode ser expresso como uma transição de um estado de dados para outro:

```
“Aplicação de Cupom”:

[Pedido Criado] + [Cupom Válido] -> [Pedido com Desconto]
```

Em aplicativos maiores, as transformações podem ser mais estendidas e os dados podem passar por várias etapas:

```
“Selecionando Recomendações de Produtos”:

[Carrinho de Produtos] + [Histórico de Compras] ->
  [Categorias de Produtos] + [Pesos de Recomendação] ->
  [Lista de Recomendações]
```

Em código mal organizado, os fluxos de trabalho de negócios não se assemelham a essas cadeias. Eles são complicados, não óbvios e muitas vezes não falam a língua do domínio. Como resultado, em vez de uma descrição clara do fluxo de trabalho, acabamos com algo como:

```
“Selecionando Recomendações de Produtos”:

[Carrinho de Produtos] + ... + [Mágica 🔮] -> [Lista de Recomendações]
```

Em código bem organizado, os fluxos de trabalho parecem lineares e os dados neles passam por várias etapas, uma de cada vez. O estado final dos dados é o resultado desejado do fluxo de trabalho.

<figure>
  <img src="../images/09-functional-pipeline.png" width="800">
  <figcaption><em>Os dados passam por uma cadeia de diferentes estados. A saída é o resultado do fluxo de trabalho desejado</em><br><br></figcaption>
</figure>

Esse tipo de organização de código é chamado de _pipeline funcional_. Ao refatorar a lógica de negócios, podemos nos concentrar nela para tornar os fluxos de trabalho no código mais claros e aparentes.

## Estados de dados

Em “Domain Modeling Made Functional”, Scott Wlaschin descreve como projetar um programa baseado em fluxos de trabalho de negócios e seus dados.[^dmmf] A sugestão é representar as etapas do fluxo de trabalho como funções separadas. Podemos usar essa ideia como base para refatoração.

Para fazer isso, primeiro precisamos destacar todos os estágios pelos quais os dados passam. Essas etapas nos ajudarão a entender como dividir o fluxo de trabalho e o que deve ser considerado no código.

Vamos analisar essa abordagem com o exemplo de uma loja online. Suponha que a função `makeOrder` compile um pedido, aplique cupons de desconto e adicione produtos promocionais:

```js
function makeOrder(user, products, coupon) {
  if (!user || !products.length) throw new InvalidOrderDataError();
  const data = {
    createdAt: Date.now(),
    products,
    total: totalPrice(products),
    discount: selectDiscount(data, coupon),
  };

  if (!selectDiscount(data, coupon)) data.discount = 0;
  if (data.total >= 2000 && isPromoParticipant(user)) {
    data.products.push(FREE_PRODUCT_OF_THE_DAY);
  }

  data.id = generateId();
  data.user = user.id;
  return data;
}
```

A função não é grande, mas faz bastante:

- Valida os dados de entrada;
- Cria um objeto de pedido;
- Aplica um desconto do cupom passado;
- Adiciona produtos promocionais sob certas condições.

À primeira vista, é difícil distinguir cada uma dessas ações no código da função. Há tantos detalhes que é difícil ver as etapas individuais do fluxo de trabalho.

| Não apenas isso 💊                                                                                                                                                                                                                          |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| O objeto _order_ muda dentro da função caoticamente. E embora formalmente, seu encapsulamento não esteja quebrado – o objeto é criado e alterado na mesma função – parece que `makeOrder` está “tentando fazer o trabalho de outra pessoa”. |

Vamos destacar as etapas do fluxo de trabalho e os estados de dados que aparecem neles:

```
“Mostrar ordem na interface do usuário”:

- “Validar dados de entrada”:
  [Entrada não validada bruta] -> [Usuário validado] + [Lista de produtos validados]

- “Criar pedido”:
  [Usuário] + [List de Produtos] -> [Pedido Criado]

- “Aplicar cupom de desconto”:
  [Pedido] + [Cupom] -> [Pedido com desconto]

- “Aplicar promoção”:
  [Pedido] + [Usuário] -> [Pedido com produtos promocionais]
```

Nosso objetivo é fazer com que o código da função se pareça com esta lista durante a refatoração. Podemos começar agrupando o código em “seções”, cada uma das quais representará uma etapa diferente do fluxo de trabalho:

```js
function makeOrder(user, products, coupon) {
  // Validar dados de entrada:
  if (!user || !products.length) throw new InvalidOrderDataError();

  // Criar pedido:
  const data = {
    createdAt: Date.now(),
    products,
    total: totalPrice(products),
  };
  data.id = generateId();
  data.user = user.id;

  // Aplicar cupom de desconto:
  const discount = selectDiscount(data, coupon);
  data.discount = discount ?? 0;

  // Aplicar promoção:
  if (data.total >= 2000 && isPromoParticipant(user)) {
    data.products.push(FREE_PRODUCT_OF_THE_DAY);
  }

  return data;
}
```

Agrupar as etapas ajudará a encontrar problemas de abstração no código: se pudermos pensar em um nome significativo para uma etapa, provavelmente poderemos extrair seu código em uma função. No exemplo acima, os comentários com os nomes das etapas refletem totalmente sua intenção. Vamos extrair as etapas em funções separadas:

```js
// Criar pedido:
function createOrder(user, products) {
  return {
    id: generateId(),
    createdAt: Date.now(),
    user: user.id,

    products,
    total: totalPrice(products),
  };
}

// Aplicar cupom de desconto:
function applyCoupon(order, coupon) {
  const discount = selectDiscount(order, coupon) ?? 0;
  return { ...order, discount };
}

// Aplicar promoção:
function applyPromo(order, user) {
  if (!isPromoParticipant(user) || order.total < 2000) return order;

  const products = [...order.products, FREE_PRODUCT_OF_THE_DAY];
  return { ...order, products };
}
```

A função `makeOrder` ficaria assim:

```js
function makeOrder(user, products, coupon) {
  if (!user || !products.length) throw new InvalidOrderDataError();

  const created = createOrder(user, products);
  const withDiscount = applyCoupon(created, coupon);
  const order = applyPromo(withDiscount, user);

  return order;
}
```

Após as alterações, as etapas do fluxo de trabalho são encapsuladas em funções separadas. Essas funções apenas alteram o objeto da ordem, mantendo-o válido. A função `makeOrder` não altera mais os dados incontrolavelmente, mas apenas chama essas funções. Isso torna os pedidos inválidos menos prováveis e o teste de transformações de dados mais fácil.

O código de `makeOrder` agora se assemelha à lista de etapas do fluxo de trabalho com as quais começamos. Os detalhes de cada etapa estão ocultos atrás do nome da função correspondente. O nome descreve toda a etapa, o que facilita a leitura do código.

Além disso, ao adicionar uma nova etapa ao fluxo de trabalho, agora só precisamos inserir uma nova função no lugar certo. As outras conversões permanecem inalteradas:

```js
function makeOrder(user, products, coupon, shipDate) {
  if (!user || !products.length) throw new InvalidOrderDataError();

  const created = createOrder(user, products);
  const withDiscount = applyCoupon(created, coupon);
  const withPromo = applyPromo(withDiscount, user);
  const order = addShipment(withPromo, shipDate); // Nova etapa do fluxo de trabalho.

  return order;
}
```

E quando removemos uma etapa, é mais fácil encontrar a função a ser excluída — excluir uma chamada de função garante a remoção de todo o código relacionado a essa etapa no processo.

| A propósito 🕵️                                                                                                                                                                                                                           |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nem sempre é fácil identificar um pipeline. A lógica de negócios pode ser “dispersa” pela base de código. Nesses casos, pode ser útil desenhar um diagrama de como as partes do aplicativo se comunicam entre si para descobrir padrões. |

## Estados Inválidos Irrepresentáveis

Alguns fluxos de trabalho de negócios só precisam de dados em determinados estados para funcionar. Por exemplo, não queremos enviar um pedido até que seja pago ou se ele perder o endereço de entrega. Esses pedidos são inválidos para este fluxo de trabalho.

Podemos tornar o código mais confiável se “proibirmos” a passagem de dados inválidos. Para fazer isso, podemos projetar o código de forma a dificultar ou impossibilitar a passagem de dados inválidos.

Por exemplo, podemos fazer isso usando tipos em linguagens com tipagem estática. Podemos descrever cada estado de dados como um tipo separado e especificar quais tipos são válidos para um fluxo de trabalho. _Isso adiciona restrições de domínio diretamente à assinatura_ de funções e métodos.

| Entretanto 👀                                                                                                                                                                                                                                                                                                                        |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Claramente, temos que nos lembrar das restrições de uma determinada linguagem. Por exemplo, no TypeScript, é mais difícil obter “validação na assinatura” e ainda será limitado pelo tempo de execução JS. Mas mesmo sem a “verdadeira validação”, essa técnica ajuda a refletir mais conhecimento de domínio diretamente no código. |

Por exemplo, vejamos o tipo `CustomerEmail`, que descreve o endereço de e-mail do usuário da loja:

```ts
type CustomerEmail = {
  value: EmailAddress;
  verified: boolean;
};
```

O tipo tem um sinalizador `verified` que mostra se o email foi verificado. O problema com a flag é que ela não explica _em quais condições_ ela será `true`. Não há conhecimento suficiente no tipo sobre verificação de e-mail.

---

**❗️ O código tem que compensar essa falta de conhecimento de alguma forma. Na maioria das vezes, com verificações de dados extras em tempo de execução**

---

Por exemplo, imagine um link na interface do usuário da loja para restaurar a conta do usuário. Ao ser clicado, ele deve encaminhar o usuário para a página “Redefinir Senha”, mas somente se seu e-mail for verificado:

```ts
function restoreAccount(email: CustomerEmail): void {
  if (email.verified) {
    // Envie o usuário para a página “Redefinir senha”.
  } else {
    return;
  }
}
```

Com a implementação atual do `CustomerEmail`, a função `restoreAccount` aceita dados inválidos na metade do tempo.

Pode ser bom se o tipo contiver apenas um desses sinalizadores. Mas quanto mais sinalizadores houver, mais estados diferentes o tipo terá simultaneamente e mais prováveis serão os erros devido aos dados inconsistentes.

Podemos resolver isso separando diferentes estados de dados em diferentes tipos:

```ts
// Para emails não verificados, usamos um tipo:
type UnverifiedEmail = {
  /*...*/
};

// ...E para e-mails verificados, usamos outro:
type VerifiedEmail = {
  /*...*/
};

// O “qualquer email” pode ser usado
// quando a verificação não é importante:
type CustomerEmail = UnverifiedEmail | VerifiedEmail;
```

Então, para diferentes fluxos de trabalho, podemos exigir diferentes tipos de dados:

```ts
// Se a função se preocupa com a verificação de e-mail,
// pode exigir um tipo específico em sua assinatura:

function restorePassword(email: VerifiedEmail): void {}
function verifyEmail(email: UnverifiedEmail): void {}

// Se a função puder lidar com qualquer email, ela poderá usar o tipo comum.
// Desta forma, podemos ver os requisitos para verificação de e-mail
// direito na assinatura da função:

function isValidEmail(email: CustomerEmail): boolean {}
```

Agora, as assinaturas de função são mais precisas na descrição do domínio porque transmitem mais conhecimento sobre ele. As funções `restorePassword` e `verifyEmail` alertam sobre seus requisitos e restrições. A função `isValidEmail` diz que está pronta para lidar com qualquer email, e a verificação não é essencial.

| No entanto 🚧                                                                                                                                                            |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| No caso do TypeScript, os aliases de tipo podem não ser suficientes. Podemos querer garantir que não possamos criar um e-mail não verificado com o tipo `VerifiedEmail`. |
| Para isso, podemos usar type branding[^typebranding] ou concordar em criar entidades apenas com classes especiais ou fábricas.                                           |
| No entanto, para fins descritivos - transferir conhecimento sobre o domínio - os aliases podem ser suficientes.                                                          |

## Validação de dados

O pipeline funcional depende da execução de código linear. As etapas de um fluxo de trabalho são executadas uma após a outra e passam os dados pela cadeia de transformações.

Para que essa ideia funcione, os dados no fluxo de trabalho devem estar seguros e não interromper o pipeline. No entanto, não podemos garantir que quaisquer dados “externos” estejam seguros. Portanto, no código, queremos separar as zonas onde os dados podem ser confiáveis e onde não podem.

| Ilhas de segurança 🏝                                                                                          |
| :------------------------------------------------------------------------------------------------------------ |
| Os fluxos de trabalho de negócios idealmente devem se tornar “ilhas” onde os dados são verificados e seguros. |

DDD tem um análogo para tais ilhas—_bounded contexts_.[^boundedcontext][^ddd][^dmmf] Simplificando, um contexto limitado é um conjunto de funções que se referem a alguma parte de um aplicativo.

De acordo com o DDD, validar dados é mais conveniente _nos limites_ dos contextos, por exemplo, na entrada do contexto. Neste caso, “dentro” do contexto, não precisamos de verificações adicionais porque os dados já estão validados e seguros.

<figure>
  <img src="../images/09-bounded-context.png" width="800">
  <figcaption><em>Toda validação ocorre nos limites; os dados dentro do contexto são considerados válidos e seguros</em><br><br></figcaption>
</figure>

Podemos usar essa regra em nosso código para nos livrarmos de verificações de dados desnecessárias em tempo de execução. Ao validar os dados no início do fluxo de trabalho, podemos assumir posteriormente que atenderá aos nossos requisitos.

Então, por exemplo, no componente `CartProducts`, em vez de verificações ad-hoc para a existência de produtos e suas propriedades dentro da função de renderização:

```js
function CartProducts({ items }) {
  return (
    !!items && (
      <ul>
        {items.map((item) =>
          item ? <li key={item.id}>{item.name ?? "—"}</li> : null
        )}
      </ul>
    )
  );
}
```

...Verificaríamos os dados uma vez no início do fluxo de trabalho:

```js
function validateCart(cart) {
  if (!exists(cart)) return [];
  if (hasInvalidItem(cart)) return [];

  return cart;
}

// ...

const validCart = validateCart(serverCart);
```

...E mais tarde o usaria sem verificações adicionais:

```js
function CartProducts({ items }) {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

### Estados ausentes

Muitas vezes, a validação de entrada nos ajuda a descobrir estados de dados que não notamos anteriormente. Por exemplo, o código do componente `CartProducts` no exemplo de código anterior ficou mais simples e as falhas dele ficaram mais fáceis de detectar:

```js
// Se renderizarmos um carrinho válido, mas vazio,
// o componente renderizará uma lista vazia:

const validEmptyCart = [];
<CartProducts items={validEmptyCart} />;

// Resultados em <ul></ul>
```

O estado “Carrinho vazio” é válido, mas representa um caso extremo. Juntamente com a validação de entrada, o pipeline funcional torna esses casos mais perceptíveis porque eles saem da execução de código “regular”. E quanto mais proeminentes forem os casos extremos, mais cedo poderemos detectá-los e tratá-los:

```js
// Para corrigir o problema com a lista vazia, podemos dividir
// os estados “Carrinho vazio” e “Carrinho com produtos”
// em diferentes componentes:

const EmptyCart = () => <p>The cart is empty</p>;
const CartProducts = ({ items }) => {};

// Então, ao renderizar, podemos primeiro lidar com todos os casos extremos,
// e então prosseguir para o Caminho Feliz:

function Cart({ serverCart }) {
  const cart = validateCart(serverCart);

  if (isEmpty(cart)) return <EmptyCart />;
  return <CartProducts items={cart} />;
}
```

| Com tudo 💡                                                                                                                                                                                                                                                          |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Lembramos que a refatoração não deve alterar a funcionalidade do código, então é melhor corrigir os bugs separadamente. Em um dos últimos capítulos, discutiremos como resolver problemas encontrados no código, mas não misturar refatoração com correções de bugs. |

Esse tratamento de casos extremos, como no componente `Cart`, nos ajuda a detectar mais casos extremos em potencial nos estágios iniciais de desenvolvimento. A consideração desses casos extremos torna o programa mais confiável e preciso na descrição dos fluxos de trabalho de negócios.

| A propósito 👀                                                                                                                                                                                                |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Lidar com casos extremos antes de trabalhar com o Caminho Feliz pode lembrá-lo da técnica chamada “Retorno antecipado”. Discutiremos isso mais de perto no capítulo sobre condições e complexidade de código. |

### DTO e Desserialização

A validação no início também é útil se os dados puderem ser corrompidos pela serialização ou desserialização.[^serialization]

Normalmente, as informações entre as partes de um sistema são transferidas como um _Data Transfer Object, DTO_.[^dto][^ddd] Esses são "pacotes de informações" que viajam de uma parte de um aplicativo para outro - do servidor para o cliente, por exemplo exemplo.

A estrutura e o formato dos DTOs são intencionalmente simples: apenas strings, números, booleanos, arrays e objetos. Por exemplo, JSON, que é frequentemente usado para comunicação entre o servidor e o cliente, não possui tipos ou estruturas complexas.

Durante a “tradução” entre tipos de domínio complexos e DTOs intencionalmente simples, algo pode dar errado e os dados podem se tornar inválidos. Esses dados podem interromper o fluxo de trabalho se não forem verificados antes do uso.

| Em detalhe 📚                                                                                                                                                                                                                  |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Você pode ler mais sobre pipeline funcional, fluxos de trabalho de negócios, contextos vinculados, validação de dados e DDD em “Domain Modeling Made Functional” de Scott Wlaschin.[^dmmf] Ótimo livro, altamente recomendado. |

## Mapeamento de dados e seletores

Podemos precisar dos mesmos dados para propósitos diferentes. Por exemplo, a interface do usuário pode renderizar um carrinho de compras de forma diferente dependendo das configurações do usuário.

O pipeline funcional sugere “preparar” dados para tais situações com antecedência. Por exemplo, podemos querer selecionar antecipadamente os fragmentos necessários dos dados originais, transformar alguns conjuntos de dados em outros ou até mesmo mesclar vários conjuntos de dados em um.

| Cuidado com a simplificação 🚧                                                                                                                                                    |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Da descrição acima, você pode se lembrar de alguns termos: mapeamento, projeção, fatia, lente e mapeamento.[^mappers][^projections][^slices][^lenses]                             |
| Decidi não usá-los neste livro para evitar a introdução de muitos conceitos novos. Em vez disso, usarei a palavra “seletor” no texto como sinônimo geral para todos esses termos. |

Os seletores de dados ajudam a desacoplar módulos que usam dados semelhantes, mas ligeiramente diferentes. Por exemplo, vamos ver o componente `CartProducts`, que renderiza um carrinho de compras:

```js
function CartProducts({ serverCart }) {
  return (
    <ul>
      {serverCart.map((item) => (
        <li key={item.id}>
          {item.product.name}: {item.product.price} × {item.count}
        </li>
      ))}
    </ul>
  );
}
```

No momento, ele depende da estrutura de dados do carrinho, que vem do servidor. Se a estrutura mudar, teremos que mudar o componente também:

```js
// Se os produtos começarem a chegar separadamente, teremos que pesquisar
// por um determinado produto durante a renderização.

function CartProducts({ serverCart, serverProducts }) {
  return (
    <ul>
      {serverCart.map((item) => {
        const product = serverProducts.find(
          (product) => item.productId === product.id
        ).name;

        return (
          <li key={item.id}>
            {product.name}:{product.price} × {item.count}
          </li>
        );
      })}
    </ul>
  );
}
```

Os seletores de dados podem nos ajudar a desacoplar a resposta do servidor e a estrutura de dados que usamos para renderização. Por exemplo, podemos representar tal seletor como uma função:

```js
// A função `toClientCart` “converte” os dados em uma estrutura,
// que os componentes do aplicativo usarão.

function toClientCart(cart, products) {
  return cart.map(({ productId, ...item }) => {
    const product = products.find(({ id }) => productId === id);
    return { ...item, product };
  });
}
```

Então, vamos converter os dados usando esta função antes de renderizar o componente:

```js
const serverCart = await fetchCart(userId)
const cart = toClientCart(serverCart, serverProducts)

// ...

<CartProducts items={cart} />
```

O componente então contará com a estrutura de dados que é definida por _nós_, não por terceiros:

```js
function CartProducts({ items }) {
  return (
    <ul>
      {items.map(({ id, count, product }) => (
        <li key={id}>
          {product.name} {product.price} × {count}
        </li>
      ))}
    </ul>
  );
}
```

Portanto, se a resposta da API for alterada, não precisaremos atualizar todos os componentes que usam esses dados. Só precisaríamos atualizar a função seletora.

| A propósito 🔌                                                                                                                                                                                |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Isso é especialmente útil se o servidor frequentemente quebrar a compatibilidade com o código do cliente. Esta técnica pode ser considerada um caso especial do padrão ”_Adapter_”.[^adapter] |

Também é conveniente usar seletores se a interface do usuário tiver representações diferentes para os mesmos dados. Por exemplo, além do carrinho, a loja pode ter uma lista de todos os produtos disponíveis com alguns deles marcados como “No carrinho”.

Para renderizar essa lista, podemos reutilizar dados já existentes, mas “prepará-los” de uma maneira um pouco diferente:

```js
// Usamos os dados do servidor existente,
// mas cria uma estrutura diferente:

function toClientShowcase(products, cart) {
  return products.map((product) => ({
    ...product,
    inCart: cart.some(({ productId }) => productId === product.id),
  }));
}
```

O componente `Showcase` não precisa saber nada sobre a resposta do servidor. Funciona apenas com o resultado do seletor:

```js
function Showcase({ items }) {
  return (
    <ul>
      {items.map(({ product, inCart }) => {
        <li key={product.id}>
          {product.name} <input type="checkbox" checked={inCart} disabled />
        </li>;
      })}
    </ul>
  );
}
```

Essa abordagem ajuda a separar a responsabilidade entre o código: os componentes se preocupam apenas com a renderização e os seletores “convertem” os dados.

Os componentes tornam-se menos acoplados ao resto do código porque dependem de estruturas que controlamos totalmente. Facilita, por exemplo, substituir um componente por outro se precisarmos atualizar a interface do usuário.

Testar transformações de dados fica mais fácil porque qualquer seletor é uma função regular. Não precisamos de uma infraestrutura complexa para testá-lo. Testar transformações em um componente, por exemplo, exigiria sua renderização.

Temos mais controle sobre os dados que queremos (ou não queremos) incluir no resultado do seletor. Podemos manter apenas os campos que um determinado componente usa e filtrar todo o resto.

Geralmente, é melhor aplicar a seleção logo após a validação dos dados. Nesse ponto, nós _já sabemos_ que os dados estão seguros, mas _não confiamos_ em sua estrutura em nenhum lugar do código ainda. Isso nos dá a capacidade de converter os dados para uma tarefa específica.

| Observação 🔗                                                                                                                                                                                                                 |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Os dados neste exemplo passam pela cadeia de estados: “Dados brutos do servidor” → “Dados válidos” → “Preparado para uma tarefa” → “Exibido na interface do usuário”.                                                         |
| O pipeline funcional ajuda a descrever _qualquer tarefa_ como uma cadeia semelhante. Isso facilita a decomposição porque as cadeias ajudam a construir um modelo mental claro do fluxo de trabalho que expressamos no código. |

[^dmmf]: “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
[^typebranding]: “Branding and Type-Tagging” by Kevin B. Greene, https://medium.com/@KevinBGreene/surviving-the-typescript-ecosystem-branding-and-type-tagging-6cf6e516523d
[^boundedcontext]: “Bounded Context in DDD” by Martin Fowler, https://www.martinfowler.com/bliki/BoundedContext.html
[^dto]: Data Transfer Object, DTO, Wikipedia, https://en.wikipedia.org/wiki/Data_transfer_object
[^ddd]: “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
[^projections]: Projection operations, C# Guide, https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/projection-operations
[^mappers]: “Data Mapper” by Martin Fowler, https://martinfowler.com/eaaCatalog/dataMapper.html
[^slices]: API Slice Overview, Redux Toolkit Docs, https://redux-toolkit.js.org/rtk-query/api/created-api/overview#api-slice-overview
[^lenses]: “Lenses in Functional Programming” by Albert Steckermeier, https://sinusoid.es/misc/lager/lenses.pdf
[^adapter]: Adapter Pattern, Refactoring Guru, https://refactoring.guru/design-patterns/adapter
[^serialization]: Serialization, Wikipedia, https://en.wikipedia.org/wiki/Serialization
