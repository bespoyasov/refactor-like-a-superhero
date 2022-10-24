# Refactoring as a Process

In the previous chapters, we primarily focused on the technical details of refactoring. However, this is only a part of improving the code in a project.

In this chapter, we'll talk about how to find time for refactoring and perform it regularly. We'll discuss how to overcome “friction” in the project and start refactoring even if the code base is large and has a lot of legacy code.

## Refactor or Rewrite

When we look at a big chunk of legacy code, the first thought is, “it's easier just to rewrite it from scratch.” Sometimes it's true, but it isn't always possible to adequately assess the state of the code at a glance.

Before we decide whether to refactor or rewrite, we should evaluate three things: team resources, the benefits of our solution, and its risks.

These parameters won't give us a direct answer to the question “What to do?” but often, this assessment alone is enough to decide. However, even if it isn't enough, it'll tell us what to investigate before we make a final decision.

## Resources

By _resources_, we'll mean the time, knowledge, and experience the team has at its disposal. These resources can be “spent” on refactoring or rewriting code to improve it.

### Available Time

To estimate the developers' free time, we can look back at the past experience with the project. We need to assess how much time the team spent on code improvements in the past and how often there were “empty holes” in the project schedule.

This assessment shows how the team spent time on development in the past. It helps predict how much time we'll have in the future.

This prediction may seem an understatement because we may want to spend more time improving the code. However, it reflects the development patterns that the team is used to. These patterns are helpful to consider because even if we agree to spend more time on refactoring, the development will fall back to familiar patterns without a change in the process.

In addition, we should always be aware of “unpredicted problems” with legacy code that will take extra time to solve.

### Accumulated Knowledge

Accumulated knowledge about the project can be assessed by the amount of documentation, valuable comments in the code, the quality of the commit history, the availability of the developers who wrote the code, and the clarity of the code itself.

Knowledge is difficult to quantify, so we can use a qualitative assessment. The more contradictions we find between different sources of information, the worse we can consider the quality of the accumulated knowledge. Conversely, the clearer the code and easier it is to find the developers who wrote it, the higher the quality of expertise.

### Experience

By experience, we mean the developers' familiarity with this project and proficiency in other projects, languages, and paradigms—the more varied the experience, the less time we'll spend developing poor non-working solutions.

| By the way 🧑‍🏫                                                                                                                                    |
| :----------------------------------------------------------------------------------------------------------------------------------------------- |
| It's also worth considering the experience of outside consultants and experts if we want them to participate, but it's harder to do objectively. |

## Benefits and Risks

We should also define the benefits and risks of refactoring or rewriting the code. They will directly depend on the work processes and the project's inner structure, so we might need internal research.

## Project Meta Information

The project's meta information reflects the process of working on the project and what parts of the code are the most important. All the necessary meta information is already there if the team uses a version control system. For example, the commit history can show us:

- What is a crucial part of the project—what files were updated with the new features and bug fixes the most;
- What implicit coupling exists between project parts—what files were modified along with other files;
- What parts of the project had the most bugs—what commits were related to bug fixes, etc.

Meta information analysis can tell us which parts of the project are the most complex or beneficial from a business perspective.

| Read more 📚                                                                                     |
| :----------------------------------------------------------------------------------------------- |
| Adam Tornhill wrote more about this assessment in the book “Your Code as a Crime Scene.”[^scene] |

If we don't have a version control system, we can try to collect indirect indicators: release notes, tech support logs, etc. However, it'll be much harder to conclude from them.

## Estimates

If we decide to refactor a particular piece of code, we'll need to evaluate the size of the task and the required time.

To understand how much time a piece of code can take to refactor, we should first point out the places that raise questions. To find out what problems we're dealing with, we can lay out those questions in the following table:

|         | Simple                                                               | Difficult                                                              |
| ------- | -------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Clear   | Research is unnecessary, clear how to solve, and takes minimal time. | Clear how to solve but definitely takes a long time.                   |
| Unclear | The problem is small and isolated but might need research.           | Definitely need research, lots of hidden connections, unfamiliar area. |

