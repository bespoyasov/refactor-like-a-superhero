# Generics, Inheritance, and Composition

During refactoring, we may discover “hidden patterns” in different parts of code. These can be similar algorithms or function details that can be generalized.

Generalization can both simplify the code and make it more complex. In this chapter, we'll talk about how to understand when it's worth generalizing similar parts of code and when it isn't. We'll also discuss hierarchies and inheritance and explain why composition is preferable to inheritance.

## Generic Algorithms

A generic algorithm is an extension of the ideas of abstraction and code deduplication.

If several functions work “by the same scheme,” we can formalize this “work scheme” as a set of operations. These operations would describe the function in an abstract, “generic” way without referring to specific variables in the code.

For example, instead of manually going through the elements of an array:

```js
const items = [1, 2, 3, 4, 5];

for (const i of items) {
  console.log(i);
}
// 1 2 3 4 5

for (const i of items) {
  console.log(i - 1);
}
// 0 1 2 3 4
```

...We can extract the “work scheme.” Formalized, it'd sound like this: “iterate over an array, apply a specified function to each element.” In the example below, the `forEach` method encapsulates this “scheme”:

```js
const printNumber = (i) => console.log(i);
const printNumberMinusOne = (i) => console.log(i - 1);

// Iterate over `items`, apply the `printNumber` function to each element:
items.forEach(printNumber);
// 1 2 3 4 5

// Iterate over `items`, apply the `printNumberMinusOne` function to each element:
items.forEach(printNumberMinusOne);
// 0 1 2 3 4
```

The generic algorithm doesn't refer to specific variables and functions. Instead, it expects them as parameters:

> Iterate over a _specified_ array; apply a _specified_ function to each element

Such an algorithm relies on arguments' _properties_. An array is iterable; a function is callable and accepts an array element as an argument. The combination of these properties is the algorithm _contract_. The basis of generalized programming is identifying such “work schemes” and defining their contracts.

To understand how generic algorithms can be helpful and how to identify them, let's look at a financial management application. In the code below, there are two functions for counting expenses and incomes throughout the history of records:

```ts
// The type describes the history of spending and income in the application:
type RecordHistory = List<Entry>;

// The record in the history contains its kind, creation date, and amount:
type EntryKind = "Spend" | "Income";
type Entry = {
  type: EntryKind;
  created: TimeStamp;
  amount: MoneyAmount;
};

// Calculates how much is spent in total:
function calculateTotalSpent(history: RecordHistory) {
  return history.reduce(
    (total, { type, amount }) => total + (type === "Spend" ? amount : 0),
    0
  );
}

// Calculates how much is added in total:
function calculateTotalAdded(history: RecordHistory) {
  let total = 0;
  for (const { type, amount } of history) {
    if (type === "Income") total += amount;
  }
  return total;
}
```

Let's say we now want to calculate a total for a certain period, for example, today. When adding it, we should check if the new function has any common features with the existing ones:

```ts
// In the `calculateSpentToday` function, you can see the “work scheme”,
// similar to the algorithms in `calculateTotalSpent` and `calculateTotalAdded`:
// - Receive a history array
// - Filter the necessary records
// - Calculate the sum

function calculateSpentToday(history: RecordHistory) {
  return history.reduce(
    (total, { type, created }) =>
      total +
      (type === "Spend" && created >= today && created < tomorrow ? amount : 0),
    0
  );
}
```

Note that each function boils down to two tasks: filter the history and summarize the `amount` field in the filtered records. We can extract these tasks and generalize them:

```ts
type HistorySegment = List<Entry>;
type EntryPredicate = (record: Entry) => boolean;

// Filtering can now be used separately.
// The `keepOnly` function can filter the history of records
// by _any_ criterion that implements the `EntryPredicate` type:

const keepOnly = (
  history: RecordHistory,
  criterion: EntryPredicate
): HistorySegment => history.filter(criterion);

// Summation can now also be used separately.
// We can pass _any_ fragment of history to the `totalOf` function,
// and it will calculate the amount of spending or income in it:

const totalOf = (history: HistorySegment): MoneyAmount =>
  history.reduce((total, { amount }) => total + amount);
```

Then we can combine the original algorithms from these two functions—this will be an implementation of the generalized algorithm. All that's _different_ is going to be used as _parameters_ of this generalized algorithm:

