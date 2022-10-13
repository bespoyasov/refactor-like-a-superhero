# Introdução

A refatoração requer recursos. A quantidade desses recursos depende do tamanho do projeto e da qualidade do código. Quanto maior o projeto e pior o código, mais difícil é limpá-lo e mais recursos ele pode exigir.

Para justificar o investimento de recursos e encontrar um equilíbrio entre custos e benefícios, precisamos entender os benefícios e limitações da refatoração.

## Benefícios para Desenvolvedores

A qualidade do código é um investimento no tempo livre dos desenvolvedores no futuro. Quanto mais simples e limpo for o código, menos tempo gastaremos corrigindo bugs e desenvolvendo novos recursos.

Os desenvolvedores podem se preocupar com diferentes propriedades do código. Por exemplo, podemos querer:

- Encontrar códigos relacionados a partes específicas do aplicativo mais rapidamente.
- Eliminar mal-entendidos sobre como o código funciona e evitar falhas de comunicação e conflitos na equipe.
- Facilitar a revisão do código e verificá-lo em relação aos requisitos de negócios.
- Adicione, altere e exclua código de forma indolor sem regressões.
- Reduza o tempo para encontrar e corrigir bugs e tornar o processo de depuração (_debugging_) mais conveniente.
- Simplifique a exploração de projetos para novos desenvolvedores.

Esta lista está incompleta. Outras propriedades podem ser necessárias para uma determinada equipe, variando de projeto para projeto.

A refatoração regular ajuda a prestar atenção às propriedades do código antes que os problemas apareçam. Isso torna o trabalho diário mais eficiente, dá aos desenvolvedores tempo e recursos extras e evita “grandes refatorações” no futuro.

## Benefícios para a Empresa

Em um processo de desenvolvimento perfeitamente organizado, não há necessidade de “vender” refatoração para a empresa. Nesses projetos, a melhoria regular do código está no centro do desenvolvimento, e o código ruim não se acumula – não há necessidade de “explicar os benefícios para a empresa” neste caso.

No entanto, existem projetos em que o desenvolvimento é organizado de forma diferente por vários motivos. Em tais projetos, por via de regra, o código legado tende a se acumular.

Podemos sentir a necessidade de melhorar o código, mas podemos não ter recursos suficientes para isso. Uma proposta de “levar uma semana para refatorar” pode causar um conflito de interesses porque, para as empresas, soa como “não faremos nada de útil por uma semana”. Esses são os casos em que podemos precisar “vender” as ideias de melhoria do código.

Os benefícios da refatoração não são evidentes para as empresas porque não são imediatos. Podemos vê-los no futuro, mas é difícil prever quando.

Normalmente, para vender a ideia de refatoração para as empresas, devemos falar a linguagem dos negócios e _vender o resultado, não o processo_. Discuta o que exatamente obteremos como resultado do tempo gasto:

- Gastaremos menos tempo corrigindo bugs, então o número de usuários insatisfeitos cairá.
- Começaremos a implementar novos recursos antes dos nossos concorrentes, para que isso acrescente novos usuários e lucro.
- Entenderemos melhor os requisitos e restrições, para reagirmos a problemas imprevistos mais rapidamente.
- Facilitaremos a integração para que novos desenvolvedores façam alterações significativas mais cedo.
- Diminuiremos a rotatividade de pessoal porque os desenvolvedores não fogem do código bom, apenas do código ruim.

Podemos usar várias métricas para medir a qualidade do código. Será muito mais fácil determinar a necessidade de refatoração confiando nos números. Por exemplo, as estatísticas de custos podem ajudar a incorporar a refatoração regular no processo de desenvolvimento sem problemas.

## Código “Bom", Código “Ruim"

Não é fácil nomear uma lista de características _universais_ de um bom código. Existem alguns, mas eles também têm limites de aplicabilidade.

| Por exemplo 💡                                                                                                                                                               |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Eu penso na complexidade ciclomática e no número de dependências como características mais ou menos universais. Mas falaremos sobre eles separadamente em capítulos futuros. |

