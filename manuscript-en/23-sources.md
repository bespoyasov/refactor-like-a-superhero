# Sources and References

My opinion of modern development and good code has been influenced by other people's work. In this appendix, I have prepared a list of books, articles, talks, methodologies, and studies that I find most useful and refer to in my own work.

Each section in this list has a title similar to the chapter topics. If you are interested in the topic of a particular chapter and want to know more, the sections in the list will help you navigate more easily among the links.

The links can repeat. I decided it was more important to compile a complete list for each topic than to get rid of duplicates. I hope you find this useful.

## Notion of Refactoring and Code Quality

What refactoring is and why it is needed. What an uncontrollably increasing complexity of a project can lead to. How to define “bad” and “good” code.

### Books

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
- “The Black Swan” by Nassim Nicholas Taleb, https://www.goodreads.com/book/show/242472.The_Black_Swan
- “Clean Code” by Robert C. Martin, https://www.goodreads.com/book/show/3735293-clean-code
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Software Design: Cognitive Aspect” by Françoise Détienne, https://www.goodreads.com/book/show/3104497-software-design-cognitive-aspect
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Talks and Video

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “Log4J & JNDI Exploit: Why So Bad?” https://youtu.be/Opqgwn8TdlM
- “Preventing the Collapse of Civilization” by Jonathan Blow, https://youtu.be/pW-SOdj4Kkk
- “Where Does Bad Code Come From?” https://youtu.be/7YpFGkG-u1w

### Blog Posts, Studies, and Examples

- “Beauty Is in Simplicity”, by Jørn Ølmheim, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_05/
- Code Readability Testing, an Empirical Study, https://www.researchgate.net/publication/299412540_Code_Readability_Testing_an_Empirical_Study
- Evaluating Code Readability and Legibility: An Examination of Human-centric Studies, https://github.com/reydne/code-comprehension-review
- “The Human Cost of Tech Debt”, by Erik Dietrich, https://daedtech.com/human-cost-tech-debt/
- How Readable Code Is, https://howreadable.com
- “Refactoring, Changing the Code without Changing its External Behavior” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Refactoring
- “Technical Debt”, DevIQ, https://deviq.com/terms/technical-debt

### Related Concepts

- Bus factor, Wikipedia, https://en.wikipedia.org/wiki/Bus_factor
- Entropy, Wikipedia, https://en.wikipedia.org/wiki/Entropy
- Murphy's Law, Wikipedia, https://en.wikipedia.org/wiki/Murphy%27s_law

### Tools

- Code Smells, Refactoring Guru, https://refactoring.guru/refactoring/smells
- Refactoring Technics, Refactoring Guru, https://refactoring.guru/refactoring/techniques

## Before Start

What to look for before refactoring. How to prepare the code for changes in order to simplify the work. How to secure the work with future changes.

### Books

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Debug It!: Find, Repair, and Prevent Bugs in Your Code” by Paul Butcher, https://www.goodreads.com/book/show/6770868-debug-it
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow
- “Willpower Doesn't Work” by Benjamin P. Hardy, https://www.goodreads.com/book/show/35604684-willpower-doesn-t-work
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Talks and Video

- “The Grand Unified Theory of Clean Architecture and Test Pyramid” by Guilherme Ferreira, https://youtu.be/gHSpj2zM9Nw
- “How do you prepare before tackling a problem?” by Fun-fun-function, https://youtu.be/mF-tVjXbO8Y

### Blog Posts and Examples

- “Before You Refactor” by Rajith Attapattu, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_06/
- “How to ask good questions” by Julia Evans, https://jvns.ca/blog/good-questions/
- “Read Code” by Karianne Berg, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_70/
- “Refactoring, Changing the Code without Changing its External Behavior” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Refactoring
- “TDD: What, How, and Why” by Alex Bespoyasov, https://bespoyasov.me/blog/tdd-what-how-and-why/

### Related Concepts

- Agile Software Development, https://en.wikipedia.org/wiki/Agile_software_development
- Working memory, Capacity, Wikipedia, https://en.wikipedia.org/wiki/Working_memory#Capacity

### Tools

- Extreme Programming, http://www.extremeprogramming.org
- Test-Driven Development, TDD, https://martinfowler.com/bliki/TestDrivenDevelopment.html
- Tools for Better Thinking, https://untools.co

## During Refactoring

What to avoid during refactoring, how to make the process easier. How to isolate changes and make sure no other code is broken. How to stay within resource budget and keep changes small.

### Books

- “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Jedi Technics” by Maxim Dorofeev, Translated Summary, https://bespoyasov.me/blog/jedi-technics/
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Rules of Work Communication” by M. Ilyahov and L. Sarycheva, Translated Summary, https://bespoyasov.me/blog/rules-of-work-communication/
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Talks and Video

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Where Does Bad Code Come From?” https://youtu.be/7YpFGkG-u1w

### Blog Posts and Examples

- “Antipatterns as a Worst Practices” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Antipatterns
- “Convenience Is not an -ility” by Gregor Hohpe, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_19/
- “Deploy Early and Often” by Steve Berczuk, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_20/
- “Don't Push: Automate Instead” by Alex Bespoyasov, https://bespoyasov.me/blog/do-not-push-automate-instead/
- “How to Get Your Code Reviewed Faster” by Artem Sapegin, https://blog.sapegin.me/all/faster-code-reviews/

### Related Concepts