```ts
// Only the filter criteria differ; everything else is the same:
const isIncome: EntryPredicate = ({ type }) => type === "Income";
const isSpend: EntryPredicate = ({ type }) => type === "Spend";
const madeToday: EntryPredicate = ({ created }) =>
  created >= today && created < tomorrow;

// The filtering criterion is passed as a “parameter” for `keepOnly`:
const added = keepOnly(history, isIncome);
const spent = keepOnly(history, isSpend);

// The sum can then be calculated for any fragment of history:
totalOf(spent);
totalOf(added);
totalOf(keepOnly(spent, madeToday));
```

Generalization is useful when we're entirely sure of the “work scheme.” If several pieces of code are the same or only slightly different, generalization can help reduce duplication.

But generalization can also make the code more complicated. If we suspect that the “scheme” may change in the future, it's probably too early to generalize. The heuristics here are the same as when dealing with code duplication. Until we're sure of our code knowledge, it's better to hold off on generalizing.

| By the way 💡                                                                                                                                                     |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| We have discussed the heuristics of working with code duplication and the lack of information about the code base in more detail in one of the previous chapters. |

## Generic Types

Sometimes we can find similar details not only in algorithms but in data types. A _type_ is a set of values with some specific properties.[^typesarenotclasses][^typesandtypeclasses] Similar types can be combined into a generic one, but we should follow the rule:

---

**❗️ Generalize only when we're sure there won't be any exceptions**

---

Data is harder to change than code. Careless type generalization can weaken typing and force us to add unnecessary checks to the code. Let's go back to the `Entry` type from the snippet above for an example:

```ts
type EntryKind = "Spend" | "Income";
type Entry = {
  type: EntryKind;
  created: TimeStamp;
  amount: MoneyAmount;
};
```

If we know that only records with these properties can be found in the history, then the type describes the domain well. But if there's a possibility that there may be other records in history, then we may have problems with this type.

For example, let's say the history can show user comments that don't contain `amount` but contain text:

```ts
type EntryKind = "Spend" | "Income" | "Comment";
type Entry = {
  type: EntryKind;
  created: TimeStamp;

  // The generalization weakens the `amount` field,
  // and with it the entire `Entry` type:
  amount?: MoneyAmount;

  // Here, a new field appears.
  // It “may or may not exist” —
  // this also weakens the type.
  content?: TextContent;
};
```

There's a contradiction in the `Entry` type. Its `amount` and `content` fields are optional, so the type “allows” entries without amount and text. But this reflects the domain incorrectly: a comment _must_ contain text, and spending records and incomes _must_ have amounts. Without this condition, the data in the history of records is invalid.

The problem is that the `Entry` type with optional fields tries to mix _different_ entities and data states. We can check this by creating a React component to display the sum of the record from the history on the screen:

```tsx
type RecordProps = {
  record: Entry;
};

// We pass an `Entry` type record to the component,
// to output the amount of spending or income:
const Record = ({ record }: RecordProps) => {
  // But internally, we have to filter this data.
  // If the record is a comment, the render has to be skipped,
  // because comments don't contain amounts:
  if (record.type === "Comment") return null;

  const sign = isIncome(record) ? "+" : "–";

  // And here, we also have to tell the compiler,
  // that `amount` is “definitely there, we checked!”
  return `${sign} ${record.amount!}`;
};
```

The comment rendering in this component doesn't really fit. To fix this problem, we first need to understand what we know about the domain:

- Can there only be comments, or can other text messages appear?
- Can there be new record types that have the `amount` field?
- Can there be new record types that will have completely different fields?

We can't always answer these questions. When we lack knowledge of the domain or program requirements, it's too early to generalize the types. In such cases, it is preferable to _compose_ types instead of generalizing:

