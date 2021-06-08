# MSTEST 

**MSTest** é uma ferramenta da Microsoft utilizada para executar teste por linha comando.

Após criarmos nossos testes e suítes de testes utilizando o Visual Studio, podemos criar rotinas de testes usando Prompt de Comando e executá-las utilizando o MSTEST. Também podemos realizar integrações com algumas API’s para criar rotinas na qual demonstramos nossos resultados

## Testes de Unidade

Testes de unidade não são um artefato comum no cenário de desenvolvimento de software corporativo, todos já ouviram falar, alguns tentam colocar em prática mas a questão da falta de design de código acaba não permitindo.

Além disso existem os famosos gestores que inibem a prática, pois acreditam que escrever um código para testar outro código é perda de tempo e aumenta o tempo de projeto.

TDD? Isso é coisa de Jedi! Apenas os desenvolvedores mais graduados na arte da codificação são capazes de escrever o teste de um código que ainda não existe.

Não é bem por ai!
Testes de unidade fazem parte da tarefa de escrever código! Código bom é aquele que pode ser testado. O que vale mais? Rodar a aplicação até o ponto desejado, colocar um break point no código e validar linha a linha se o código está realmente cumprindo a necessidade ou escrever um teste que valide isto?

Além do teste lhe prover um feedback em milissegundos, você só precisa escrever ele uma vez. Rodar a aplicação e realizar o debug pode demorar minutos e este tipo de validação precisa ser repetida dezenas de vezes (ou até mais).

TDD não é a uma técnica complicada, pelo contrário, é simples! Não existem motivos para não testar, na verdade muitos problemas do dia-a-dia seriam evitados com os testes.

## Então por que não testamos?

Acredito que o principal fator é que o desenvolvedor não sabe testar! Como eu costumo dizer, isto é uma questão de “tooling”. Tooling seria o ferramental, a prática necessária e ferramentas utilizadas para a escrita de testes de unidade.

- Meu objetivo não é provar que testes de unidade vão resolver muitos dos seus problemas e melhorar a qualidade das suas entregas (mas vão sim, acredite).

- Meu objetivo é apresentar um ótimo conjunto de ferramentas para que você comece a escrever testes de unidade da melhor forma possível!

Todas as ferramentas são open-source e gratuitas 😉

## Frameworks de Testes

São os responsáveis por rodar os testes de unidade e fazer as asserções (Assert) para garantir que o resultado do teste bateu com o esperado.

É a ferramenta mais importante e é necessário fazer uma boa escolha, afinal depois de ter escrito centenas de testes mudar de framework não será simples.

Para .NET não temos tantas opções (ainda bem)

- MSTest
- NUnit
- XUnit

O **MSTest** é o framework padrão do Visual Studio, desenvolvido pela Microsoft e utilizado em muitos projetos. Eu não costumo utilizar, mas também não o julgo como ruim.

O **NUnit** é o framework mais utilizado entre desenvolvedores .NET, possui anos de história e atende muito bem as necessidades, podemos ver o NUnit como uma portabilidade do JUnit do Java para .NET.

**XUnit** é o mais novo desta lista e atualmente meu preferido. Ele foi criado pelos mesmos desenvolvedores do NUnit. A ideia do XUnit surgiu em simplificar e modernizar a forma de escrever testes. Estas mudanças seriam impossíveis de serem aplicadas no NUnit, pois resultaria num gigantesco breaking change nos testes já existentes.

Mas esse XUnit é confiável mesmo? Os desenvolvedores do novo stack do .NET estão utilizando o XUnit. Sim o nosso .NET Core, ASP.NET Core, EF Core, etc estão sendo testados com XUnit. Acho que somente este fato já nos trás bastante segurança certo?

### Um pouco mais sobre o XUnit

Escrever testes com o XUnit é um pouco diferente, afinal algumas coisas fogem do padrão, não é necessário fazer Setup, Tear Down etc, basta usar o construtor da clase e o dispose. É um framework muito flexível e é possível criar extensões para diversas finalidades.

Eu sugiro que você leia a comparação feita entre o XUnit e os outros frameworks para começar a ter uma ideia das mudanças. Não deixe também de ler a documentação e visitar o projeto no GitHub.

Para que o Visual Studio reconheça seus testes (uma vez que é o MSTest o padrão) é necessário instalar um “Runner“, existem runners para Visual Studio, Xamarin, Console, MSBuild e etc.

Eu preparei um projeto implementando todas as ferramentas listadas e com exemplos de como utilizar. O link está no final do post (mas continue lendo, tem muito mais).

