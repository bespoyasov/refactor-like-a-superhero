# During Refactoring

After we've defined clear boundaries, examined the code, and covered it with tests, we can start refactoring.

As we work, we want the changes to be as useful as possible, while staying within the boundaries we've set. In this chapter, we'll discuss heuristics that help us do this.

## Small Steps

A small step is a minimal change that makes sense. A good example of a small step is an _atomic commit_.[^atomic] Such a commit contains a meaningful functionality change and evolves the code from one working state to another. With atomic commits, we can evolve and change the code base while keeping it valid at every point in time.

| By the way 💡                                                                                                                                                                                                                    |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Atomic commits allow us to bisect the repository in the future while searching for bugs.[^bisect] When code is valid in each of the commits, we can “time travel” through the repository by switching between different commits. |

The size of commits depends on the habits and preferences of the team. Personally, my rule of thumb is “the smaller the commit, the better.” For example, on my projects, I commit separately even renaming of methods and variables.

Smaller steps encourage you to decompose tasks into simpler ones. In this way, big features turn into sets of compact tasks, which are not so scary to merge into the main repository branch more often. Without decomposition, such a task could turn into a blocker that drags the whole team down.

| About CI 🔬                                                                                                                                                                                                                                                                                              |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The point of _continuous integration, CI_ is for the team to synchronize code changes with each other as often as possible. This approach and its benefits are very well described in “Code That Fits in Your Head” by Mark Seemann and “Beyond Legacy Code” by Scott Bernstein.[^codethatfits][^beyond] |

Blocking tasks should be avoided in general, but especially when refactoring code. If the whole team is busy with refactoring, it can become expensive. The precedent of being expensive can deprive the project of resources for refactoring at all in the future.

Except for that, small steps allow you to “postpone” refactoring at any time and switch to another task. If we're working with git, we can use “stash” to save unfinished work via `git stash`.

And lastly, small changes are easier to describe in commit messages. The scope of such changes is less, so it's easier to describe their meaning in a short sentence.

## Small but Detailed Pull Requests

This section follows on from the previous one, but I want to focus on it separately. The problem with big pull requests is that...

---

**❗️ No one really reviews big pull requests**

---

If we want to _improve_ the code, we need the pull request to _be reviewed_. So our job is to make the reviewer's job easier. To do this, we can:

- Limit the size of changes. So it's easier to find time for a review and understand the PR's goals and meaning. This way, the review won't look like a big chunk of new work.
- Describe the context of the task in the message to the PR. The reasons, goals, and constraints of the task will help share the understanding of the task with the reviewer. This way we can anticipate likely questions and answer them in advance. It will speed up the review.

The desire for small but detailed PR also helps to break down large tasks into smaller ones and do refactoring in small steps.

## Tests for Every Change

In order for the code to evolve through valid states, we will test _every_ change, no matter how small.

When using unit tests, we can keep them running next to the editor. When using longer-running tests (like E2E), we can run them before each commit (e.g. on a pre-commit hook), so that invalid code doesn't get into the repository.

The tests should drive us to _commit only valid code_. Each commit then will contain a set of _complete and meaningful_ changes.

| By the way 🧪                                                                                                                                                                       |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| It will only work if tests are trustworthy. That's why in the latest chapter we emphasized edge-cases and detailed specification of the result. That help make tests more reliable. |

## One Technique at a Time

Frequent commits are anchor points in code evolution. The more frequent such points are, the more compact the changes in between. This is useful, for example, when examining changes from the latest commit that we just made. Small changes are faster to study, easier to understand, and they contain less work so it's emotionally easier to roll them back in case we need to.

During refactoring though it's not always obvious how to make changes smaller and more often. I prefer to follow this rule:

---

**❗️ Don't mix different refactoring techniques in the same commit**

---

This rule makes us commit code more often: renamed a function—commit, extracted a variable—commit, added code for a future replacement—commit, and so on.

As long as we don't mix different techniques in one commit, it's easier for us to track code changes by diffs and find errors like name conflicts.

Complex techniques, that are too big for a single commit, can be broken up into steps. Each of these steps can be committed separately. In this case, it's important to define the steps so that each of them also leaves the code in a valid state.

## No Features, No Bug Fixes

During refactoring, we may find an idea for a feature or a piece of code that doesn't work correctly. We may want to “fix it along the way”, but it's better not to mix bug fixes and new features with refactoring.

Refactoring _should not_ change the code functionality. If we add a feature during refactoring, and then it needs to be rolled back, we'll have to cherry-pick specific commits or even lines of code into the repo manually.

Instead, it's better to put all the found feature ideas into a separate list and come back to them after refactoring. If we find a bug, we should postpone refactoring (`git stash`) and return to it after the fix. Again, it's more convenient to work in small steps for this kind of maneuverability.

## Transformation Priority Premise

_Transformation Priority Premise, TPP_ is a list of actions that help develop code from the naive simplest implementation to a more complex one that meets all the project requirements.[^tpp]

TPP helps _avoid doing everything at once_. It encourages you to update a piece of code gradually, recording each step in the version control system, and integrating changes into the main repository branch more often.

For me personally, TPP helps me not overload my head with details while I'm writing code. All improvement ideas that come up along the way, I put in a separate list. I then compare this list with TPP and choose what to implement next.

| Why bother 🧠                                                                                                                                                                   |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| “Offloaded” details free up the brain's “working memory”.[^shorttermmemory] The freed resources can be spent on the details of the task. This improves attentiveness and focus. |

## Refactoring Tests Separately

Tests and application code cover each other. Tests verify that we haven't made mistakes in the application code, and vice versa. If we refactor them at the same time, the probability of missing a bug becomes higher.

It's better to refactor the application code and tests one at a time. If while refactoring an application we notice that we need to refactor a test, we should:

- Stash the application code changes;
- Refactor the desired test;
- Verify that the test breaks for the reason it should;
- Get the previous changes from the stash and continue working on them.

| In detail 🔬                                                                          |
| :------------------------------------------------------------------------------------ |
| We'll talk a little more about this technique in the “Refactoring Test Code” chapter. |

[^atomic]: Atomic Commit, Wikipedia https://en.wikipedia.org/wiki/Atomic_commit
[^bisect]: git-bisect, Use binary search to find the commit that introduced a bug, https://git-scm.com/docs/git-bisect
[^codethatfits]: “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
[^beyond]: “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
[^tpp]: Transformation Priority Premise, Wikipedia https://en.wikipedia.org/wiki/Transformation_Priority_Premise
[^shorttermmemory]: Working memory, Capacity, Wikipedia, https://en.wikipedia.org/wiki/Working_memory#Capacity
