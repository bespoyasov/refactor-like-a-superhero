# Static Typing

In previous chapters, when talking about readability, we mainly focused on common language features like variables, functions, and modules. We occasionally paid attention to types but didn't dive into the details of working with them.

However, static typing can also be a tool for writing expressive code. We can use types and interfaces to convey additional information to the reader or to make the app design more explicit.

In this chapter, we'll discuss how to express more domain knowledge through types and make invalid data transformations unrepresentable. We'll look at how to use ubiquitous language in type signatures and track down errors in API design using types.

| Before we start 💬                                                                                                                                |
| :------------------------------------------------------------------------------------------------------------------------------------------------ |
| We won't discuss if one _should_ use static typing or not. Instead, we'll focus on how to use types as a refactoring tool.                        |
| Static typing is a controversial topic, not everyone likes it, and it's fine. If your team doesn't use or like typing, you can skip this chapter. |

## Ubiquitous Language

In “Domain Modeling Made Functional”, Scott Wlaschin warns readers against primitive obsession.[^dmmf][^primitiveobsession] To avoid it, he suggests using types to describe the domain.

| Be careful 🚨                                                                                                                                    |
| :----------------------------------------------------------------------------------------------------------------------------------------------- |
| The idea of replacing primitives with domain types can be controversial in the team. Discuss this idea with other developers before applying it. |

Data types and function signatures can carry information about the task the code is solving. They can reflect the context of the task, the interaction between entities, and even the business workflow model as a whole. When used thoughtfully, types can even become an alternative to documentation:

```ts
// Primitive types don't reflect the context,
// they lack the details of the domain:

type Account = {
  date: string;
  user: number;
  value: number;
};

// Domain types help describe entity relations
// and how they work together:

type Account = {
  date: DateTimeIso;
  user: UserId;
  value: MoneyAmount;
};
```

When type names use domain terms, they form a part of the _ubiquitous language_[^ddd][^ubiquitouslanguage]. It's the language used by product owners and people who are directly related to business workflows.

| Read more 👀                                                                                         |
| :--------------------------------------------------------------------------------------------------- |
| We talked in more detail about ubiquitous language in the chapters on entity names and architecture. |

The benefit of ubiquitous language is its _unambiguity_. If the whole team, including the non-developers, uses the same terms, there's less chance of “translation loss.” It makes it easier to spot bugs and errors in the domain model at the early stages.

## Domain Modeling

In many statically typed languages, data transformations are conveniently expressed through functional types.[^functionaltype] A set of such types can describe the business workflows—model the domain. The benefit of such a model is that the cost of error in it is lower than in the workflow implementation.

Types help to build a top-level understanding of how the system works. In such a model, we can see the interaction of its parts, the module contracts, and the data used. But it also shows design errors and inconsistencies between the model and the real world.

Errors in types are easier to fix than errors in implementation. With types, it's possible to design an application _before_ we start implementing it. We write a draft, look at the flaws in the model, fix them, and check the model again:

```ts
// Describe the data used in the application.
// Indicate different states it goes through
// at different stages of the app life cycle:

type CreatedOrder = {
  createdAt: TimeStamp;
  user: UserId;
  items: ProductList;
};

type ValidatedOrder = {
  /*...*/
};

type DiscountedOrder = {
  /*...*/
};

type Order = CreatedOrder | ValidatedOrder | DiscountedOrder;

// Design the domain workflows
// that transform the data:

type CreateOrder = (user: UserId, items: ProductList) => CreatedOrder;
type ValidateOrder = (order: CreatedOrder) => ValidatedOrder;
type ApplyDiscount = (order: ValidatedOrder, value: Price) => DiscountedOrder;

// If we notice an error in any of the types,
// (for example, the designed workflow is different from what happens in reality)
// we can quickly and relatively cheaply fix the model:

type ApplyDiscount = (
  order: ValidatedOrder,
  coupon: DiscountCoupon
) => DiscountedOrder;
```

During refactoring, such checks help to detect workflows that don't meet the project requirements or violate the domain constraints:

```ts
type Divider = (a: number, b: number) => number;
const divide: Divider = (a, b) => a / b;

// Should `divide` take zero as a second argument?
// If yes, how to handle the division by zero?
// Should we return the container from this function?

// We can answer these questions beforehand by enriching
// the domain model with additional data and constraints:

type RealNumber = // ...Represents any number.
type NaturalNumber = // ...Represents integers bigger than 0.

type Divider =  (a: NaturalNumber, b: NaturalNumber) => RealNumber;
const divide: Divider = (a, b) => a / b;

// Now from the `Divider` type,
// we can see that the division by zero
// should be handled by the calling side.
```

