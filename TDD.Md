# TDD

## O que é TDD e por quê ele é uma boa idéia?

O conceito de TDD vem crescendo muito com a adoção da metodologia "Extreme Programming" (XP), mas pode ser usado em separado, dentro de qualquer outra metodologia de desenvolvimento de software.

A grande vantagem de TDD é produzir código confiável em menos tempo, e com menos bugs. Entre outras coisas, essa técnica permite que você, após fazer uma determinada mudança (grande ou pequena) no seu sistema, possa testá-lo e ter certeza de que tudo ainda funciona, em questão de segundos. Certamente muito melhor do que ter que testar todo o sistema manualmente, tentando pensar em todo tipo de erro possível, ou ter que seguir uma checklist, não?

### O processo de TDD é simples:

- Antes de começar a codificar uma determinada função, pense em testes que você poderia fazer pra ter certeza que ela funciona;
- Declare a sua função, mas apenas o "envelope", não entre nenhum código propriamente dito;
- Crie os testes pra essa função: eles irão falhar, afinal, sua função ainda não faz nada;
- Escreva o código da função, e rode os testes; ajuste o código até que todos testes passem;

#### Repita o passo 1 para a próxima função.

Como você pode ver, uma vantagem dessa metodologia é que ela é extremamente pragmática, e faz que você programe guiado por intenção, ou seja, primeiro você pensa qual o objetivo que você quer alcançar pra só então começar a escrever o código que alcance esses objetivos. Uma outra consequência interessante é que esse método reduz a possibilidade de que você sobrecarregue o código, ou seja, escreva código desnecessário: se você pensou em todos testes possíveis, e seu código passa em todos eles, é porque você terminou o seu trabalho.

### Um exemplo prático

Vamos ver como isso funcionaria então, com um exemplo bem simples: suponha que eu precise criar uma função para cálculo de impostos sobre um valor, com as seguintes regras:

a) Caso o valor seja menor que 1.000, está isento de impostos;
b) Para valores entre 1.000 e 10.000 é cobrado 3% de imposto;
c) Para valores acima de 10.000 é cobrado 3% de imposto mais 250 reais de sobretaxa.

Para executarmos os testes, precisaremos utilizar uma ferramenta que nos permita rodá-los rapidamente. Obviamente poderíamos criar funções-teste em uma biblioteca à parte e criarmos algumas janelas ou páginas para testá-las (provavelmente você, assim como eu, já fez isso muito na vida), mas isso significaria uma perda de tempo enorme, além de termos que nos preocupar não só com os bugs do programa em si mas também com os bugs do formulário-teste, uma distração desnecessária. Uma ferramenta feita especificamente pra testes é fundamental, e já existe: chama-se NUnit.

# NUnit

**NUnit** é software de código-aberto, escrito em C#, baseado no **JUnit** pra Java. A interface do NUnit é bem simples: ela nos mostra todos os testes, com o seguinte código de cores ao lado de cada um: vermelho se o teste não passou, e verde se passou. Uma barra de progresso à direita dá um resultado geral (ou seja, verde só se todos passaram)