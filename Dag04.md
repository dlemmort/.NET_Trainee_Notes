`CultureInfo.CurrentCulture = CultureInfo.InvariantCulture` 
[[Dag 04]]

## Arrays
- Een array is een verzameling van elementen
- Alle elementen hebben hetzelfde type
- Betreft een reference-type
- Elements are initialized to 0/null

```C#
int[] nummers = new int[4];   //Lege array met ruimte voor 4
int[] nummers = {0,1,2,3};    //gevulde int array
```

- met foreach kun je door de lijst heen, heb je de index nodig? Dan beter de for loop gebruiken.
- met foreach kun je de variabele niet aanpassen:

```C#
foreach (int nummer in nummers)
{
	nummer++;   //niet toegestaan
}
```

```C#
int[] myTable
int[5] [] myTable1;   //Array met daarin 5 pointers naar andere arrays
int[5,3] myTable2;    //2D array met 5 kolommen en 3 rijen
```

- Een 2d array is sneller dan een array van arrays, een array van arrays kan arrays van een verschillende grootte bevatten.

`long thirdElementFromTheRight = row[^3];`

```C#
int[] nummers = new int[] {2, 3, 5, 7};
int[] drieEnVrijf = nummers[1..3]; //Array uit een array (tot)
```

### Oefeningen
Blauw:   
- array met de getallen 2, 3, 5, 7, 11, 13, 17
	- afdrukken
	- som van alle getallen afdrukken

- methode die het grootste getal uit een array teruggeeft

Rood:
- Maak een "array" met alle tafeluitkomsten


## Unit testen

- Nieuwe project MSTest Test project

```C#
namespace Dag04.ArrayHellperDemo.Test

	[TestClass]
	public class ArrayHelperTest

		[TestMethod]
		public void TestMethod1()
		{
			//Arrange
			int[] legeArray = new int[0];
			ArrayHelper arrayHelper = new ArrayHelper();

			//Act
			int som = arrayHelper.SomVanElementen(legeArray);

			//Assert
			Assert.AreEqual(0,som);
		}
```

In dependencies van het test project referentie aanmaken

Test Driven Development (namespaces, projects, references...)


## Strings

- String is een lijst van karakters 

```C#
string s = "Hello";
var firstLetter = s[0];
string t = s;

s = s + " World";

Console.WriteLine(s); //Hello World (pointer naar kopie van hello en kopie van world)

Console.WriteLine(t); //Hello (pointer naar kopie van hello)
```

