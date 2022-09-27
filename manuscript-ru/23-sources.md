# Список литературы

На моё мнение о современной разработке и хорошем коде повлияли работы других людей. В этом приложении я подготовил список книг, докладов, статей, методологий и исследований, которые считаю самыми полезными и использую в работе сам.

Я разбил список по темам, схожим с темами глав. Если вас заинтересовала тема конкретной главы и вы хотите узнать больше, разделы в списке помогут вам удобнее ориентироваться среди ссылок.

Материалы могут повторяться, если относятся к нескольким темам. Я решил, что важнее собрать полный список для каждой темы, чем избавиться от дублей. Надеюсь, это окажется вам полезным.

## Понятие рефакторинга и качества кода

О том, что такое рефакторинг вообще, зачем он нужен. К чему приводит неуправляемо растущая сложность проекта. Как определять «плохой» и «хороший» код.

### Книги

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
- “The Black Swan” by Nassim Nicholas Taleb, https://www.goodreads.com/book/show/242472.The_Black_Swan
- “Clean Code” by Robert C. Martin, https://www.goodreads.com/book/show/3735293-clean-code
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Software Design: Cognitive Aspect” by Françoise Détienne, https://www.goodreads.com/book/show/3104497-software-design-cognitive-aspect
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Видео

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “Log4J & JNDI Exploit: Why So Bad?” https://youtu.be/Opqgwn8TdlM
- “Preventing the Collapse of Civilization” by Jonathan Blow, https://youtu.be/pW-SOdj4Kkk
- “Where Does Bad Code Come From?” https://youtu.be/7YpFGkG-u1w
- «Рефакторинг: причины, цели, техники и процесс» Тимур Шемсединов, https://youtu.be/z73wmpdweQ4

### Статьи и исследования

- “Beauty Is in Simplicity”, by Jørn Ølmheim, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_05/
- Code Readability Testing, an Empirical Study, https://www.researchgate.net/publication/299412540_Code_Readability_Testing_an_Empirical_Study
- Evaluating Code Readability and Legibility: An Examination of Human-centric Studies, https://github.com/reydne/code-comprehension-review
- “The Human Cost of Tech Debt”, by Erik Dietrich, https://daedtech.com/human-cost-tech-debt/
- How Readable Code Is, https://howreadable.com
- “Technical Debt”, DevIQ, https://deviq.com/terms/technical-debt
- «Попасть в окно рефакторинга» Иван Немытченко, https://web.archive.org/web/20201208134906/dopo.st/inem/200530110137

### Связанные понятия

- Автобусный фактор, Википедия, https://ru.wikipedia.org/wiki/Фактор_автобуса
- Закон Мёрфи, Википедия, https://ru.wikipedia.org/wiki/Закон_Мерфи
- Энтропия, Википедия, https://ru.wikipedia.org/wiki/Энтропия

### Инструменты

- Code Smells, Refactoring Guru, https://refactoring.guru/refactoring/smells
- Refactoring Technics, Refactoring Guru, https://refactoring.guru/refactoring/techniques

## Прежде, чем начать

На что обратить внимание до начала рефакторинга. Как подготовить код к изменениям, чтобы упростить работу. Как обезопасить работу с будущими изменениями.

### Книги

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Debug It!: Find, Repair, and Prevent Bugs in Your Code” by Paul Butcher, https://www.goodreads.com/book/show/6770868-debug-it
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow
- “Willpower Doesn't Work” by Benjamin P. Hardy, https://www.goodreads.com/book/show/35604684-willpower-doesn-t-work
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Видео

- “The Grand Unified Theory of Clean Architecture and Test Pyramid” by Guilherme Ferreira, https://youtu.be/gHSpj2zM9Nw
- “How do you prepare before tackling a problem?” by Fun-fun-function, https://youtu.be/mF-tVjXbO8Y
- «Изолируй это» Илья Климов, https://youtu.be/BFhhrg6IzI8
- «Рефакторинг: причины, цели, техники и процесс» Тимур Шемсединов, https://youtu.be/z73wmpdweQ4

### Статьи

- “Before You Refactor” by Rajith Attapattu, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_06/
- “How to ask good questions” by Julia Evans, https://jvns.ca/blog/good-questions/
- “Read Code” by Karianne Berg, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_70/
- «Попасть в окно рефакторинга» Иван Немытченко, https://web.archive.org/web/20201208134906/dopo.st/inem/200530110137
- TDD: зачем и как, https://bespoyasov.ru/blog/tdd-what-how-and-why/

### Связанные понятия

- Agile, гибкая методология разработки, Википедия, https://ru.wikipedia.org/wiki/Гибкая_методология_разработки
- Оценка емкости рабочей памяти, Википедия, https://ru.wikipedia.org/wiki/Рабочая_память#Оценка_емкости_рабочей_памяти

### Инструменты

- Extreme Programming, http://www.extremeprogramming.org
- Test-Driven Development, TDD, https://martinfowler.com/bliki/TestDrivenDevelopment.html
- Tools for Better Thinking, https://untools.co

## Во время рефакторинга

Чего избегать во время рефакторинга, как облегчить процесс. Как изолировать изменения и убедиться, что не поломался другой код. Как не выходить за рамки бюджета и держать изменения небольшими.

### Книги

- “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- «Джедайские техники» Максим Дорофеев, https://www.goodreads.com/book/show/34656521
- «Новые правила деловой переписки» М. Ильяхов, Л. Сарычева, https://www.goodreads.com/book/show/41070833

