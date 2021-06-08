# NUnit 

## Teste de unidade em C# com NUnit e .NET Core

Este tutorial apresenta uma experiência interativa de compilação de uma solução de exemplo passo a passo para aprender os conceitos do teste de unidade. Se você preferir acompanhar o tutorial usando uma solução interna, veja ou baixe o exemplo de código antes de começar. Para obter instruções de download, consulte Exemplos e tutoriais.

Este artigo é sobre testar um projeto .NET Core. Se você estiver testando um projeto ASP.NET Core, consulte testes de integração em ASP.NET Core.

### Pré-requisitos

:heavy_check_mark: SDK do .NET Core 2.1 ou versões posteriores.
:heavy_check_mark: Um editor de texto ou editor de código de sua escolha.

## Criando o projeto de origem

Abra uma janela do shell. Crie um diretório chamado unit-testing-using-nunit para armazenar a solução. Nesse diretório, execute o seguinte comando a fim de criar um novo arquivo de solução para a biblioteca de classes e o projeto de teste:

CLI do .NET

| dotnet new sln | 

Em seguida, crie um diretório PrimeService. A seguinte estrutura de tópicos mostra a estrutura de arquivo e o diretório até aqui:

Console

/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService

Torne PrimeService o diretório atual e execute o seguinte comando para criar o projeto de origem:

CLI do .NET

 | dotnet new classlib  | 

Renomeie Class1.cs como PrimeService.cs. Crie uma implementação com falha da classe PrimeService:

C#

using System;

namespace Prime.Services
{
    public class PrimeService
    {
        public bool IsPrime(int candidate)
        {
            throw new NotImplementedException("Please create a test first.");
        }
    }
}

Altere o diretório de volta para o diretório unit-testing-using-nunit. Execute o seguinte comando para adicionar o projeto de biblioteca de classes à solução:

CLI do .NET

 |  dotnet sln add PrimeService/PrimeService.csproj | 

### Criando o projeto de teste

Em seguida, crie o diretório PrimeService.Tests. O seguinte esquema mostra a estrutura do diretório:

Console

/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests

Torne o diretório PrimeService.Tests o diretório atual e crie um novo projeto usando o seguinte comando:

CLI do .NET
 |  dotnet new nunit  | 

O comando dotnet new cria um projeto de teste que usa o NUnit como a biblioteca de teste. O modelo gerado configura o executor de teste no arquivo PrimeService.Tests.csproj:

XML

<ItemGroup>
  <PackageReference Include="nunit" Version="3.13.2" />
  <PackageReference Include="NUnit3TestAdapter" Version="3.17.0" />
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="16.10.0" />
</ItemGroup>

O projeto de teste requer outros pacotes para criar e executar testes de unidade. O dotnet new, na etapa anterior, adicionou o SDK de teste da Microsoft, a estrutura de teste do NUnit e o adaptador de teste do NUnit. Agora, adicione a biblioteca de classes PrimeService como outra dependência ao projeto. Use o dotnet add reference comando:

CLI do .NET

 | dotnet add reference ../PrimeService/PrimeService.csproj  | 

Você pode ver o arquivo inteiro no repositório de exemplos no GitHub.

O seguinte esquema mostra o layout da solução final:

Console

/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
        Test Source Files
        PrimeService.Tests.csproj

Execute o seguinte comando no diretório unit-testing-using-nunit:

CLI do .NET

dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj

### Criando o primeiro teste

Escreva um teste com falha, faça-o ser aprovado e, em seguida, repita o processo. No diretório PrimeService.Tests, renomeie o arquivo UnitTest1.cs para PrimeService_IsPrimeShould.cs e substitua todo o seu conteúdo pelo código a seguir:

C#

using NUnit.Framework;
using Prime.Services;

namespace Prime.UnitTests.Services
{
    [TestFixture]
    public class PrimeService_IsPrimeShould
    {
        private PrimeService _primeService;

        [SetUp]
        public void SetUp()
        {
            _primeService = new PrimeService();
        }

        [Test]
        public void IsPrime_InputIs1_ReturnFalse()
        {
            var result = _primeService.IsPrime(1);

            Assert.IsFalse(result, "1 should not be prime");
        }
    }
}

O atributo [TestFixture] indica uma classe que contém testes de unidade. O atributo [Test] indica um método que é um método de teste.

Salve esse arquivo e execute dotnet test para compilar os testes e a biblioteca de classes e, em seguida, execute os testes. O executor de teste do NUnit contém o ponto de entrada do programa para executar os testes. dotnet test inicia o executor de teste usando o projeto de teste de unidade que você criou.

O teste falha. Você ainda não criou a implementação. Faça esse teste ser aprovado escrevendo o código mais simples na classe PrimeService que funciona:

C#

public bool IsPrime(int candidate)
{
    if (candidate == 1)
    {
        return false;
    }
    throw new NotImplementedException("Please create a test first.");
}

No diretório unit-testing-using-nunit, execute dotnet test novamente. O comando dotnet test executa uma compilação para o projeto PrimeService e, depois, para o projeto PrimeService.Tests. Depois de compilar os dois projetos, ele executará esse teste único. Ele é aprovado.

### Adicionando mais recursos

Agora que você fez um teste ser aprovado, é hora de escrever mais. Existem alguns outros casos simples de números primos: 0, -1. Você pode adicionar novos testes com o atributo [Test], mas isso se torna entediante rapidamente. Há outros atributos de NUnit que permitem que você grave um pacote com testes semelhantes. Um atributo [TestCase] é usado para criar um pacote com testes que executa o mesmo código, mas têm diferentes argumentos de entrada. Você pode usar o atributo [TestCase] para especificar valores para essas entradas.

Em vez de criar novos testes, aplique esse atributo para criar um único teste controlado por dados. O teste controlado por dados é um método que testa vários valores menores que dois, que é o menor número primo:

C#

[TestCase(-1)]
[TestCase(0)]
[TestCase(1)]
public void IsPrime_ValuesLessThan2_ReturnFalse(int value)
{
    var result = _primeService.IsPrime(value);

    Assert.IsFalse(result, $"{value} should not be prime");
}

Execute dotnet test, e dois desses testes falham. Para fazer com que todos os testes sejam aprovados, altere a cláusula if no início do método Main no arquivo PrimeService.cs:

C#

 | if (candidate < 2) | 

Continue iterando adicionando mais testes, mais teorias e mais código na biblioteca principal. Você tem a versão concluída dos testes e a implementação completa da biblioteca.

Você criou uma pequena biblioteca e um conjunto de testes de unidade para essa biblioteca. Você estruturou a solução para que a adição de novos pacotes e testes fizesse parte do fluxo de trabalho normal. Você concentrou grande parte do seu tempo e esforço em resolver as metas do aplicativo.