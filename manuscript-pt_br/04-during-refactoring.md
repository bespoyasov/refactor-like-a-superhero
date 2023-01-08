# Durante a Refatoração

Depois de definirmos limites claros, examinarmos o código e cobri-lo com testes, podemos começar a refatorar.

À medida que trabalhamos, queremos que as mudanças sejam tão úteis quanto possível, mantendo-nos dentro dos nossos limites. Neste capítulo, discutiremos heurísticas que nos ajudam a fazer isso.

## Pequenos Passos

Um pequeno passo é uma mudança mínima que faz sentido. Um excelente exemplo de um pequeno passo é um _commit atômico_.[^atomic] Esse commit contém uma alteração de funcionalidade significativa e evolui o código de um estado de trabalho para outro. Podemos desenvolver e alterar a base de código com commits atômicos, mantendo-a válida em todos os pontos.

| A propósito 💡                                                                                                                                                                                                                                                             |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Os commits atômicos nos permitem dividir o repositório no futuro enquanto procuramos por bugs.[^bisect] Quando o código é válido e pode ser compilado em cada um dos commits, nós podemos “viajar no tempo” pelo repositório alternando entre diferentes commits atômicos. |

O tamanho dos commits depende dos hábitos e preferências da equipe. Pessoalmente, minha regra geral é “quanto menor o commit, melhor”. Por exemplo, eu faço commit separadamente até mesmo para renomeações de funções e variáveis em meus projetos.

Passos menores nos encorajam a decompor as tarefas em outras mais simples. Dessa forma, recursos extensos se transformam em conjuntos de tarefas compactas, que não são tão assustadoras para fazer _merge_ com a _main branch_ do repositório com mais frequência. Sem decomposição, tal tarefa pode se transformar em um bloqueador que arrasta toda a equipe para baixo.

| Sobre CI 🔬                                                                                                                                                                                                                                                                                                   |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| O ponto da _integração contínua, CI_ é que a equipe sincronize as alterações de código entre si com a maior frequência possível. Essa abordagem e seus benefícios estão bem descritos em “_Code That Fits in Your Head_” de Mark Seemann e “_Beyond Legacy Code_” de Scott Bernstein.[^codethatfits][^beyond] |

As tarefas bloqueantes devem ser evitadas em geral, mas especialmente ao refatorar o código. Se toda a equipe estiver ocupada com a refatoração, isso pode se tornar caro. O precedente de ser caro pode privar o projeto de recursos para refatoração no futuro.

Exceto por isso, pequenos passos permitem que você “adie” a refatoração a qualquer momento e mude para outra tarefa. Se estivermos trabalhando com git, podemos usar “_stash_” para salvar trabalhos inacabados via `git stash`.

E por último, pequenas mudanças são mais fáceis de descrever em mensagens de commit. O escopo de tais mudanças é menor, então é mais fácil explicar seu significado em uma frase curta.

## Pequenos mas Detalhados Pull Requests

Esta seção segue a anterior, mas quero focar nela separadamente. O problema com grandes _pull requests_ é que:

---

**❗️ Ninguém revisa grandes pull requests**

---

Se quisermos _melhorar_ o código, precisamos que o _pull request_ seja revisado. Então nosso trabalho é facilitar o trabalho do revisor. Para fazer isso, podemos:

- Limitar o tamanho das alterações. Assim, é mais fácil encontrar tempo para uma revisão e entender os objetivos e o significado do PR. Dessa forma, a revisão de código não parecerá um grande trabalho novo.
- Descreva o contexto da tarefa na mensagem do PR. As razões, objetivos e restrições da tarefa ajudarão a compartilhar o entendimento da tarefa com o revisor. Dessa forma, podemos antecipar perguntas prováveis e respondê-las com antecedência. Isso vai acelerar a revisão.

O desejo de pequeno mas detalhado PR, também ajuda a dividir grandes tarefas em pequenas e refatorá-las em pequenos passos.

## Testes para Cada Mudança

Para que o código evolua através de estados válidos, testaremos _cada_ mudança, não importa quão pequena.

Ao usar testes unitários, podemos mantê-los rodando ao lado do editor. Ao usar testes de execução mais longa (como E2E), podemos executá-los antes de cada commit (por exemplo, em um _pre-commit hook_) para que o código inválido não entre no repositório.

Os testes devem nos levar a fazer _commit apenas de código válido_. Cada _commit_ conterá um conjunto de mudanças _completas e significativas_.

| A propósito 🧪                                                                                                                                                                                                |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Isso só funcionará se os testes forem confiáveis. É por isso que enfatizamos casos extremos e especificações detalhadas do resultado no capítulo mais recente. Isso ajuda a tornar os testes mais confiáveis. |

## Uma Técnica de Cada Vez

