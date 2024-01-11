# Prefácio

A ideia para este livro surgiu após minha palestra “Refactoring Like a Superhero”,[^talk] que eu estava fazendo em janeiro de 2022. Para essa palestra, reuni várias técnicas e heurísticas de refatoração que queria compartilhar.

Em algum momento, ficou claro que eu não conseguia encaixar tudo o que queria discutir em um intervalo de 40 minutos. Eu tive que reduzir o material para a palestra para mostrar as práticas e técnicas mais valiosas.

O material reduziado não era inútil, no entanto. Continha detalhes e esclarecimentos sobre o uso e aplicabilidade da técnica. Decidi que faria mais sentido não jogar fora tudo o que não coubesse, mas mudar o formato e lançá-lo como livro online. Foi assim que surgiu este projeto.

## Quem pode querer ler este livro

Este livro pode ser útil para desenvolvedores de serviços para web (_web-services_) e aplicativos de usuário (_user applications_) que escrevem em linguagens de alto nível e têm alguns anos de experiência.

Os desenvolvedores de bibliotecas também podem encontrar algumas ideias, mas vou me concentrar principalmente em aplicativos de usuário, pois tenho mais experiência nessa área.

Este livro não considera as necessidades e restrições do desenvolvimento de baixo nível. Algumas heurísticas podem contradizer as melhores práticas de código de baixo nível. Se você escrever códigos “mais perto do hardware”, tenha cuidado e prossiga por sua conta e risco 😃

## O que este livro não é

Eu não pretendo mostrar “a única maneira correta” de refatorar e escrever código neste livro. Se você tem muita experiência, provavelmente já conhece a maioria das técnicas descritas e tem sua própria opinião.

Além disso, este não é um “manual passo-a-passo” universalmente aplicável a todos os projetos. Minha experiência de desenvolvimento influencia meus hábitos de codificação e estilo de programação. Sua experiência, projetos e hábitos podem ser diferentes dos meus, então nossas opiniões podem não ser as mesmas, e está tudo bem.

O objetivo deste livro é descrever um conjunto de práticas e heurísticas que _me_ ajudaram a começar a escrever código que _parece bom_ para mim e para as pessoas com quem trabalhei.

Nem todas as práticas devem ser aplicadas em todos os momentos. O uso de uma ideia depende significativamente do projeto, do contexto, dos recursos disponíveis e do objetivo da refatoração. Procure escolher técnicas que tragam mais benefícios com menor custo.

Se algo no livro parecer útil para você, discuta-o com seus colegas e outros desenvolvedores. Certifique-se de que você e sua equipe tenham a mesma opinião sobre os benefícios e custos da ideia escolhida. Não aplique algo controverso à sua equipe.

## Limitações e Aplicabilidade

Todas as técnicas descritas aqui são uma compilação dos livros que li e minha experiência em desenvolvimento. Eu passei a maior parte do meu tempo desenvolvendo aplicativos de usuários de tamanhos médios e, às vezes, grandes.

Minha experiência marca a maneira como vejo um bom código. Basicamente, o livro inteiro é uma visão da minha percepção de desenvolvimento em 2022. Minha opinião pode ter mudado se você estiver lendo isso no futuro.

| A propósito 🐝                                                                                                               |
| :--------------------------------------------------------------------------------------------------------------------------- |
| Atualizarei o texto do livro à medida que minha opinião mudar, mas não posso garantir que o farei prontamente e sem atrasos. |

Ao ler o livro, esteja ciente dos vieses cognitivos do autor. Pese as técnicas antes de usá-las e considere sua aplicabilidade ao seu projeto.

## Bom saber antes de ler

No texto, eu presumo que você já esteja familiarizado com o conceito de refatoração[^term] e que tenha alguns anos de experiência em programação. Espero que você já tenha encontrado problemas com decomposição de tarefas, abstrações “vazadas”, separação de interesses entre módulos, etc.

Espero que você já tenha ouvido falar sobre algumas das “palavras da moda de programação” nesta lista:

- Separação de interesses (_Separation of concerns_)
- Acoplamento e coesão (_Coupling and cohesion_)
- Estilo declarativo (_Declarative style_)
- Camadas de abstração (_Abstraction layers_)
- Separação de consulta de comando (_Command-query separation_)
- Transparência referencial (_Referential transparency_)
- Pipeline funcional (_Functional pipeline_)
- Núcleo funcional em um shell imperativo (_Functional core in an imperative shell_)
- Arquitetura em camadas, “Portas e Adaptadores” (_Layered architecture, “Ports and Adapters”_)
- Direção de dependências (_Direction of dependencies_)
- Imutabilidade e apatridia (_Immutability and statelessness_)
- aplicações de 12 fatores (_12-factor applications_)

Você não precisa _conhecer tudo_. Exploraremos os termos e as técnicas à medida que o livro avança. Mas é bom se você tiver uma ideia aproximada sobre o significado básico de cada termo.

## Por que outro livro

Existem muitos livros sobre refatoração, por que precisamos de outro?

Em geral, não, nós não precisamos. Todos os princípios e técnicas são descritos em outros livros, provavelmente de uma forma muito mais detalhada, lógica e correta.

Preciso deste texto, antes de tudo, para mim:

- Para sistematizar o que sei hoje;
- Para se referir a um tópico específico ao discutir um problema;
- Para marcar o nível do meu conhecimento para que eu entenda onde melhorar e o que aprender.

No entanto, eu pensei que talvez este texto pudesse ser útil para outras pessoas, então aqui estamos. Espero que você ache este livro útil!

[^talk]: “Refactor Like a Superhero” Talk, https://bespoyasov.me/talks/refactor-like-a-superhero/
[^term]: Code Refactoring, Wikipedia, https://en.wikipedia.org/wiki/Code_refactoring