### Types in TypeScript

In TypeScript, we can use several ways to model the domain and create domain types:[^typealias][^typebranding][^factorymethod]

- Type aliases
- Classes
- Type branding

The easiest, but the most unreliable way to create domain types is to use type aliases.[^typealias] They're convenient to give primitive types informative names but difficult to convey the _constraints_ of the domain. For example, such code is quite valid _syntactically_ but not from the _domain point of view_:

```ts
// A type alias can give the primitive a useful name,
// which reflects the type meaning according to the domain:
type NaturalNumber = number;

// But it won't force the domain constraints:
const x: NaturalNumber = -1;
```

By default, type aliases don't validate assigned values, so there's no guarantee that `NaturalNumber` will be a natural number:

```ts
function divide(a: NaturalNumber, b: NaturalNumber): RealNumber {
  return a / b;
}

// Compiler's happy but, at the runtime, there's an error:
divide(1, 0);
```

So if we need to distinguish between types and enforce value validation, we have to use classes or branded types:[^typebranding][^factorymethod]

```ts
// When using classes, we can add validation
// to the class constructor:

class NaturalNumber {
  constructor(value) {
    if (value <= 0 || Math.floor(value) !== value) {
      throw new Error("The value must be a positive integer.");
    }

    this.value = value;
  }
}

// Then it'll be impossible to create an incorrect value:

new NaturalNumber(-1); // Error!
new NaturalNumber(42); // NaturalNumber

// But the classes are quite verbose
// and aren't convenient to use as wrappers over a primitive.
// It takes a lot of code to create “numbers” via `new NaturalNumber(42)`
// and somehow implement arithmetic operations with these values.

// The second option is to use type branding:

type Tagged<T, S> = T & { __tag: S };
type NaturalNumber = Tagged<number, "natural">;

// And create values only via factory functions.
// The validation then can be placed in those functions:

function naturalFrom(x: number): NaturalNumber {
  if (value <= 0 || Math.floor(value) !== value) {
    throw new Error("The value must be a positive integer.");
  }

  return x as NaturalNumber;
}

naturalFrom(-1); // Error!
naturalFrom(42); // NaturalNumber
```

The problem with classes and type branding is that we have to watch if they're used correctly. That is, we'll need to write linter rules for their use or search for mistakes during code reviews. This isn't reliable.

It's hard to recommend a particular method here, it all depends on the project and the needs of the team. However, we can note that for _purely descriptive_ purposes, even the type aliases work quite fine. Often a domain model built with type aliases is already enough to find previously undetected design errors.

## Model and Reality

Business workflows transform data from one state to another. Types can help to syntactically fixate these states and name them. When each step of the transformation has a name, it's easier for us to reason about the whole process and find errors in its logic.

In the example below, the `sendRecoverLink` function accepts an object of type `User` as an argument. This type has a `verified` flag but there are no rules explaining _when and why_ this flag becomes `true`:

```ts
type User = {
  id: string;
  verified?: boolean;
};

async function sendRecoverLink(user: User) {
  if (!user.verified) return false;
  await api.recoverPassword(user.id);
}
```

With the current `User` type implementation, the `sendRecoverLink` function accepts data that is invalid half the time. We can prevent passing invalid data by making it more difficult at the type level.

User verification is probably a separate business workflow that _results in_ a verified user object. This causal relationship can be expressed directly in types after we separate the types of verified and unverified users:

```ts
// Describe the states of verified
// and unverified users, as different types:

type CreatedUser = { name: string };
type VerifiedUser = { name: string; verified: true };

type User = CreatedUser | VerifiedUser;

// Show the causality in the functional type of the verification process.
// Describe what steps the data goes through and how the user becomes verified:

type VerifyUser = (user: CreatedUser) => Promise<VerifiedUser>;

// Fixate restrictions on password recovery.
// Only a verified user should be able to restore the password:

type RecoverPassword = (user: VerifiedUser) => Promise<void>;

// Now it becomes impossible to send a recovery link to an unverified user.
// The invalid data transformation becomes unrepresentable in the code:

const sendRecoverLink: RecoverPassword = async (user) => {
  await api.recoverPassword(user.id);
};

sendRecoverLink(unverifiedUser); // Error!
```

Again, in TypeScript, it's difficult to achieve bullet-proof “unrepresentability” of invalid transformations, in other typed languages it may be much easier. But even just showing different data states as types helps to notice errors in the business logic at the design stage.

