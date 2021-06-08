# Biblioteca xUnit

O xUnit é uma biblioteca open source de testes unitários, que foi criada pelo mesmo criador da segunda versão do NUnit, James Newkirk. Desta forma, ambas as bibliotecas possuem funcionalidades similares.

## Criando o projeto que será testado
Como no artigo anterior, para exemplificar a biblioteca xUnit, iremos criar uma solução:

| dotnet new sln -n teste-unitario-com-xunit |

E nela adicionar uma biblioteca de classes:

| dotnet new classlib -n calculos |

Que conterá uma classe Calculadora:


public class Calculadora
{
    public int Soma(int operador1, int operador2) => operador1 + operador2;
    public int Subtracao(int operador1, int operador2) => operador1 - operador2;
    public int Multiplicao(int operador1, int operador2) => operador1 * operador2;
    public int Divisao(int dividento, int divisor) => dividento / divisor;
    public (int quociente, int resto) RestoDivisao(int dividento, int divisor) => (dividento / divisor, dividento % divisor);
}

Adicione a referência deste projeto na solução:

| dotnet sln teste-unitario-com-xunit.sln add calculos/calculos.csproj |

## Criando o projeto de teste

Um projeto de teste trata-se de uma biblioteca de classe que contenha a referência xunit. Felizmente no .NET Core, há a opção de um template de projeto já com esta biblioteca adicionada. Para criar este tipo de projeto utilize o comando abaixo:

| dotnet new xunit -n calculos.tests |

Adicione este projeto na solução:

| dotnet sln teste-unitario-com-xunit.sln add calculos.tests/calculos.tests.csproj |

E adicione nele a referência da biblioteca de cálculos nele:

| dotnet add calculos.tests/calculos.tests.csproj reference calculos/calculos.csproj |

Pronto, agora podemos adicionar testes unitários na nossa aplicação.

## Adicionando testes unitários

No projeto de teste criado, remova o arquivo UnitTest1.cs e adicione uma classe chamada CalculadoraTest, nela adicione o método abaixo:

public class CalculadoraTest
{
    [Fact]
    public void Soma_DeveRetornarOValorCorreto()
    {
        Calculadora c = new Calculadora();
        var resultado = c.Soma(10, 20);
        //Verifica se o resultado é igual a 30        
        Assert.AreEqual(30, resultado);
    }
}

Note que no código acima é utilizado o atributo em cima do método Soma_DeveRetornarOValorCorreto. Isso indica que este é um método de teste. É a graças a ele que os tests runner sabem qual método deve ser chamado quando um teste é iniciado.

Este teste pode ser executado através do comando dotnet test, o Test Explorer do Visual Studio ou o xUnit Runner Console.

Com o teste definido, ele pode ser executado:


| dotnet test |

Onde teremos resultado:

Note que ele indica que o teste definido passou.

Também é possível criar uma “teoria”. Teoria executa o mesmo teste com uma série de parâmetros. Caso algum dos parâmetros gere um resultado inesperado, ela é considerada falha.

[Theory]
[InlineData(1)]
[InlineData(2)]
[InlineData(3)]
[InlineData(5)]

public void RestoDivisao_DeveRetornarZero(int value)
{
    Calculadora c = new Calculadora();
    var resultado = c.RestoDivisao(12, value);
    //Verifica se o resto da divisão é 0
    Assert.Equal(0, resultado.resto);
}

Repare que a teoria é definida com a anotação Theory e que cada parâmetro testado é indicado pela anotação InlineData.

Caso o teste falhe, é indicado com qual valor isso ocorreu:

Para finalizar e para obtermos uma cobertura dos testes de 100%, vamos definir testes para os demais métodos de Calculadora:

public class CalculadoraTest
{
    [Fact]
    public void Soma_DeveRetornarOValorCorreto()
    {
        Calculadora c = new Calculadora();
        var resultado = c.Soma(10, 20);
        //Verifica se o resultado é igual a 30        
        Assert.Equal(30, resultado);
    }

    [Theory]
    [InlineData(1)]
    [InlineData(2)]
    [InlineData(3)]
    public void RestoDivisao_DeveRetornarZero(int value)
    {
        Calculadora c = new Calculadora();
        var resultado = c.RestoDivisao(12, value);
        //Verifica se o resto da divisão é 0
        Assert.Equal(0, resultado.resto);
    }

    [Fact]
    public void RestoDivisao_DeveRetornarOValorCorreto()
    {
        Calculadora c = new Calculadora();
        var resultado = c.RestoDivisao(10, 3);
        //Verifica se o quociente da divisão é 3 e o resto 1
        Assert.Equal(3, resultado.quociente);
        Assert.Equal(1, resultado.resto);
    }

    [Fact]
    public void Subtracao_DeveRetornarOValorCorreto()
    {
        Calculadora c = new Calculadora();
        var resultado = c.Subtracao(20, 10);
        //Verifica se o resultado é igual a 10
        Assert.Equal(10, resultado);
    }

    [Fact]
    public void Divisao_DeveRetornarOValorCorreto()
    {
        Calculadora c = new Calculadora();
        var resultado = c.Divisao(100, 10);
        //Verifica se o resultado é igual a 10
        Assert.Equal(10, resultado);
    }

    [Fact]
    public void Multiplicao_DeveRetornarOValorCorreto()
    {
        Calculadora c = new Calculadora();
        var resultado = c.Multiplicao(5, 2);
        //Verifica se o resultado é igual a 10
        Assert.Equal(10, resultado);
    }
}

## Demais comparadores do xUnit

O exemplo mostrado aqui é simples, nele fazemos uso apenas do comparador Assert.Equal. Para o caso do exemplo apenas este comparador foi necessário. Agora sempre que um método da classe for alterado, os testes podem ser executados para verificar se a alteração gerou algum impacto nos demais métodos dela.

Infelizmente a documentação do xUnit não explica os comparadores que podem ser utilizados com a biblioteca. Eles só são listados na comparação com outras bibliotecas de teste.