The amount of time needed directly depends on the proportion of topics in this table. The more complex and unclear questions we have, the harder it is to plan the development iteration.

Tasks from the last table cell are the most difficult to plan. For such problems, we can suggest the team use the “Hypothesis per Iteration” method. Each assumption about the problem would be a separate development iteration in this method. Disproving or confirming that assumption is the goal of the iteration.

Testing one assumption doesn't take as much time as examining the whole problem. Iterations with such checks are easier to plan and conduct. At the same time, with each tested assumption, we understand more about the problem, and sooner or later, it'll move from the lower right cell to another cell in the table. Then we can plan the development more thoroughly.

| By the way 👀                                                                                                                                                                                                                                                          |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Evaluating the scope and size of the task can be helpful even if the team follows the “No Estimates” methodology.[^noestimates] For example, with the evaluation, it is easier to decide when to change the work strategy without wasting extra resources on the task. |

## Refactoring Large Chunks of Code

If a piece of code is too big and cannot be refactored all at once, it's helpful to follow the “Strangler Fig” method.

The goal of the approach is _not to replace_ a piece of code under refactoring but to implement similar functionality _beside_ that code. We can develop this copy considering all the knowledge we now have and all the problems we want to avoid. When the copy is done, we replace the old code with it.

| By the way 🌳                                                                                                                                                                                                                                                                                                                     |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The name of this approach comes from the analogy to the strangler fig plant.[^strangler] This plant winds around a tree, hardens, and when the tree dies, it stays in the shape of that tree. This method of refactoring does the same. We first “cover” a feature with new code and remove the old code inside after it's ready. |

When working with the “Strangler Fig” approach, it's important to frequently merge the results into the main branch of the repo. This technique will help prevent merge conflicts in the version control system and keep the refactoring process from taking too much time.

## Frequency and Hygiene

In general, it's better to refactor as often as possible. It's most convenient to refactor the code right after making changes to it. The best option is if we can come back to the code after a short rest to take a fresh look at it. This kind of review helps us identify poor solutions in the code sooner.

When working with legacy code, we should refactor the code _before_ fixing bugs in it or adding new features. After refactoring, the code will be clearer, better covered by tests, and more pleasant to work with. Thus, we'll reduce the time needed to fix a bug or add a feature.

Finally, when touching any code, it's worth remembering the “Boy-Scout Rule”:[^opportunistic][^codethatfits]

---

**❗️ Leave the code behind in a better state than we found it**

---

## Metrics

Although refactoring relies on subjective measures such as beauty and readability, we can still use quantitative metrics to assess the changes' quality.

As the basis, we'll take the metrics from the talk “Where does bad code come from?” and expand the list.[^wherefrom] As a result, we'll get a list of 7 metrics, each of which answers the question, “How long does it take to XXX?”

- **S**earch, how long it takes to find the required place in the code.
- **W**rite, to write a new feature and cover it with tests.
- **A**gree, to get developers to agree that “code is good enough.”
- **R**ead, to read and understand what a piece of code does.
- **M**odify, to modify the code to fit the new requirements.
- **E**xecute, to execute/build/deploy a piece of code.
- **D**ebug, to find and fix a bug.

If we measure these metrics before and after refactoring, we can compare the quality of the changes we've made. Some metrics are still quite subjective but can be expressed in numbers. A quantitative description of characteristics will help us to notice a trend of improvement or deterioration when estimating code quality.

[^scene]: “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene
[^strangler]: “Strangler Fig Application” by Martin Fowler https://martinfowler.com/bliki/StranglerFigApplication.html
[^opportunistic]: “Opportunistic Refactoring” by Martin Fowler https://martinfowler.com/bliki/OpportunisticRefactoring.html
[^codethatfits]: “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
[^wherefrom]: “Where Does Bad Code Come From?” https://youtu.be/7YpFGkG-u1w
[^noestimates]: “No Estimates” by Allen Holub, https://youtu.be/QVBlnCTu9Ms