It's easier to notice such errors in types because, unlike in implementation, branching in types is much more verbose and difficult. This forces us to describe workflow types declaratively and linearly. If we then notice ambiguity in the described model, we can act on it.

### Violation of Agreements

Explicit types help with detecting violations of agreements or project rules. For example, the `sendRecoverLink` function from the previous example violates CQS:

```ts
async function sendRecoverLink(user: User) {
  if (!user.verified) return false;
  await api.recoverPassword(user.id);
}

type RecoverPassword = (user: User) => Promise<false | void>;

// Here, `false` is an attribute of a query,
// while `void` is an attribute of a command.
```

Types draw attention to such contradictions. We can further improve the function, for example, by using the `try*` pattern in its name:

```ts
async function trySendRecoverLink(user: User) {
  if (!user.verified) return false;
  await api.recoverPassword(user.id);
}

type TryRecoverPassword = (user: User) => Promise<false | void>;

// The `try*` pattern in the name explicitly says that the function
// is still a command but may return `false` in some cases.
```

This way we at least make the expectations of the function more explicit. But it's better, of course, to go further and refactor the function according to CQS.

| In detail 💡                                                                                      |
| :------------------------------------------------------------------------------------------------ |
| We discussed CQS and separating logic from effects in more detail in the chapter on side effects. |

## API Design

During refactoring, static typing can help to check how clear the API of a module or function is. For example, from the signature of a function, we can check the clarity of its API by “xxxing out” its name and the names of its arguments:

```ts
function getPostContents(user: number, post: string): Promise<string> {}
// ->
function xxx(xxx: number, xxx: string): Promise<string> {}
```

If the function signature makes little sense, we can improve it until we see the purpose of the function:

```ts
function xxx(xxx: number, xxx: string): Promise<string> {}
// ->
function xxx(xxx: UserId, xxx: PostSlug): Promise<string> {}
// ->
function xxx(xxx: UserId, xxx: PostSlug): Promise<PostContents> {}

// number -> UserId: the first argument is the user ID;
// string -> PostId: the second argument is the publication URL;
// string -> PostContents: the result is the content of the publication.

// From the signature, the function's work becomes clearer:
// we query the content of a particular post using a key
// built from the user ID and the publication URL.
```

Informative signatures make the function meaning clearer and the information density of the code higher. Such signatures carry part of the task context, so we can convey additional knowledge to the reader in the names of functions and arguments:

```ts
function fetchPost(authorId: UserId, post: PostSlug): Promise<PostContents> {}

// getPostContents -> fetchPost: means that the data is requested over the network;
// userId -> authorId: tells how exactly the user is associated with these posts.
```

| By the way 📚                                                                                                           |
| :---------------------------------------------------------------------------------------------------------------------- |
| The “xxxing out” technique is described in more detail in “Code That Fits in Your Head” by Mark Seemann.[^codethatfits] |

The same rule can be used to check if the code follows engineering practices required in the project, for example, the CQS principle:

```ts
class PostReader {
  constructor(private postSource: PostStorage) {}

  getPost(id) {
    this.contents = this.postSource.fetchPost(id);
  }
}

// The signature of the `getPost` method is more like a command than a query:

type GetPost = (id: PostId) => void;

// Perhaps we should rework the API or choose another name for the method:

class PostReader {
  // ...

  readPost(id: PostId): void {
    this.contents = this.postSource.fetchPost(id);
  }
}
```

| By the way ❌                                                                                                                                                |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The “xxxing out” technique is more useful for designing public APIs to make them more informative. For non-public functions, it may be a bit less important. |

[^dmmf]: “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
[^ddd]: “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
[^ubiquitouslanguage]: “Ubiquitous Language” by Martin Fowler, https://martinfowler.com/bliki/UbiquitousLanguage.html
[^typealias]: Type Aliases, TypeScript Handbook, https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases
[^typebranding]: “Branding and Type-Tagging” by Kevin B. Greene, https://medium.com/@KevinBGreene/surviving-the-typescript-ecosystem-branding-and-type-tagging-6cf6e516523d
[^factorymethod]: Factory Method, Refactoring Guru, https://refactoring.guru/design-patterns/factory-method/typescript/example
[^codethatfits]: “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
[^primitiveobsession]: “Primitive Obsession”, Refactoring Guru, https://refactoring.guru/smells/primitive-obsession
[^functionaltype]: More on Functions, TypeScript Documentation, https://www.typescriptlang.org/docs/handbook/2/functions.html