- Agile Software Development, https://en.wikipedia.org/wiki/Agile_software_development
- Atomic Commit, Wikipedia https://en.wikipedia.org/wiki/Atomic_commit
- Continuous Integration, Wikipedia, https://en.wikipedia.org/wiki/Continuous_integration
- Decomposition, Wikipedia, https://en.wikipedia.org/wiki/Decomposition_(computer_science)
- Transformation Priority Premise, Wikipedia https://en.wikipedia.org/wiki/Transformation_Priority_Premise

### Tools

- Continuous Integration, Wikipedia, https://en.wikipedia.org/wiki/Continuous_integration
- Getting Started, About Version Control, https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control
- Oh Shit, Git!?! https://ohshitgit.com
- Transformation Priority Premise, Wikipedia https://en.wikipedia.org/wiki/Transformation_Priority_Premise
- Use binary search to find the commit that introduced a bug, https://git-scm.com/docs/git-bisect

## Formatters, Linters, and Language Features

How to use all the capabilities of automated code refactoring tools and analyzers. Reasons to know the peculiarities of the language and environment in which the code is executed. How to benefit from automation. How consistency helps you to solve problems more quickly.

### Books

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Autopilot: The Art & Science of Doing Nothing” by Andrew Smart, https://www.goodreads.com/book/show/18053732-autopilot
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow

### Talks and Video

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o

### Blog Posts and Studies

- “Automate Your Coding Standard” by Filip van Laenen, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_04/
- “Code Layout Matters” by Steve Freeman, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_13/
- How Readable Code Is, https://howreadable.com
- “Know Your IDE” by Heinz Kabutz, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_45/
- “Know Your Next Commit” by Dan Bergh Johnsson, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_47/
- “Why robots should format our code for us”, https://blog.sapegin.me/all/prettier/

### Tools

- Can I Use, support tables for web, https://caniuse.com
- List of EcmaScript Proposals, https://proposals.es
- Prettier, an opinionated code formatter, https://prettier.io
- Refactoring Source Code in VSCode, https://code.visualstudio.com/docs/editor/refactoring

## Naming

Why naming is important, how variable and function names affect code perception and development speed. Why terminology synchronization improves team collaboration. How to identify “bad” and “good” names. What to do with lying names.

### Books

- “Clean Code” by Robert C. Martin, https://www.goodreads.com/book/show/3735293-clean-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Software Design: Cognitive Aspect” by Françoise Détienne, https://www.goodreads.com/book/show/3104497-software-design-cognitive-aspect
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Talks and Video

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Transforming Code into Beautiful, Idiomatic Python” https://youtu.be/OSGv2VnC0go

### Blog Posts, Studies, and Examples

- “Antipatterns as a Worst Practices” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Antipatterns
- “Code in the Language of the Domain” by Dan North, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_11/
- “Coding with Clarity” by Brandon Gregory, https://alistapart.com/article/coding-with-clarity/
- Evaluating Code Readability and Legibility: An Examination of Human-centric Studies, https://github.com/reydne/code-comprehension-review/blob/master/list-papers/AllPhasesMergedPapers-Part1.md
- “Give it five minutes” by Jason Fried, https://signalvnoise.com/posts/3124-give-it-five-minutes
- “How to ask good questions” by Julia Evans, https://jvns.ca/blog/good-questions/
- “Read Code” by Karianne Berg, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_70/
- “Refactoring, Changing the Code without Changing its External Behavior” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Refactoring
- “Stop using isLoading booleans” by Kent C. Dodds, https://kentcdodds.com/blog/stop-using-isloading-booleans

### Related Concepts

- Abstraction layer, Wikipedia, https://en.wikipedia.org/wiki/Abstraction_layer
- Bus factor, Wikipedia, https://en.wikipedia.org/wiki/Bus_factor
- Declarative Programming, Wikipedia, https://en.wikipedia.org/wiki/Declarative_programming
- Decomposition, Wikipedia, https://en.wikipedia.org/wiki/Decomposition_(computer_science)
- Entropy, Wikipedia, https://en.wikipedia.org/wiki/Entropy

### Tools

- Conventional Commits, https://www.conventionalcommits.org/en/v1.0.0/
- Naming Cheat Sheet, https://github.com/kettanaito/naming-cheatsheet
- Refactoring Source Code in VSCode, https://code.visualstudio.com/docs/editor/refactoring
- Refactoring Techniques, Refactoring Guru, https://refactoring.guru/refactoring/techniques
- Ubiquitous Language, https://martinfowler.com/bliki/UbiquitousLanguage.html

## Code Duplication

How to distinguish between code duplication and lack of knowledge about the system. Why and how to use duplication as a tool. The benefits of regular code audits and how to get into the habit of doing them.

### Books

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Jedi Technics” by Maxim Dorofeev, Translated Summary, https://bespoyasov.me/blog/jedi-technics/
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Blog Posts and Examples

- “Antipatterns as a Worst Practices” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Antipatterns
- “Beware the Share” by Udi Dahan, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_07/
- “Copypaste in Code” by Alex Bespoyasov, https://bespoyasov.me/blog/copy-paste/
- “How to ask good questions” by Julia Evans, https://jvns.ca/blog/good-questions/
- “Refactoring, Changing the Code without Changing its External Behavior” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Refactoring
- “The Single Responsibility Principle” by Robert C. Martin, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_76/
- “When You SHOULD Duplicate Code” by Marius Bongarts, https://medium.com/@mariusbongarts/when-you-should-duplicate-code-b0d747bc1c67

