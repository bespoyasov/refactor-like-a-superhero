# Comments and Documentation

Usually, refactoring only affects the application code and its tests. But sometimes, we'll need to improve comments in the code, project documentation, and development process during refactoring.

In this chapter, we'll talk about how and why to refactor these things. We'll discuss how to look for conflicting sources of information about the project and what to pay attention to in the development process.

## Sources of Truth

Work on a project consists of various tasks. We write code and test it, discuss constraints and the domain, clarify the project requirements, etc. In the process, we produce code, documentation, project plans, backlogs, and more.

From the _application_ point of view, the only important part is the code. The code directly affects the program execution, and everything else doesn't.

| By the way 💬                                                                                                                                                                      |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| In JavaScript, some tools use JSDoc comments as signatures,[^jsdocts] but these are more _types_ than comments. By comments, we'll mean those that don't affect program execution. |

On the other hand, developers consider the code and everything “around it.” We pay attention to comments, documentation, and code reviews to better understand a particular task and the whole project.

Usually, everything “around the code” gets out of date faster than the code itself. Because of this, information from different sources can be contradictory, and we might doubt what to trust. For example, in the snippet below, the comment conflicts with the function signature:

```js
/**
 * @param {User} Current user object.
 * @param {Product[]} List of products for the order.
 * @return {Order}
 */
function createOrder(userId, products) {
  /**
   * In the annotation, the first argument is typed as `User`
   * but the implementation seems to use `UserId`.
   * This contradiction questions how the code should really work:
   * - What's correct: the code or the annotation?
   * - What has changed last? Why has that decision been made?
   * - What does the documentation say? What do the product owners say?
   */
}
```

The snippet above offers us two contradictory statements. Such contradictions make it hard to read the code and slow down development. We can't be sure we understand the code correctly until we resolve those contradictions. Such a resolution can take effort and time.

To avoid such problems, we can conduct reviews of comments, documentation, and other sources of information about the project. During these reviews, we should identify the contradictions and resolve them.

## Comments

We can use several tools and heuristics to “refactor” comments:

### Correct or Delete False Comments

Comments can lie and tell the reader incorrect information. Sometimes the lies can be blatant:

```js
/** @obsolete Not used anymore across the code base. */
function isEmpty(cart) {}

// ...

// The comment above lies the function _is_ used.
if (!isEmpty(cart)) {
}
```

However, sometimes the lie can be less noticeable. In this case, to see if a suspicious comment is false, we should try to disprove its statement.

To do so, we can ask for help from other developers or documentation. For example, we can find developers who know exactly how the function should work and ask them about the suspicious comment.

When there's no way to contact the team or use documentation, we can conduct a series of experiments on the suspicious code. We'll need to cover this code with tests and observe how they behave when we change the function.

If we're convinced that the comment is false, we should delete it or rewrite it to remove all contradictions. It's important to note this step in the commit message. It's helpful to describe why we suspected the comment was false, and what exactly helped reveal the lie.

| By the way 💡                                                                                                                                                                                                         |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Just like other refactoring techniques, comment refactoring is better done in small steps and separate commits. It helps to describe the context of the problem and the purpose of the changes in the commit message. |

### Clarify Vague Comments

For vague, inaccurate, or ambiguous comments, we should add details. We can add examples of function calls, links to specific pull requests, tasks, or issues in the task tracker. For example, such a comment isn't constructive:

```js
// Custom sorting function:
function sort(a, b) {}
```

The fact that the `sort` function “sorts stuff” is clear from its name. Instead, it'd be better to describe what sorting algorithm is used, add examples of how it works, and links to a detailed description of the algorithm:

```js
/**
 * Implements the most efficient sorting algorithm
 * by comparing electron movement in the circuitry.
 * @see https://wiki.our-project.app/sorting/electron-movement-sort/
 * @example ...
 *
 * @param {Sortable} a
 * @param {Sortable} b
 * @return {CompareResult}
 */
function superFastSort(a, b) {}
```

### Inline Small Details in the Code

Comments containing small details can be “inlined” in the types and names of variables, functions, or methods:

```ts
// Fetches post contents by the author's ID.
async function getPost(user) {}

// We can express it like this ↓
async function fetchPostContents(authorId) {}

// Or even like this using types ↓
async function fetchPost(authorId: UserId): Promise<PostContents> {}
```

This technique doesn't always work. It can make the variable or function name too long. In such cases, it's better not to apply the changes.

