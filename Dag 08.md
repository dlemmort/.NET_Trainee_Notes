
## Code review
-  Lookup array
- Data test method
- default switch InvalidEnumArgumentException() => goed idee!, mogelijk wordt datatype toch aangepast.
- `Assert.AreEqual(a,b,0.1)` er mag 0.1 verschil zitten in de uitkomst.

## Inheritance
```C#
public class Animal
{
	public double Weight {get; private set;}
	
	public Animal(double weight)
	{
		Weight = weight;
	}

	public Eat(double food)
	{
		Weight += food;
	}

	public virtual string MakeNoise()
	{
		return "grmpff";
	}
}

public class Bird : Animal
{
	public Bird(double weight) : base(weight)
	{
	}

	public override string MakeNoise()
	{
		return "Tjilp tjilp";
	}

	public string Fly()
	{
		return "I'm flying";
	}
}
```
- Child class krijgt de public methodes van de parent class.
- Een `protected` class is wél `private` maar wordt doorgegeven aan child classes.
- Een `internal` class is alleen zichtbaar binnen hetzelfde project (assembly), dus niet in de test-class, dat is immers een ander project.
	- Om dit toch voor mekaar te krijgen moet er een *AssemblyInfo* in het betreffende project worden aangemaakt met daarin `[assembly: InternalsVisibleTo("Dag08.InheritanceDemo.Test")]`
- `virtual`, mag in de class worden overschreven.
- Bij `virtual` methodes wordt in runtime gekeken van welk type het object is en wordt de meest passende functie gekozen. Dit betekent wel dat `non-virtual` methodes sneller zijn.
- Als we hier een `Animal` aanmaken, kan deze alleen functionaliteit gebruiken die onder `animal` vallen. `Fly` valt onder `Bird` en kan dus niet worden gebruikt.
- Casten werkt hierbij ook, een Animal van het type Bird kan worden gecast naar een `Bird`. Het betreft echter geen casten maar *parent/child conversion*.
- Parent / Child of Base / Derived


```C#
public class BirdTest
{
	private Bird _sut;
	
	[TestInitialize]
	public void TestInitialize()
	{
		_sut = new Bird(0.2);           //Wordt vóór iedere test opnieuw aangemaakt.
	}

	[TestMethod]
	public void Eat_Bird_GainsWeight()
	{
		var seeds = 0.03;

		_sut.Eat(seeds);

		Assert.AreEqual(0.23, _sut.Weight, 0.0001)
	}
}
```


## Abstracte klasse
- Je mag geen instantie meer aanmaken van deze klasse.
- Je mag sommige methodes ook abstract maken, dat betekent dat je in de base-klasse geen functionaliteit meegeef.
	- `public abstract string MakeNoise()`
	- Deze methode MOET je overriden in de derived class.
- De conventie is om alle klasses waarvan nog afgeleid wordt `<abstract>` te maken.  