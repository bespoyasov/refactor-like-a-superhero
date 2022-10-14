# Prefacio

La idea para este libro apareció después de mi charla “Refactoring Like a Superhero,”[^talk] que realicé en Enero del 2022. Para esta charla había recopilado técnicas y metodologías de refactorización. Quería compartirlas.

En algún punto, se hizo evidente que no podía meter todo lo que quería explicar en 40 minutos.

Sin embargo, el material recortado no era inútil. Contenía detalles y explicaciones sobre el uso y aplicación de las técnicas. Decidí que, tendría más sentido no eliminar todo lo que no cabía sino cambiar el formato y publicarlo como un libro online. Así es como empezó este proyecto.

## Quién podría querer leer este libro

Este libro puede ser de gran ayuda para desarrolladores de servicios web y aplicaciones de usuarios, que escriben código en un lenguaje de alto nivel y tienen algunos años de experiencia.

Los desarrolladores de librerías también pueden encontrar ideas, pero me he centrado en las aplicaiones de usuario, ya que tengo más experiencia en esa area.

Este libro no considera las necesidades y limitaciones del desarrollo de bajo nivel. Algunas metodologías en él pueden contradecir las buenas prácticas del código de bajo nivel. Si tu desarrollas "cerca del hardware", por favor, se cuidadoso y hazlo bajo tu propio riesgo. 😃

## Que no es este libro

No pretendo mostrarte el "único camino correcto" de refactorizar y escribir código en este libro. Si tienes mucha experiencia, probablemente ya sepas la mayoría de técnicas descritas y tienes tu propia opinión.

Además, no se trata de un manual paso a paso de aplicación universal a todos los proyectos.

El propósito de este libro es el de describir un conjunto de prácticas y metodologías que me ayudaron, a mí y a las personas con las que he trabajado, a escribir un código mejor.

No todas las prácticas son aplicables en todos los casos. El uso de una idea depende en gran medida del proyecto, el contexto, los recursos disponibles, y el propósito de la refactorización. Trata de elegir aquellas técnicas que te proporcionan un mayor beneficio y un menor costo.

Si algo del libro te parece útil, discútelo con tus colegas y otros desarrolladores. Asegúrate de que tú y tu equipo tenéis la misma opinión sobre los beneficios y los costes de la idea elegida. No apliques algo que resulte controvertido a tu equipo.

## Limitaciones y aplicación

Todas las técnicas descritas son una recopilación de libros que he leído y de mi experiencia en desarrollo. He pasado la mayoría de mi tiempo desarrollando medianas, y a veces grandes, aplicaciones de usuario.

Mi experiencia influye en mi forma de ver el buen código. Básicamente, todo el libro es una muestra de mi percepción del desarrollo en 2022. Mi opinión puede haber cambiado si estás leyendo esto en el futuro.

| Por cierto 🐝                                                                                             |
|:----------------------------------------------------------------------------------------------------------|
| Actualizaré los textos si mi opinión cambia, pero no puedo garantizar de que lo haga rápido y sin demora. |

Al leer el libro, ten en cuenta los sesgos cognitivos del autor. Sopesa las técnicas antes de utilizarlas y considera su aplicabilidad a tu proyecto.

## Good to Know Before Reading

In the text, I assume you're already familiar with the concept of refactoring[^term] and that you have a couple of years of programming experience. I expect you've already encountered issues with task decomposition, “leaky” abstractions, separation of concerns between modules, etc.

I expect you've heard about some of the “programming buzzwords” on this list:

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

You don't have to _know_ them. We'll be exploring the terms and techniques as the book progresses. But it's good if you have a rough idea about their basic meaning.

## Why Another Book

There are many books on refactoring, why do we need another one?

By and large, we don't. All the principles and techniques are described in other books, most likely in a much more detailed, logical, and correct way.

I need this text, first of all, for myself:

- To systematize what I know today;
- To refer to a specific topic when discussing a problem;
- To mark the level of my knowledge so that I understand where to improve and what to learn.

However, I thought that maybe this text could be helpful to someone else, so here we are. I hope you'll find this book helpful!

[^talk]: “Refactor Like a Superhero” Talk, https://bespoyasov.me/talks/refactor-like-a-superhero/
[^term]: Code Refactoring, Wikipedia, https://en.wikipedia.org/wiki/Code_refactoring
