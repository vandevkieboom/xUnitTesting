# xUnit Testing

## Inhoudsopgave

1. [Basisconcepten](#basisconcepten)
   - [Constructor Setup](#constructor-setup)
   - [Fact en Theory](#fact-en-theory)
2. [Assertions](#assertions)
   - [Strings](#strings)
   - [Collections](#collections)
   - [Numbers](#numbers)
   - [Exceptions](#exceptions)
   - [Types](#types)

## Basisconcepten

### Constructor Setup

Wanneer we in elke test dezelfde code moeten schrijven, kunnen we deze setup in één keer doen bij het opstarten van de test met behulp van een constructor. Hieronder volgt een voorbeeld:

```csharp
public class UnitTests
{
    private readonly Mock<ISomeInterface> _someInterface;
    private readonly SomeClass _someClass;

    public UnitTests() // constructor shortcut is 'ctor'
    {
        _someInterface = new Mock<ISomeInterface>();
        _someClass = new SomeClass(_someInterface.Object);
    }
}
