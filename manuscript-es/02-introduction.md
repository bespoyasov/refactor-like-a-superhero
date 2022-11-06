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

It isn't easy to name a list of _universal_ characteristics of a good code. There are a few, but they have limits in applicability, too.

| For example 💡                                                                                                                                                      |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| I think of cyclomatic complexity and the number of dependencies as more or less universal characteristics. But we'll talk about them separately in future chapters. |

Most of the books I've read also describe good code subjectively.[^workingeffectively][^readablecode][^cleancode] Different authors use different words, but they always emphasize “readability.”

Some studies have tried to determine this “readability.”[^evaluatingstudies][^readability][^howreadable] However, their samples are either small or skewed, so it's difficult to conclude the universal rules of the “good” code.

In practice, we can try to look for a “bad” code rather than a “good” one. It's easier because we can use the help of heuristics and “cognitive alarms” when searching for it.

Cognitive alarms are the feelings we get when reading bad code. I believe that a code needs refactoring if one of these thoughts arises while reading it:

#### Hard to Read

- It's hard for us to read code if it's unformatted, intertwined, or noisy.
- If there are a lot of unnecessary details in the code, there is no clear entry point.
- If it's hard to follow the code execution, if we need to jump between screens, files, or lines constantly.
- If the code is inconsistent, if it doesn't follow the project rules.

#### Hard to Change

- Code is hard to change if we need to update many files or double-check the entire application when adding a feature.
- If we aren't sure, we can painlessly remove a particular piece of code.
- If there's no clear entry point or we can't relate a feature with a specific module.
- If there's too much boilerplate code or copypaste.

#### Hard to Test

- Code is hard to test if we need a “complex infrastructure” for tests or need to mock a lot of functionality.
- If we must emulate the whole app running to check a single function.
- If tests for a task require data irrelevant to the task.

#### Hard to “Fit in the Head”

- Code doesn't fit in our heads if it's hard to keep track of what's going on in it.
- If by the middle of the module, it's hard to remember what happened at the beginning.
- If the code is “too complicated” and drawing diagrams doesn't help to understand it.

#### Code Smells

Some of those problems have already been shaped in the form of code smells. _Code smells_ are antipatterns that lead to problems.[^smells]

There are solutions for most of the code smells. Sometimes it's enough to look at the code, find the smell, and apply a specific solution to it.

| About smells 🦨                                                                                                                                                                                                                 |
|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Most often, examples of code smells are given in code written in OOP style, which may not be as valuable in the JavaScript world. Nevertheless, some of the smells are universal and applicable to OOP and multi-paradigm code. |

[^workingeffectively]: “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
[^readablecode]: “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
[^cleancode]: “Clean Code” by Robert C. Martin, https://www.goodreads.com/book/show/3735293-clean-code
[^evaluatingstudies]: Evaluating Code Readability and Legibility: An Examination of Human-centric Studies, https://github.com/reydne/code-comprehension-review/blob/master/list-papers/AllPhasesMergedPapers-Part1.md
[^readability]: Code Readability Testing, an Empirical Study, https://www.researchgate.net/publication/299412540_Code_Readability_Testing_an_Empirical_Study
[^howreadable]: How Readable Code Is, a Readability Experiment https://howreadable.com
[^smells]: Code Smells, Refactoring Guru, https://refactoring.guru/refactoring/smells
