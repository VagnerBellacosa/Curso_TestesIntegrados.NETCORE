# Bogus

## Criando dados fake com o Bogus

O Bogus é uma biblioteca que foi criada para gerar dados fake, ela foi desenvolvida baseada no **facker.js** e sua sintaxe de utilização foi inspirada no **FluentValidation**. A biblioteca foi desenvolvida por **Brian Chavez** e funciona hoje com *C#*, *F#* e *VB.NET*.

A ideia principal da biblioteca é gerar dados fake para a aplicação de forma simples e prática e principalmente para que seja possível fazer testes sem dificuldades.

## Usando o Bogus

1. Vamos ao que interessa, para o exemplo vamos criar um projeto Console App no Visual Studio.
2. Após isso, vamos adicionar uma nova classe ao nosso projeto:
3. Vamos chamar nossa classe de “Pessoa”.

Nossa classe vai ficar da seguinte forma:

public class Pessoa
{
    public Guid Id { get; set; }
    public string Nome { get; set; }
    public DateTime DataNascimento { get; set; }
}

Com isso já temos o necessário para utilizar o Bogus.

## Instalando o Bogus

Para fazer a instalação do Bogus basta adicionar a referência no Nuget Packge:

Com a biblioteca istalada já temos o necessário para criar nossos objetos fake utilizando o Bogus.

### Criando nosso fake

O Bogus permite você gerar diversos dados, como nome, datas, endereço e diversos tipos de dados. Você também tem a opção de trabalhar com gêneros para a geração de dados e ele ainda tem suporte a vários idiomas, abaixo vamos preencher uma classe pessoa com sexo feminino e com os dados em português, vejamos o exemplo:

var pessoa = new Faker<Pessoa>("pt_BR")
                .RuleFor(c => c.Id, f => f.Random.Guid())
                .RuleFor(c => c.Nome, f => f.Name.FullName(Bogus.DataSets.Name.Gender.Female))
                .RuleFor(c => c.DataNascimento, f => f.Date.Past(15))
                .Generate();

Vamos entender o que o código faz, primeiro passo é definir um objeto “Faker<Pessoa>” ele é do tipo genérico e vai gerar o nosso objeto pessoa. Como parâmetro estamos passando o locale “pt_BR”, que vai ser responsável por trazer os dados na nossa língua.

Após isso vamos utilizar a linguagem fluente para configurar nosso objeto, o “RuleFor” é responsável por gerar nossos dados, no primeiro parâmetro é informada a propriedades que desejamos alimentar, no segundo é onde utilizamos os fakes do Bogus para alimentar cada propriedade, temos diversos fakes disponíveis.

No exemplo estamos utilizando os fakes de geração randômica de guid, nome e data de nascimento. Podemos observar que o fake do nome é  definido para gerar um nome completo do sexo feminino.

Ao final chamamos o método “Generate”, onde ele gera uma instancia da nossa classe pessoa com os dados. Se pedir para imprimir os dados em telas teremos o seguinte resultado:

Vale lembrar que a cada nova execução os dados gerados são diferentes, veja:

Os dados são gerados aleatoriamente a cada execução, assim tendo casos dinâmicos de dados.

### Gerando Listas

E se você precisar gerar uma lista de pessoas ao invés de uma única pessoa?

É muito simples também, basta passar no método “Generate” a quantidade de objetos que você deseja gerar, abaixo vamos mostrar como seria a geração de uma lista com 3 pessoas:

var pessoas = new Faker<Pessoa>("pt_BR")
                .RuleFor(c => c.Id, f => f.Random.Guid())
                .RuleFor(c => c.Nome, f => f.Name.FullName())
                .RuleFor(c => c.DataNascimento, f => f.Date.Past(15))
                .Generate(3);

No exemplo foi retirado do método “FullName” o parâmetro de sexo, assim o Bogus já entende que ele tem que gerar nomes com sexo aleatórios.

## Conclusão

Podemos ver que com o Bogus podemos gerar facilmente dados fake, uma boa aplicação dessa biblioteca é no cenário de testes de unidade, assim deixando os testes mais ricos. O Bogus é muito rico, no github você pode ver tudo que pode ser gerado com o Bogus.

[Código de exemplo]( https://github.com/tborgesvieira/BogusUtilizacao)

[Referência](https://github.com/bchavez/Bogus)