Commits frequentes são pontos de ancoragem na evolução do código. Quanto mais frequentes forem esses pontos, mais compactas serão as mudanças entre eles. É útil, por exemplo, ao examinar as alterações do último commit que acabamos de fazer. Pequenas mudanças são mais rápidas de estudar, mais fáceis de entender e contêm menos trabalho, então é emocionalmente mais fácil revertê-las caso precisemos.

Durante a refatoração, porém, nem sempre é óbvio como fazer alterações menores e com mais frequência. Eu prefiro seguir esta regra:

---

**❗️ Não misture diferentes técnicas de refatoração no mesmo commit**

---

Essa regra nos faz executarmos _commit_ do código com mais frequência: renomeamos uma função – fizemos um commit, extraímos uma variável – fizemos um commit, adicionamos código para uma substituição futura – fizemos um commit e assim por diante.

Contanto que não misturemos técnicas diferentes em um commit, é mais fácil rastrear alterações de código por _diffs_ e encontrar erros como conflitos de nome.

Técnicas complexas que são grandes demais para um único commit podem ser divididas em etapas. Podemos fazer _commit_ de cada uma dessas etapas separadamente. Mas ao dividir a tarefa, devemos lembrar que cada etapa deve deixar o código em um estado válido.

## Sem Funcionalidades, Sem Correções de Bugs

Durante a refatoração, podemos encontrar uma ideia para uma funcionalidade ou um pedaço de código que não funciona corretamente. Podemos querer “corrigir ao longo do caminho”, mas é melhor não misturar correções de bugs e novas funcionalidades com refatoração.

A refatoração _não deve_ alterar a funcionalidade do código. Se, durante a refatoração, adicionarmos uma funcionalidade e, posteriormente, ela precisar ser revertida, teremos que selecionar manualmente commits específicos ou até mesmo linhas de código no repositório.

Em vez disso, é melhor colocar todas as ideias de funcionalidades encontrados em uma lista separada e retornar a elas após a refatoração. Se encontrarmos um bug, devemos adiar a refatoração (`git stash`) e retornar a ela após a correção. Novamente, trabalhar em pequenos passos é mais conveniente para essa manobrabilidade.

## _Transformation Priority Premise_

_Transformation Priority Premise, TPP_ é uma lista de ações que ajudam a desenvolver código desde a implementação ingênua e direta até uma mais complexa que atenda a todos os requisitos do projeto.[^tpp]

O TPP ajuda a _evitar fazer tudo de uma vez_. Ele nos encoraja a atualizar gradualmente um pedaço de código, registrando cada etapa no sistema de controle de versão e integrando as alterações na _main branch_ do repositório com mais frequência.

Para mim, o TPP me ajuda a não sobrecarregar minha cabeça com detalhes enquanto escrevo código. Todas as ideias de melhoria que surgem ao longo do caminho, coloco em uma lista separada. Eu então comparo esta lista com o TPP e escolho o que implementar em seguida.

| Porque se importar 🧠                                                                                                                                                                        |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Os detalhes “descarregados” liberam a “memória de trabalho” do cérebro.[^shorttermmemory] Os recursos liberados podem ser gastos nos detalhes da tarefa. Eles melhorarão a atenção e o foco. |

## Refatorando Testes Separadamente

Os testes e o código do aplicativo cobrem um ao outro. Os testes verificam se não cometemos erros no código do aplicativo e vice-versa. Se os refatorarmos simultaneamente, a probabilidade de deixar passar um bug se torna maior.

É melhor refatorar o código do aplicativo e testar o código um de cada vez. Se, ao refatorar um aplicativo, percebermos que precisamos refatorar um teste, devemos:

- Fazer _stash_ das alterações de código do aplicativo;
- Refatorar o teste desejado;
- Verificar se o teste quebra por uma razão que deveria;
- Obter as últimas alterações do _stash_ e continuar trabalhando nelas.

| Em detalhes 🔬                                                                        |
| :------------------------------------------------------------------------------------ |
| Falaremos um pouco mais sobre essa técnica no capítulo “Refatorando Código de Teste”. |

[^atomic]: Atomic Commit, Wikipedia https://en.wikipedia.org/wiki/Atomic_commit
[^bisect]: git-bisect, Use binary search to find the commit that introduced a bug, https://git-scm.com/docs/git-bisect
[^codethatfits]: “Code That Fits in Your Head” by Mark Seemann, https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head
[^beyond]: “Beyond Legacy Code” by David Scott Bernstein, https://www.goodreads.com/book/show/26088456-beyond-legacy-code
[^tpp]: Transformation Priority Premise, Wikipedia https://en.wikipedia.org/wiki/Transformation_Priority_Premise
[^shorttermmemory]: Working memory, Capacity, Wikipedia, https://en.wikipedia.org/wiki/Working_memory#Capacity
