
## Code review
- Eerste test betreft een test om te zien of de constructor werkt (maakt hij een juist object aan met de juiste waardes?).
- Volgorde in code: eerst de properties, dan de constructor en vervolgens de methodes.
- Voor het schrijven van testen nadenken over getallen: in het geval van 20 - 10 = 10 heb je 2 keer 10, dat is minder makkelijk: wat is wat? Liever dus 10 - 2 = 8.
- Gebruik van TestOnInit is wel mooi, maar denk ook aan de leesbaarheid, aangezien  `//Arrange` in de methode dan niet meer nodig is. Het is dan niet in een oogopslag duidelijk is. 

## Records
- Een reference type (net als class)
- Bedoeld om complexe waardes mee te maken (net als structs)
- Kunnen worden vergeleken (de properties worden dan vergeleken).
- Heeft een andere implementatie van een `.ToString()` class.
- Mag afleiden van een andere record.
```C#
public record Balk{
	public double Hoogte {get;}
	public double Breedte {get;}
	public double Lengte {get;}

	public Balk (double hoogte, double breedte, double lengte)
	{
		Hoogte = hoogte;
		Breedte = breedte;
		Lengte = lengte;
	}
}


//Alternatief
public record Balk (double Hoogte, double Breedte, double Lengte);

Balk balk1 = new Balk(2,3,4);
Balk balk2 = new Balk(2,3,4);
Console.WriteLine(balk1 == balk2)      //true (alle properties zijn gelijk)
```

- Volgend bestand komt dan in z'n geheel in een apart .cs bestand.
```C#
public abstract record Vorm 
{
	public double Diamteter() => this switch        //pattern matching
	{
		Kubus(double z) => Math.Sqrt(3*z*z),
		Balk(doublex, double y, double z) => Math.Sqrt(x*x+y*y+z*z),
		Bol(double r) => 2 * r,
	}
}

public record Balk (double hoogte, double breedte, double diepte) : Vorm
public record Bol (double straal) : Vorm
public record Kubus (double zijde) : Vorm
```

## Collections

### Implementatie van `List<T> class`
- IEnumerable implementeren om m.b.v. foreach door de lijst te lopen.
```C#
public class IntList : IEnumerable
{
	private int[] _items;
	private int _count;
	public int Length {get {return _count;}}

	public IntList()
	{
		_items = new int[4];
		_count = 0;
	}

	// indexer
	public int this[int index]
	{
		get 
		{
			CheckBounds(index);
			return _items[index];}
		set
		{
			CheckBounds(index);
			_items[index] = value;
		}
	}

	public void Add(int item)
	{
		if (_count >= _items.Length)
		{
			Resize(ref _items, _items.Length * 2);
		}
		_items[_count] = item;
		_count++;
	}
	
	private void CheckBounds(int index)
	{
		if (index >= _count || index < 0)
		{
			throw new IndexOutOfRangeException();
		}
		
	}

	private void Resize(ref int[] array, int newSize)
	{
		int[] newArray = new int[newSize];
		for (int index = 0; index < array.Length; index++)
		{
			newArray[index] = array[index];
		}
		array = newArray; 
	}
	public IEnumerator GetEnumerator()     //Alternatief voor onderstaande d.m.v.
	{                                      //yield.
		for (int index = 0; index < _count; index++)
		{
			yield return _items[index];
		}
	}

	public IEnumerator GetEnumerator()     //Implementeren volgens IEnumerable
	{
		return new MyEnumerator(this);
	}

	class MyEnumerator : IEnumerator       //Private klasse binnen andere klasse
	{                                      //daardoor kan deze klasse bij de private
		private IntList _parent;           //variabelen in de list.
		private int _count;
	
		public MyEnumerator(IntList parent)
		{
			_parent = parent;
			_cursor = -1;
		}

		public object Current => _parent._items[_cursor];

		public bool MoveNext()
		{
			_cursor++;
			return _cursor < _parent._count;
		}

		public void Reset()
		{
			_cursor = -1;
		}
	}
}
```




## Generics

```C#
public class List<T> : IEnumerable
{
	private T[] _items
	private int _count;

where T : IComparable<T>        //type constraint
}
```


## .NET Collections

| Veel gebruikt | Weinig gebruikt|
|---|---|
|`List<T>`|`Stack<T>`|
|`Dictionary<K,V>`|`Queue<T>`|
| `IEnumerable<T>`|`LinkedList<T>`|
||`SortedList<K,V>`|
|`int[]`, `string[]`...|`SortedSet<T>`|
|    |  |
|`HashSet<T>`|`BlockingCollection<T>`|
||`ConcurrentStack<T>`| 