### Видео

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Where Does Bad Code Come From?” https://youtu.be/7YpFGkG-u1w
- «Антипаттерны общие для всех парадигм» Тимур Шемсединов, https://youtu.be/NMUsUiFokr4

### Статьи

- “Convenience Is not an -ility” by Gregor Hohpe, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_19/
- “Deploy Early and Often” by Steve Berczuk, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_20/
- “How to Get Your Code Reviewed Faster” by Artem Sapegin, https://blog.sapegin.me/all/faster-code-reviews/
- Не надо заставлять, надо автоматизировать, https://bespoyasov.ru/blog/do-not-push-automate-instead/

### Связанные понятия

- Agile, гибкая методология разработки, Википедия, https://ru.wikipedia.org/wiki/Гибкая_методология_разработки
- Atomic Commit, Wikipedia https://en.wikipedia.org/wiki/Atomic_commit
- Transformation Priority Premise, Wikipedia https://en.wikipedia.org/wiki/Transformation_Priority_Premise
- Декомпозиция, Википедия https://ru.wikipedia.org/wiki/Декомпозиция
- Непрерывная интеграция, Википедия, https://ru.wikipedia.org/wiki/Непрерывная_интеграция

### Инструменты

- Oh Shit, Git!?! https://ohshitgit.com
- Transformation Priority Premise, Wikipedia https://en.wikipedia.org/wiki/Transformation_Priority_Premise
- Use binary search to find the commit that introduced a bug, https://git-scm.com/docs/git-bisect
- Непрерывная интеграция, Википедия, https://ru.wikipedia.org/wiki/Непрерывная_интеграция
- О системе контроля версий, https://git-scm.com/book/ru/v2/Введение-О-системе-контроля-версий#ch01-getting-started

## Форматирование, линтинг и возможности языка

Как использовать все возможности автоматических инструментов рефакторинга и анализа кода. Зачем знать нюансы языка и окружения, в котором исполняется код. В чём польза автоматизации. Как консистентность помогает быстрее решать задачи.

### Книги

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Autopilot: The Art & Science of Doing Nothing” by Andrew Smart, https://www.goodreads.com/book/show/18053732-autopilot
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow

### Видео

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o
- «ESLint & Prettier — Партизанщина» Илья Климов, https://youtu.be/HzUfCNgzkb0

### Статьи и исследования

- “Automate Your Coding Standard” by Filip van Laenen, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_04/
- “Code Layout Matters” by Steve Freeman, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_13/
- How Readable Code Is, https://howreadable.com
- “Know Your IDE” by Heinz Kabutz, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_45/
- “Know Your Next Commit” by Dan Bergh Johnsson, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_47/
- “Why robots should format our code for us”, https://blog.sapegin.me/all/prettier/

### Инструменты

- Can I Use, support tables for web, https://caniuse.com
- List of EcmaScript Proposals, https://proposals.es
- Prettier, an opinionated code formatter, https://prettier.io
- Refactoring Source Code in VSCode, https://code.visualstudio.com/docs/editor/refactoring

## Именование сущностей

Почему именование важно, как имена переменных и функций влияют на восприятие кода и скорость разработки. Почему синхронизация терминологии улучшает взаимодействие в команде. Как определить «плохие» и «хорошие» имена. Что делать с именами, которые врут.

### Книги

- “Clean Code” by Robert C. Martin, https://www.goodreads.com/book/show/3735293-clean-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Software Design: Cognitive Aspect” by Françoise Détienne, https://www.goodreads.com/book/show/3104497-software-design-cognitive-aspect
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Видео

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Transforming Code into Beautiful, Idiomatic Python” https://youtu.be/OSGv2VnC0go
- «Антипаттерны общие для всех парадигм» Тимур Шемсединов, https://youtu.be/NMUsUiFokr4
- «Рефакторинг: причины, цели, техники и процесс» Тимур Шемсединов, https://youtu.be/z73wmpdweQ4

### Статьи и исследования

- “Code in the Language of the Domain” by Dan North, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_11/
- “Coding with Clarity” by Brandon Gregory, https://alistapart.com/article/coding-with-clarity/
- Evaluating Code Readability and Legibility: An Examination of Human-centric Studies, https://github.com/reydne/code-comprehension-review/blob/master/list-papers/AllPhasesMergedPapers-Part1.md
- “Give it five minutes” by Jason Fried, https://signalvnoise.com/posts/3124-give-it-five-minutes
- “How to ask good questions” by Julia Evans, https://jvns.ca/blog/good-questions/
- “Read Code” by Karianne Berg, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_70/
- “Stop using isLoading booleans” by Kent C. Dodds, https://kentcdodds.com/blog/stop-using-isloading-booleans
- Как теория близости работает в интерфейсе? https://bureau.ru/bb/soviet/20160503/
- Теория близости, Ководство, https://www.artlebedev.ru/kovodstvo/sections/136/

### Связанные понятия

- Автобусный фактор, Википедия, https://ru.wikipedia.org/wiki/Фактор_автобуса
- Декларативность, Википедия, https://ru.wikipedia.org/wiki/Декларативное_программирование
- Декомпозиция, Википедия https://ru.wikipedia.org/wiki/Декомпозиция
- Уровни абстракции, Википедия, https://ru.wikipedia.org/wiki/Уровень_абстракции_(программирование)
- Энтропия, Википедия, https://ru.wikipedia.org/wiki/Энтропия

### Инструменты

- Conventional Commits, https://www.conventionalcommits.org/en/v1.0.0/
- Naming Cheat Sheet, https://github.com/kettanaito/naming-cheatsheet
- Refactoring Source Code in VSCode, https://code.visualstudio.com/docs/editor/refactoring
- Refactoring Techniques, Refactoring Guru, https://refactoring.guru/refactoring/techniques
- Ubiquitous Language, https://martinfowler.com/bliki/UbiquitousLanguage.html

