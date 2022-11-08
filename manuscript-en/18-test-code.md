# Refactoring Test Code

Tests help us refactor application code by indicating errors we may have made. But the tests are code, too, so we must refactor them from time to time.

In this chapter, we'll talk about how not to break tests during refactoring, how to keep their reliability, and what to pay attention to when searching for problems with the test code.

## “Tests” for Tests

A reliable test fails when the code doesn't work as expected. Tests “cover our back” during the refactoring of application code because they will catch an error in the app behavior. The application code, on the other hand, covers the tests because it helps check if they fail for the right reasons.

When tests change along with the application code, we can't check if everything works _as before_. The updated tests may contain bugs or check something _different_ from the original functionality, and we might not even notice it.

As a result, we might start trusting tests that don't work or work incorrectly. To avoid this, while refactoring test code, we should follow the rule:

---

**❗️ Alternate refactoring tests and the application code. Avoid doing it at the same time**.

---

If, during refactoring, we realize that we need to refactor the test code, we should:

- Stash the application code changes from the last commit (using `git stash`)
- Refactor the tests
- Check that they fail for the specified reasons
- Commit test changes
- Unstash the application code changes
- Continue refactoring

This way, we turn the refactoring into “ping-ponging” between refactoring the tests and the application code. They support and cover each other, ensuring the overall program behavior stays the same.

<figure>
  <img src="../images/18-refactoring-ping-pong.png" width="800" alt="The sequence of code changes on the diagram is zigzagging between changes in production and test code">
  <figcaption><em>When we change the code, the behavior is captured by the tests. When we change the tests, the application code captures the behavior</em><br><br></figcaption>
</figure>

This technique doesn't guarantee that we won't make any mistakes but reduces their probability. With it, there's always at least one part (either tests or code) that _hasn't changed since the last moment everything worked_. So we're more confident that everything works as before.

| By the way 📚                                                                                                                                                                                               |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| In “Code That Fits in Your Head,”[^codethatfits] Mark Seemann recommends doing the same thing. Besides the technique itself in the book, he also describes how to avoid weakening tests during refactoring. |

## “Brittle” Tests

Sometimes tests feel “brittle” and unreliable. Most of the time, this happens because of _mocks_. Mocks are demanding about their internal structure, the order and manner in which they're called, and the structure of results they return. In some cases, an application can become “over-mocked”—when mocks replace almost all modules in tests.

In such cases, any changes to the application code, even the smallest ones, result in many updates to the test code. The tests require more resources to support and slow down the development. This effect is called _test-induced damage_.[^testinduceddamage]

Unlike mocks, _stubs and simple test data_ help write more change-resistant tests.[^testingprinciples] They're more straightforward in use and forgive far more than they demand. They help us avoid test-induced damage and spend less time updating test code.

| In detail 🥸                                                                                                                                                |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The difference between stubs, mocks, and other fake objects, is well described by Microsoft.[^unittestsdotnet] We'll use their terminology in this chapter. |

To make the tests less brittle, we can use this heuristic:

---

**❗️ Use fewer mocks. Make stubs and test data simpler**

---

For example, to reduce the number of mocks, we can organize business logic to test it without mocks.[^codethatfits][^testingprinciples] It isn't easy to achieve when the logic is mixed with various effects. So it's better to keep effects separated and describe the logic in the form of pure functions.

Pure functions are intrinsically testable. They don't require a fancy test infrastructure and only need the test data and the expected result to be tested.

Let's look at the difference between code where logic and effects are mixed and code where they're separated. Notice how brittle their tests seem:

```js
// In this function, the logic and effects are mixed:

function fetchPostList(kind) {
  const directory = path.resolve("content", kind);
  const onlyMdx = fs.readDirSync(directory).filter((f) => f.endsWith(".mdx"));
  const postNames = onlyMdx.map((f) => f.replace(/\.mdx$/, ""));
  return postNames;
}

// For a unit test of such a function, we need to mock `fs`.
// We need to describe the work of the used method,
// specify the results of that method,
// reset the mock after the test:

it("should return a list of post names with the given kind", () => {
  jest.spyOn(fs, "readDirSync").mockImplementation(() => testFileList);
  const result = fetchPostList("blogPost");
  expect(result).toEqual(expected);
  jest.restoreAllMocks();
});
```

In the second case, the logic and the effects are separated. The data transformation can be tested using only stubs and test data:

```ts
function namesFromFiles(fileList) {
  return fileList
    .filter((f) => f.endsWith(".mdx"))
    .map((f) => f.replace(/\.mdx$/, ""));
}

// To test the function, it's enough
// to only have test data and the expected result:

it("should convert file list into a list of post names", () => {
  const result = namesFromFiles(testList);
  expect(result).toEqual(expected);
});
```

The test structure becomes simpler, and updating the test data doesn't take a lot of resources. With this code organization, we can completely abandon static test data and generate it automatically based on predefined properties. Such testing is sometimes called _property-based_.

| By the way 🧪                                                                                                                                                                                                                                   |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| It's usually convenient to use additional tools to generate test data in property-based tests.[^codethatfits] In the JavaScript ecosystem, libraries like faker.js,[^faker] create objects with random data according to predefined properties. |