### Related Concepts

- Don't Repeat Yourself, Wikipedia https://ru.wikipedia.org/wiki/Don’t_repeat_yourself
- Separation of Concerns, Wikipedia, https://en.wikipedia.org/wiki/Separation_of_concerns

### Tools

- Copy/paste detector, jscpd, https://www.npmjs.com/package/jscpd
- Detect copy-pasted and structurally similar code, jsinspect, https://github.com/danielstjules/jsinspect
- Duplicate Code Smell, Refactoring Guru, https://refactoring.guru/smells/duplicate-code

## Abstraction and Separation of Concerns

How and why to use abstraction. Reasons to separate intention from implementation and consider the limits of the working memory of the human brain. How to give the reader information about the system in a controlled way. How to efficiently decompose complex tasks into simpler ones. How to make sure the application data is always in the correct state.

### Books

- “And Suddenly the Inventor Appeared: Triz, the Theory of Inventive Problem Solving” by Genrich Altshuller, https://www.goodreads.com/book/show/161916.And_Suddenly_the_Inventor_Appeared
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Designing Data-Intensive Applications” by Martin Kleppmann https://dataintensive.net
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “The Humane Interface” by Jef Raskin, https://www.goodreads.com/book/show/344726.The_Humane_Interface
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow
- “Software Design: Cognitive Aspect” by Françoise Détienne, https://www.goodreads.com/book/show/3104497-software-design-cognitive-aspect
- “Structure and Interpretation of Computer Programs” by Harold Abelson, Gerald Jay Sussman, Julie Sussman, https://www.goodreads.com/book/show/43713.Structure_and_Interpretation_of_Computer_Programs
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- “You Don't Know JS Yet: Scope & Closures” by Kyle Simpson, https://github.com/getify/You-Dont-Know-JS/blob/2nd-ed/scope-closures/ch8.md
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Talks and Video

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Design, Composition, and Performance” by Rich Hickey, https://youtu.be/MCZ3YgeEUPg
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0

### Blog Posts and Examples

- “Antipatterns as a Worst Practices” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Antipatterns
- “Climbing the infinite ladder of abstraction” by Alexis King, https://lexi-lambda.github.io/blog/2016/08/11/climbing-the-infinite-ladder-of-abstraction/
- “Coupling, Cohesion & Connascence” by Khalil Stemmler, https://khalilstemmler.com/wiki/coupling-cohesion-connascence/
- “Declarative Data Validation with Rule-Based Approach and Functional Programming” by Alex Bespoyasov, https://bespoyasov.me/blog/declarative-rule-based-validation/
- “Maintain a Single Layer of Abstraction at a Time” by Khalil Stemmler, https://khalilstemmler.com/articles/oop-design-principles/maintain-a-single-layer-of-abstraction/
- “Missing Abstraction” by Alex Bespoyasov, https://bespoyasov.me/blog/missing-abstraction/
- “The Power of Composition” by Scott Wlaschin, https://fsharpforfunandprofit.com/composition/
- “Prefer Domain-Specific Types to Primitive Types” by Einar Landre, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_65/

### Related Concepts

- Abstraction, Wikipedia, https://en.wikipedia.org/wiki/Abstraction
- Abstraction layer, Wikipedia, https://en.wikipedia.org/wiki/Abstraction_layer
- Cohesion, Wikipedia, https://en.wikipedia.org/wiki/Cohesion_(computer_science)
- Coupling, Wikipedia, https://en.wikipedia.org/wiki/Coupling_(computer_programming)
- Decomposition, Wikipedia, https://en.wikipedia.org/wiki/Decomposition_(computer_science)
- Encapsulation, Wikipedia, https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)
- Separation of Concerns, Wikipedia, https://en.wikipedia.org/wiki/Separation_of_concerns
- Working memory, Capacity, Wikipedia, https://en.wikipedia.org/wiki/Working_memory#Capacity

### Tools

- Single Responsibility Principle, Principles of OOD, http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod
- Tiny Types in TypeScript, https://janmolak.com/tiny-types-in-typescript-4680177f026e
- Tools for Better Thinking, https://untools.co
- What is Connascence? https://connascence.io
- TRIZ, Wikipedia, https://en.wikipedia.org/wiki/TRIZ

## Functional Pipeline and Linear Code Execution

How and why to express data states of business workflows in code. The benefits of linear code execution are. How to disallow passing invalid data to functions and isolate data validation. The benefits of functional programming are.

### Books

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow

### Talks and Video

- “Is Domain-Driven Design Overrated?” by Stefan Tilkov, https://youtu.be/ZZp9RQEGeqQ
- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- “Professor Frisby Introduces Composable Functional JavaScript” by Brian Lonsdorf, https://egghead.io/courses/professor-frisby-introduces-composable-functional-javascript
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o
- “Why Your Architecture is Functional” by Roman Nevolin, RU with Subtitles, https://youtu.be/9s_4wpzENhg

### Blog Posts