## Дублирование кода

Как разделять дублирование и недостаток знаний, зачем использовать дублирование в качестве инструмента. В чём польза регулярных аудитов кода и как выработать привычку их заводить.

### Книги

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- «Джедайские техники» Максим Дорофеев, https://www.goodreads.com/book/show/34656521

### Видео

- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- «Антипаттерны общие для всех парадигм» Тимур Шемсединов, https://youtu.be/NMUsUiFokr4
- «Рефакторинг: причины, цели, техники и процесс» Тимур Шемсединов, https://youtu.be/z73wmpdweQ4

### Статьи

- “Beware the Share” by Udi Dahan, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_07/
- “How to ask good questions” by Julia Evans, https://jvns.ca/blog/good-questions/
- “The Single Responsibility Principle” by Robert C. Martin, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_76/
- “When You SHOULD Duplicate Code” by Marius Bongarts, https://medium.com/@mariusbongarts/when-you-should-duplicate-code-b0d747bc1c67
- «Копипаста в коде», https://bespoyasov.ru/blog/copy-paste/

### Связанные понятия

- Don't Repeat Yourself, Wikipedia https://ru.wikipedia.org/wiki/Don’t_repeat_yourself
- Разделение ответственности, Википедия, https://ru.wikipedia.org/wiki/Разделение_ответственности

### Инструменты

- Copy/paste detector, jscpd, https://www.npmjs.com/package/jscpd
- Detect copy-pasted and structurally similar code, jsinspect, https://github.com/danielstjules/jsinspect
- Duplicate Code Smell, Refactoring Guru, https://refactoring.guru/smells/duplicate-code

## Абстракция и разделение ответственности

Как и зачем использовать абстракцию. Зачем делить намерение и реализацию. Чем мешает ограничение рабочей памяти человеческого мозга. Как выдавать информацию о системе дозировано и контролируемо. Что помогает декомпозировать сложные задачи на более простые. Как удостовериться, что данные в корректном состоянии.

### Книги

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

### Видео

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Design, Composition, and Performance” by Rich Hickey, https://youtu.be/MCZ3YgeEUPg
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- «Антипаттерны общие для всех парадигм» Тимур Шемсединов, https://youtu.be/NMUsUiFokr4

### Статьи

- “Climbing the infinite ladder of abstraction” by Alexis King, https://lexi-lambda.github.io/blog/2016/08/11/climbing-the-infinite-ladder-of-abstraction/
- “Coupling, Cohesion & Connascence” by Khalil Stemmler, https://khalilstemmler.com/wiki/coupling-cohesion-connascence/
- “Maintain a Single Layer of Abstraction at a Time” by Khalil Stemmler, https://khalilstemmler.com/articles/oop-design-principles/maintain-a-single-layer-of-abstraction/
- “The Power of Composition” by Scott Wlaschin, https://fsharpforfunandprofit.com/composition/
- “Prefer Domain-Specific Types to Primitive Types” by Einar Landre, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_65/
- «Полиморфизм простыми словами» Sergey Ufocoder, https://medium.com/devschacht/polymorphism-207d9f9cd78
- «Декларативная валидация с помощью правило-ориентированного подхода и функционального программирования», https://bespoyasov.ru/blog/declarative-rule-based-validation/
- «Потерянная абстракция», https://bespoyasov.ru/blog/missing-abstraction/

### Связанные понятия

- Абстракция, Википедия, https://ru.wikipedia.org/wiki/Абстракция
- Декомпозиция, Википедия https://ru.wikipedia.org/wiki/Декомпозиция
- Зацепление в программировании, Википедия, https://ru.wikipedia.org/wiki/Зацепление_(программирование)
- Инкапсуляция в программировании, Википедия, https://ru.wikipedia.org/wiki/Инкапсуляция_(программирование)
- Оценка емкости рабочей памяти, Википедия, https://ru.wikipedia.org/wiki/Рабочая_память#Оценка_емкости_рабочей_памяти
- Разделение ответственности, Википедия, https://ru.wikipedia.org/wiki/Разделение_ответственности
- Связность в программировании, Википедия, https://ru.wikipedia.org/wiki/Связность_(программирование)
- Уровни абстракции, Википедия, https://ru.wikipedia.org/wiki/Уровень_абстракции_(программирование)

### Инструменты

- Single Responsibility Principle, Principles of OOD, http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod
- Tiny Types in TypeScript, https://janmolak.com/tiny-types-in-typescript-4680177f026e
- Tools for Better Thinking, https://untools.co
- What is Connascence? https://connascence.io
- Теория решения изобретательских задач, Википедия, https://ru.wikipedia.org/wiki/Теория_решения_изобретательских_задач

## Функциональный пайплайн и линейное выполнение

Как и зачем выделять состояния данных в бизнес-процессах. В чём польза линейного выполнения кода. Как «запретить» передачу невалидных данных и обособить валидацию. В чём польза принципов функционального программирования.

### Книги

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Thinking, Fast and Slow” by Daniel Kahneman, https://www.goodreads.com/book/show/11468377-thinking-fast-and-slow

### Видео

