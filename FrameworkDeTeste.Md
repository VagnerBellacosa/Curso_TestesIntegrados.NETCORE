# O que é um Framework de automação de testes?

Com tantas opções disponíveis de frameworks de automação de testes, resolvemos montar uma lista com os prós e contra dos pontos mais importantes para ajudar na hora da escolha.

## O que é um framework?

### Abordagem Lean para Automação de Testes

Um **framework** é considerado uma combinação de protocolos, regras, padrões e diretrizes que podem ser incorporadas ou seguidas como um todo ou em partes. Ele estabelece as bases técnicas e estruturais que deverão ser usadas pela equipe. As aplicabilidades de um framework são grandes.

Para exemplificar o que seria um framework, pensemos num elevador, é preciso seguir algumas regras durante a utilização de um, para garantir o melhor uso e uma vida útil prolongada. Pense nas seguintes diretrizes que os usuários de elevadores seguem:

- Preste atenção na capacidade máxima do elevador e evite entrar em um quando esta capacidade for atingida.
- Aperte o botão de alarme em caso de emergência ou problemas técnicos.
- Deixe os passageiros saírem antes de embarcar e não obstrua a porta.
- No caso de incêndio no edifício, evite usar o elevador.
- Não pule ou faça movimentos muito bruscos dentro do elevador.
- Não fume dentro do elevador.
- Chame por ajuda/assistência técnica caso as portas não abram ou o elevador não funcione.
- Não tente abrir as portas na base da força.

Existem outras regras e diretrizes além destas, mas se estas forem seguidas, a vida útil é prolongada e o uso do elevador se torna muito melhor para todos.

Com este exemplo em mente, vamos seguir explicando o papel dos frameworks de automação de testes.

Um framework de automação de teste é uma base e apoio para os ambientes de execução de scripts. O framework oferece ao usuário vários benefícios que o ajudam a desenvolver, executar e criar relatórios de scripts de automação de testes. Pode-se ver também o framework como um sistema criado especificamente para auxiliar na automação de testes.

Em outras palavras, pode-se afirmar que um framework é um conjunto de várias práticas, códigos de conduta, conceitos, processos, diretrizes, hierarquia de arquivos, módulos, mecanismos de relatórios, dados de testes e etc. Desta forma, o usuário pode seguir este conjunto de diretrizes enquanto vai automatizando as funções, aumentando a produtividade.

Os benefícios podem ser facilidade de codificação, escalabilidade, modularidade, compreensibilidade, definição de processos, reutilização, redução de custo, simplificar a manutenção, etc.

A necessidade do uso de um framework de automação de testes padrão surge quando a equipe tem vários desenvolvedores trabalhando em partes diferentes do mesmo aplicativo e torna-se necessário evitar a situação em que cada desenvolvedor escolheu uma abordagem diferente para a automação.

Observe que um framework de testes é sempre independente de aplicativo, ou seja, ele pode ser usado com qualquer aplicativo.

## Recapitulando – As vantagens de um framework de automação de testes.

- Reaproveitamento do código
- Maior cobertura dos testes
- Recuperação de cenários
- Baixo custo de manutenção

## Tipos de frameworks de automação de testes

Agora que o básico já foi explicado, iremos abordar agora os vários tipos de frameworks. Existem vários tipos de frameworks disponíveis atualmente, a diferença entre eles pode ser o suporte de palavras chaves, reutilização, facilidade de manutenção, etc.

Os tipos de frameworks de automação de testes mais populares são:

- Framework baseado em módulos
- Framework de arquitetura de biblioteca
- Framework baseado em dados
- Framework orientado por palavras-chave
- Framework de Testes Híbridos
- Framework de Desenvolvimento Orientado pelo Comportamento

### Framework baseado em módulos

Este framework é inspirado no famoso conceito POO. O framework divide inteiramente o “aplicativo sob testes” em vários módulos lógicos e isolados. Para cada módulo, é criado um script de teste separado e independente. Assim, quando estes scripts são agrupados, eles criam um script de testes maior com mais de um módulo presente.

Os módulos são separados por uma camada abstrata de forma que as mudanças feitas em um segmento do aplicativo não irão afetar o módulo.

#### Pontos positivos:

- O framework oferece um alto nível de customização dos módulos, o que leva a uma manutenção mais fácil com um bom custo benefício.
- O framework é bastante escalável.
- Se mudanças são feitas em uma parte do aplicativo, apenas o script de testes desta parte precisa ser consertada para deixar o resto intacto.

#### Pontos negativos:

- Quando são implementados scripts de testes para cada módulo separadamente, os dados dos testes são embutidos nos scripts. Desta forma, sempre que forem feitos testes com dados diferentes, será preciso modificar os dados manualmente.

### Framework de arquitetura de biblioteca

