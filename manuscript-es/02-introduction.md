# Introducción

Refactorizar requiere recursos. La cantidad de estos recursos depende del tamaño del proyecto y de la calidad de su código. Cuanto más grande sea el proyecto y peor la calidad de su código, más difícil es limpiarlo y más recursos requerirá.  

Para justificar la inversión de recursos y buscar un balance entre costes y beneficios. necesitamos entender los beneficios y limitaciones de refactorizar.

## Beneficios para desarrolladores

La calidad del código es una inversión en los desarrolladores, más tiempo libre en el futuro. Cuanto más simple y limpio sea el código, menos tiempo invertiremos en arreglar bugs y desarrollar nuevas funcionalidades.

Los desarrolladores pueden preocuparse por diferentes propiedades del código. Por ejemplo, podríamos querer:

- Encontrar código relacionado con partes específicas de la aplicación más rápidamente.
- Eliminar malentendidos sobre como el código trabaja y evitar mala comunicación y conflictos en el equipo.
- Hacer más fáciles las revisiones de código y comprobar que se cumplen los requerimientos de negocio.
- Añadir, cambiar y eliminar código sin problemas y sin regresiones.
- Reduce the time to find and fix bugs and make debugging process more convenient.
- Reducir el tiempo de buscar y arreglar bugs y hacer el proceso de depuración más fácil.
- Simplificar la exploración del proyecto a los nuevos desarrolladores.

Esta lista está incompleta. Otras cualidades pueden ser necesarias para un equipo en particular, variando de proyecto a proyecto.

Refactorizar periodicamente ayuda a prestar atención a las propiedades del código antes de que los problemas aparezcan. hace que trabajemos de forma más eficiente, dando a los desarrolladores tiempo y recursos extras y previene "grandes refactorizaciones" en el futuro.

## Beneficios para la empresa

En un proceso de desarrollo perfectamente organizado, no es necesario "vender" la refactorización a la empresa. En este tipo de proyectos, la mejora periódica del código es el nucleo del desarrollo, y el mal código no se acumula: no es necesario "no es necesario explicar los beneficios a la empresa" en este caso.

Sin embargo, hay proyectos en los que el desarrollo se organiza de forma diferente por diversas razones. En estos proyectos, por regla general, el código heredado tiende a acumularse.

Podemos sentir la necesidad de mejorar el código, pero no podemos tener suficientes recursos para eso. Proponer "tomar una semana para refactorizar" puede causar un conflicto de intereses por que, para negocio, suena como "no haremos nada util durante una semana". Estos son los casos donde podemos necesitar "vender" las ideas de mejora de código.

Los beneficios de la refactorización no son evidentes para el negocio porque no son inmediatos. Podemos verlos en el futuro, pero es difícil de predecir cuando.

Normalmente, para vender la idea de refactorizar a negocio, deberíamos de hablar en su mismo idioma, y _vender el resultado, not el proceso_. Discutir exactamente sobre el resultado que obtendremos con el tiempo invertido:

- Dedicaremos menos tiempo a corregir errores, por lo que el número de usuarios descontentos disminuirá.
- Empezaremos a implementar nuevas funciones antes que nuestros competidores, por lo que generarán nuevos usuarios y beneficios.
- Entenderemos mejor los requisitos y las limitaciones, por lo que reaccionaremos más rápidamente ante problemas imprevistos.
- Facilitaremos la incorporación de nuevos desarrolladores para que realicen cambios significativos antes.
- Disminuiremos la rotación de personal porque los desarrolladores no huyen del buen código, sino del malo.

Podemos usar varios tipos de métricas para medir la calidad del código. Será mucho más fácil determinar la necesidad de la refactorización apoyándose en las cifras. Por ejemplo, las estadísticas de costes podrían ayudar a incorporar la refactorización regular en el proceso de desarrollo sin problemas. 

## “Buen” Código, “Mal” Código

