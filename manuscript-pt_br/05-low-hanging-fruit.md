# Frutos mais baixos

A refatoração de um trecho de código pode exigir várias alterações e pode ser um desafio escolher como começar. Para resolver isso, podemos classificar as mudanças de simples para complexas e começar com as mais simples. Isso ajuda a “soprar a poeira do código” e começar a ver problemas mais graves nele.

Por mudanças simples, queremos dizer formatação de código, erros de linter e substituição de código auto-escrito por funcionalidades da linguagem ou ambiente. Neste capítulo, discutiremos essas melhorias e veremos por que elas são úteis no início da refatoração.

## Formatação de Código

A formatação do código é uma questão de gosto, mas tem uma função útil. Se o código no projeto for _consistente_, levará menos tempo para os leitores entendê-lo. É assim que os hábitos funcionam: as “formas” familiares de código nos ajudam a focar no significado em vez de caracteres e palavras.

```
// Código não formatado
// Nós temos que focar mais para entender o significado:

function ProductList({ products }) {
   return <ul>{products.map((product) =><li key={product.name}>
       <Product product={product} /></li>)}</ul>}


// Código formatado faz a leitura ser mais fácil:

function ProductList({ products }) {
  return (
    <ul>
      {products.map((product) => (
        <li key={product.name}>
          <Product product={product} />
        </li>
      ))}
    </ul>
  );
}
```

É melhor automatizar a formatação do código. No exemplo acima, usei código automático formatado chamado Prettier,[^prettier], mas a ferramenta específica não é tão importante aqui quanto a abordagem geral. Se a equipe não estiver satisfeita com o Prettier, podemos escolher outro formatador e usá-lo. O objetivo é _automatizar o processo_.

Às vezes, o formatador pode quebrar o código, por exemplo, quando move um fragmento descuidadamente para uma nova linha:

```
// Antes de aplicar o formatador:

function setDiscount(discount) {
  if (user.isVip) order.discount = discount; order.total -= discount
}

// Depois:

function setDiscount(discount) {
  if (user.isVip) {
    order.discount = discount;
  }

  order.total -= discount;
}
```

Para perceber esses erros mais rapidamente, precisamos de testes. Se os testes forem executados ao lado do editor, veremos instantaneamente o que exatamente foi quebrado pelo formatador.

A formatação pode ser uma técnica de refatoração separada, então o resultado pode ser um _commit_ ou até mesmo um PR independente. O objetivo principal é integrar a _main branch_ o mais cedo possível para que não tenhamos que lidar com conflitos de _merge_ complexos entre a formatação e outras alterações de código feitas por outros desenvolvedores.

## Linting de Código

Depois de transformar “alertas” de linter em “erros”, podemos ter uma lista de tais erros. Essa lista pode ser uma lista de tarefas para a iteração de refatoração atual.

Eu prefiro salvar o trabalho em cada regra de linter como um _commit_ ou PR separado. Por exemplo, poderíamos remover todo o código não utilizado, torná-lo um commit e passar para o próximo problema da lista.

<figure>
  <img src="../images/05-linter.png" width="800">
  <figcaption><em>Linter destaca código não utilizado que pode ser removido</em><br><br></figcaption>
</figure>

Se o linter apresentar muitos erros, podemos transformar as regras uma a uma em vez de todas simultaneamente. Quanto menores os passos, mais fácil é dividir o problema em vários e resolver cada um separadamente.

Depois de corrigir cada regra, precisaremos verificar se os testes foram aprovados. No futuro, deixarei de enfatizar a verificação de teste para encurtar o texto. Vamos ter em mente que verificamos os testes após _cada_ alteração.

## Funcionalidades da Linguagem

As linguagens modernas evoluem e ganham novos recursos. Isso é especialmente verdadeiro para JavaScript, pois a especificação ES é atualizada anualmente.[^proposals]