- “Is Domain-Driven Design Overrated?” by Stefan Tilkov, https://youtu.be/ZZp9RQEGeqQ
- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- “Professor Frisby Introduces Composable Functional JavaScript” by Brian Lonsdorf, https://egghead.io/courses/professor-frisby-introduces-composable-functional-javascript
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o
- «Почему ваша архитектура функциональная и как с этим жить» Роман Неволин, https://youtu.be/9s_4wpzENhg
- «Быстрорастворимое проектирование» Максим Аршинов, https://youtu.be/qJPwSvDLmQE

### Статьи

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

### Связанные понятия

- Data Mapper, https://martinfowler.com/eaaCatalog/dataMapper.html
- Immutable Object, Wikipedia, https://en.wikipedia.org/wiki/Immutable_object
- Projection operations (C#), https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/projection-operations
- Декларативность, Википедия, https://ru.wikipedia.org/wiki/Декларативное_программирование
- Композиция функций, Википедия, https://ru.wikipedia.org/wiki/Композиция_функций
- Объект передачи данных, DTO, Википедия, https://ru.wikipedia.org/wiki/DTO
- Функциональное программирование, Википедия, https://ru.wikipedia.org/wiki/Функциональное_программирование
- Функция-предикат, Википедия, https://ru.wikipedia.org/wiki/Предикат
- Чистые функции, Википедия, https://ru.wikipedia.org/wiki/Чистота_функции

### Инструменты

- Bounded Context in DDD by Martin Fowler, https://www.martinfowler.com/bliki/BoundedContext.html
- Design Patterns, Refactoring Guru, https://refactoring.guru/design-patterns
- Domain Model, https://martinfowler.com/eaaCatalog/domainModel.html
- Specification for interoperability of common algebraic structures in JavaScript, Fantasy Land, https://github.com/fantasyland/fantasy-land
- Typed functional programming in TypeScript, fp/ts, https://github.com/gcanti/fp-ts
- The library which provides useful monads, interfaces, and lazy iterators, sweet-monads, https://github.com/JSMonk/sweet-monads
- Zen of Python, https://peps.python.org/pep-0020/

## Условия и сложность

Как организовать условия в коде, чтобы не увеличить сверх меры когнитивную нагрузку кода. Какие метрики использовать для измерения сложности. Как использовать автоматические инструменты для управления сложностью. Зачем «выпрямлять» выполнение кода и «выворачивать» условия. Как использовать законы булевой алгебры, чтобы упрощать условия. Какие шаблоны проектирования могут помочь это сделать. Как применять функциональное программирование для этих же целей.

### Книги

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Debug It!: Find, Repair, and Prevent Bugs in Your Code” by Paul Butcher, https://www.goodreads.com/book/show/6770868-debug-it
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Видео

- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- “Maybe Not” by Rich Hickey, https://youtu.be/YR5WdGrpoug
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o
- «Антипаттерны общие для всех парадигм» Тимур Шемсединов, https://youtu.be/NMUsUiFokr4
- «Почему ваша архитектура функциональная и как с этим жить» Роман Неволин, https://youtu.be/9s_4wpzENhg
- «Рефакторинг: причины, цели, техники и процесс» Тимур Шемсединов, https://youtu.be/z73wmpdweQ4

### Статьи и исследования

- “Anti-if, the Missing Patterns” by Joe Wright, https://code.joejag.com/2016/anti-if-the-missing-patterns.html
- “Bringing Pattern Matching to TypeScript” by Gabriel Vergnaud, https://dev.to/gvergnaud/bringing-pattern-matching-to-typescript-introducing-ts-pattern-v3-0-o1k
- “Cognitive Complexity. A new way of measuring understandability” by G. Ann Campbell, SonarSource SA, https://www.sonarsource.com/docs/CognitiveComplexity.pdf
- “A conditional sandwich example” by Mark Seemann, https://blog.ploeh.dk/2022/02/14/a-conditional-sandwich-example/
- “Decoupling decisions from effects” by Mark Seemann, https://blog.ploeh.dk/2016/09/26/decoupling-decisions-from-effects/
- “Destroy all `if`s” by John A De Goes, https://degoes.net/articles/destroy-all-ifs
- “Functional design is intrinsically testable” by Mark Seemann, https://blog.ploeh.dk/2015/05/07/functional-design-is-intrinsically-testable/
- “Parse, don’t validate” by Alexis King, https://lexi-lambda.github.io/blog/2019/11/05/parse-don-t-validate/
- “Why should you return early?” by Szymon Krajewski https://szymonkrajewski.pl/why-should-you-return-early/
- «Распутываем сложные условия в коде» isqua, https://isqua.ru/blog/2017/07/09/simplify-if-else/
- «Такой простой `Boolean`» isqua, https://isqua.ru/blog/2016/08/14/takoi-prostoi-boolean/
- «Управление состоянием приложения с помощью конечного автомата», https://bespoyasov.ru/blog/fsm-to-the-rescue/

### Связанные понятия

- “Cognitive Complexity. A new way of measuring understandability” by G. Ann Campbell, SonarSource SA, https://www.sonarsource.com/docs/CognitiveComplexity.pdf
- Control flow graph & cyclomatic complexity for following procedure, Stackoverflow, https://stackoverflow.com/a/2670135/3141337
- Граф потока управления, Википедия, https://ru.wikipedia.org/wiki/Граф_потока_управления
- Сопоставление с образцом, Википедия, https://ru.wikipedia.org/wiki/Сопоставление_с_образцом
- Цикломатическая сложность, Википедия, https://ru.wikipedia.org/wiki/Цикломатическая_сложность

### Инструменты

- `complexity`, ES Lint, https://eslint.org/docs/latest/rules/complexity
- Design Patterns, Refactoring Guru, https://refactoring.guru/design-patterns/
- ECMAScript Pattern Matching Proposal, https://github.com/tc39/proposal-pattern-matching
- `switch-exhaustiveness-check`, ES Lint TypeScript, https://typescript-eslint.io/rules/switch-exhaustiveness-check/
- `ts-pattern`, Library for Pattern Matching in TypeScript, https://github.com/gvergnaud/ts-pattern
- Законы де Моргана, Википедия, https://ru.wikipedia.org/wiki/Законы_де_Моргана

## Работа с сайд-эффектами

Почему сайд-эффекты делают программу сложнее и непредсказуемее. Как уменьшать количество эффектов в коде и что делать с эффектами, необходимыми для работы приложения. В чём польза чистых функций и ссылочной прозрачности. Как тестировать эффекты, зачем отделять логику от эффектов. Что такое команды и запросы, в чём смысл деления кода на них.

### Книги

- “Clean Architecture” by Robert C. Martin, https://www.goodreads.com/book/show/18043011-clean-architecture
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Видео

- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- «Быстрорастворимое проектирование» Максим Аршинов, https://youtu.be/qJPwSvDLmQE
- «Грамотная работа с ошибками» Андрей Мелихов, https://youtu.be/eh5flHypkDg
- «Монады — не приговор» Виталий Брагилевский, https://youtu.be/IkXg_mjNgG4
- «Почему ваша архитектура функциональная и как с этим жить» Роман Неволин, https://youtu.be/9s_4wpzENhg

### Статьи

- “Command-Query Separation” by Martin Fowler, https://martinfowler.com/bliki/CommandQuerySeparation.html
- “Command-Query Responsibility Segregation” by Martin Fowler, https://martinfowler.com/bliki/CQRS.html
- “CQS versus server generated IDs” by Mark Seemann, https://blog.ploeh.dk/2014/08/11/cqs-versus-server-generated-ids/
- “Functional architecture is Ports and Adapters” by Mark Seemann, https://blog.ploeh.dk/2016/03/18/functional-architecture-is-ports-and-adapters/
- “Functional Core in Imperative Shell” by Gary Bernhardt, https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell
- “Impureim Sandwich” by Mark Seemann, https://blog.ploeh.dk/2020/03/02/impureim-sandwich/
- “Message Passing Leads to Better Scalability in Parallel Systems” by Russel Winder, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_57/
- “A pipe operator for JavaScript” by Axel Rauschmayer, https://2ality.com/2022/01/pipe-operator.html
- «Разделение функций на команды и запросы», https://bespoyasov.ru/blog/commands-and-queries/

### Связанные понятия

- Design Patterns, Refactoring Guru, https://refactoring.guru/design-patterns
- Immutable Object, Wikipedia, https://en.wikipedia.org/wiki/Immutable_object
- Referential Transparency, Haskell Wiki, https://wiki.haskell.org/Referential_transparency
- Чистые функции, Википедия, https://ru.wikipedia.org/wiki/Чистота_функции

## Обработка ошибок

Какие бывают виды ошибок, чем они отличаются. Как и чем мешает запутанная обработка ошибок. На что обращать внимание при рефакторинге обработки ошибок в JS-коде. Какие использовать техники при наличии ограничений на технологии, парадигму или методологии.

### Книги

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Patterns for Fault Tolerant Software” by Robert Hanmer, https://www.goodreads.com/book/show/3346135-patterns-for-fault-tolerant-software
- “The Pragmatic Programmer” by Andy Hunt, https://www.goodreads.com/book/show/4099.The_Pragmatic_Programmer
- “What I've Learned From Failure” by Reg Braithwaite, https://leanpub.com/shippingsoftware/read

### Видео

- “Error handling: doing it right!” by Ruben Bridgewater, https://youtu.be/bJ3glfA-jqo
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- “Maybe Not” by Rich Hickey, https://youtu.be/YR5WdGrpoug
- «Грамотная работа с ошибками» Андрей Мелихов, https://youtu.be/eh5flHypkDg
- «Монады — не приговор» Виталий Брагилевский, https://youtu.be/IkXg_mjNgG4
- «(Не|ну)жная монада Either на практике и в теории» Д. Махнёв, А. Кобзарь, https://youtu.be/T6Os27MKUCQ

### Статьи

- “Against Railway-Oriented Programming” by by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/against-railway-oriented-programming/
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
- «Декларативная валидация с помощью правило-ориентированного подхода и функционального программирования», https://bespoyasov.ru/blog/declarative-rule-based-validation/

### Связанные понятия

- Cross-Cutting Concern, Wikipedia, https://en.wikipedia.org/wiki/Cross-cutting_concern
- Fail-fast, Wikipedia, https://en.wikipedia.org/wiki/Fail-fast

### Инструменты

- Decorator Pattern, Refactoring Guru https://refactoring.guru/design-patterns/decorator
- `pipe`, fp/ts, https://gcanti.github.io/fp-ts/modules/function.ts.html#pipe
- `neverthrow`, Type-Safe Errors for JS & TypeScript, https://github.com/supermacro/neverthrow
- `sweet-monads`, Easy-to-use monads implementation with static types definition, https://github.com/JSMonk/sweet-monads
- `switch-exhaustiveness-check`, ES Lint TypeScript, https://typescript-eslint.io/rules/switch-exhaustiveness-check/
- `true-myth`, A library for safer and smarter error- and “nothing”-handling in TypeScript, https://github.com/true-myth/true-myth
- Предохранители в React, https://ru.reactjs.org/docs/error-boundaries.html

## Интеграция модулей

Что такое зацепление и связность. Как делить приложение на модули и потом компоновать эти модули друг с другом. Зачем и по какому принципу декомпозировать задачи. В чём польза контрактов и гарантий между модулями. Как максимально расцепить модули друг от друга, но при этом оставить возможность для их общения. В чём разница между объектной и функциональной композицией. Как работать с зависимостями. Как добиться целостности и согласованности данных.

### Книги

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Debug It!: Find, Repair, and Prevent Bugs in Your Code” by Paul Butcher, https://www.goodreads.com/book/show/6770868-debug-it
- “Dependency Injection in .NET” by Mark Seemann, https://www.goodreads.com/book/show/9407722-dependency-injection-in-net
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Видео

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o
- «Антипаттерны общие для всех парадигм» Тимур Шемсединов, https://youtu.be/NMUsUiFokr4
- «Контрактное программирование» Тимур Шемсединов, https://youtu.be/K5_kSUvbGEQ
- «Почему ваша архитектура функциональная и как с этим жить» Роман Неволин, https://youtu.be/9s_4wpzENhg

### Статьи

- “Dependency Injection Using the Reader Monad” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/dependencies-3/
- “Dependency Interpretation” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/dependencies-4/
- “Dependency Rejection” by Mark Seemann, https://blog.ploeh.dk/2017/02/02/dependency-rejection/
- “Evans Classification” by Martin Fowler, https://martinfowler.com/bliki/EvansClassification.html
- “Inversion of Control Containers and the Dependency Injection pattern” by Martin Fowler, https://martinfowler.com/articles/injection.html
- “Six approaches to dependency injection” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/dependencies/
- Внедрение зависимостей с TypeScript на практике, https://bespoyasov.ru/blog/di-ts-in-practice/

### Связанные понятия

- Design By Contract, https://wiki.c2.com/?DesignByContract
- Message Broker, https://docs.microsoft.com/en-us/previous-versions/msp-n-p/ff648849(v=pandp.10)
- Message Bus, https://docs.microsoft.com/en-us/previous-versions/msp-n-p/ff647328(v=pandp.10)
- Message Queue, Wikipedia, https://en.wikipedia.org/wiki/Message_queue
- Зацепление в программировании, Википедия, https://ru.wikipedia.org/wiki/Зацепление_(программирование)
- Разделение ответственности, Википедия, https://ru.wikipedia.org/wiki/Разделение_ответственности
- Связность в программировании, Википедия, https://ru.wikipedia.org/wiki/Связность_(программирование)

### Инструменты

- Design Patterns, Refactoring Guru, https://refactoring.guru/design-patterns/
- React software design patterns, https://github.com/themithy/react-design-patterns
- Reactive Extensions Library for JavaScript, RxJS, https://rxjs.dev
- REpresentational State Transfer, REST, https://restfulapi.net
- What is Connascence? https://connascence.io

## Обобщения и иерархии

Как понять, когда нужен обобщённый алгоритм или тип. Почему в большей части случаев композиция предпочтительнее наследования. Как использовать принцип подстановки Лисков в качестве интеграционного линтера.

### Видео

- “The Grand Unified Theory of Clean Architecture and Test Pyramid” by Guilherme Ferreira, https://youtu.be/gHSpj2zM9Nw
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- «Контрактное программирование» Тимур Шемсединов, https://youtu.be/K5_kSUvbGEQ
- «Метапрограммирование и мультипарадигменное программирование» Тимур Шемсединов, https://youtu.be/Bo9y4IxdNRY

### Статьи

- “A behavioral notion of subtyping” by Barbara H. Liskov, Jeannette M. Wing, https://dl.acm.org/doi/10.1145/197320.197383
- “Coupling, Cohesion & Connascence” by Khalil Stemmler, https://khalilstemmler.com/wiki/coupling-cohesion-connascence/
- “The Ins and Outs of Generic Variance in Kotlin” by Dave Leeds, https://typealias.com/guides/ins-and-outs-of-generic-variance/
- “Maintain a Single Layer of Abstraction at a Time” by Khalil Stemmler, https://khalilstemmler.com/articles/oop-design-principles/maintain-a-single-layer-of-abstraction/
- “The Power of Composition” by Scott Wlaschin, https://fsharpforfunandprofit.com/composition/
- “Why variance matters” by Ted Kaminski https://www.tedinski.com/2018/06/26/variance.html

### Связанные понятия

- Covariance and Contravariance, Wikipedia, https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)
- Design By Contract, https://wiki.c2.com/?DesignByContract
- Generics in TypeScript, https://www.typescriptlang.org/docs/handbook/2/generics.html
- The Liskov Substitution Principle, https://web.archive.org/web/20151128004108/http://www.objectmentor.com/resources/articles/lsp.pdf
- Types and Typeclasses, http://learnyouahaskell.com/types-and-typeclasses