- “Apply Functional Programming Principles” by Edward Garson, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_02/
- “Constructive vs predicative data” by Hillel Wayne, https://www.hillelwayne.com/post/constructive/
- “Design a microservice domain model”, MSDN, https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/microservice-domain-model
- “Designing with types: Making illegal states unrepresentable” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/designing-with-types-making-illegal-states-unrepresentable/
- “Functional design is intrinsically testable” by Mark Seemann, https://blog.ploeh.dk/2015/05/07/functional-design-is-intrinsically-testable/
- “Immutability: Making your code predictable” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/correctness-immutability/
- “Lenses in Functional Programming” by Albert Steckermeier, https://sinusoid.es/misc/lager/lenses.pdf
- “Making Illegal States Unrepresentable” by Hillel Wayne, https://buttondown.email/hillelwayne/archive/making-illegal-states-unrepresentable/
- “Parse, don’t validate” by Alexis King, https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/
- “The Power of Composition” by Scott Wlaschin, https://fsharpforfunandprofit.com/composition/

### Related Concepts

- Data Mapper, https://martinfowler.com/eaaCatalog/dataMapper.html
- Data Transfer Object, DTO, Wikipedia, https://en.wikipedia.org/wiki/Data_transfer_object
- Declarative Programming, Wikipedia, https://en.wikipedia.org/wiki/Declarative_programming
- Function Composition, Wikipedia, https://en.wikipedia.org/wiki/Function_composition
- Functional Programming, Wikipedia, https://en.wikipedia.org/wiki/Functional_programming
- Immutable Object, Wikipedia, https://en.wikipedia.org/wiki/Immutable_object
- Predicate, Wikipedia, https://en.wikipedia.org/wiki/Predicate_(mathematical_logic)
- Projection operations (C#), https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/projection-operations
- Pure Functions, Wikipedia, https://en.wikipedia.org/wiki/Pure_function

### Tools

- Bounded Context in DDD by Martin Fowler, https://www.martinfowler.com/bliki/BoundedContext.html
- Design Patterns, Refactoring Guru, https://refactoring.guru/design-patterns
- Domain Model, https://martinfowler.com/eaaCatalog/domainModel.html
- Specification for interoperability of common algebraic structures in JavaScript, Fantasy Land, https://github.com/fantasyland/fantasy-land
- Typed functional programming in TypeScript, fp/ts, https://github.com/gcanti/fp-ts
- The library which provides useful monads, interfaces, and lazy iterators, sweet-monads, https://github.com/JSMonk/sweet-monads
- Zen of Python, https://peps.python.org/pep-0020/

## Conditions and Complexity

How to organize the conditions to decrease the cognitive load of the code. Metrics to use to measure complexity. How to use automated tools to manage complexity. Reasons to “straighten” code execution and “turn” conditions inside out. How to use Boolean algebra to simplify conditions. Design patterns that can help do this. How to apply functional programming principles to simplify conditions.

### Books

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Debug It!: Find, Repair, and Prevent Bugs in Your Code” by Paul Butcher, https://www.goodreads.com/book/show/6770868-debug-it
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Talks and Video

- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- “Maybe Not” by Rich Hickey, https://youtu.be/YR5WdGrpoug
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o
- “Why Your Architecture is Functional” by Roman Nevolin, RU with Subtitles, https://youtu.be/9s_4wpzENhg

### Blog Posts, Studies, and Examples

- “Anti-if, the Missing Patterns” by Joe Wright, https://code.joejag.com/2016/anti-if-the-missing-patterns.html
- “Antipatterns as a Worst Practices” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Antipatterns
- “Application State Management with Finite State Machines” by Alex Bespoyasov, https://bespoyasov.me/blog/fsm-to-the-rescue/
- “Bringing Pattern Matching to TypeScript” by Gabriel Vergnaud, https://dev.to/gvergnaud/bringing-pattern-matching-to-typescript-introducing-ts-pattern-v3-0-o1k
- “Cognitive Complexity. A new way of measuring understandability” by G. Ann Campbell, SonarSource SA, https://www.sonarsource.com/docs/CognitiveComplexity.pdf
- “A conditional sandwich example” by Mark Seemann, https://blog.ploeh.dk/2022/02/14/a-conditional-sandwich-example/
- “Decoupling decisions from effects” by Mark Seemann, https://blog.ploeh.dk/2016/09/26/decoupling-decisions-from-effects/
- “Destroy all `if`s” by John A De Goes, https://degoes.net/articles/destroy-all-ifs
- “Functional design is intrinsically testable” by Mark Seemann, https://blog.ploeh.dk/2015/05/07/functional-design-is-intrinsically-testable/
- “Parse, don’t validate” by Alexis King, https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/
- “Refactoring, Changing the Code without Changing its External Behavior” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Refactoring
- “Why should you return early?” by Szymon Krajewski https://szymonkrajewski.pl/why-should-you-return-early/

### Related Concepts

- “Cognitive Complexity. A new way of measuring understandability” by G. Ann Campbell, SonarSource SA, https://www.sonarsource.com/docs/CognitiveComplexity.pdf
- Control flow graph & cyclomatic complexity for following procedure, Stackoverflow, https://stackoverflow.com/a/2670135/3141337
- Control-Flow Graph, Wikipedia, https://en.wikipedia.org/wiki/Control-flow_graph
- Cyclomatic Complexity, Wikipedia, https://en.wikipedia.org/wiki/Cyclomatic_complexity
- Pattern Matching, Wikipedia, https://en.wikipedia.org/wiki/Pattern_matching

### Tools

- `complexity`, ES Lint, https://eslint.org/docs/latest/rules/complexity
- De Morgan's Laws, Wikipedia, https://en.wikipedia.org/wiki/De_Morgan%27s_laws
- Design Patterns, Refactoring Guru, https://refactoring.guru/design-patterns/
- ECMAScript Pattern Matching Proposal, https://github.com/tc39/proposal-pattern-matching
- `switch-exhaustiveness-check`, ES Lint TypeScript, https://typescript-eslint.io/rules/switch-exhaustiveness-check/
- `ts-pattern`, Library for Pattern Matching in TypeScript, https://github.com/gvergnaud/ts-pattern

## Working with Side Effects

Reasons why side effects make the program more complex and unpredictable. How to reduce the number of effects in your code and what to do with effects needed for the application to work. The benefits of pure functions and referential transparency. Options to test effects and reasons to separate logic from effects. The point of dividing code into commands and queries.

### Books

- “Clean Architecture” by Robert C. Martin, https://www.goodreads.com/book/show/18043011-clean-architecture
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Talks and Video

- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- “Why Your Architecture is Functional” by Roman Nevolin, RU with Subtitles, https://youtu.be/9s_4wpzENhg

### Blog Posts

- “Command-Query Responsibility Segregation” by Martin Fowler, https://martinfowler.com/bliki/CQRS.html
- “Command-Query Separation” by Martin Fowler, https://martinfowler.com/bliki/CommandQuerySeparation.html
- “Command-Query Separation” by Alex Bespoyasov, https://bespoyasov.me/blog/commands-and-queries/
- “CQS versus server generated IDs” by Mark Seemann, https://blog.ploeh.dk/2014/08/11/cqs-versus-server-generated-ids/
- “Functional architecture is Ports and Adapters” by Mark Seemann, https://blog.ploeh.dk/2016/03/18/functional-architecture-is-ports-and-adapters/
- “Functional Core in Imperative Shell” by Gary Bernhardt, https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell
- “Impureim Sandwich” by Mark Seemann, https://blog.ploeh.dk/2020/03/02/impureim-sandwich/
- “Message Passing Leads to Better Scalability in Parallel Systems” by Russel Winder, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_57/
- “A pipe operator for JavaScript” by Axel Rauschmayer, https://2ality.com/2022/01/pipe-operator.html

### Related Concepts

- Design Patterns, Refactoring Guru, https://refactoring.guru/design-patterns
- Immutable Object, Wikipedia, https://en.wikipedia.org/wiki/Immutable_object
- Pure Functions, Wikipedia, https://en.wikipedia.org/wiki/Pure_function
- Referential Transparency, Haskell Wiki, https://wiki.haskell.org/Referential_transparency

## Error Handling

Kinds of errors exist and how they differ. Problems entangled error handling lead to. What to pay attention to when refactoring error handling in JavaScript code. Techniques to use when there are technology, paradigm, or methodology constraints.

### Books

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Patterns for Fault Tolerant Software” by Robert Hanmer, https://www.goodreads.com/book/show/3346135-patterns-for-fault-tolerant-software
- “The Pragmatic Programmer” by Andy Hunt, https://www.goodreads.com/book/show/4099.The_Pragmatic_Programmer
- “What I've Learned From Failure” by Reg Braithwaite, https://leanpub.com/shippingsoftware/read

### Talks and Video

- “Error handling: doing it right!” by Ruben Bridgewater, https://youtu.be/bJ3glfA-jqo
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- “Maybe Not” by Rich Hickey, https://youtu.be/YR5WdGrpoug

### Blog Posts

- “Against Railway-Oriented Programming” by by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/against-railway-oriented-programming/
- “Declarative Data Validation with Rule-Based Approach and Functional Programming” by Alex Bespoyasov, https://bespoyasov.me/blog/declarative-rule-based-validation/
- “Distinguish Business Exceptions from Technical” by Dan Bergh Johnssonm https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_21/
- “Don't Ignore that Error!” by Pete Goodliffe, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_26/
- “The Error Model” by Joe Duffy, http://joeduffyblog.com/2016/02/07/the-error-model/
- “Errors Are Not Exceptions” by swyx, https://www.swyx.io/errors-not-exceptions
- “Functional Error Handling with Express.js and DDD | Enterprise Node.js + TypeScript” by Khalil Stemmler, https://khalilstemmler.com/articles/enterprise-typescript-nodejs/functional-error-handling/
- “GraphQL error handling to the max with Typescript, codegen and fp-ts” by Ghislain Thau, https://www.the-guild.dev/blog/graphql-error-handling-with-fp
- “A Monad in Practicality: First-Class Failures” by Quil, https://robotlolita.me/articles/2013/a-monad-in-practicality-first-class-failures/
- “A mostly complete guide to error handling in JavaScript” by Valentino Gagliardi, https://www.valentinog.com/blog/error/
- “Notification” by Martin Fowler, https://martinfowler.com/eaaDev/Notification.html
- “Parse, don’t validate” by Alexis King, https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/
- “Railway Oriented Programming” by Scott Wlaschin, https://fsharpforfunandprofit.com/rop/
- “Replacing Throwing Exceptions with Notification in Validations” by Martin Fowler, https://martinfowler.com/articles/replaceThrowWithNotification.html
- “Stop catching errors in TypeScript; Use the Either type to make your code predictable” by Anthony Manning-Franklin, https://antman-does-software.com/stop-catching-errors-in-typescript-use-the-either-type-to-make-your-code-predictable
- “Using Results in TypeScript” by Dan Imhoff, https://imhoff.blog/posts/using-results-in-typescript
- “Why should you return early?” by Szymon Krajewski https://szymonkrajewski.pl/why-should-you-return-early/

### Related Concepts

- Cross-Cutting Concern, Wikipedia, https://en.wikipedia.org/wiki/Cross-cutting_concern
- Fail-fast, Wikipedia, https://en.wikipedia.org/wiki/Fail-fast

### Tools

- Decorator Pattern, Refactoring Guru https://refactoring.guru/design-patterns/decorator
- Error Boundaries in React, https://reactjs.org/docs/error-boundaries.html
- `pipe`, fp/ts, https://gcanti.github.io/fp-ts/modules/function.ts.html#pipe
- `neverthrow`, Type-Safe Errors for JS & TypeScript, https://github.com/supermacro/neverthrow
- `sweet-monads`, Easy-to-use monads implementation with static types definition, https://github.com/JSMonk/sweet-monads
- `switch-exhaustiveness-check`, ES Lint TypeScript, https://typescript-eslint.io/rules/switch-exhaustiveness-check/
- `true-myth`, A library for safer and smarter error- and “nothing”-handling in TypeScript, https://github.com/true-myth/true-myth

## Module Integration

Coupling and cohesion. How to divide an application into modules and then compose these modules together. Why and how to decompose tasks. The benefits of contracts and guarantees between modules are. How to decouple modules from each other as much as possible but still leave room for them to communicate. The difference between object and functional composition is. How to manage dependencies. How to achieve data integrity and consistency.

### Books

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Debug It!: Find, Repair, and Prevent Bugs in Your Code” by Paul Butcher, https://www.goodreads.com/book/show/6770868-debug-it
- “Dependency Injection in .NET” by Mark Seemann, https://www.goodreads.com/book/show/9407722-dependency-injection-in-net
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Talks and Video

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o
- “Why Your Architecture is Functional” by Roman Nevolin, RU with Subtitles, https://youtu.be/9s_4wpzENhg

### Blog Posts and Examples

- “Antipatterns as a Worst Practices” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Antipatterns
- “Dependency Injection Using the Reader Monad” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/dependencies-3/
- “Dependency Injection with TypeScript in Practice” by Alex Bespoyasov, https://bespoyasov.me/blog/di-ts-in-practice/
- “Dependency Interpretation” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/dependencies-4/
- “Dependency Rejection” by Mark Seemann, https://blog.ploeh.dk/2017/02/02/dependency-rejection/
- “Evans Classification” by Martin Fowler, https://martinfowler.com/bliki/EvansClassification.html
- “Inversion of Control Containers and the Dependency Injection pattern” by Martin Fowler, https://martinfowler.com/articles/injection.html
- “Six approaches to dependency injection” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/dependencies/

### Related Concepts

- Cohesion, Wikipedia, https://en.wikipedia.org/wiki/Cohesion_(computer_science)
- Coupling, Wikipedia, https://en.wikipedia.org/wiki/Coupling_(computer_programming)
- Design By Contract, https://wiki.c2.com/?DesignByContract
- Message Broker, https://docs.microsoft.com/en-us/previous-versions/msp-n-p/ff648849(v=pandp.10)
- Message Bus, https://docs.microsoft.com/en-us/previous-versions/msp-n-p/ff647328(v=pandp.10)
- Message Queue, Wikipedia, https://en.wikipedia.org/wiki/Message_queue
- Separation of Concerns, Wikipedia, https://en.wikipedia.org/wiki/Separation_of_concerns

### Tools

- Design Patterns, Refactoring Guru, https://refactoring.guru/design-patterns/
- React software design patterns, https://github.com/themithy/react-design-patterns
- Reactive Extensions Library for JavaScript, RxJS, https://rxjs.dev
- REpresentational State Transfer, REST, https://restfulapi.net
- What is Connascence? https://connascence.io

## Generics and Hierarchies

How to understand when you need a generalized algorithm or type. Why composition is preferable to inheritance in most cases. How to use the Liskov substitution principle as an integration linter.

### Talks and Video

- “The Grand Unified Theory of Clean Architecture and Test Pyramid” by Guilherme Ferreira, https://youtu.be/gHSpj2zM9Nw
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0

### Blog Posts

- “A behavioral notion of subtyping” by Barbara H. Liskov, Jeannette M. Wing, https://dl.acm.org/doi/10.1145/197320.197383
- “Coupling, Cohesion & Connascence” by Khalil Stemmler, https://khalilstemmler.com/wiki/coupling-cohesion-connascence/
- “The Ins and Outs of Generic Variance in Kotlin” by Dave Leeds, https://typealias.com/guides/ins-and-outs-of-generic-variance/
- “Maintain a Single Layer of Abstraction at a Time” by Khalil Stemmler, https://khalilstemmler.com/articles/oop-design-principles/maintain-a-single-layer-of-abstraction/
- “The Power of Composition” by Scott Wlaschin, https://fsharpforfunandprofit.com/composition/
- “Why variance matters” by Ted Kaminski https://www.tedinski.com/2018/06/26/variance.html

### Related Concepts

- Covariance and Contravariance, Wikipedia, https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)
- Design By Contract, https://wiki.c2.com/?DesignByContract
- Generics in TypeScript, https://www.typescriptlang.org/docs/handbook/2/generics.html
- The Liskov Substitution Principle, https://web.archive.org/web/20151128004108/http://www.objectmentor.com/resources/articles/lsp.pdf
- Types and Typeclasses, http://learnyouahaskell.com/types-and-typeclasses