The effects we separated earlier can be tested by integration tests or E2E tests. Depending on how we organize the work with dependencies, it may be enough to test only adapters to them. In most cases, the complexity of mocks in such tests will be lower.

For example, in tests of such an adapter for `fs`, it's enough to check that the correct method has been called with the required argument:

```ts
function postsByType(kind) {
  const directory = path.resolve("content", kind);
  const fileList = fs.readDirSync(directory);
  return fileList;
}

// We don't need to mock the whole service implementation anymore,
// it's enough just to expose the API similar to the service interface.
// This kind of mock is much more resistant to changes in application code
// and causes less test-induced damage.

describe("when called with a post kind", () => {
  it("should read file list from the correct directory", () => {
    const spy = jest.spyOn(fs, "readDirSync");
    postsByType("blogPost");
    expect(spy).toHaveBeenCalledWith("/content/blogPost/");
  });
});
```

| In detail 🧩                                                                                                            |
| :---------------------------------------------------------------------------------------------------------------------- |
| More about dependency and effect organization strategies we discussed in the chapters on architecture and side effects. |

Then the `fetchPostList` function now becomes a “composition” of logic with effects:

```ts
function fetchPostList(kind) {
  // Read Effect:
  const fileList = postsByType(kind);

  // Pure Logic:
  return namesFromFiles(fileList);
}
```

Such a function may no longer need to be tested by unit tests. Since it combines the functionality of different modules (units), we can think about integration or E2E testing.

### Test Duplicates

The test-induced damage slows down development because, after each code change, we have to update the tests. One reason for this slowdown can be tests that check the same functionality multiple times.

Ideally, we want only _one_ test to be responsible for a particular unit of behavior. When there're more, we start spending unnecessary time updating them. The more duplicates, the greater the “time tax.”

For example, if we wrote an additional unit test for the `fetchPostList` function in the example above, it would most likely be redundant and duplicate the tests of the `postsByType` and `namesFromFiles` functions. Then for every change to those functions, we would need to update not one but two tests.

The duplicate tests could hint at one of several problems:

1. There may indeed be duplication in the application code. It's a reason to perform a review and reduce the duplication in the code.
1. The modules' responsibilities aren't clearly divided, so tests of one module check something already checked by others. It's a signal to define the concerns of different modules more clearly or reconsider the testing strategy to avoid overlapping.

| Clarification 🧪                                                                                                                                                                                    |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Tests of _different kinds_ can sometimes overlap for reliability. An integration test may take over some of the functionality tested by unit tests if it's more convenient to test the application. |
| I try to keep the number of such overlaps to a minimum, but testing strategies may differ from project to project, so it's difficult to give general recommendations here.                          |

### Never-Failing Tests

A test should be responsible for a specific problem and _must fail_ when it occurs. If the test never fails, it's harmful: it has no value but takes resources for support. Such a test should be removed or rewritten to fail when the specified problem occurs.

| By the way 🙃                                                                                                                                                                                                                                          |
| :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Most often, I've encountered never-failing tests in over-mocked systems, where the infrastructure and test arrangement consisted almost entirely of calling mocks. Such tests often pass the result of one mock to another and end up testing nothing. |

### Tests for Simple Functions

When choosing what and how to test, we should compare the benefits of the test and its costs. For example, we can pay attention to the cyclomatic complexity of the function this test covers.

If the complexity of the function equals one and the test brings more additional work than real benefit, we can abandon the test.[^testingprinciples] For example, a separate unit test for the `fullName` function may be unnecessary:

```js
const fullName = (user) => `${user.firstName} ${user.lastName}`;
```

| Clarification 🚧                                                                                                                                                                                                                   |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| We're not saying that simple functions don't need tests. The decision whether to test or not depends on the specific situation. The main idea is that if a test brings more costs than benefits, we should consider its necessity. |

### Regressions

There are cases when simple functions still _need_ to be tested though, for example, if a function once had a regression. Regressions pay attention not to potential but actual bugs in the code, which _could and once did happen_.

Anything that comes up in a regression should be covered with tests. If the test seems too simple, and someone might find it useless and delete it, we can add a note in the comment with a link to the regression:

```js
/**
 * @regression JIRA-420: Users had full names in an incorrect format where the last name came before the first.
 * @see https://some-project.atlassian.com/...
 */
describe("when called with a user object", () => {
  it("should return a full name representation with first name at start", () => {
    const name = fullName(42);
    expect(name).toEqual(expected);
  });
});
```

[^codethatfits]: “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
[^testinduceddamage]: “Test-Induced Design Damage” by David Heinemeier Hansson, https://dhh.dk/2014/test-induced-design-damage.html
[^unittestsdotnet]: Unit testing best practices with .NET Core and .NET Standard, Microsoft Docs, https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices
[^faker]: Faker, Generate fake (but realistic) data for testing and development, https://fakerjs.dev
[^testingprinciples]: “Unit Testing: Principles, Practices, and Patterns” by Vladimir Khorikov, https://www.goodreads.com/book/show/48927138-unit-testing
