# Low-Hanging Fruit

Refactoring a piece of code can require various changes, and it can be difficult to choose how to start. To solve this, we can rank the changes from simple to complex and start with the simplest ones. It helps to “blow the dust off the code” and start to see more serious problems in it.

By simple changes we mean code formatting, linter errors and replacing self written code with features of the language or environment. In this chapter, we'll discuss these improvements and see why they're useful at the start of refactoring.

## Code Formatting

Code formatting is a matter of taste, but it has one useful function. If the code in the project is written _consistently_, it takes less time for readers to understand it. That's how habits work: the familiar “shapes” of code help you focus on the meaning instead of characters and words.

```
// Unformatted code.
// We have to focus harder to see its meaning:

function ProductList({ products }) {
   return <ul>{products.map((product) =><li key={product.name}>
       <Product product={product} /></li>)}</ul>}


// Formatted code makes it easier to read it:

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

It's better to automate code formatting. In the example above I used automatic code formatted called Prettier,[^prettier] but the particular tool is not as important here as the approach in general. If the team is not satisfied with Prettier, you can choose another formatter and use it. The point is to _automate the process_.

Sometimes it might happen that the formatter breaks the code, for example, when it carelessly moves a fragment to a new line:

```
// Before applying formatter:

function setDiscount(discount) {
  if (user.isVip) order.discount = discount; order.total -= discount
}

// After:

function setDiscount(discount) {
  if (user.isVip) {
    order.discount = discount;
  }

  order.total -= discount;
}
```

To notice such errors quicker, we need tests. If the tests run besides the editor, we'll instantly see what exactly was broken by the formatter.

Formatting can be a separate refactoring technique, so the result can be a commit or even a separate PR. Main goal is to integrate to the main branch as early as possible, so we don't have to handle complex merge conflicts between formatting and other code changes made by other developers.

## Code Linting

After turning linter “warnings” to “errors”, we might have a list of such errors. This list can be used as a task list for the current refactoring iteration.

I prefer to save the work on each of the linter rules as a separate commit or PR. For example, we could remove all unused code, make it a commit, and move on to the next problem on the list.

<figure>
  <img src="../images/05-linter.png" width="800">
  <figcaption><em>Linter highlights unused code that can be removed</em><br><br></figcaption>
</figure>

If there are a lot of errors after we turn on the linter, we can enable one rule at a time rather than all at once. The smaller the steps, the easier it is to break the problem into several and solve each one separately.

After fixing each rule, we'll need to check if the tests are broken. In the future, I will stop emphasizing test-checking to shorten the text. Let's just keep in mind that we check the tests after _each_ change.

## Language Features

Modern languages evolve and get new features. This is especially true for JavaScript, since the ES specification is updated every year.[^proposals]

Sometimes there are features in a new version of a language that can replace old self-written helper-functions. Built-in language features are faster, more reliable, and easier to work with. So if there's a chance for replacement we can use it.

| By the way 🥫                                                                                                              |
| :------------------------------------------------------------------------------------------------------------------------- |
| On the frontend, we might need to ensure the feature has required browser support. We can check it with Caniuse.[^caniuse] |

```js
// Self-written helper for checking the beginning of a string:
const startsWith = (str, chunk) => str.indexOf(chunk) === 0;
const yup = startsWith("Some String", "So");

// ...Can be replaced with the native string method:
const yup = "Some String".startsWith("So");
```

| However 💡                                                                                                                                                                                                                                               |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| If the self-written implementation is different from the native one and we can't replace it, I'd prefer to mention the difference in the documentation. That way it will be clear why we're using self-written function instead of the language feature. |

Removing code is beneficial: the less code there is, the fewer potential points of failure in the application. In general, my rule of thumb is “give most of the work to the language or environment than write myself”. It's usually more reliable.

## Environment Features

Along with the language features, we should also highlight the features of the text editor or IDE we're working with. If they have automated refactoring tools, it's worth learning them.

“Rename Symbol”, “Extract into Function” and other tools speed up the work and reduce the cognitive load. For example, in VS Code, we can change the name of a function or variable everywhere by using the hotkeys:[^vscode]

<figure>
  <img src="../images/05-rename-symbol.png" width="800">
  <figcaption><em>“Rename Symbol” updates the variable name everywhere</em><br><br></figcaption>
</figure>

However, we should double-check the result of applying these tools. For example, Rename Symbol may “miss” some name or add unnecessary renaming:

```tsx
// For example, we want to replace `name`
// with `firstName` in the `AccountProps`:

type AccountProps = { name: string };
const Account = ({ name }: AccountProps) => <>{name}</>;

// After applying “Rename Symbol”,
// there might appear an “extra renaming”:

type AccountProps = { firstName: string };
const Account = ({ firstName: name }: AccountProps) => <>{name}</>;
```

To avoid this, we should study the diff from the latest commit and check what was renamed and how.

<figure>
  <img src="../images/05-git-diff.png" width="800">
  <figcaption><em>Git shows exactly what has changed since the latest commit</em><br><br></figcaption>
</figure>

Strategy of small steps helps with simplifying and maximizing the benefit of such checks. If we apply only one refactoring technique per commit, there's no noise in the diffs and we can better see how exactly the changes affect the code.

Linters and tests help us avoid name conflicts and other bugs. For example, we can set up rules that forbid identical variable names, so the linter will error if there's a name conflict. If linter is running in the background with refactoring, we will see the error immediately and be able to fix it.

[^prettier]: Prettier, an opinionated code formatter, https://prettier.io
[^proposals]: List of EcmaScript Proposals, https://proposals.es
[^caniuse]: Can I Use, support tables for web, https://caniuse.com
[^vscode]: Refactoring Source Code in VSCode, https://code.visualstudio.com/docs/editor/refactoring