## Архитектура и ограничения

Чем может мешать слабая архитектура при рефакторинге. Как использовать повсеместный язык для улучшения архитектуры. Как строить взаимодействие с внешним миром и работу с зависимостями. В чём польза портов и адаптеров. Зачем разделять UI-логику от бизнес-логики. Как архитектура влияет на тестируемость.

### Книги

- “Clean Architecture” by Robert C. Martin, https://www.goodreads.com/book/show/18043011-clean-architecture
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Designing Data-Intensive Applications” by Martin Kleppmann https://dataintensive.net
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Enterprise Integration Patterns” by Gregor Hohpe, https://www.goodreads.com/book/show/85012.Enterprise_Integration_Patterns
- “Software Architecture in Practice” by L. Bass, P. Clements, R. Kazman, https://www.goodreads.com/book/show/70143.Software_Architecture_in_Practice
- “What I've Learned From Failure” by Reg Braithwaite, https://leanpub.com/shippingsoftware/read
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Видео

- “Design, Composition, and Performance” by Rich Hickey, https://youtu.be/MCZ3YgeEUPg
- “Is Domain-Driven Design Overrated?” by Stefan Tilkov, https://youtu.be/ZZp9RQEGeqQ
- “The Grand Unified Theory of Clean Architecture and Test Pyramid” by Guilherme Ferreira, https://youtu.be/gHSpj2zM9Nw
- “Functional architecture: The pits of success” by Mark Seemann, https://youtu.be/US8QG9I1XW0
- “Functional Design Patterns” by Scott Wlaschin, https://youtu.be/srQt1NAHYC0
- «Почему ваша архитектура функциональная и как с этим жить» Роман Неволин, https://youtu.be/9s_4wpzENhg
- «Быстрорастворимое проектирование» Максим Аршинов, https://youtu.be/qJPwSvDLmQE