```ts
// We split the type into several.
// Unnecessary fields are gone,
// data states are separated more clearly,
// and static typing is more helpful this way.
type Spend = { type: "Spend"; created: TimeStamp; amount: MoneyAmount };
type Income = { type: "Income"; created: TimeStamp; amount: MoneyAmount };
type Comment = { type: "Comment"; created: TimeStamp; content: TextContent };

// Yup, we “wrote more code,” but in the early stages of the project,
// it's more important for us to understand the domain
// and grasp the app essence and the relationships between entities.

// Atomic types like the ones above are more flexible,
// because we can compose them by different properties
// that are important to us in a particular situation.

// For example, in the `FinanceEntry` type below
// we compose only the records that _contain an amount_.
// We can walk through the lists of such records with the `amount` adder:
type FinanceEntry = Spend | Income;

// In the type `MessageEntry` we compose records _containing text_.
// Right now, there's only one such type, so `MessageEntry` is just a type alias,
// but if there are more records with text, we can extend this type further.
type MessageEntry = Comment;

// The most general type `Entry` is represented as a choice of _all_ possible options.
// With these records, we can do only what we can do with _any_ record:
type Entry = FinanceEntry | MessageEntry;

// For example, `Entry` can be sorted by the `created` date,
// since the `created` field is guaranteed for all records:
const sortByDate = (a: Entry, b: Entry) => a.created - b.created;
```

After refactoring, the `Record` component no longer needs extra checks:

```ts
// In the props, we don't specify the `Entry` type but `FinanceEntry`.
// By definition, it has the `amount` field, so we don't need
// any additional checks during the rendering of the component anymore.

type RecordProps = {
  record: FinanceEntry;
};

const Record = ({ record }: RecordProps) => {
  const sign = isIncome(record) ? "+" : "–";
  return `${sign} ${record.amount}`;
};
```

| By the way 👀                                                                                                                                                                                      |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Even if we consider TypeScript's “bad typing,” this is still useful. If we design an app to avoid premature generalizations, we'll save the code from unnecessary checks.                          |
| Of course, there's no guarantee that we won't get the wrong data type in the component's props in production. But with proper alert monitoring and error handling, we can quickly find and fix it. |

The basic idea of composition is not to generalize _early_. Type composition is easier to extend as application requirements become more complex. For example, assume we needed to add overdraft records and system text messages. Let's add the `Overdraft` and `Warning` types:

```ts
type Spend = { type: "Spend"; created: TimeStamp; amount: MoneyAmount };
type Income = { type: "Income"; created: TimeStamp; amount: MoneyAmount };
type Overdraft = { type: "Overdraft"; created: TimeStamp; amount: MoneyAmount }; // New.

type Comment = { type: "Comment"; created: TimeStamp; content: TextContent };
type Warning = { type: "Warning"; created: TimeStamp; content: TextContent }; // New.

// In the unions, we only need to add a new variant
// to each place that needs to be expanded:
type FinanceEntry = Spend | Income | Overdraft;
type MessageEntry = Comment | Warning;
type Entry = FinanceEntry | MessageEntry;

// If necessary, we can recompose the types from scratch,
// to compose them by other properties.
```

Knowing enough about the domain allows us to see patterns and generalize types. (We know enough when the code of the entities has stopped changing.) But this isn't a necessary step:

```ts
// If we're sure of the type structure now,
// we can generalize the types.

type FinanceEntry = {
  type: "Spend" | "Income" | "Overdraft";
  created: TimeStamp;
  amount: MoneyAmount;
};

type MessageEntry = {
  type: "Comment" | "Warning";
  created: TimeStamp;
  content: TextContent;
};
```

## Inheritance and Composition

In the example above, it's easy to start using _generic types_ for describing `Entry`.[^generics] The motivation might be that the generic type would allow determining the record kind “on the fly”:

```ts
type FinanceEntryKind = "Spend" | "Income" | "Overdraft";
type MessageEntryKind = "Comment" | "Warning";
type EntryKind = FinanceEntryKind | MessageEntryKind;

// Type fields and behavior will then be defined
// by the type-argument `TKind`:

type Entry<TKind extends EntryKind> = {
  type: TKind;
  created: TimeStamp;
  amount: TKind extends FinanceEntry ? MoneyAmount : never;
  content: TKind extends MessageEntryKind ? TextContent : never;
};

type Income = Entry<"Income">;
type Comment = Entry<"Comment">;
```

But it's best not to rush with generics, either. Generics are like “blueprints” for types. To use them, we have to make sure that the structure of the type doesn't change.

| Be careful 🚧                                                                                              |
| :--------------------------------------------------------------------------------------------------------- |
| This section is one big IMHO. I could be totally wrong. Stay alert, and don't believe me without question. |

Generics are fine for cases where we know the type structure but don't know its details. For example, in the code above, we had a type `EntryPredicate`:

