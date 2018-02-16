Pepino
======

Um dialeto de [Cucumber Gherkin](https://github.com/cucumber/cucumber/wiki/Gherkin) para .NET/CShap.

A ideia é produzir testes unitários utilizando um jargão [BusinessReadableDSL](https://martinfowler.com/bliki/BusinessReadableDSL.html), meio que parecido com isso:

```csharp
using E5R.Test.Pepino;

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
            => Assert.Equal(expected.r, result.r));

    Expectancy()
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
    public class Input
    {
        public int n1 { get; set; }
        public int n2 { get; set; }
    }

    public class Output
    {
        public int r { get; set; }
    }

    public class Result
    {
        public int r { get; set; }
    }
}
```

Para facilitar a leitura, no código inicial fizemos um _folding_ da parte declarativa
da classe. Em uma IDE como o [Visual Studio](https://visualstudio.com) ou [Visual Studio Code] (https://code.visualstudio.com/docs/editor/codebasics#_folding) fica bem legal a visualização e
o foco é direcionado a descrição do cenário de teste.

```csharp
#region HEADER
    public class Target1Tests : Pepino<Input, Output, Resul> {
    public Target1Tests() => {
#endregion

    // Seu código de especificação do teste está aqui!

#region FOOTER
    }}
#endregion
```

