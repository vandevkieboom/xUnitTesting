# xUnitTesting

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
