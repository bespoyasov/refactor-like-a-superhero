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

## Es bueno saberlo antes de leerlo

En el texto, asumo que ya conoces el concepto de refactorización[^term] y que tienes algunos años de experiencia programando. Espero que ya hayas encontrado problemas con la descomposición de tareas, "débiles" abstracciones, separación de conceptos entre módulos, etc.

Espero que hayas escuchado algunas de "las palabras de moda en la programación" de esta lista:

- Separación de conceptos
- Acoplamiento y cohesión
- Estilo declarativo
- Capas de abstracción
- Separación entre comandos y consultas
- Transparencia referencial
- Canalización funcional
- Núcleo funcional en un caparazón imperativo
- Arquitectura en capas, "Puertos y adaptadores"
- Dirección de las dependencias
- Inmutabilidad y ausencia de estado
- Aplicaciones de 12 factores

No tienes por qué conocerlos. Exploraremos estos términos y técnicas a medida que vayamos progresando. Pero, es bueno si tienes una ligera idea sobre su significado.

## Por qué otro libro

Hay muchos libros escritos sobre refactorización, ¿Por qué necesitamos otro?

En general, no lo hacemos. Todos los principios y técnicas se describen en otros libros, muy probablemente de forma mucho más detallada, lógica y correcta.

Necesito este texto, en primer lugar, para mí:

- Para sistematizar lo que hoy sé;
- Para referirme a un tema concreto cuando discuta un problema;
- Para marcar el nivel de mis conocimientos, de modo que entienda dónde debo mejorar y qué debo aprender.

Sin embargo, pensé que tal vez este texto podría ser útil para alguien más, así que aquí está. Espero que este libro le resulte útil.

[^talk]: “Refactor Like a Superhero” Charla, https://bespoyasov.me/talks/refactor-like-a-superhero/
[^term]: Refactorización del código, Wikipedia, https://en.wikipedia.org/wiki/Code_refactoring
