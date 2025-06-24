# Cheat Sheet on Refactoring Techniques

This appendix contains a list of short recommendations for finding and fixing problems with the code.

The recommendations are based on the topics from the previous chapters. Before applying them, it's worth reading the chapters to assess whether these techniques fit your project, style, and coding habits.

It isn't a list of strict rules. Instead, it's a set of tips you can consider.

## Searching for Problems

- Search for code smells in the project.
- Pay attention to feelings like “hard to read, change, test.”

## Before Refactoring

- Define the boundaries of the changes and find the “seams” in the code.
- Cover the selected part of the code with tests.
- Describe as many edge cases as possible with tests.
- Set up automatic test runner and code linter.
- Mark the compiler and linter warnings as errors.

## During Refactoring

- Use small steps and atomic commits.
- Create compact but detailed pull requests.
- Apply just one refactoring technique at a time.
- Fix bugs and add features separately from refactoring.
- Respect the transformation priority premise.
- Do not mix refactoring of tests and application code.
- Apply the automated refactoring tools in IDE.
- Look at diffs to find inconsistencies before committing changes.
- Run all changes through tests, even the smallest ones.

## Where to Start

- Implement automatic code formatting.
- Test the code with linters and static analyzers.
- Replace self-written helpers with language and environment features.

## Improving Names

- Clarify obscure names.
- Decipher undocumented abbreviations.
- Decompose entities with long names.
- Use different names for different entities.
- Use ubiquitous language.
- Fix lying names.

## Dealing with Code Duplication

- Separate duplication from the lack of information about the system.
- Conduct regular reviews of duplicated code.
- Extract repetitive data into variables.
- Extract repetitive actions in functions.

## Abstraction as a Tool

- Describe intent in function names.
- Structure code to give information that is needed to the reader at a particular moment.
- Remember the limits of humans' working memory; limit the number of entities in the function.
- Split information by levels of abstraction.
- Decompose tasks based on the data used in them.
- Make sure the function has only one reason to change.
- Ensure that the module guarantees the validity of its data itself.

## Linear Code Execution

- Explicitly express the data states in the application.
- Make it difficult to pass invalid data in code.
- Validate the data before use.
- Use selectors to “prepare” data for different use cases.
- Set a limit on the cyclomatic and cognitive complexity of the code.
- “Straighten” the conditions:
  - Use the early return;
  - Apply de Morgan's laws;
  - Use predicates for dynamic conditions;
  - Use design patterns (e.g. Strategy, Null-object);
  - Apply pattern-matching when possible.

## Working with Side Effects

- Use pure functions more often.
- Aim for referential transparency for easier debugging.
- Treat data and code as immutable and stateless by default.
- Move side effects to the edges of the module, function, or use case.
- Test the simplicity of code with tests: the easier it is to write tests, the simpler the code is.
- Write adapters to reduce coupling and create fewer mocks in tests.
- Separate effects into commands and queries.
- Identify CQS violations by type signatures and function names.
- Use different models to read and write data when appropriate.

## Handling Errors

- Identify different types of errors in the application.
- Centralize error handling.
- Get to error handling as early as possible.
- Try to express possible errors in the function signature.
- Check input data before using it.
- Use logging and analytics in the “last resort” handlers.
- Use decorators for composing cross-cutting concerns.

## Integrating Application Parts

- Keep coupling low and cohesion high.
- Identify cohesion by input data, output data, and code dependencies.
- Designate module guarantees through its public API.
- Make module communication less coupled:
  - Use messages or events when appropriate;
  - Apply patterns (e.g., Observer).
- Limit the number of module dependencies.
- Compose data transformations instead of side effects.
- Separate logic from side effects.
- Separate data from behavior.

## Working with Generics

- Do not rush to generalize.
- Compose complex types from simple types instead of using inheritance.
- Use generics when you are sure that the type structure will not change.
- Pay attention to conditions; they can indicate a violation of the Liskov substitution principle.

## Architecture and Communication with the World

- Use ubiquitous language to model the domain.
- Express the stages of the application data lifecycle in their states.
- Try not to depend directly on third-party code.
- Add an anti-corruption layer where the data format or API may change.
- Separate UI logic from business logic.
- Separate configuration from code.

## Static Typing as a Tool

- Use types to describe the domain and the business workflows.
- Make invalid data states unrepresentable in types.
- Identify CQS violations by method and function signatures.
- Apply the “xxxing” technique to increase the information density of signatures and function names.

## Refactoring Tests

- Do not mix refactoring tests and application code.
- Use more stubs and test data and fewer mocks.
- Get rid of test duplicates and never-failing tests.

## Refactoring Documentation and Comments

- Get rid of conflicts between the code and documentation.
- Clarify vague comments: add context, examples, and reasons behind the current implementation.
- Conduct regular reviews of comments, documentation, and project issues.
- Evaluate resources, benefits, and risks when choosing between refactoring or rewriting from scratch.
- Use project meta information from the version control system to analyze project health.
- Use the “Strangler Fig” technique when refactoring large chunks of code.
- Refactor code regularly.
- Remember the “Boy-Scout Rule.”
- Rely on measurable metrics when evaluating improvements.