## Application Architecture

How poor architecture can hamper refactoring. How to use ubiquitous language to improve the architecture. How to build interaction with the outside world and manage dependencies. How ports and adapters are useful. Reasons to separate UI-logic from business-logic. How architecture affects testability.

### Books

- “Clean Architecture” by Robert C. Martin, https://www.goodreads.com/book/show/18043011-clean-architecture
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Designing Data-Intensive Applications” by Martin Kleppmann https://dataintensive.net
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Enterprise Integration Patterns” by Gregor Hohpe, https://www.goodreads.com/book/show/85012.Enterprise_Integration_Patterns
- “Software Architecture in Practice” by L. Bass, P. Clements, R. Kazman, https://www.goodreads.com/book/show/70143.Software_Architecture_in_Practice
- “What I've Learned From Failure” by Reg Braithwaite, https://leanpub.com/shippingsoftware/read
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Talks and Video

- “Design, Composition, and Performance” by Rich Hickey, https://youtu.be/MCZ3YgeEUPg
- “Is Domain-Driven Design Overrated?” by Stefan Tilkov, https://youtu.be/ZZp9RQEGeqQ
- “The Grand Unified Theory of Clean Architecture and Test Pyramid” by Guilherme Ferreira, https://youtu.be/gHSpj2zM9Nw
- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- “Why Your Architecture is Functional” by Roman Nevolin, RU with Subtitles, https://youtu.be/9s_4wpzENhg

