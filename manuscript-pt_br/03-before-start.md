# Antes da refatoração

Para refatorar o código mais rápido, sem regressões e sem “muito trabalho extra”, precisamos primeiro _estudar e preparar_ o código. Em particular, devemos:

- Definir o escopo e os limites da refatoração.
- Cobrir o código sob refatoração com testes.
- Configurar os linters e, se necessário, o compilador de forma mais rigorosa.

Neste capítulo, discutiremos por que essas etapas são úteis e como torná-las mais simples.

## Definir o escopo e os limites

A refatoração não deve demorar para sempre. Em vez disso, deve ser um pequeno pedaço de trabalho com limites claros e uma definição de pronto. Isso é essencial por dois motivos:

- Queremos ficar dentro do nosso _orçamento de tempo e recursos_.
- Alterações limitadas são _mais fáceis de acompanhar_ para ver mais rápido se irão quebrar o aplicativo.

Os limites de refatoração definem o escopo do trabalho. Eles são os pontos de junção entre o código que vamos alterar e todo o resto. Eles mostram onde nossas mudanças devem parar, ou seja, qual código _não_ mudará.

Definir limites no código nem sempre é óbvio, especialmente se os módulos estiverem fortemente acoplados. Nesses casos, devemos prestar atenção aos _dados e dependências_ do código.

Quanto mais diferentes os dados com os quais dois pedaços de código trabalham, mais provável é que sejam partes diferentes e independentes do programa. A junção dessas partes é o limite que deve impedir que as alterações se espalhem pela base de código.

| A propósito 🪡                                                                                                                                        |
| :---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Penas em “_Working Effectively with Legacy Code_” chamam essas junções de “costuras”.[^workingeffectively] Às vezes, usarei esse termo como sinônimo. |

Os limites evitam que a refatoração demore para sempre e ajudam você a integrar as alterações na _main branch_ do repositório com mais frequência.

| Em detalhes 🔬                                                                  |
| :------------------------------------------------------------------------------ |
| Falaremos um pouco mais sobre encontrar e usar limites nos capítulos seguintes. |

## Cobrir com Testes

Precisamos cobrir o código dentro dos limites com testes. Nós os usaremos para verificar se algo foi quebrado durante a refatoração constantemente. Para fazer mais uso dos testes, devemos atender a várias condições:

### Encontre Mais Casos Extremos

Casos extremos ajudam a evitar regressões e garantem que não quebraremos o comportamento do código em circunstâncias “exóticas”. Bugs (“exóticos” são muito mais difíceis de corrigir.[^debugit])

Quanto mais diversos os casos extremos, mais fácil é preparar e sistematizar dados de teste para diferentes situações. O conhecimento do comportamento do aplicativo nessas situações será útil no futuro, mesmo após a refatoração.

### Definir Entrada Explícita e Implícita

A entrada explícita são os argumentos de funções ou métodos. A entrada implícita são dependências, estado compartilhado ou global e o contexto de funções e métodos. A sistematização dos dados de entrada simplificará a criação de casos de teste.

### Especifique o Resultado Desejado

Durante a refatoração, não podemos alterar a funcionalidade do código, portanto, o resultado desejado é o comportamento atual real do programa. Podemos capturá-lo como dados de saída (por exemplo, o resultado de uma função) ou como um efeito colateral desejado (por exemplo, mudança de estado ou chamada de API).

Idealmente, o resultado deve existir _ no limite_ do código sob refatoração. A verificação dos resultados no limite é mais fácil, enquanto a quantidade de código afetado é mínima.

<figure>
  <img src="../images/03-result-on-edge.png" width="800">
  <figcaption><em>Resultado na fronteira geralmente é mais fácil de verificar</em></figcaption>
</figure>

### Configurar Testes Automatizados

Ao refatorar, queremos testar _cada_ mudança, mesmo uma minúscula. Testes manuais podem ser cansativos. Podemos ficar com preguiça ou esquecer de testar as alterações com testes manuais.

Testes automatizados não têm esse problema. Eles podem ser executados em paralelo enquanto refatoram e testam novamente o código toda vez que salvamos um arquivo. Dessa forma, os resultados dos testes estão sempre a frente de nossos olhos e podemos perceber mais rapidamente qual alteração quebrou o código.

Que tipo de testes usar depende da situação e não é tão importante quanto sua existência. Se os testes unitários forem suficientes, podemos usá-los apenas. Se tivermos que testar o funcionamento de vários módulos, podemos precisar de um teste de integração ou E2E. O ponto principal aqui é _automação_. Quanto menos verificações fazemos à mão, menos erros humanos aparecem.

## Torne as Configurações do Linter e do Compilador Nais Rígidas

Esta seção é um pouco opcional, mas eu gosto de usá-la. Na minha opinião, configurações de linter e compilador mais agressivas ajudam a detectar bugs ocasionais e práticas ruins. As configurações “mais agressivas” podem incluir:

### Use “Erros” ao invés de “Alertas"

Os alertas de linter são uma experiência coletiva da indústria. Eles podem ser valiosos, mas são fáceis de serem ignorados porque não fazem a compilação do código “travar com um erro”.

Com erros, por outro lado, o código “não compila”. Erros nos forçarão a atualizar o código ou alterar nossas regras de linter.

| Tenha em mente❗️                                                                                                                                                                                                           |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Nem todas as regras de linter são igualmente úteis. Podemos escolher regras que achamos mais importantes e descartar outras. No entanto, uma vez que tenhamos selecionado um conjunto de regras, devemos segui-las à risca. |

### Configurar mais Verificações Automatizadas

É um ótimo momento para experimentar novas regras de linter ou outras ferramentas automatizadas se a equipe quiser experimentá-las.

Mas vale lembrar que nem todas as regras são igualmente valiosas e apropriadas para cada projeto. Devemos tentar não ir contra a equipe. Se a maioria dos outros desenvolvedores da equipe achar que uma regra de linter específica é desnecessária, não devemos introduzi-lá.

As equipes podem debater regras e práticas para determinar o mais adequado para elas. Mas lembre-se que nenhuma regra vale a pena ter um conflito na equipe.

[^workingeffectively]: “Working Effectively with Legacy Code” by Michael C. Feathers, https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
[^debugit]: “Debug It!: Find, Repair, and Prevent Bugs in Your Code” by Paul Butcher, https://www.goodreads.com/book/show/6770868-debug-it
