# Лаба 4. Поведенческие паттерны проектирования

## Strategy (стратегия)
```C#
public interface ISortingStrategy
{
    void Sort(int[] array);
}

public class BubbleSortStrategy : ISortingStrategy
{
    public void Sort(int[] array)
    {
        Console.WriteLine("Sorting using Bubble Sort");
    
    }
}

public class QuickSortStrategy : ISortingStrategy
{
    public void Sort(int[] array)
    {
        Console.WriteLine("Sorting using Quick Sort");

    }
}

public class Sorter
{
    private ISortingStrategy strategy;

    public void SetStrategy(ISortingStrategy strategy)
    {
        this.strategy = strategy;
    }

    public void SortArray(int[] array)
    {
        this.strategy.Sort(array);
    }
}

public static void Main(string[] args)
{
    Sorter sorter = new Sorter();

    sorter.SetStrategy(new BubbleSortStrategy());
    int[] array1 = { 5, 3, 8, 4, 2 };
    sorter.SortArray(array1);

    sorter.SetStrategy(new QuickSortStrategy());
    int[] array2 = { 5, 3, 8, 4, 2 };
    sorter.SortArray(array2);
}
```


## Responsibility chain (цепочка обязанностей) 
```C#
public interface IHandler
{
    void HandleRequest(Request request);
    void SetNextHandler(IHandler nextHandler);
}

public class ConcreteHandlerA : IHandler
{
    private IHandler nextHandler;

    public void HandleRequest(Request request)
    {
        if (request.Type == RequestType.TypeA)
        {
            Console.WriteLine("ConcreteHandlerA handled the request.");
        }
        else if (nextHandler != null)
        {
            this.nextHandler.HandleRequest(request);
        }
    }

    public void SetNextHandler(IHandler nextHandler)
    {
        this.nextHandler = nextHandler;
    }
}

public class ConcreteHandlerB : IHandler
{
    private IHandler nextHandler;

    public void HandleRequest(Request request)
    {
        if (request.Type == RequestType.TypeB)
        {
            Console.WriteLine("ConcreteHandlerB handled the request.");
        }
        else if (_nextHandler != null)
        {
            this.nextHandler.HandleRequest(request);
        }
    }

    public void SetNextHandler(IHandler nextHandler)
    {
        this.nextHandler = nextHandler;
    }
}

public class Request
{
    public RequestType Type { get; }

    public Request(RequestType type)
    {
        this.Type = type;
    }
}

public enum RequestType
{
    TypeA,
    TypeB
}

public static void Main(string[] args)
{
    IHandler handlerA = new ConcreteHandlerA();
    IHandler handlerB = new ConcreteHandlerB();

    handlerA.SetNextHandler(handlerB);

    Request requestA = new Request(RequestType.TypeA);
    Request requestB = new Request(RequestType.TypeB);

    handlerA.HandleRequest(requestA); 
    handlerA.HandleRequest(requestB); 
}
```


## Iterator (итератор)
```C#
public interface IIterator<T>
{
    bool HasNext();
    T Next();
}

public class ArrayIterator<T> : IIterator<T>
{
    private readonly T[] items;
    private int position;

    public ArrayIterator(T[] items)
    {
        this.items = items;
        this.position = 0;
    }

    public bool HasNext()
    {
        return this.position < this.items.Length;
    }

    public T Next()
    {
        if (this.HasNext())
        {
            return this.items[this.position++];
        }
        throw new IndexOutOfRangeException("No more elements in the collection.");
    }
}

public static void Main(string[] args)
{
    char[] items = { 'a', 'b', 'c' };
    var iterator = new ArrayIterator<string>(items);

    while (iterator.HasNext())
    {
        Console.WriteLine(iterator.Next());
    }
}
```