### Blog Posts

- “DDD, Hexagonal, Onion, Clean, CQRS, … How I put it all together” by Herberto Graça, https://herbertograca.com/2017/11/16/explicit-architecture-01-ddd-hexagonal-onion-clean-cqrs-how-i-put-it-all-together/
- “Functional Architecture is Ports and Adapters” by Mark Seemann, https://blog.ploeh.dk/2016/03/18/functional-architecture-is-ports-and-adapters/
- “Functional Core in Imperative Shell” by Gary Bernhardt, https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell
- “How to ask good questions” by Julia Evans, https://jvns.ca/blog/good-questions/
- “Impureim Sandwich” by Mark Seemann, https://blog.ploeh.dk/2020/03/02/impureim-sandwich/
- “The pedantic checklist for changing your data model in a web application” by Raphael Gaschignard, https://rtpg.co/2021/06/07/changes-checklist.html
- “Ports & Adapters Architecture” by Herberto Graça, https://herbertograca.com/2017/09/14/ports-adapters-architecture/
- “The Software Architecture Chronicles” by Herberto Graça, https://herbertograca.com/2017/07/03/the-software-architecture-chronicles/
- “Test-Induced Design Damage” by David Heinemeier Hansson, https://dhh.dk/2014/test-induced-design-damage.html
- “View Models”, Reactive UI, https://www.reactiveui.net/docs/handbook/view-models/

