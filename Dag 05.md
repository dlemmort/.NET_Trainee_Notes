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
//AUTO KLASSE
public class Auto
{
	public string Kleur;        //field, member, instance variable in C#
	public string Kenteken;
	public string Merk;
	public double _snelheid;    //private variabelen beginnen met underscore
								//kleine letter

	public Auto (string kenteken, string kleur)
	{      
		if (kenteken == null)
		{
			throw new ArgumentNullException(nameof(kenteken), "Kenteken is verplicht");
		}                     
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
			throw new ArgumentOutOfRangeException(nameof(nieuweSnelheid), 
									"De snelheid moet tussen 0 en 300 liggen");
		}
	//Snelheid = Math.Min(0, (Math.Max(nieuweSnelheid, 300)));
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
		Assert.AreEqual(ex.Message.StartsWith("De snelheid moet tussen 0 en 300 liggen"));
	}
	
}
```

```C#
//CONSOLE APP
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



### Getter / Setter

```C#
public class Auto
{
	public string Kleur;        //field, member, instance variable in C#
	public string Kenteken;
	public string Merk;
	public double _snelheid;    //private variabelen beginnen met underscore
								//kleine letter

	public Auto (string kenteken, string kleur)
	{                           
		Kenteken = kenteken;
		Kleur = kleur;
		Snelheid = 0.0;         //default waarde schrijven we toch op:
		                        //leesbaarheid, en laten zien dat het bewust is.
	}

public double Snelheid          // Nu kunnen we auto.Snelheid; aanroepen ip
{                               // auto.GetSnelheid();
	get {return _snelheid;}
	set
	{
		if (0 <= value && value <= 300) // LET OP: value hier ipv variabele -
		{                               // naam
			_snelheid = value;
		}
		else
		{
			throw new ArgumentOutOfRangeException(nameof(nieuweSnelheid), 
									"De snelheid moet tussen 0 en 300 liggen");
		}
	}
}
```


### Korte Getter / Setter
```C#
public class Auto
{
	public string Kleur {get; private set;};        
	public string Kenteken {get;}
	public string Merk {get; set;}
	public double _snelheid;

	....
}
```

- Onder water maakt de compiler hier een private variable van met de 2 methodes Get en Set.
- Kenteken kan nu ook in de klasse (behalve de constructor) niet meer worden gewijzigd, maar wél worden opgevraagd.
- Kleur kan nu binnen de klasse wel worden gewijzigd, daarbuiten niet. Hij kan daarbuiten wel worden opgevraagd.


### Gebruik meerdere constructors
```C#
public class Auto
{
	public string Kleur;        //field, member, instance variable in C#
	public string Kenteken;
	public string Merk;
	public double _snelheid;    //private variabelen beginnen met underscore
								//kleine letter

	
	public Auto (string kenteken) : this(kenteken, "saai grijs")
	{      
		/// hier mogelijk
		/// extra code
		/// toevoegen
	}

	//Oplossing 2: default waarde meegeven
	public Auto (string kenteken, string kleur = "saai grijs")
	{      
		if (kenteken == null)
		{
			throw new ArgumentNullException(nameof(kenteken), "Kenteken is verplicht");
		}                     
		Kenteken = kenteken;
		Kleur = kleur;
		Snelheid = 0.0;         //default waarde schrijven we toch op:
		                        //leesbaarheid, en laten zien dat het bewust is.
	}

	public void Overspuiten(string Kleur)
	{
		this.Kleur = Kleur;     //this.Kleur verwijst naar klasse variabele
		                        //Kleur verwijst naar methode variabele
	}
```

## Opdracht 

Object regel maken met:
- aantal
- naam
- stuksprijs
- prijs (totaal)

Object bon maken met:
- Array met regels (niet zichtbaar)
- Totaalprijs
- Methode: `Scan(string naam, decimal stuksprijs)`
	- als er eenzelfde object wordt gescand wordt er aan de regel 1 toegevoegd
- Methode: .ToString() print de gehele bon uit.

## Static
- Staat los van een instantie
- Gebruiken om functies aan te maken zonder specifieke eigenschappen van een bepaalde klasse.
- Gebruiken om bijvoorbeeld ID aan te maken:
```C#
public class Auto
{
	public static Long ID = 100;
	private long _id;
	

	public Auto()
	{
		_id = ID + 1;
	}

	public static bool HebbenDezelfdeKleur(Auto a, Auto b)
	{
		return a.Kleur == b.Kleur;
	}
}
```

## Interface
- Je kunt van meerdere interfaces overerven, maar van maar 1 klasse.
- Je kunt een lijst maken van objecten die een bepaalde interface implementeren.