### Статьи

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
- «System Design для амых маленьких», Виктор Карпов, https://github.com/vitkarpov/coding-interviews-blog-archive/blob/main/posts/what-is-system-design.md

### Связанные понятия

- List of System Quality Attributes, Wikipedia, https://en.wikipedia.org/wiki/List_of_system_quality_attributes
- Software Requirements, Wikipedia, https://en.wikipedia.org/wiki/Software_requirements
- Ubiquitous Language, https://martinfowler.com/bliki/UbiquitousLanguage.html
- Реактивное программирование, Википедия, https://ru.wikipedia.org/wiki/Реактивное_программирование
- Model-View-ViewModel, Википедия, https://ru.wikipedia.org/wiki/Model-View-ViewModel

### Инструменты

- Anti-corruption Layer Pattern, https://docs.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer
- Evey Programmer Should Know, Architecture, https://github.com/mtdvio/every-programmer-should-know#architecture
- Feature-Sliced Design, Architectural methodology for frontend projects, https://feature-sliced.design
- Ubiquitous Language, https://martinfowler.com/bliki/UbiquitousLanguage.html

## Декларативность

В чём польза декларативного стиля кода по сравнению с императивным. В каких ситуациях наоборот предпочесть императивный стиль. Зачем могут пригодиться конечные автоматы во фронтенд-разработке.