### Related Concepts

- Model-View-ViewModel, Wikipedia, https://en.wikipedia.org/wiki/Model–view–viewmodel
- List of System Quality Attributes, Wikipedia, https://en.wikipedia.org/wiki/List_of_system_quality_attributes
- Reactive Programming, Wikipedia, https://en.wikipedia.org/wiki/Reactive_programming
- Software Requirements, Wikipedia, https://en.wikipedia.org/wiki/Software_requirements
- Ubiquitous Language, https://martinfowler.com/bliki/UbiquitousLanguage.html

### Tools

- Anti-corruption Layer Pattern, https://docs.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer
- Feature-Sliced Design, Architectural methodology for frontend projects, https://feature-sliced.design
- Ubiquitous Language, https://martinfowler.com/bliki/UbiquitousLanguage.html

## Declarative Style

The benefits of a declarative code style over an imperative one. Situations when to prefer the imperative style. Why finite state machines can be useful in frontend development.

### Books

- “Antifragile: Things That Gain from Disorder” by Nassim Nicholas Taleb, https://www.goodreads.com/book/show/13530973-antifragile
- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Talks and Video

- “Design, Composition, and Performance” by Rich Hickey, https://youtu.be/MCZ3YgeEUPg
- “Designing Declarative APIs” by Ilya Birman, https://youtu.be/uCQ3JFuQ7bQ
- “Goodbye, useEffect” by David Khourshid, https://youtu.be/HPoC-k7Rxwo
- “Metaprogramming and Multiparadigm Programming” by Timur Shemsedinov, RU with EN Slides, https://youtu.be/Bo9y4IxdNRY
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o
- “Type Level Programming in TypeScript” by Gabriel Vergnaud, https://youtu.be/vGVvJuazs84

### Blog Posts

- “Application State Management with Finite State Machines” by Alex Bespoyasov, https://bespoyasov.me/blog/fsm-to-the-rescue/
- “Declarative Data Validation with Rule-Based Approach and Functional Programming” by Alex Bespoyasov, https://bespoyasov.me/blog/declarative-rule-based-validation/
- “Maintain a Single Layer of Abstraction at a Time” by Khalil Stemmler, https://khalilstemmler.com/articles/oop-design-principles/maintain-a-single-layer-of-abstraction/
- “Prefer Domain-Specific Types to Primitive Types” by Einar Landre, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_65/
- “Simplicity Comes from Reduction” by Paul W. Homer, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_75/