Este framework é fundamental e essencialmente montado no Framework Baseado em Módulos, mas com algumas melhorias. Ao invés de dividir o aplicativo sob testes em scripts, o aplicativo é separado em funções específicas ou básicas que podem ser usadas por outras partes do aplicativo também. Com isso, é criada uma biblioteca com funções comuns para os aplicativos sob testes e esta biblioteca é de fácil acesso para os testadores.

O princípio fundamental deste framework é a definição das funções que se repetem, comuns, e agrupá-las em uma biblioteca e incluir estas funções nos scripts de testes sempre que necessário.

Exemplo: Os passos do login podem ser combinados numa função dentro da biblioteca. Assim, todos os scripts de testes que envolvem o login do aplicativo podem acessar a função na biblioteca, ao invés de ter que escrever o código todo de novo.

#### Pontos positivos:

Assim como o framework baseado em módulos, este framework também introduz um alto nível de customização de módulos, o que manutenção fácil, com um bom custo benefício e escalonável.
A medida que mais funções comuns são criadas na biblioteca, mais facilmente poderá criar scripts de testes. Com isso, este framework oferece uma alta taxa de reutilização.

#### Pontos negativos:

Também como o framework baseado em módulos, os dados dos testes são armazenados nos scripts, assim, qualquer mudança feita nestes dados também afetará os scripts.
Com a introdução de bibliotecas, o framework se torna um pouco mais complicado.

### Framework baseado em dados

Quando automatizando ou testando qualquer aplicativo, em alguns momentos pode ser preciso testar a mesma função repetidas vezes, mas com inputs de dados diferentes. Nestes casos não se pode deixar os dados dos testes embutidos nos scripts. Por isso, é recomendado armazenar os dados dos testes em outro lugar, não apenas embutidos nos scripts.

Os Frameworks baseados em dados ajudam o usuário a separar a lógica dos dados do script de teste. Isto permite o usuário armazenar os dados em um banco de dados externo. Os bancos de dados externos podem ser arquivos de propriedades, arquivos xml, arquivos do Excel, arquivos de texto, CSV, repositórios ODBC etc. 

Os dados são normalmente armazenados em pares “Key-Value”. Com isso, os pares podem ser usados para acessar e incrementar os dados dos scripts de testes.

Os dados armazenados em um arquivo externo podem pertencer a uma matriz de valores esperados, assim como uma matriz de inputs.

#### Pontos positivos:

A função mais importante deste framework é que ele reduz consideravelmente o número total de scripts necessários para cobrir todas as combinações de cenários de testes possíveis. Desta forma, o código não precisará ser tão extenso.

- Qualquer mudança na matriz de dados não prejudicará o script do código de testes.
- Maior flexibilidade e facilidade na manutenção
- Um único cenário de testes pode ser rodado usando valores de dados alternados.

#### Pontos negativos:

O processo é complexo e requer um esforço extra para conseguir uma fonte de dados e mecanismos de leitura.
Requer uma habilidade considerável em linguagens de programação que são usadas para desenvolver scripts de testes.

## Framework orientado por palavras-chave

Este framework, em um ponto, é uma extensão do framework baseado em dados, já que não apenas separa os dados dos testes dos scripts, mas também mantém uma certa parte do código pertencente ao script de teste em um arquivo de dados externo. Estes códigos são chamados de palavras-chave e por isso o nome do framework. Palavras-chave são meios de encontrar que ações precisam ser feitas no aplicativo. As palavras-chave e os dados dos testes são armazenados numa estrutura tabular, e por isso, este framework também é conhecido como o framework baseado em tabelas. Leva em consideração que palavras-chave e os dados de testes são entidades independentes das ferramentas de automação de testes utilizadas.

#### Pontos positivos:

Além das vantagens dos testes baseados em dados, o framework baseado em palavras-chave não precisa que o usuário possua conhecimento de codificação, diferente do baseado em dados.
Uma palavra-chave pode ser usada em vários scripts de testes.
Pontos negativos:

O testador tem que ter bom conhecimento do mecanismo de criação de palavras-chave para conseguir usufruir o melhor possível do framework.

O framework se torna cada vez mais complicado à medida que o número de palavras-chave vai aumentando gradualmente.

## Framework de Testes Híbridos

Como o nome já demonstra, o framework de testes híbridos é uma combinação de mais de um dos frameworks já mencionados. O melhor desta configuração são os benefícios acumulados de todos os frameworks envolvidos.

## Framework de Desenvolvimento Orientado pelo Comportamento

Este framework permite a automação de validação de funções em um formato de fácil entendimento e leitura para analistas de negócios, desenvolvedores, testadores, etc. Tal framework não precisa necessariamente que o usuário tenha familiaridade com linguagens de programação. Existem ferramentas disponíveis para o BDD, como Cucumber, Jbehave,etc.                                                                               

## Conclusão

Os tipos de frameworks apresentados acima são os mais populares. Existem outros tipos de frameworks disponíveis, mas ter conhecimento destes, já é uma boa base de conhecimento.
