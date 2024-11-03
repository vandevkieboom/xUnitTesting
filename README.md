# xUnitTesting

1. [Basisconcepten](#basisconcepten)
   - [Constructor Setup](#constructor-setup)
   - [Fact en Theory](#fact-en-theory)
2. [Assertions](#assertions)
   - [Strings](#strings)
   - [Collections](#collections)
   - [Numbers](#numbers)
   - [Exceptions](#exceptions)
   - [Types](#types)

## basisconcepten

### constructor-setup

indien we telkens voor elke test dezelfde code moeten schrijven kunnen we dit in 1 keer doen bij het opstarten van de test aan de hand van een constructor.
voorbeeld:
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
```

### fact-en-theory

een `Fact` is een test die altijd wordt uitgevoerd met een vaste set aanvoergegevens. Het wordt gebruikt voor eenvoudige tests die een specifieke functionaliteit verifiëren.
```csharp
[Fact]
public void Test_SimpleAddition()
{
    int result = 2 + 2;
    Assert.Equal(4, result); // controleert of de uitkomst gelijk is aan 4
}
```

een `Theory` is een test die meerdere sets van gegevens accepteert. Dit is nuttig voor parametrische tests waarbij de test met verschillende invoerwaarden kan worden uitgevoerd.
```csharp
[Theory]
[InlineData(2, 2, 4)]  // test met deze invoerwaarden
[InlineData(3, 3, 6)]  // test met andere invoerwaarden
public void Test_Addition(int a, int b, int expected)
{
    int result = a + b;
    Assert.Equal(expected, result); // controleert of de uitkomst gelijk is aan de verwachte waarde
}
```


## asserts

/*
    strings
*/
Assert.Equal(expectedString, actualString); // controleert of de twee strings exact gelijk zijn.
Assert.StartsWith(expectedString, stringToCheck); // controleert of de string begint met de verwachte waarde.
Assert.EndsWith(expectedString, stringToCheck); // controleert of de string eindigt met de verwachte waarde.

// optionele parameters kunnen ook worden meegegeven
Assert.Equal(expectedString, actualString, ignoreCase: true); // controleert of de twee strings gelijk zijn, negeert hoofdlettergevoeligheid.
Assert.StartsWith(expectedString, stringToCheck, StringComparison.OrdinalIgnoreCase); // controleert of de string begint met de verwachte waarde, negeert hoofdlettergevoeligheid.

/*
    collections
*/
Assert.Contains(expectedThing, collection); // controleert of de collectie het verwachte item bevat.
// overload-methode voor `Contains`
Assert.Contains(collection, item => item.Contains(thingToCheck)); // controleert of de collectie een item bevat dat voldoet aan de voorwaarde.
Assert.DoesNotContain(expectedThing, collection); // controleert of de collectie het verwachte item niet bevat.
Assert.Empty(collection); // controleert of de collectie leeg is.
Assert.All(collection, item => Assert.False(string.IsNullOrWhiteSpace(item))); // controleert of alle items in de collectie aan de opgegeven voorwaarde voldoen (hier: geen lege of witte ruimte).

/*
    numbers
*/
Assert.InRange(thingToCheck, lowRange, highRange); // controleert of de waarde binnen het opgegeven bereik ligt.

/*
    exceptions
*/
Assert.Throws<T>(() => sut.Method()); // controleert of een specifieke uitzondering wordt opgegooid door de methode.

/*
    types
*/
Assert.IsType<T>(thing); // controleert of het object van het verwachte type is.
Assert.IsAssignableFrom<T>(thing); // controleert of het object kan worden toegewezen aan het opgegeven type.
Assert.Same(obj1, obj2); // controleert of beide objecten naar dezelfde instantie verwijzen.
Assert.NotSame(obj1, obj2); // controleert of de objecten niet naar dezelfde instantie verwijzen.