## Frameworks de Mock
O que seriam dos testes sem os Mocks? Para quem não conhece o termo, realizar o Mock de algo é criar em tempo de execução uma instância de uma classe ou a implementação de uma interface. Imagine que seu teste depende de ler algo do banco de dados, você não precisa que ele vá até o banco de fato, basta “fingir” que foi.

O Mock é uma instância de determinado objeto mas nós podemos ensinar este objeto “mockado” a responder conforme nossas necessidades, por exemplo ao invés de ir até o banco simplemente traga uma lista de objetos que já foi definida como retorno. Para isto basta “ensinar” o método do objeto mockado o que ele deve fazer ao ser invocado.

### Para Mock também existem algumas opções:

- RhinoMocks
- MOQ
- NSubstitute
- FakeItEasy

Existem mais opções além dos listados, estes são os mais utilizados atualmente.

O meu preferido e o que utilizo atualmente é o MOQ.

#### Este MOQ é o melhor? Devo-utilizar?

Não existe o melhor, é questão de afinidade também, mas com certeza o MOQ é um dos melhores e temos mais uma vantagem. A Microsoft utiliza o MOQ junto com o XUnit para desenvolver nosso novo stack do .NET Core.

##### Um exemplo de Mock com MOQ

Vamos simular que desejamos validar o retorno de um objeto que seria recuperado do banco de dados através de um repositório.

[Fact]
public void CustomerRepository_GetCustomer_ShouldReturnUniqueCustomer(){

    // Arrange

    var customer = new Customer(Guid.NewGuid(),

                                "Eduardo Pires");

    var repository = new Mock<ICustomerRepository>();
    repository.Setup(r => r.GetById(customer.Id))
        .Returns(customer);
    // Act
    var customerRet = repository.Object.GetById(customer.Id);
    // Assert
    Assert.Equal(customer, customerRet);
    repository.Verify(r => r.GetById(It.IsAny<Guid>()),
                           Times.Once);
}

Criamos uma instância do objeto baseado na interface, “ensinamos” ao método o que ele devolveria retornar ao receber determinado parâmetro e pronto.

O MOQ ainda nos oferece o Verify que é um método de Asserção para validarmos se aquele método foi realmente chamado e quantas vezes foi.

Poderíamos ter parado por aqui, pois apenas um bom framework de teste e um de mock já nos bastariam para escrever nossos testes de unidade, mas tem como melhorar bastante esta experiência.

## Apresentando o AutoMoq

Imagine que você precisa criar a instância de um objeto mas este objeto recebe diversas dependências injetadas no construtor. Obviamente no teste você não terá um container de DI, nesse caso você precisaria criar um mock de cada dependência e depois passar todas no construtor.

Tranquilo! Mas dá para ser melhor! E se fosse possível criar de uma vez só este objeto com todos os mocks gerados e injetados? Pouparia bastante código né?

É para isto que existe o AutoMoq! Sim você vai querer muito ele em seu projeto.

O AutoMoq gera os mocks do MOQ, está tudo em casa!

A classe CustomerService possui duas dependências injetadas no construtor


public class CustomerService : ICustomerService{
    private readonly ICustomerRepository _customerRepository;
    private readonly IMediator _mediator;
    public CustomerService(
                ICustomerRepository customerRepository,
                IMediator mediator){
        _customerRepository = customerRepository;
        _mediator = mediator;
    }
}

Vamos criar uma instância dela e automaticamente seus mocks com o AutoMoq

[Fact]
public void CustomerService_RegisterNewCustomer_ShouldRegisterWithSuccess()
{
    // Arrange
    var customer = new Customer(Guid.NewGuid(),
        "Eduardo Pires");
 
    var mocker = new AutoMoqer();
    mocker.Create<CustomerService>();
    var customerService = mocker.Resolve<CustomerService>();
    var repository = mocker.GetMock<ICustomerRepository>();
    // Act
    customerService.Register(customer);
    // Assert
    repository.Verify(r => r.Add(It.IsAny<Customer>()),
                           Times.Once);
}

Note de que além de criar o objeto com seus mocks injetados é possível ainda obter estes mocks para que possam ser “ensinados” ou apenas validados como no caso do nosso teste, isso significa que o repositório salvou o cliente, logo podemos entender que o teste conseguiu validar o processo.

O AutoMoq é indispensável para nos poupar da tarefa de criar e injetar cada mock e é basicamente isto que ele faz.

#### Bogus

Você já deve ter imaginado que é muito chato ter que ficar criando dados de testes (e-mails, telefone, nome, sobrenome, endereço etc). Além disso muitas vezes é necessário simular uma grande lista de dados únicos. Como fazer?