A maioria dos livros que li também descreve um bom código subjetivamente.[^workingeffectively][^readablecode][^cleancode] Diferentes autores usam palavras diferentes, mas sempre enfatizam a “legibilidade”.

Alguns estudos tentaram determinar essa “legibilidade”.[^evaluatingstudies][^readability][^howreadable] No entanto, suas amostras são pequenas ou distorcidas, então é difícil concluir as regras universais do código “bom”.

Na prática, podemos tentar procurar um código “ruim” em vez de um “bom”. É mais fácil porque podemos usar a ajuda de heurísticas e “alarmes cognitivos” ao procurá-lo.

Alarmes cognitivos são os sentimentos que temos ao ler um código ruim. Eu acredito que um código precisa de refatoração se um desses pensamentos surgir durante a leitura de um código:

#### Difícil de Ler

- É difícil para nós ler um código não formatado, entrelaçado ou "barulhento".
- Se houver muitos detalhes desnecessários no código, não há um ponto de entrada claro.
- Se for difícil acompanhar a execução do código, se precisarmos pular entre telas, arquivos ou linhas constantemente.
- Se o código for inconsistente, se não seguir as regras do projeto.

#### Difícil para Alterar

- O código é difícil de alterar se precisarmos atualizar muitos arquivos ou verificar novamente toda a aplicação ao adicionar um recurso.
- Se não tivermos certeza, se podemos remover uma parte específica do código sem problemas.
- Se não houver um ponto de entrada claro ou não pudermos relacionar um recurso com um módulo específico.
- Se houver muito código clichê ou copia e cola (_copypaste_).

#### Difícil para Testar

- O código é difícil de testar se precisarmos de uma “infraestrutura complexa” para testes ou precisarmos simular muitas funcionalidades.
- Se devemos emular todo o aplicativo em execução para verificar uma única função.
- Se os testes para uma tarefa exigirem dados irrelevantes para a tarefa.

#### Difícil para "Entrar na cabeça"

- O código não cabe em nossas cabeças se for difícil acompanhar o que está acontecendo nele.
- Se no meio do módulo, é difícil lembrar o que aconteceu no início.
- Se o código for “muito complicado” e desenhar diagramas não ajuda a entendê-lo.

#### Cheiros de código (_Code Smells_)

Alguns desses problemas já foram moldados na forma de cheiros de código. _Cheiros de código_ são antipadrões que levam a problemas.[^smells]

Existem soluções para a maioria dos cheiros de código. Às vezes é suficiente olhar para o código, encontrar o cheiro e aplicar uma solução específica a ele.

| Sobre cheiros 🦨                                                                                                                                                                                                                                                                      |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Na maioria das vezes, exemplos de cheiros de código são fornecidos em código escrito no estilo Programação Orientada a Objetos (POO), o que pode não ser tão valioso no mundo JavaScript. No entanto, alguns dos cheiros são universais e aplicáveis ao código POO e multi-paradigma. |

[^workingeffectively]: “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
[^readablecode]: “The Art of Readable Code” by Dustin Boswell, Trevor Foucher, https://www.goodreads.com/book/show/8677004-the-art-of-readable-code
[^cleancode]: “Clean Code” by Robert C. Martin, https://www.goodreads.com/book/show/3735293-clean-code
[^evaluatingstudies]: Evaluating Code Readability and Legibility: An Examination of Human-centric Studies, https://github.com/reydne/code-comprehension-review/blob/master/list-papers/AllPhasesMergedPapers-Part1.md
[^readability]: Code Readability Testing, an Empirical Study, https://www.researchgate.net/publication/299412540_Code_Readability_Testing_an_Empirical_Study
[^howreadable]: How Readable Code Is, a Readability Experiment https://howreadable.com
[^smells]: Code Smells, Refactoring Guru, https://refactoring.guru/refactoring/smells
