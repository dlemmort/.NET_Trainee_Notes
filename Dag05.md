- String **lijkt** op een character array, maar is niet hetzelfde. String.Reverse geeft geen string terug maar een IEnumerable van characters, en is daarom ook niet te vergelijken.

- functie met <> betreft een extension method.

- DLL => dynamic link library: soort van bibliotheek met allerlei functies die in de applicatie gebruikt kunnen worden.

- Ieder project wordt een .dll

- Per klasse 1 testbestand

## Code review

```C#
string woord = "water";
string nieuwWoord = "";

foreach (char letter in woord)
{
	nieuwWoord += letter switch
	{
		'a' => 'e',
		'e' => 'i',
		_ => letter,
	};
}
```

- Regex


## Objectgeorienteerd programmeren

- `new` roept de constructor van een klasse aan, reserveert het benodigde geheugen op de heap, en het geheugen wordt 'leeg geveegd' (op 0 gezet). Op de stack komt een pointer naar het object op de heap.

- private gebruiken om code te verbergen, vooral om leesbaarheid richting de 'buitenwereld'
	- zorgt er ook voor dat instance variables buiten de klasse niet meer zomaar bekeken en gewijzigd mogen worden. 

- aanmaken constructor:
	- ctor tab tab 
	- ctrl + . en vervolgens generate constructor nog eenvoudiger

- Doordat er op de heap allemaal 0 worden neergezet, zijn de defaultwaardes van de instance reference variables `null`, integers zijn 0, doubles 0.0 en booleans `false`.

- In de constructor zetten we de parameters die we nodig hebben om het object aan te maken.

```C#
public class Auto
{
	public string Kleur;        //field, member, instance variable in C#
	public string Kenteken;
	public string Merk;
	public double Snelheid;

	public Auto (string kenteken, string kleur)
	{                           
		Kenteken = kenteken;
		Kleur = kleur;
		Snelheid = 0.0;         //default waarde schrijven we toch op:
		                        //leesbaarheid, en laten zien dat het bewust is.
	}

	public double GetSnelheid()
	{
		return Snelheid;
	}

	public void SetSnelheid(double nieuweSnelheid)
	{
		if (0 <= nieuweSnelheid && nieuweSnelheid <= 300)
		{
			Snelheid = nieuweSnelheid;
		}
		else
		{
			
		}
	}

	public void GasGeven()
	{
		Snelheid++;
	}

	public void Remmen()
	{
		Snelheid--;
	}
}
```

```C#
// Testmethode met exceptiontest
[TestClass]
public class AutoTest
{
	[TestMethod]
	public void SetSnelheid_ZetOp50_GeeftSnelheid50()
	{
		//arrange
		Auto auto = new Auto("XX-12-XX", "Geel");

		//act
		auto.SetSnelheid(50);

		//assert
		Assert.AreEqual(50, auto.GetSnelHeid());
	}
	
	[TestMethod]
	public void SetSnelheid_ZetOp350_GeeftGeeftArgumentAutOfRange()
	{
		//arrange
		Auto auto = new Auto("XX-12-XX", "Geel");

		//act
		Action act = () =>
		{
			auto.SetSnelheid(350); //niet uitvoeren want als er een
		};                         //exceptie komt wordt de test niet uitgevoerd

        //ALTERNATIEF
        void act()
        {
	        auto.SetSnelheid(350);
        }

		//assert
		var ex = Assert.ThrowsException<ArgumentOutOfRangeException>(act);
		          //Doe dus net alsof je het uitvoert om te kijken of er dan
		          //een exceptie komt
		Assert.AreEqual("De snelheid moet tussen 0 en 300 liggen", ex.Message);
	}
	
}
```

```C#
static void Main(string[] args)
{
	Auto auto 1 = new Auto("5-SPT-20", "Blauw");
	auto1.Merk = "Skoda";

	Auto auto3 = new Auto("5-SPT-20", "Blauw") { Merk = "Skoda"};

	Auto raceAuto = new Auto("XX-12-XX", "Geel");
	raceAuto.GasGeven();
	Console.WriteLine($"Snelheid is: {raceAuto.Snelheid}")
	
}
```

Bij een constructor zonder parameters kun je de haakjes zelfs weglaten:
```C#
Persoon p = new Persoon { Naam = "Daan", Leeftijd = 34};
```

- Class Invariant: iets wat altijd waar is voor objecten
	- Snelheid is altijd tussen de 0 en 300
	- Kenten of kleur is altijd ingevuld