O Bogus é um gerador de dados aleatórios! Existem outros que fazem isto, mas o Bogus é mais humano. Ele gera dados reais e em diversos idiomas \o/

Vamos simular que desejo criar uma lista de 100 clientes

var customerTests = new Faker<Customer>("pt-BR")
    .CustomInstantiator(f => new Customer(
        Guid.NewGuid(),
        f.Name.FirstName(Name.Gender.Male),
        f.Name.LastName(Name.Gender.Male),
        f.Date.Past(80, DateTime.Now.AddYears(-18)),
        f.Internet.Email().ToLower(),
        true,
        DateTime.Now)).Generate(100);

O resultado é muito interessante! Dados humanos gerados com muita facilidade e a garantia de unicidade. Note que não é problema criar um objeto que depende de um construtor.

#### Bogus Random Data

Uma grande chateação a menos! O Bogus é capaz de gerar diversos tipos de informações e até imagens, recomendo que leia a documentação.

##### FluentAssertions

É a asserção que garante o resultado do teste, os frameworks tradicionais possuem uma série de métodos para validar se é igual, verdadeiro, falso, maior, menor e etc. Mas a escrita de diversos Asserts pode ser chata e deixar o código inexpressível.

O Fluent Assertions pode te ajudar nisto! Além de utilizar a sintaxe fluente existem inúmeros tipos de asserções que podem ser feitas deixando nosso código mais reduzido e fácil de entender.

1
[Fact]
2
public void CustomerService_GetAllActive_ShouldReturnsOnlyActiveCustomers()
3
{
4
    // Arrange
5
    var customerService = Fixture.GetCustomerService();
6
    Fixture.CustomerRepositoryMock.Setup(c => c.GetAll())
7
        .Returns(Fixture.GetMixedCustomers());
8
 
9
    // Act
10
    var customers = customerService.GetAllActive().ToList();
11
 
12
    // Assert Fluent Assertions
13
    customers.Should().HaveCount(c => c > 1)
14
        .And.OnlyHaveUniqueItems();
15
 
16
    customers.Should().NotContain(c => !c.Active);
17
}
É possível mesclar diversas asserções em uma única linha de código.
Bonito né? Não deixe de conferir a documentação.

Outras ferramentas
O meu set básico é este, porém contamos com outras ferramentas como por exemplo o AutoFixture que facilita a criação de objetos comumente utilizados, tudo depende muito das ferramentas que utiliza, o XUnit por exemplo possui um ótimo suporte a Fixture Collections e resolve com elegância esta questão (confira implementação no meu projeto).

Escrever testes é preciso, mas como validar se a cobertura de código está boa?
Ferramentas de Code Coverage servem para validar se o seus testes estão cobrindo bem todo seu código. Não adianta dizer que tem mais de 5.000 testes se estes estão cobrindo apenas 50% do seu código.

Isso significa que metade do seu código corre o risco de ter bugs ou até pior, significa que numa manutenção de rotina você pode mandar um bug para produção por que os testes não estão validando partes da sua aplicação.

O Visual Studio possui uma ótima ferramenta de Code Coverage, mas só está disponível para as versões mais completas. Mas não se preocupe existem boas alternativas.

NCrunch

O NChunch reúne diversas ferramentas para atender nossas necessidades

Code coverage
Métricas de performance
Detalhes das exceptions
Execução inteligente dos testes
NDepend

O NDepend possui ferramentas mais avançadas que são muito importantes para quem leva teste de unidade a sério e possui processos de integração contínua, automatização de build e etc.

Ferramentas Gratuitas

Algumas destas features que citei podem ser obtidas separadamente através de outras ferramentas, é questão de buscar e testar!

Uma que eu recomendo é o OpenCover que faz o Code Coverage para qualquer versão do Visual Studio.

Dicas sobre testes
Testar é necessário. Leia, treine, pratique! O “tooling” só desenvolve que bota a mão na massa. Aprenda a testar, aplique esta prática no dia-a-dia e mostre aos seus colegas de trabalho que as vantagens são muito grandes.

Uma ótima sensação é ver um teste falhar após efetuar uma manutenção e pensar:
“Poxa! Eu teria deixado esse bug escapar! Mais um problema evitado!”

Após dominar o processo de escrever bons testes de unidade coloque o TDD em prática e comece a escrever código limpo, desacoplado e testável!

Recomendo dois livros para quem quiser aprender TDD, sugiro ler nesta mesma ordem.

Teste e Design no Mundo Real com .NET
Test Driven Development: By Example