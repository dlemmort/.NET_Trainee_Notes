
## Code Review
- In Unit tests liever het antwoord berekenen dan een formule gebruiken.
	- We kunnen daarvoor bijvoorbeeld kijken of de uitkomst tussen 2 waardes ligt:
```C#
Assert.IsTrue(523 <= inhoudBol && inhoudBol <= 524)
```

- Tests kun je ook debuggen, en er breakpoints in zetten. Dit doe je in de test explorer voor debug te kiezen.

```C#
public decimal Prijs
{
	get {return _stukprijs * _aantal}
}

//Is gelijk aan:
public decimal Prijs => _stukprijs * aantal; //Computed Property
```

## CTS

- CTS staat voor 'Common Type System'
- Het is een *afsprakensysteem* hoe bepaalde types eruit zien onder water.
- Is het typesysteem voor alle .NET talen
	- dit geldt voor data-types maar ook voor klassen
	- onder water in MSIL-code is een VB.NET integer gelijk aan een C# integer
```C#
System.Int32 a = 4;     //CTS
int a = 4;              //keyword in C#, deze types zijn exact gelijk 

String s = "Hello"      //CTS
string s = "Hello"      //keyword in C#, deze types zijn exact gelijk 
```

>[ILSPY](https://apps.microsoft.com/store/detail/ilspy/9MXFBKFVSQ13?hl=nl-nl&gl=nl&rtc=1) is een gratis tooltje om IL code te lezen

|Value Types|Reference Types|
|---|---|
| - Enum | - Class|
|- Struct|- Record|
|- int, bool, double, decimal, long, char short|- Interface|
|- DateTime, Point, int?|- Delegate|
||- string, array, list, string?|
|- Vaak op stack|- Altijd op heap|
|- Assignment by Value|- Assignment by reference|
|- Operations on one cannot affect others|- Operations on one may affect others|




## enum
- In visual studio een klasse aanmaken:
```C#
public enum Weather
{
	Sunny,
	Cloudy,
	Rainy
}

static void Main(string[] args)
{
	Weather w = Weather.Sunny;        //Alleen keuze uit de opties uit de enum
	int weergetal = (int) weather;    //We kunnen van een enum een integer maken
	                                  //dat wordt dan de index.
}
```

- Flags
```C#
[Flags]
public enum FileAccess
{
	Read = 1,
	Write = 2
}
static void Main(string[] args)
{
	FileAccess x = FileAccess.Read | FileAccess.Write;
	Console.WriteLine(x);        //Read, Write
}

```


## Extension Method
- Is een static method in een static class
- de eerste parameter heeft het *this*-keyword
- Breidt een bestaande enum / klasse uit.
- Het wordt wel vrij ingewikkeld om bij te houden.

```C#
public static string JasAdvies(this Weather weather) => weather switch
{
	Weather.Sunny => "geen jas",
	Weather.Cloudy => "jas",
	Weather.Rainy => "regenjas",
	Weather.Snowy => "winterjas",
}

string jasAdvies = weather.JasAdvies
```

```C# 
public static class IntExtensions
{
	public static string AddOne(this int number)
	{
		return number++;
	}
}

int getal = 8;
int nieuwGetal = getal.AddOne(getal);    //De integer heeft nieuwe functionaliteit
```



## Casting & Boxing
```C#
int i = 3;
long l = i;        //Dit mag (implicit casting), een integer past altijd in een long

long l1 = 13;
int i1 = l1;       //Dit mag niet 

long l2 = 26;
int i2 = (int)l2;  //Dit kan wel, rekening houden met mogelijke overflow, explicit cast

long grootGetal = 1L + int.MaxValue; 
int i3 = (int) grootGetal    //Dit kan wel, value wordt -2147483647, hij begint 
                             //dus gewoon weer van voor af aan.
```

```C#
checked                        //Nu treed er wel een overflow exception op
{
	int x = int.MaxValue;
	Console.WriteLine($"{x}")
	x++;
	Console.WriteLine($"{x}")
}
```

### Boxing
- Er wordt een kopie gemaakt van de value op de heap, waar het object naar verwijst.
```C#
object obj = i;        //boxing
```

### Unboxing
```C#
int i = 3;
object obj = i;
int j = (int)obj;      //Er wordt een kopie van de integer op de heap op de stack
                       //geplaatst.
```

## structs
- Betreft een value type die je zelf kun definieren. 
- Ze kunnen hetzelfde als wat een klasse kan.
- Omdat het een value type is staat hij geheel op de stack, daardoor is het sneller dan een klasse.
```C#
public struct Time
{
	public int Hours {get; }
	public int Minutes {get;}

	public Time (int hours, int minutes)
	{
		Hours = hours + minutes / 60;
		Minutes = minutes % 60;
	}

	public Time AddMinutes(int minutes)
	{
		Time newTime = new Time(Hours, Minutes + minutes)
	}
}

Time t = new Time(3,15);
```


## Operator overloading
- Gebruiken van de bestaande operators voor eigen klasses en structs
``` C#
public static Time operator+ (Time a, Time b)    //Overload + operator
{
	return new Time (a.hours + b.hours, a.minutes + b.minutes);
}

public override string ToString()    //Overload to String (bijv in console writeline)
{
	return $"{Hours} : {Minutes} uur";
}

public static explicit operator Time (double timeValue)  //Overload cast operator
{
	int hours = (int)timeValue;
	int minutes = (int)((timevalue - hours) * 100)
	return new Time(hours, minutes);
}

public static bool operator == (Time a, Time b)     //Dit kan maar dan ook de != 
{                                                  //implementeren
	return a.Hours == b.Hours && a.Minutes == b.Minutes;
}

public static bool operator != (Time a, Time b)
{
	return !(a==b);
}

public override bool Equals(object? obj)          //Moet worden geimplenteerd voor ==
{
	return obj !+ null &&
	       obj.GetType() == typeof(TIme) &&
	       this == (Time)obj;
	       
	//Korter alternatief:
	return obj is Time &&
	       this == (Time)obj;
	       
	//Nog korter alternatief
	return (obj is Time that) &&
	       this == that;
}

public override int GetHashCode()                  //Moet worden geimplenteerd voor ==
{
	return Hours * 60 + Minutes
}
```

>Gebruik van `#region` en `#endregion` om code leesbaar te houden (kan dan ingeklapt worden).