Às vezes, um novo recurso de versão da linguagem pode substituir as antigas funções auxiliares auto-escritas. As funcionalidades de linguagem integradas são mais rápidas, confiáveis e fáceis de trabalhar. Então, se houver uma chance de substituição, podemos usá-la.

| A propósito 🥫                                                                                                                               |
| :------------------------------------------------------------------------------------------------------------------------------------------- |
| No frontend, talvez seja necessário garantir que o recurso tenha o suporte necessário ao navegador. Podemos verificar com Caniuse.[^caniuse] |

```js
// Método auxiliar Auto-escrito para verificar o começo de uma string.
const startsWith = (str, chunk) => str.indexOf(chunk) === 0;
const yup = startsWith("Some String", "So");

// ...Pode ser substituído com o método de string nativo:
const yup = "Some String".startsWith("So");
```

| Entretando 💡                                                                                                                                                                                                                                   |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Se a implementação auto-escrita for diferente da nativa e não pudermos substituí-la, eu prefiro mencionar a diferença na documentação. Dessa forma, ficará claro por que estamos usando uma função auto-escrita em vez do recurso de linguagem. |

A remoção de código é benéfica: quanto menos código houver, menos pontos potenciais de falha no aplicativo. Podemos usar a regra “dar a maior parte do trabalho à linguagem ou ambiente do que escrever nós mesmos”. Geralmente é mais confiável.

## Recursos do Ambiente

Junto com os recursos de linguagem, também devemos destacar os recursos do editor de texto ou IDE com o qual estamos trabalhando. Se eles tiverem ferramentas de refatoração automatizadas, vale a pena aprendê-las.

“_Rename Symbol_”, “_Extract into Function_” e outras ferramentas aceleram o trabalho e reduzem a carga cognitiva. Por exemplo, no VS Code, podemos alterar o nome de uma função ou variável em qualquer lugar usando as teclas de atalho:[^vscode]

<figure>
  <img src="../images/05-rename-symbol.png" width="800">
  <figcaption><em>“Rename Symbol” atualiza o nome da variável em todos os lugares</em><br><br></figcaption>
</figure>

No entanto, devemos verificar novamente o resultado da aplicação dessas ferramentas. Por exemplo, Rename Symbol pode “perder” algum nome ou adicionar renomeação desnecessária:

```tsx
// Por exemplo, nós queremos subtituir `name`
// por `firstName` em `AccountProps`:

type AccountProps = { name: string };
const Account = ({ name }: AccountProps) => <>{name}</>;

// Depois da aplicação do “Rename Symbol”
// Pode ser que apareça um “renoamento extra”:

type AccountProps = { firstName: string };
const Account = ({ firstName: name }: AccountProps) => <>{name}</>;
```

Para evitar isso, devemos estudar o diff do último commit e verificar o que foi renomeado e como.

<figure>
  <img src="../images/05-git-diff.png" width="800">
  <figcaption><em>Git mostra exatamente o que mudou desde o último commit</em><br><br></figcaption>
</figure>

Uma estratégia de pequenos passos ajuda a simplificar e maximizar os benefícios dessas verificações. Se aplicarmos apenas uma técnica de refatoração por commit, não haverá ruído nos _diffs_ e poderemos ver melhor como as alterações afetam o código.

Linters e testes nos ajudam a evitar conflitos de nomes e outros bugs. Por exemplo, podemos configurar regras que proíbem nomes de variáveis idênticos, para que o linter apresente um erro se houver um conflito de nomes. Se o linter estiver sendo executado em segundo plano com refatoração, veremos o erro imediatamente e poderemos corrigi-lo.

[^prettier]: Prettier, an opinionated code formatter, https://prettier.io
[^proposals]: List of EcmaScript Proposals, https://proposals.es
[^caniuse]: Can I use, support tables for the web, https://caniuse.com
[^vscode]: Refactoring Source Code in VSCode, https://code.visualstudio.com/docs/editor/refactoring
