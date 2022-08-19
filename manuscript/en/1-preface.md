# Preface

The idea for this book appeared after my talk “Refactoring Like a Superhero”[^talk], which I was making in January 2022.

At some point, it became obvious that I couldn't fit everything I wanted to discuss into a 40-minute slot. I had to cut and trim the material for the talk to show the most useful practices and technics of refactoring.

The cut-out material wasn't useless though. It had details and clarifications about usage and constraints of the technics, it told about when they were applicable and when they weren't. I decided it would make more sense not to throw out everything that didn't fit, but to change the format and release it as a small online book. That's how this project came to be.

## Who Should Read This Book

This book might be useful for developers of web services and user applications who write in high-level languages and have a couple of years of experience.

Library developers might also find some ideas in it but I'll mostly focus on user applications since I have more experience in that area.

This book doesn't consider the needs and constraints of low-level development. Some heuristics in it might contradict best practices of low-level code. If you write “closer to the hardware”, please, be careful, and proceed at your own risk 😃

## What This Book Isn't

In this book, I don't claim to show “the only correct way” to refactor and write code. If you have a lot of experience, you probably already know most of the techniques described and have your own opinion about them.

Also, this is not a “step-by-step manual” that is universally applicable to all projects. My coding habits and programming style are biased by my development experience. Your experience, projects, and habits may be different from mine, so our views may not be the same. That's okay.

The purpose of this book is to describe a set of practices, heuristics, and approaches that helped _me_ start writing code that _looks good_ to me and the people I worked with.

Not all practices have to be applied at all times. The decision whether to apply an idea depends greatly on the project, the context, resources, and the purpose of refactoring. Try to choose ideas that bring more benefit at less cost.

If something in the book seems useful to you, discuss it with your colleagues and other developers. Make sure you and your team have the same opinion about the benefits and the costs of the chosen idea. Don't apply something controversial with your team.

## Limits and Constraints

All of the techniques described here are a compilation of the books I've read and my experience in development. I have spent most of my time developing medium and sometimes large user applications.

My experience imprints on the way I see good code. Basically, the whole book is a snapshot of my perception of development in 2022. If you're reading this in the future, my opinion may have changed.

| By the way 🐝                                                                                                            |
| :----------------------------------------------------------------------------------------------------------------------- |
| I will update the text of the book as my opinion changes, but I can't guarantee I will do so promptly and without delay. |

As you read the book, be aware of the author's cognitive biases. Weigh the technics before using them and think about their applicability to your project.

## Good to Know Before Reading

In the text, I assume you're already familiar with the concept of refactoring[^term] and that you have a couple of years of programming experience. I expect that you've already encountered issues with problem decomposition, “leaking” abstractions, separation of concerns between modules, etc.

I expect you've heard of some of the “programming buzzwords” on this list:

- Separation of concerns
- Coupling and cohesion
- Declarative style
- Abstraction layers
- Command-query separation
- Referential transparency
- Functional pipeline
- Functional core in an imperative shell
- Layered architecture, “Ports and Adapters”
- Direction of dependencies
- Immutability and statelessness
- 12-factor applications

You don't have to know them all. We'll be revealing the meaning of the terms and techniques as the book progresses. But it's good if you have a rough idea about their basic meaning.

## Why Another Book

There are many books on refactoring, why do we need another one?

By and large, we don't. All the principles and techniques are described in other books, and most likely in a much more detailed, logical, and correct way.

I need this text, first of all, for myself:

- To systematize what I know today;
- To refer to a specific topic when discussing a problem;
- To mark the level of my knowledge, so that I understand where to improve and what to learn.

However, I thought that maybe this text could be useful to someone else. That's how this project came about.

[^talk]: “Refactor Like a Superhero” Talk, https://bespoyasov.me/talks/refactor-like-a-superhero/
[^term]: Code Refactoring, Wikipedia, https://en.wikipedia.org/wiki/Code_refactoring
