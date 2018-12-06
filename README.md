![](kino-banner.png)

__Kino__ é um dialeto de [Cucumber Gherkin](https://github.com/cucumber/cucumber/wiki/Gherkin) para .NET/CShap.

A ideia é produzir testes unitários utilizando um jargão [BusinessReadableDSL](https://martinfowler.com/bliki/BusinessReadableDSL.html), meio que parecido com isso:

```csharp
using E5R.Kino;

namespace My.App.Utils.Test.Target1
{
    // [HEADER]
    
    Feature("Somar")
           ("Calcula a soma entre números inteiros,")
           ("produzindo resultados matematicamente coerentes.");

    Scenario("Números positivos devem ser somados")
        .Given("Número 1 como @n1")
        .And("Número 2 como @n2")
        .When("Eu acionar somar!", (input)
            => new Result {
                r = Calculadora.Somar(input.n1, input.n2) })
        .Then("O resultado obtido é @r", (expected, result)
            => Assert.Equal(expected.r, result.r))

        .Expectancy()
            .In(new Input {
                n1 = 999,
                n2 = 1 })
            .Out(new Output{
                r = 1000 })

            .In(new Input {
                n1 = 47,
                n2 = 53 })
            .Out(new Output{
                r = 100 });
            
    // [FOOTER]
}
```

Você teria uma assinatura de entradas, saídas e expectativas:

```csharp
namespace My.App.Utils.Test.Target1
{
    public struct Input
    {
        public int n1;
        public int n2;
    }

    public struct Output
    {
        public int r;
    }

    public struct Result
    {
        public int r;
    }
}
```

Para facilitar a leitura, no código inicial fizemos um _folding_ da parte declarativa
da classe. Em uma IDE como o [Visual Studio](https://visualstudio.com) ou [Visual Studio Code](https://code.visualstudio.com/docs/editor/codebasics#_folding) fica bem legal a visualização e
o foco é direcionado a descrição do cenário de teste.

```csharp
#region HEADER
    public class Target1Tests : Kino<Input, Output, Result> {
    public Target1Tests() => {
#endregion

    // Seu código de especificação do teste está aqui!

#region FOOTER
    }}
#endregion
```

A execução dos testes produziria uma saída parecida com essa:

```console
$ kino MyCompany.MyLib.dll
Starting test execution, please wait...

[MyCompany.Target1Tests]...................................[FAIL]
  > Feature: Somar                                         [fail]
  # Scenario 1: Números positivos devem ser somados        [fail]
  ---------------------------------------------------------------
  Given  Número 1 como 999
    And  Número 2 como 1
   When  Eu acionar somar!
   Then  O resultado obtido é 1000                           [ok]
  
  Given  Número 1 como 47
    And  Número 2 como 53
   When  Eu acionar somar!
   Then  O resultado obtido é 100                          [fail]
  .....  Error Message: Assert.Equal() Failure
  .....  Expected: 100
  .....  Actual:   98
  ---------------------------------------------------------------
```