### Describe Context Instead of “Rephrasing Names”

Some comments are unhelpful because they only rephrase the name of the commented entity with different words:

```js
// Compares strings.
function compareString(a, b) {}
```

In such cases, again, it'd be more beneficial to put what isn't known to the developers in the comment and describe the task's context.

For example, for the `compareString` function, it's better to indicate _why_ we use our own implementation of the comparison function. The reason behind the function will convey more details about the problem and the project as a whole:

```js
/**
 * Implements compare for our limited alphabet with custom diacritic rules.
 * - Required for the correct handling of ...
 * - Justified as a part of R&D in ...
 * @see https://wiki.our-project.app/sorting/custom-diacritic-comparer/
 *
 * @example compareString('a', 'ä') === -1
 * @example compareString('a', 't') === -1
 */
function compareString(a, b) {}
```

### Turn TODOs and FIXMEs into Tasks

Sometimes comments contain `TODO` and `FIXME` tags describing bugs or flaws in the code. It's useful to turn the contents of such comments into tickets in the project's task tracker.

Unlike comments, tasks in the tracker are visible to other developers and the rest of the team. By the number of such tasks, we can evaluate the state of the project and estimate the accumulated technical debt.

The description of the created tasks is worth specifying how and why the code works now and what we want to change. Links to the created tasks can be left in a comment next to the code. Then a comment like this:

```js
// TODO: improve post-conditions.
function ensureMessageSent() {}
```

...Will turn into:

```js
/**
 * For now, it's unclear what criteria to use for the verification.
 * Update post-conditions when the criteria are known:
 * @see https://our-team.tasktracker.com/our-product/task-42
 */
function ensureMessageSent() {}
```

The most effective strategy for this is to conduct regular reviews, look for tags, and turn them into tasks. Then developers can still create `TODO` and `FIXME` tags in the code while writing code to “stay focused on a task.” But then, during regular reviews, these comments will be converted to tickets, keeping the number of such tags under control.

## Documentation

The project documentation keeps the history of the application development and the reasons for the technical decisions made. It's useful because it answers developers' questions, but its problem is that it becomes obsolete faster than the code.

The documentation isn't usually integrated into the code but exists separately. Because of this, we have to update it _in addition to changes in the code_. This separation makes it harder to remember to update documentation after the code. It increases the number of discrepancies between the docs and the code.

Updating documentation and code requires a _process_. To keep the documentation from becoming obsolete, we should have a regular task to review it. Once every certain period of time (say, a month), we compare differences between documentation and code and fix them.

Regular tasks limit the size of discrepancies because they don't last long and don't have time to accumulate. When developers periodically “clean up” discrepancies, their negative effect is less.

| By the way 🎥                                                                                                                                                                                                                                                       |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| To make regular reviews less excessive, we should store in the docs mostly code-agnostic information, which is more related to the domain and the business and is unlikely to change as quickly as the code.                                                        |
| On the other hand, storing architecture-important decisions closer to the code can be more useful. If these decisions affect the organization of the code or the way the application works, it should be easy for developers to refer to them without wasting time. |
| One approach to dealing with such decisions is known as _Architectural Decision Records, ADR_.[^adr]                                                                                                                                                                |

## Knowledge Accessibility

It's also helpful to make knowledge about the project and the subject area as _available to the development team_ as possible. The easier and quicker developers can find and refer to a piece of knowledge, the more accessible it is.

When developers know about the subject area, they make fewer errors in the application code and find inconsistencies in the project earlier. Accessible knowledge makes development more convenient and thus speeds it up.

Knowledge can be stored in different ways: in documentation, repository metadata, code comments, or the code itself. The accessibility of different options varies. For example, information in the code is most accessible because the code is always at the developer's fingertips. Repository metadata or documentation is less accessible because it exists separately and has to be accessed differently.

To make information more accessible, we can move it “closer to the code” during regular reviews:

- Turn details from comments into variables, functions, and types;
- Turn notes from code reviews into code, documentation, or commit messages;
- Turn insights from “conversations at the cooler” into documentation or issues in the repo or task tracker.

This technique may not work if the project already has a process governing documentation or repository working strategies. But in projects “without a process,” the idea of knowledge accessibility can be helpful.

[^jsdocts]: JSDoc Reference, TypeScript Documentation https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html
[^adr]: Architectural Decision Records, ADR, https://adr.github.io