```ts
type EntryPredicate = (record: Entry) => boolean;
```

A predicate is _by definition_ a function that returns a boolean value, so we know the structure exactly:

```ts
type SomethingPredicate = (x: Something) => boolean;
```

If, for some reason, we need to create _multiple_ predicates with different arguments, which we _don't know in advance_, the generic would describe such a “blueprint” perfectly:

```ts
// Predicate with an “abstract parameter”:
type Predicate<T> = (x: T) => boolean;

// Predicates with “specific parameters”:
type EntryPredicate = Predicate<Entry>;
type HistoryPredicate = Predicate<History>;
type WhateverPredicate = Predicate<Whatever>;
```

Here we're sure that the _structure of the type is known and won't change_. We can pass any type argument, and it won't affect the type structure or its behavior. In `Entry<TKind>`, we can't give such a guarantee, at least in the early stages of design.

### Inheritance

Since we're talking about composition and entities' “blueprints”, let's talk about OOP and inheritance. In OOP-style projects, it's better to avoid deep inheritance hierarchies:

```ts
class Task {
  public start(): void {}
}

class AdvancedTask extends Task {
  public configure(settings: TaskSettings): void {}
}

class UserTask extends AdvancedTask {
  public definedBy: User;
}

// Class `UserTask` is the 3rd in the inheritance chain:
// Task -> AdvancedTask -> UserTask
```

Deep hierarchies are brittle. They claim to know the domain perfectly, but the model cannot describe the world _perfectly_, so sooner or later, the hierarchy will divert from the real world. If we haven't provided some functionality in the base class, the broken hierarchy will force us to add it to it or redefine the inheritance chain.

Instead of inheritance, it's preferable to use composition. In OOP code, composition usually means the implementation of interfaces:

```ts
// Declare several interfaces,
// each of them describes a cohesive set of features:

interface SimpleTask {
  start(): void;
}

interface Configurable {
  configure<TSettings>(settings: TSettings): void;
}

interface UserDefined {
  definedBy: User;
}

// A class can implement several interfaces,
// bypassing unnecessary inheritance steps:

class UserTask implements SimpleTask, Configurable, UserDefined {}

// Then, when adding a new feature, it'll be enough to
// extend the list of interfaces with the new one and implement it:

interface Cancellable {
  stop(): void;
}

class UserTask
  implements SimpleTask, Restartable, Configurable, UserDefined, Cancellable {}

// In the case of inheritance, we would have to
// change one of the base classes
// or change the inheritance structure.
```

| However 📝                                                                                                                                                                                                  |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| We're not talking about the abstract class implementation here.[^abstractclass] It, on the contrary, can be helpful. But even with abstract classes, avoiding hierarchies deeper than 1-2 levels is better. |

In JavaScript, however, there's one use case where a deeper inheritance can be useful. If in a project, we use panics to handle errors but want to avoid verbose checks with `instanceof` for _each_ error type, we can use “error layer” hierarchies:

```ts
// Application error base class:
class AppError extends Error {}

// Error classes divided by “app layers”:
// API layer and its subclasses per API error.
class ApiError extends AppError {}
class NotFound extends ApiError {}
class BadRequest extends ApiError {}

// Validation layer and subclasses per validation error.
class ValidationError extends AppError {}
class InvalidUserDto extends ValidationError {}
class MissingPostData extends ValidationError {}

// Then, when handling we can reduce the number of checks,
// by replacing the checks for every possible type:
if (e instanceof NotFound) {
} else if (e instanceof BadRequest) {
} else if (e instanceof InvalidUserDto) {
} else if (e instanceof MissingPostData) {
}

// ...With checks for the app layer errors:
if (e instanceof ApiError) {
} else if (e instanceof ValidationError) {
}
```

This way, we can reduce the number of `instanceof` checks, but it'll only work if the error handling is the same for _all_ errors from the same layer.

### The Liskov Substitution Principle

Previously, we gave an example with a component that had excessive checks:

```ts
type Entry = Spend | Income | Comment;
type RecordProps = { record: Entry };

const Record = ({ record }: RecordProps) => {
  if (record.type === "Comment") return null; // Filters out comments.

  const sign = isIncome(record) ? "+" : "–";
  return `${sign} ${record.amount}`;
};
```

