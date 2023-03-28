## Daily scrum
- Meestal List gebruiken, soms willen we wel een array gebruiken, bijvoorbeeld als we naar buiten communiceren in een functie/methode.


## Code review
- Testprojecten eindigen met .test
- Bitwise not operator `~`
	- 6       00000110
	- ~6    11111001

```C#
for (int i = _count; i >= 0; i--)
{
	if (item.CompareTo(_items[i-1]) < 0) //item is kleiner dan item op vorige positie
	{
		_items[i] = _items[i-1]
	}
	else
	{
		_items[i] = item;
		break;
	}
}
```


## Delegates
```C#

public class Lightbulb
{
	public void Burn(bool on)
	{
		Console.WriteLine($"Lightbulb is {(on?"ON":"OFF")}");
	}
}

public class TubeLight
{
	public void Ignite(bool on)
	{
		Console.WriteLine($"Tubelight is {(on?"ON":"OFF")}");
	}
}

public delegate void Wire(bool b);    //Signature functie: void return en bool parameter

public class Switch
{
	public Wire wire;
	private bool _isOn = false;
	public void SwitchMe()
	{
		_isOn != !_isOn;
		wire.Invoke(_isOn);
	}
}

internal class Electricien
{
	static void Main(string[] args)
	{
		Lightbulb peertje = new Lightbulb();
		Wire draadje = new Wire(peertje.Burn);
		Switch schakelaar = new Switch();
		Tubelight tl = new Tubelight();
		Wire draadjeTL = new Wire(tl.Ignite); //Geen haakjes, dan voer je de functie uit
		Wire totaalDraad = draadje + draadjeTl; //Draadjes samengevoegd tot 1 draad

		schakelaar.wire = totaalDraad; //Samengevoegde draad doorgeven aan schakelaar

		Lightbulb peertje2 = new Lightbulb();
		Wire draadje2 = peertje2.Burn;   //new Wire mag hier worden weggelaten.

		schakelaar.wire += peertje2.Burn; //Vorige regel kan nu worden weggelaten!

		Console.WriteLine("Let's turn ON the lights");
		schakelaar.SwitchMe();
			//Lightbulb is ON
			//Tubelight is ON



		foreach (Wire w in schakelaar.wire.GetInvocationList())
		{
			if (w.Target is Tubelight)
			{
				w.Invoke(false);
				w.Invoke(true);
			}
		}

		Console.WriteLine("Let's turn OFF the lights");
		schakelaar.SwitchMe();
			//Lightbulb is OFF
			//Tubelight is OFF
	}
}
```

- Delegate is een 'koffertje' waar een functie in zit.
- In een delegate passen alleen een 'bepaald soort functies'
- Functies:
	- `.Invoke()`
	- `.Target()`
	- `.Method()`
- In het geval van een Multicast delegate moet de methode void terug geven. Geeft deze bijvoorbeeld string terug, dan geeft hij meerdere strings terug en daardoor pakt hij alleen de laatste.
- Bij opdeling in klassen komt een Delegate in een apart bestandje.



## Events
```C#

public class Lightbulb
{
	public void Burn(bool on)
	{
		Console.WriteLine($"Lightbulb is {(on?"ON":"OFF")}");
	}
}

public class TubeLight
{
	public void Ignite(bool on)
	{
		Console.WriteLine($"Tubelight is {(on?"ON":"OFF")}");
	}
}

public delegate void Wire(bool b);    //Signature functie: void return en bool parameter

public class Switch
{
	public Wire wire;
	private bool _isOn = false;
	public void SwitchMe()
	{
		_isOn != !_isOn;
		wire.Invoke(_isOn);
	}
}

internal class EventDemo
{
	static void Main(string[] args)
	{
		Lightbulb peertje = new Lightbulb();
		Wire draadje = new Wire(peertje.Burn);
		Switch schakelaar = new Switch();
		Tubelight tl = new Tubelight();
		Wire draadjeTL = new Wire(tl.Ignite); //Geen haakjes, dan voer je de functie uit
		Wire totaalDraad = draadje + draadjeTl; //Draadjes samengevoegd tot 1 draad

		schakelaar.wire = totaalDraad; //Samengevoegde draad doorgeven aan schakelaar

		Lightbulb peertje2 = new Lightbulb();
		Wire draadje2 = peertje2.Burn;   //new Wire mag hier worden weggelaten.

		schakelaar.wire += peertje2.Burn; //Vorige regel kan nu worden weggelaten!

		Console.WriteLine("Let's turn ON the lights");
		schakelaar.SwitchMe();
			//Lightbulb is ON
			//Tubelight is ON



		foreach (Wire w in schakelaar.wire.GetInvocationList())
		{
			if (w.Target is Tubelight)
			{
				w.Invoke(false);
				w.Invoke(true);
			}
		}

		Console.WriteLine("Let's turn OFF the lights");
		schakelaar.SwitchMe();
			//Lightbulb is OFF
			//Tubelight is OFF
	}
}