### Книги

- “Antifragile: Things That Gain from Disorder” by Nassim Nicholas Taleb, https://www.goodreads.com/book/show/13530973-antifragile
- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Видео

- “Design, Composition, and Performance” by Rich Hickey, https://youtu.be/MCZ3YgeEUPg
- “Designing Declarative APIs” by Ilya Birman, https://youtu.be/uCQ3JFuQ7bQ
- “Goodbye, useEffect” by David Khourshid, https://youtu.be/HPoC-k7Rxwo
- “Refactoring with Cognitive Complexity” by G. Ann Campbell, https://youtu.be/el9OKGrqU6o
- “Type Level Programming in TypeScript” by Gabriel Vergnaud, https://youtu.be/vGVvJuazs84
- «Метапрограммирование и мультипарадигменное программирование» Тимур Шемсединов, https://youtu.be/Bo9y4IxdNRY

### Статьи

- “Simplicity Comes from Reduction” by Paul W. Homer, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_75/
- “Maintain a Single Layer of Abstraction at a Time” by Khalil Stemmler, https://khalilstemmler.com/articles/oop-design-principles/maintain-a-single-layer-of-abstraction/
- “Prefer Domain-Specific Types to Primitive Types” by Einar Landre, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_65/
- «Декларативная валидация с помощью правило-ориентированного подхода и функционального программирования», https://bespoyasov.ru/blog/declarative-rule-based-validation/
- «Управление состоянием приложения с помощью конечного автомата», https://bespoyasov.ru/blog/fsm-to-the-rescue/

### Связанные понятия

- Transformation Priority Premise, Wikipedia https://en.wikipedia.org/wiki/Transformation_Priority_Premise
- Конечный Автомат, Википедия, https://ru.wikipedia.org/wiki/Конечный_автомат
- Сопоставление с образцом, Википедия, https://ru.wikipedia.org/wiki/Сопоставление_с_образцом

### Инструменты

- 12 Factor Apps, https://12factor.net
- JavaScript and TypeScript finite state machines and statecharts, XState, https://github.com/statelyai/xstate
- Zen of Python, https://peps.python.org/pep-0020/

## Статическая типизация

Как использовать типы для передачи дополнительных данных о предметной области. Как сделать невалидные состояния данных невыразимыми в коде. Как использовать типы для проверки кода на нарушение договорённостей и принципов разработки, принятых в команде.

### Книги

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Domain Modeling Made Functional” by Scott Wlaschin, https://www.goodreads.com/book/show/34921689-domain-modeling-made-functional
- “Domain-Driven Design” by Eric Evans, https://www.goodreads.com/book/show/179133.Domain_Driven_Design

### Видео

- “Type Level Programming in TypeScript” by Gabriel Vergnaud, https://youtu.be/vGVvJuazs84

### Статьи