When we see such conditions, we should check if the Liskov substitution principle is violated.[^lsp][^solid] This principle in Martin's formulation sounds like this:

> Functions that use references to the base type must be able to use its subtypes without knowing it[^martinlsp]

In practice, the `Entry` type describes _any_ history record. It doesn't make a difference between a comment, spending, or an income. Therefore, we can only pass it to a function that is _ready to work with any record_.

That is, if a function works only with expenses or incomes, but _not comments_, then the `Entry` type cannot be passed to such a function. It can't because the function may want to read the `amount` field on the record, but the comment doesn't have this field. So to prevent errors, we have to _filter out_ the comments before using the function.

Conversely, if the function can work with any record and only uses the `created` field, for example, it can be passed arguments of the `Entry` type since the `created` field exists on all the `Entry` “subtypes.”

| Read more 🔬                                                                                                                                                                                                                                               |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| For a better understanding of the LSP, it's worth diving into the concept of variance,[^variance][^variancematters][^variancekotlin] but that's a big separate topic. We'll skip it now, but I'll leave some references to the subject in the source list. |

To avoid violating the substitution principle, we have to pass the “smallest common type” to the `Record` component:

```ts
type FinanceEntry = Income | Spend;
type RecordProps = { record: FinanceEntry };

const Record = ({ record }: RecordProps) => {
  // No additional checks are required.
  // The `FinanceEntry` type for sure contains
  // everything the function can work with.

  const sign = isIncome(record) ? "+" : "–";
  return `${sign} ${record.amount}`;
};
```

The substitution principle helps to see what types can be composed together. We can use it as an “integration linter,” highlighting premature generalizations and incorrect abstractions.

It also helps to find places for _correct_ generalizations. For example, when we split the problem of `total` calculation into filtering and summarizing, we extracted a function that can filter not only by type but generally by whatever:

```ts
type EntryPredicate = (record: Entry) => boolean;
type HistorySegment = List<Entry>;

type HistoryFilter = (
  history: RecordHistory,
  criterion: EntryPredicate
) => HistorySegment;

// As filter criteria, we can use not only these functions:
const isIncome = ({ type }) => type === "Income";
const isSpend = ({ type }) => type === "Spend";

// But also these:
const beforeToday = ({ created }) => created < today;
const madeToday = ({ created }) => created >= today && created < tomorrow;

// And these:
const addedToday = (record) => isIncome(record) && madeToday(record);
const spentBeforeToday = (record) => isSpend(record) && beforeToday(record);
```

In `HistoryFilter` we can now pass _any_ implementation of type `EntryPredicate`. This composition flexibility is the main benefit of the substitution principle. We can search for such places using the LSP and refactor them in the code first.

| Simplification 🚧                                                                                                                                                                                               |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| We won't discuss how the substitution principle relates to pre- and postconditions and how those should behave. But I'll leave some links that I can recommend you to read about this topic.[^designbycontract] |

[^typesarenotclasses]: “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
[^typesandtypeclasses]: Types and Typeclasses, Learn You Haskell, http://learnyouahaskell.com/types-and-typeclasses
[^generics]: Generics in TypeScript, TypeScript Docs https://www.typescriptlang.org/docs/handbook/2/generics.html
[^lsp]: “A behavioral notion of subtyping” by Barbara H. Liskov, Jeannette M. Wing, https://dl.acm.org/doi/10.1145/197320.197383
[^solid]: The Principles of OOD, Robert C. Martin, http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod
[^martinlsp]: The Liskov Substitution Principle, https://web.archive.org/web/20151128004108/http://www.objectmentor.com/resources/articles/lsp.pdf
[^variance]: Covariance and Contravariance, Wikipedia, https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)
[^variancematters]: “Why variance matters” by Ted Kaminski https://www.tedinski.com/2018/06/26/variance.html
[^variancekotlin]: “The Ins and Outs of Generic Variance in Kotlin” by Dave Leeds, https://typealias.com/guides/ins-and-outs-of-generic-variance/
[^designbycontract]: “Design By Contract”, c2.com, https://wiki.c2.com/?DesignByContract
[^abstractclass]: Abstract Classes and Members, TypeScript Handbook, https://www.typescriptlang.org/docs/handbook/2/classes.html#abstract-classes-and-members