No es fácil de nombrar una lista de características _universales_ del buen código. Hay algunas, pero también tienen limites de aplicabilidad.

| Por ejemplo 💡                                                                                                                                                |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Creo que la complejidad ciclomática y el número de dependencias son más o menos características universales. Pero hablaremos sobre ello en futuros capítulos. |

Muchos de los libros que he leído también describen el código de forma subjetiva.[^workingeffectively][^readablecode][^cleancode] Diferentes autores usan diferentes palabras, pero siempre enfatizan la "legibilidad".

Algunos estudios han intentado determinar esta "legibilidad".[^evaluatingstudies][^readability][^howreadable] Sin embargo, sus ejemplos son pequeños o sesgados, por lo que es difícil concluir las reglas universales del "buen" código.
 
En la práctica, podemos intentar buscar código "malo" en vez de "bueno". Es más fácil porque podemos utilizar la ayuda de la heurística y las "alarmas cognitivas" al buscarlo.

Las alarmas cognitivas son las sensaciones que tenemos al leer un código malo. Creo que un código necesita ser refactorizado si uno de estos pensamientos surge al leerlo:

#### Difícil de leer

- Nos resulta difícil leer el código si está sin formato, entrelazado o con mucho ruido.
- Si hay muchos detalles innecesarios en el código, no hay un punto de entrada claro.
- Si es difícil seguir la ejecución del código, si tenemos que saltar entre pantallas, archivos o líneas constantemente.
- Si el código es inconsistente, si no sigue las reglas del proyecto.

#### Difícil de cambiar

- El código es difícil de cambiar si tenemos que actualizar muchos archivos o volver a comprobar toda la aplicación al añadir una función.
- Si no estamos seguros, podemos eliminar sin problemas un trozo de código concreto.
- Si no hay un punto de entrada claro o no podemos relacionar una característica con un módulo específico.
- Si hay demasiado código repetitivo o copy-paste.

#### Difícil de testear

- El código es difícil de probar si necesitamos una "infraestructura compleja" para las pruebas o tenemos que simular muchas funciones.
- Si debemos emular toda la aplicación en ejecución para comprobar una sola función.
- Si las pruebas de una tarea requieren datos irrelevantes para la misma.

#### Difícil de “Meter en la cabeza”

- El código no entra en nuestras cabezas si es difícil seguir la pista de lo que ocurre en él.
- Si a la mitad del módulo es difícil recordar lo que ocurrió al principio.
- Si el código es "demasiado complicado" y dibujar diagramas no ayuda a entenderlo.

#### Code Smells

Algunos de esos problemas ya se han plasmado en forma de code smells. _Code smells_ son anti-patrones que generan problemas.[^smells]

Hay soluciones para la mayoría de code smells. A veces es suficiente con mirar el código, encontrar el code smell, y aplicar una solución específica.

| Sobre smells 🦨                                                                                                                                                                                                                                              |
|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A menudo, la mayoría de ejemplos de code smells son mostrados en código escrito en POO. que pueden no ser tan valiosos en el mundo de JavaScript. No obstante, algunos de los code smells son universales y aplicables a la POO y al código multi-paradigma. |

[^workingeffectively]: “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
[^readablecode]: “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
[^cleancode]: “Clean Code” by Robert C. Martin, https://www.goodreads.com/book/show/3735293-clean-code
[^evaluatingstudies]: Evaluating Code Readability and Legibility: An Examination of Human-centric Studies, https://github.com/reydne/code-comprehension-review/blob/master/list-papers/AllPhasesMergedPapers-Part1.md
[^readability]: Code Readability Testing, an Empirical Study, https://www.researchgate.net/publication/299412540_Code_Readability_Testing_an_Empirical_Study
[^howreadable]: How Readable Code Is, a Readability Experiment https://howreadable.com
[^smells]: Code Smells, Refactoring Guru, https://refactoring.guru/refactoring/smells
