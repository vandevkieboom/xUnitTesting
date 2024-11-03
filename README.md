# xUnitTesting - dit is enkel voor xUnit!

1. [Basisconcepten](#Basisconcepten)
   - [Constructor setup](#Constructor-setup)
   - [Fact en Theory](#Fact-en-Theory)
2. [Assertions](#Assertions)
   - [Strings](#Strings)
   - [Collections](#Collections)
   - [Numbers](#Numbers)
   - [Exceptions](#Exceptions)
   - [Types](#Types)
3. [Fluent Assertions](#Fluent-Assertions)
   - [Strings](#Strings)
   - [Collections](#Collections)
   - [Numbers](#Numbers)
   - [Exceptions](#Exceptions)
   - [Types](#Types)
4. [Mocking](#Mocking)
   - [Mocking basic stuff](#Mocking-basic-stuff)
5. [Zombie](#Zombie)
   - [Zorg voor onderhoudbaarheid](#Zorg-voor-onderhoudbaarheid)
   - [Oplossingsgerichtheid](#Oplossingsgerichtheid)
   - [Modulariteit](#Modulariteit)
   - [Beveiliging](#Beveiliging)
   - [Integratie](#Integratie)
   - [Efficiëntie](#Efficiëntie)

## Basisconcepten

### Constructor-setup
Indien voor elke test dezelfde code moeten schrijven kunnen we dit in één keer doen bij het opstarten van de test aan de hand van een constructor.
```csharp
public class UnitTests
{
    private readonly Mock<ISomeInterface> _someInterfaceMock;
    private readonly SomeClass _someClass;

    public UnitTests() // constructor shortcut is 'ctor'
    {
        _someInterfaceMock = new Mock<ISomeInterface>();
        _someClass = new SomeClass(_someInterfaceMock.Object);
    }
}
```

### Fact-en-Theory
Een `Fact` is een test die altijd wordt uitgevoerd met een vaste set aanvoergegevens. Het wordt gebruikt voor eenvoudige tests die een specifieke functionaliteit verifiëren.
```csharp
[Fact]
public void Test_SimpleAddition()
{
    int result = 2 + 2;
    Assert.Equal(4, result); // controleert of de uitkomst gelijk is aan 4
}
```

Een `Theory` is een test die meerdere sets van gegevens accepteert. Dit is nuttig voor parametrische tests waarbij de test met verschillende invoerwaarden kan worden uitgevoerd.
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

<br />

## Assertions
### Strings
```csharp
Assert.Equal(expectedString, actualString); // controleert of de twee strings exact gelijk zijn.
Assert.StartsWith(expectedString, stringToCheck); // controleert of de string begint met de verwachte waarde.
Assert.EndsWith(expectedString, stringToCheck); // controleert of de string eindigt met de verwachte waarde.

// optionele parameters kunnen ook worden meegegeven
Assert.Equal(expectedString, actualString, ignoreCase: true); // controleert of de twee strings gelijk zijn, negeert hoofdlettergevoeligheid.
Assert.StartsWith(expectedString, stringToCheck, StringComparison.OrdinalIgnoreCase); // controleert of de string begint met de verwachte waarde, negeert hoofdlettergevoeligheid.
```

### Collections
```csharp
Assert.Contains(expectedThing, collection); // controleert of de collectie het verwachte item bevat.
// overload-methode voor `Contains`
Assert.Contains(collection, item => item.Contains(thingToCheck)); // controleert of de collectie een item bevat dat voldoet aan de voorwaarde.
Assert.DoesNotContain(expectedThing, collection); // controleert of de collectie het verwachte item niet bevat.
Assert.Empty(collection); // controleert of de collectie leeg is.
Assert.All(collection, item => Assert.False(string.IsNullOrWhiteSpace(item))); // controleert of alle items in de collectie aan de opgegeven voorwaarde voldoen (hier: geen lege of witte ruimte).
```

### Numbers
```csharp
Assert.InRange(thingToCheck, lowRange, highRange); // controleert of de waarde binnen het opgegeven bereik ligt.
```

### Exceptions
```csharp
Assert.Throws<T>(() => sut.Method()); // controleert of een specifieke uitzondering wordt opgegooid door de methode.
```

### Types
```csharp
Assert.IsType<T>(thing); // controleert of het object van het verwachte type is.
Assert.IsAssignableFrom<T>(thing); // controleert of het object kan worden toegewezen aan het opgegeven type.
Assert.Same(obj1, obj2); // controleert of beide objecten naar dezelfde instantie verwijzen.
Assert.NotSame(obj1, obj2); // controleert of de objecten niet naar dezelfde instantie verwijzen.
```

<br />

## Fluent-Assertions
Fluent Assertions is een populaire bibliotheek voor het schrijven van unit tests in .NET, die het gemakkelijker maakt om begrijpelijke en leesbare tests te schrijven. Install FluentAssertions NuGet package van dennisdoomen.

### Strings
```csharp
actualString.Should().Be(expectedString); // controleert of de twee strings exact gelijk zijn.
actualString.Should().StartWith(expectedString); // controleert of de string begint met de verwachte waarde.
actualString.Should().EndWith(expectedString); // controleert of de string eindigt met de verwachte waarde.

// optionele parameters kunnen ook worden meegegeven
actualString.Should().Be(expectedString, "omdat we ze willen vergelijken"); // voegt een boodschap toe aan de foutmelding
```

### Collections
```csharp
collection.Should().Contain(expectedThing); // controleert of de collectie het verwachte item bevat.
collection.Should().NotContain(expectedThing); // controleert of de collectie het verwachte item niet bevat.
collection.Should().BeEmpty(); // controleert of de collectie leeg is.
collection.Should().OnlyContain(item => !string.IsNullOrWhiteSpace(item)); // controleert of alle items in de collectie niet leeg of alleen witruimte zijn.
```

### Numbers
```csharp
thingToCheck.Should().BeInRange(lowRange, highRange); // controleert of de waarde binnen het opgegeven bereik ligt.
```

### Exceptions
```csharp
Action act = () => sut.Method();
act.Should().Throw<T>(); // controleert of een specifieke uitzondering wordt opgegooid door de methode.
```

### Types
```csharp
thing.Should().BeOfType<T>(); // controleert of het object van het verwachte type is.
thing.Should().BeAssignableTo<T>(); // controleert of het object kan worden toegewezen aan het opgegeven type.
obj1.Should().BeSameAs(obj2); // controleert of beide objecten naar dezelfde instantie verwijzen.
obj1.Should().NotBeSameAs(obj2); // controleert of de objecten niet naar dezelfde instantie verwijzen.
```

<br />

## Mocking
Heel basic moq stuff dat we hebben gezien zoals een setup en verify en het forceren van een exception dus een methode laten mislukken via de setup. [Officiële Moq documentatie](https://documentation.help/Moq/)
### Mocking-basic-stuff
```csharp
_someInterfaceMock.Setup(d => d.Roll()).Returns(17); // zet een verwachte waarde voor de `Roll` methode.
_someInterfaceMock.Setup(d => d.Roll()).Throws<Exception>(); // configureert de `Roll` methode om een exception te werpen.

```

```csharp
_someInterfaceMock.Verify(d => d.Roll(), Times.Once); // checkt of de methode `Roll` exact 1 keer is uitgevoerd.
_someInterfaceMock.Verify(d => d.Roll(), Times.Never); // checkt of de methode `Roll` nooit is uitgevoerd.
```

<br />

## Zombie

Het "zombie principe" herinnert ontwikkelaars aan belangrijke aspecten van softwareontwikkeling. Hieronder worden de letters van "ZOMBIE" uitgelegd met hun betekenis en voorbeelden.

### Zorg voor onderhoudbaarheid
**Betekenis**: Dit betekent dat je code moet schrijven die gemakkelijk te begrijpen en te wijzigen is. Onderhoudbaarheid is belangrijk omdat software vaak moet worden bijgewerkt of aangepast.

**Voorbeeld**: 
Als je een functie schrijft die gegevens verwerkt, zorg er dan voor dat je duidelijke naamgeving en commentaar gebruikt, zodat een andere ontwikkelaar (of jijzelf later) de functie snel kan begrijpen en wijzigen als dat nodig is.

---

### Oplossingsgerichtheid
**Betekenis**: Dit verwijst naar de aanpak om problemen op te lossen door creatieve en praktische oplossingen te vinden in plaats van je te concentreren op wat er misgaat.

**Voorbeeld**: 
Stel dat je een fout in je code tegenkomt. In plaats van je te frustreren, kun je systematisch debuggen en proberen verschillende oplossingen uit om het probleem op te lossen.

---

### Modulariteit
**Betekenis**: Dit houdt in dat je software moet opdelen in onafhankelijke modules of componenten die afzonderlijk kunnen worden ontwikkeld en getest.

**Voorbeeld**: 
In een webapplicatie kun je verschillende modules hebben voor gebruikersauthenticatie, databasemanagement en de gebruikersinterface, zodat je ze onafhankelijk van elkaar kunt ontwikkelen.

---

### Beveiliging
**Betekenis**: Dit benadrukt het belang van het implementeren van beveiligingsmaatregelen om gegevens en systemen te beschermen tegen ongeautoriseerde toegang of aanvallen.

**Voorbeeld**: 
Bij het ontwikkelen van een applicatie voor het opslaan van persoonlijke gegevens, moet je ervoor zorgen dat je encryptie gebruikt voor gegevens in rust en tijdens verzending.

---

### Integratie
**Betekenis**: Dit betreft de mogelijkheid om verschillende systemen, modules of componenten effectief met elkaar te laten samenwerken.

**Voorbeeld**: 
Wanneer je een microservices-architectuur gebruikt, moet je ervoor zorgen dat elke microservice via API's goed kan communiceren met andere microservices.

---

### Efficiëntie
**Betekenis**: Dit verwijst naar het optimaliseren van code en processen om de snelheid en de middelen te verbeteren die een applicatie gebruikt.

**Voorbeeld**: 
In plaats van een brute-force benadering te gebruiken om een probleem op te lossen, kun je een algoritme gebruiken dat minder tijd en middelen vereist, zoals een zoekalgoritme met een betere tijdcomplexiteit.

---

## Conclusie
Het "zombie principe" herinnert ontwikkelaars eraan om deze belangrijke principes in gedachten te houden bij het schrijven van code en het ontwikkelen van software. Dit zorgt ervoor dat de software niet alleen functioneel is, maar ook onderhoudbaar, veilig en efficiënt.