- “Branding and Type-Tagging” by Kevin B. Greene, https://medium.com/@KevinBGreene/surviving-the-typescript-ecosystem-branding-and-type-tagging-6cf6e516523d
- “Bringing Pattern Matching to TypeScript” by Gabriel Vergnaud, https://dev.to/gvergnaud/bringing-pattern-matching-to-typescript-introducing-ts-pattern-v3-0-o1k
- “Code in the Language of the Domain” by Dan North, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_11/
- “Constructive vs predicative data” by Hillel Wayne, https://www.hillelwayne.com/post/constructive/
- “Designing with types: Making illegal states unrepresentable” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/designing-with-types-making-illegal-states-unrepresentable/
- “Making Illegal States Unrepresentable” by Hillel Wayne, https://buttondown.email/hillelwayne/archive/making-illegal-states-unrepresentable/
- “Prefer Domain-Specific Types to Primitive Types” by Einar Landre, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_65/

### Связанные понятия

- Ubiquitous Language, https://martinfowler.com/bliki/UbiquitousLanguage.html
- Type Aliases, TypeScript Handbook, https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-aliases

### Инструменты

- Tiny Types in TypeScript, https://janmolak.com/tiny-types-in-typescript-4680177f026e
- `switch-exhaustiveness-check`, ES Lint TypeScript, https://typescript-eslint.io/rules/switch-exhaustiveness-check/

## Рефакторинг тестового кода

Как не сломать тесты во время рефакторинга, чем проверять тесты после него. Что делает тесты «хрупкими». Как найти баланс между высоким покрытием и низким «уроном от тестов».

### Книги

- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code

### Видео

- “7 Ineffective Coding Habits of Many Programmers” by Kevlin Henney, https://youtu.be/ZsHMHukIlJY
- «Рефакторинг: причины, цели, техники и процесс» Тимур Шемсединов, https://youtu.be/z73wmpdweQ4

### Статьи

- “Choosing properties for property-based testing” by Scott Wlaschin, https://fsharpforfunandprofit.com/posts/property-based-testing-2/
- “Test-Induced Design Damage” by David Heinemeier Hansson, https://dhh.dk/2014/test-induced-design-damage.html
- “Unit testing best practices with .NET Core and .NET Standard”, MSDN, https://docs.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices
- «TDD: зачем и как», https://bespoyasov.ru/blog/tdd-what-how-and-why/

### Инструменты

- Faker, Generate fake (but realistic) data for testing and development, https://fakerjs.dev
- Test-Driven Development, TDD, https://martinfowler.com/bliki/TestDrivenDevelopment.html
- `git stash`, Stash the changes in a dirty working directory away, https://git-scm.com/docs/git-stash

## Комментарии, документация и процессы

Как синхронизировать источники правды в проекте и предотвратить их рассинхронизацию. Как и зачем делать комментарии информационно насыщеннее. Как увеличить пользу документации, не увеличив затраты на её ведение. Чему в проекте уделять внимание «кроме кода».

### Книги

- “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Envisioning Information” by Edward R. Tufte, https://www.goodreads.com/book/show/17745.Envisioning_Information
- “The Newspaper Designer's Handbook” by Tim Harrower, https://www.goodreads.com/book/show/61881153-the-newspaper-designer-s-handbook
- «Справочник издателя и автора» Аркадий Мильчин, https://www.goodreads.com/book/show/13611575

### Статьи и исследования

- “How to ask good questions” by Julia Evans, https://jvns.ca/blog/good-questions/
- “Information Density and Linguistic Encoding”, by Matthew W. Crocker, Vera Demberg, Elke Teich https://www.researchgate.net/publication/283938800_Information_Density_and_Linguistic_Encoding_IDeaL
- «Копипаста в коде», https://bespoyasov.ru/blog/copy-paste/

### Инструменты

- Architectural Decision Records, ADR, https://adr.github.io
- Evey Programmer Should Know, Practices, https://github.com/mtdvio/every-programmer-should-know#practices
- JSDoc Reference, TypeScript Documentation https://www.typescriptlang.org/docs/handbook/jsdoc-supported-types.html

## Частота, процессы и метрики

Как понять, рефакторить или переписывать код. Какую информацию о проекте собрать перед началом работы. Чем руководствоваться при расчёте времени на задачу. Какими метриками измерять влияние рефакторинга на код.

### Книги

- “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
- “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
- “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
- “Refactoring” by Martin Fowler, Kent Beck, https://www.goodreads.com/book/show/44936.Refactoring
- “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- “Your Code As a Crime Scene” by Adam Tornhill, https://www.goodreads.com/book/show/23627482-your-code-as-a-crime-scene

### Видео

- “All the Little Things” by Sandi Metz, https://youtu.be/8bZh5LMaSmE
- “Preventing the Collapse of Civilization” by Jonathan Blow, https://youtu.be/pW-SOdj4Kkk
- “Where Does Bad Code Come From?” https://youtu.be/7YpFGkG-u1w
- «Рефакторинг: причины, цели, техники и процесс» Тимур Шемсединов, https://youtu.be/z73wmpdweQ4
- «Антипаттерны общие для всех парадигм» Тимур Шемсединов, https://youtu.be/NMUsUiFokr4

### Статьи

- “The Boy Scout Rule” by Robert C. Martin, https://97-things-every-x-should-know.gitbooks.io/97-things-every-programmer-should-know/content/en/thing_08/
- “Strangler Fig Application” by Martin Fowler https://martinfowler.com/bliki/StranglerFigApplication.html
- “Opportunistic Refactoring” by Martin Fowler https://martinfowler.com/bliki/OpportunisticRefactoring.html
- “When to Refactor” by Mark Seemann, https://blog.ploeh.dk/2022/09/19/when-to-refactor/

### Инструменты

- Evey Programmer Should Know, Engineering Philosophy, https://github.com/mtdvio/every-programmer-should-know#engineering-philosophy
- Evey Programmer Should Know, Practices, https://github.com/mtdvio/every-programmer-should-know#practices
