# A pirâmide de testes

A função da **pirâmide de testes** é basicamente definir níveis de testes e te dar um norte quanto à quantidade de testes que você deveria ter em cada um desses níveis.

No topo da pirâmide, temos os testes de ponta a ponta (**end to end** ou **e2e**, pra resumir). O objetivo deles é imitar o comportamento do usuário final nas nossas aplicações (seja ele uma pessoa, uma api, ou qualquer outro tipo de cliente).

Na base, temos os testes de unidade, onde verificamos o funcionamento da menor unidade de código testável da nossa aplicação.

Entre essas duas camadas, temos os testes de integração. A ideia deles é verificar se um conjunto de unidades se comporta da maneira correta, só que de forma menos abrangente do que os testes de ponta a ponta.

## Testes de ponta a ponta

São testes que simulam o ambiente real, isto é, sobem a aplicação ou abrem o navegador, preenchem formulários, clicam em botões e, por fim, verificam se aconteceu o que era esperado. A diferença desse tipo de teste para um usuário real é que os testes end to end geralmente acontecem em um ambiente controlado (não o de produção) e quem executa as ações é um robozinho (não o usuário real). 

Vale lembrar que quando falamos *“usuário”* não estamos falando necessariamente do cliente que vai acessar sua página ou aplicação desktop: se seu software for uma API, então o usuário é quem vai consumir essa API. 

Esses testes são complexos de escrever e costumam demorar um tempo considerável pra rodar (afinal, interações do usuário com a aplicação são complexas e demoradas). Além disso, quando um teste de ponta a ponta falha, não é trivial inferir onde está o problema da aplicação, já que o teste é bem abrangente. Por esses motivos, geralmente são testes que cobrem apenas os fluxos principais da aplicação.

## Testes de unidade

Os testes de unidade verificam o funcionamento da menor unidade de código testável da nossa aplicação, independente da interação dela com outras partes do nosso código.

A unidade geralmente é vista como um método público em uma classe (quando falamos em Orientação a Objetos), mas pode ser vista também como um conjunto de classes/métodos/objetos interagindo entre si. Deixo a discussão de qual deveria ser o tamanho de cada unidade para outro artigo, mas independente do que você ou o seu time usem como padrão, a unidade vai ser sempre definida como a menor parte testável do seu sistema.

## Dinâmica de testes de unidade, usando “test doubles” ao invés de colaboradores reais

O que é interessante sobre testes de unidade é que eles, além de pequenos (por testarem uma parte pequena do seu sistema), são independentes de colaboradores externos à unidade.
Para que isso seja atingido, precisamos de alguns objetos “falsos” que imitam o comportamento do objeto real (mas de forma mais rápida e determinística), para usar no lugar do colaborador de verdade. Inclusive, usando esse tipo de objetos, não é nem necessário que o colaborador real exista quando você está fazendo o teste para a sua unidade. 

Com isso, nosso teste de unidade fica independente de qualquer agente externo real, podendo ser realmente pequeno, simples e, portanto, rápido de rodar. Outra vantagem que temos com isso é que quando um teste de unidade falha, você sabe exatamente onde está o problema: não precisa nem pensar!

Uma curiosidade: A tradução correta de unit test é teste de unidade, não teste unitário. A palavra “unitário” remete a “único”, que nesse contexto quer dizer “um único teste” (e não é isso que esperamos).

Outro ponto bem legal de testes de unidade é que eles podem guiar o design do seu código, especialmente quando o desenvolvimento orientado a testes (Test Driven Development - TDD) é usado. Porque o teste age como o primeiro usuário do código, acabamos escrevendo um código mais simples e coeso.

## Testes de Integração

A porta funciona perfeitamente, a fechadura funciona perfeitamente, mas a integração entre ambas não rolou muito bem.

Testes de unidade são muito legais, muito simples e muito rápidos, mas eles não são suficientes.
Podemos ter testado duas unidades que interagem entre si separadamente, usando os test doubles mencionados acima, e concluído que ambas então funcionando como esperado. Ainda assim, é possível que as duas unidades não funcionem em conjunto (procure “Two unit tests, no integration tests” no Google Images que você vai entender do que estou falando).

Para resolver esse problema, temos os testes de integração, que testam algumas unidades funcionando em conjunto. Diferente dos testes de ponta a ponta, são testes que testam funcionalidades, e não o sistema como um todo (vemos muitos desse tipo em testes de endpoints, por exemplo). As implicações disso são que testes de integração são:

- Mais complicados (de fazer e manter) e demorados que os testes de unidade, por testarem uma funcionalidade inteira (muitas vezes com persistência de dados);

- Bem mais simples (de fazer e manter) e rápidos que os testes de ponta a ponta, por testarem uma única funcionalidade de cada vez, sem precisar subir a aplicação inteira.

Voltando para a pirâmide de testes, é importante sempre lembrar que a base da pirâmide é mais fácil de fazer e mais rápida para rodar, enquanto o topo é mais difícil e lento.

Com isso em mente, a pirâmide nos mostra a importância de que a maior parte do seu código seja coberto por testes de unidade, já que eles rodam muito rápido e são muito simples (de fazer e manter).

Já o nível de teste mais complexo e demorado (os de ponta a ponta), deve possuir menos testes (assim o deploy não fica travado por mais de 1h enquanto os testes estão sendo rodados).

Os testes de integração existem para os cenários que não podem ser cobertos por testes de ponta a ponta, e para cenários que os testes de unidade já cobrem muito bem (até porque não é necessário ter testes redundantes). Nessa lógica, temos menos testes de integração que de unidade, e (bem) menos testes de ponta a ponta que de integração.

Na Creditas, essa é mais ou menos a relação que temos em um de nossos sistemas de quantidade de testes por nível, comparadas ao tempo que cada conjunto de testes demora em média para rodar.