### Related Concepts

- Transformation Priority Premise, Wikipedia https://en.wikipedia.org/wiki/Transformation_Priority_Premise
- Finite State Machine, Wikipedia, https://en.wikipedia.org/wiki/Finite-state_machine
- Pattern Matching, Wikipedia, https://en.wikipedia.org/wiki/Pattern_matching

### Tools

- 12 Factor Apps, https://12factor.net
- JavaScript and TypeScript finite state machines and statecharts, XState, https://github.com/statelyai/xstate
- Zen of Python, https://peps.python.org/pep-0020/

## Static Typing

How to use types to convey more knowledge about the domain. How to make invalid data states unrepresentable in code. How to use types to detect development principles violation.

### Books

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design

### Talks and Video

- “Type Level Programming in TypeScript” by Gabriel Vergnaud, https://youtu.be/vGVvJuazs84

### Blog Posts

- “Branding and Type-Tagging” by Kevin B. Greene, https://medium.com/@KevinBGreene/surviving-the-typescript-ecosystem-branding-and-type-tagging-6cf6e516523d
- “Bringing Pattern Matching to TypeScript” by Gabriel Vergnaud, https://dev.to/gvergnaud/bringing-pattern-matching-to-typescript-introducing-ts-pattern-v3-0-o1k
- “Code in the Language of the Domain” by Dan North, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_11/
- “Constructive vs predicative data” by Hillel Wayne, https://www.hillelwayne.com/post/constructive/
- “Designing with types: Making illegal states unrepresentable” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/designing-with-types-making-illegal-states-unrepresentable/
- “Making Illegal States Unrepresentable” by Hillel Wayne, https://buttondown.email/hillelwayne/archive/making-illegal-states-unrepresentable/
- “Prefer Domain-Specific Types to Primitive Types” by Einar Landre, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_65/

### Related Concepts

- Ubiquitous Language, https://martinfowler.com/bliki/UbiquitousLanguage.html
- Type Aliases, TypeScript Handbook, https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases

### Tools

- Tiny Types in TypeScript, https://janmolak.com/tiny-types-in-typescript-4680177f026e
- `switch-exhaustiveness-check`, ES Lint TypeScript, https://typescript-eslint.io/rules/switch-exhaustiveness-check/

## Refactoring Test Code

How not to break tests during refactoring. What makes tests brittle.” How to find a balance between high coverage and low test-induced damage.

### Books

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Talks and Video

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY

### Blog Posts and Examples

- “Choosing properties for property-based testing” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/property-based-testing-2/
- “Refactoring, Changing the Code without Changing its External Behavior” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Refactoring
- “TDD: What, How, and Why” by Alex Bespoyasov, https://bespoyasov.me/blog/tdd-what-how-and-why/
- “Test-Induced Design Damage” by David Heinemeier Hansson, https://dhh.dk/2014/test-induced-design-damage.html
- “Unit testing best practices with .NET Core and .NET Standard”, MSDN, https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices

### Tools

- Faker, Generate fake (but realistic) data for testing and development, https://fakerjs.dev
- Test-Driven Development, TDD, https://martinfowler.com/bliki/TestDrivenDevelopment.html
- `git stash`, Stash the changes in a dirty working directory away, https://git-scm.com/docs/git-stash

## Everything Around the Code

How to synchronize the sources of information in a project. How and why to make comments more informative. How to increase the benefits of documentation without increasing the cost of maintaining it.

### Books

- “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Envisioning Information” by Edward R. Tufte, https://www.goodreads.com/book/show/17745.Envisioning_Information
- “The Newspaper Designer's Handbook” by Tim Harrower, https://www.goodreads.com/book/show/61881153-the-newspaper-designer-s-handbook

### Blog Posts and Studies

- “Copypaste in Code” by Alex Bespoyasov, https://bespoyasov.me/blog/copy-paste/
- “How to ask good questions” by Julia Evans, https://jvns.ca/blog/good-questions/
- “Information Density and Linguistic Encoding”, by Matthew W. Crocker, Vera Demberg, Elke Teich https://www.researchgate.net/publication/283938800_Information_Density_and_Linguistic_Encoding_IDeaL

### Tools

- Architectural Decision Records, ADR, https://adr.github.io
- JSDoc Reference, TypeScript Documentation https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html

## Refactoring as a Process

How to decide whether to refactor or rewrite the code. The information to collect about the project before the start. How to estimate the time required for a task. Metrics to use to measure the effect of refactoring on the code.

### Books

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Talks and Video

- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Preventing the Collapse of Civilization” by Jonathan Blow, https://youtu.be/pW-SOdj4Kkk
- “Where Does Bad Code Come From?” https://youtu.be/7YpFGkG-u1w

### Blog Posts and Examples

- “Antipatterns as a Worst Practices” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Antipatterns
- “The Boy Scout Rule” by Robert C. Martin, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_08/
- “Strangler Fig Application” by Martin Fowler https://martinfowler.com/bliki/StranglerFigApplication.html
- “Opportunistic Refactoring” by Martin Fowler https://martinfowler.com/bliki/OpportunisticRefactoring.html
- “When to Refactor” by Mark Seemann, https://blog.ploeh.dk/2022/09/19/when-to-refactor/
- “Refactoring, Changing the Code without Changing its External Behavior” by Timur Shemsedinov, https://github.com/HowProgrammingWorks/Refactoring
