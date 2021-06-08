#  AAA Testing (Arrange Act Assert)

## Testando com AAA (Arrange, Act, Assert)

Todo teste passa por uma preparação, seja de ambiente, variáveis, banco de dados, etc. Essa preparação pode ser chamada em inglês de **Arrange**. Nosso primeiro A.

Em seguida, todo teste passa por um momento onde estimulamos o sistema sendo testado (**System Under Test** em inglês, ou **SUT**). Isso é o **Act**, nosso segundo A.

E logo em seguida, verificarmos se os resultados obtidos batem com os resultados esperados. Isso é o **Assert**, o terceiro A.

Essa não é a única forma de definir o ciclo de testes, há outras. Mas essa é uma das mais fáçeis de entender. Vamos pegar um teste comum, e vamos transformá-lo num testes triple-A. Vejam o teste abaixo, onde testo uma calculadora:

[TestClass]
public class TestaCalc
{
    [TestMethod]
    public void TesteSoma()
    {
        var calculadora = new Calculadora();
        var resultado = calculadora.Somar(2, 3);
        Assert.AreEqual(5, resultado);
    }
}
 
public class Calculadora
{
    public int Somar(int a, int b)
    {
        return a + b;
    }
}

### Posso evidenciar os três As assim:

[TestClass]
public class TestaCalc
{
    [TestMethod]
    public void TesteSoma()
    {
        //arrange:
        var calculadora = new Calculadora();
        //act:
        var resultado = calculadora.Somar(2, 3);
        //assert:
        Assert.AreEqual(5, resultado);
    }
}

Mas isso não muda nada o que eu já estava fazendo.

Já ouviram falar que cada teste só pode testar uma coisa? Já tentou fazer isso? Já viu como é difícil se não pensarmos de forma estruturada? E o problema é mesmo a estrutura. Vou propor uma mudança estrutural.

Inicio separando a preparação (*Arrange*) e o estímulo (*Act*), das verificações ({Asserts*). Isso é fácil de fazer, a primeira ação é criar a ideia de contexto, que no teste anterior da calculadora não existia, e na prática, ficava encapsulado no método. O contexto vai ser aplicado à classe, e não mais ao método. O contexto do teste é a calculadora. O contexto é preparado e estimulado na inicialização do teste.

Toda classe fica com mais ou menos essa estrutura:

[TestClass]
public class Teste
{
    [TestInitialize]
    public void Inicializar()
    {
        Arrange();
 
        Act();
    }
 
    private void Arrange()
    {
        //prepara
    }
 
    private void Act()
    {
        //executa
    }
 
    [TestMethod]
    public void VerificaAlgo()
    {
        //faz as verificaoes
    }
 
    [TestMethod]
    public void VerificaAlgoAMais()
    {
        //faz outras verificaoes
    }
}

Para traduzir o teste de calculadora fica fácil. Coloco a criação da calculadora no arrange, a soma no Act, e as verificações no teste em si.

[TestClass]
public class DadaUmaCalculadoraQueSoma2Mais3
{
    private Calculadora _calculadora;
    private int _resultado;
 
    [TestInitialize]
    public void Inicializar()
    {
        Arrange();
 
        Act();
    }
    private void Arrange()
    {
        _calculadora = new Calculadora();
    }
    private void Act()
    {
        _resultado = _calculadora.Somar(2, 3);
    }
    [TestMethod]
    public void Totaliza5()
    {
        Assert.AreEqual(5, _resultado);
    }
}

Com isso, apenas separamos o teste em três métodos diferentes, e pode ser que não esteja valendo a pena.

A ideia começa a ficar mais interessantes quando o teste fica mais complexo. O teste passa a ser auto-explicativo, e as falhas passam a ficar mais evidentes. O exemplo a seguir deixa isso claro:

[TestClass]
public class TestaServicoEmissaoNFEmitindoNF
{
    private ServicoEmissaoNF _servico;
    private Pedido _pedido;
    private NotaFiscal _nf;
    private EnviadorDeMensagensFake _enviador;
 
    [TestInitialize]
    public void Inicializar()
    {
        Arrange();
 
        Act();
    }
 
    private void Arrange()
    {
        _pedido = new Pedido(500);
        _enviador = new EnviadorDeMensagensFake();
        _servico = new ServicoEmissaoNF(_enviador);
    }
 
    private void Act()
    {
        _nf = _servico.Emitir(_pedido);
    }
 
    [TestMethod]
    public void NFNaoÉNula()
    {
        Assert.IsNotNull(_nf);
    }
 
    [TestMethod]
    public void ValorDaNFÉIgualAoValorDoPedido()
    {
        Assert.AreEqual(_pedido.Valor, _nf.Valor);
    }
 
    [TestMethod]
    public void MensagemEhEnviada()
    {
        Assert.IsFalse(string.IsNullOrWhiteSpace(_enviador.MensagemEnviada));
    }
}

Nesse cenário, crio uma nota fiscal a partir de um pedido, e verifico 3 coisas: a NF é criada (não é nula), o valor da NF é igual ao valor do pedido, e uma mensagem é enviada durante o processo. Cada um é um único teste.

Poderia fazer isso sem o AAA explícito? Poderia. Ficaria assim:

[TestClass]
public class TestaServicoEmissaoNF
{
    [TestMethod]
    public void EmissaoDaNF()
    {
        //arrange
        var pedido = new Pedido(500);
        var enviador = new EnviadorDeMensagensFake();
        var servico = new ServicoEmissaoNF(enviador);
        //act
        var nf = servico.Emitir(pedido);
        //assert
        Assert.IsNotNull(nf);
        Assert.AreEqual(pedido.Valor, nf.Valor);
        Assert.IsFalse(string.IsNullOrWhiteSpace(enviador.MensagemEnviada));
    }
}

## Porque eu não faria isso num cenário como esse?

O teste testa 3 coisas diferentes;

Se o primeiro assert falhar, eu não sei se os outros passaram;

O método testa coisa demais, isso fica evidente pelo nome do método de teste
É uma troca de expressividade e manutenibilidade por velocidade na escrita do código. Como a diferença de velocidade é mínima, e a perda de expressividade é alta, eu decido por não fazer.

Isso fica ainda mais fácil se você criar uma classe abstrata para testes. Algo assim:

public abstract class TesteBase
{
    [TestInitialize]
    public void Inicializar()
    {
        Arrange();
 
        Act();
    }
 
    protected virtual void Arrange()
    {
    }
 
    protected abstract void Act();
}

Assim, o Arrange é opcional, o Act é obrigatório, e os testes você implementa como quiser.