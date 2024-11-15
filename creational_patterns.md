# Лаба 3. Порождающие паттерны проектирования

## Singleton (одиночка) 
```C#
public class Singleton
{
    private static Singleton instance;
    
    private Singleton() {
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    } 
}

public static void Main(string[] args)
{
    Singleton firstInstance = Singleton.getInstance();
    Singleton secondInstance = Singleton.getInstance();

    Console.WriteLine(ReferenceEquals(firstInstance, secondInstance) ? "OK" : "WR");

    Console.WriteLine($"1: {firstInstance.GetHashCode()}");
    Console.WriteLine($"2: {secondInstance.GetHashCode()}");
}
```


## Factory method (Фабричный метод) 
```C#
public interface ILogger
{
    void Log(string message);
}

public class FileLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine("Logging to file: " + message);
    }
}

public class ConsoleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine("Logging to console: " + message);
    }
}

public abstract class LoggerFactory
{
    public abstract ILogger CreateLogger();

    public void LogMessage(string message)
    {
        ILogger logger = CreateLogger();
        logger.Log(message);
    }
}

public class FileLoggerFactory : LoggerFactory
{
    public override ILogger CreateLogger()
    {
        return new FileLogger();
    }
}

public class ConsoleLoggerFactory : LoggerFactory
{
    public override ILogger CreateLogger()
    {
        return new ConsoleLogger();
    }
}

public static void Main(string[] args)
{
    LoggerFactory fileLoggerFactory = new FileLoggerFactory();
    Console.WriteLine("FileLoggerFactory:");
    fileLoggerFactory.LogMessage("FileLoggerMessage");

    Console.WriteLine();

    LoggerFactory consoleLoggerFactory = new ConsoleLoggerFactory();
    Console.WriteLine("ConsoleLoggerFactory:");
    consoleLoggerFactory.LogMessage("ConsoleLoggerMessage");
}
```


## Abstract factory (Абстрактная фабрика) 
```C#
public interface IButton
{
    void Paint();
}

public class WindowsButton : IButton
{
    public void Paint()
    {
        Console.WriteLine("You have created a Windows button.");
    }
}

public class MacButton : IButton
{
    public void Paint()
    {
        Console.WriteLine("You have created a Mac button.");
    }
}

public interface ICheckbox
{
    void Paint();
}

public class WindowsCheckbox : ICheckbox
{
    public void Paint()
    {
        Console.WriteLine("You have created a Windows checkbox.");
    }
}

public class MacCheckbox : ICheckbox
{
    public void Paint()
    {
        Console.WriteLine("You have created a Mac checkbox.");
    }
}

public interface GUIFactory
{
    IButton CreateButton();
    ICheckbox CreateCheckbox();
}

public class WindowsFactory : GUIFactory
{
    public IButton CreateButton()
    {
        return new WindowsButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new WindowsCheckbox();
    }
}

public class MacFactory : GUIFactory
{
    public IButton CreateButton()
    {
        return new MacButton();
    }

    public ICheckbox CreateCheckbox()
    {
        return new MacCheckbox();
    }
}

public class Application
{
    private readonly IButton button;
    private readonly ICheckbox checkbox;

    public Application(GUIFactory factory)
    {
        button = factory.CreateButton();
        checkbox = factory.CreateCheckbox();
    }

    public void Paint()
    {
        button.Paint();
        checkbox.Paint();
    }
}

public static void Main(string[] args)
{
    Application app1 = new Application(new WindowsFactory());
    app1.Paint();

    Console.WriteLine();

    Application app2 = new Application(new MacFactory());
    app2.Paint();
}
```


## Builder (Строитель)
```C#
public class Pizza
{
    private string dough;
    private string sauce;
    private string topping;

    public void SetDough(string dough)
    {
        this.dough = dough;
    }

    public void SetSauce(string sauce)
    {
        this.sauce = sauce;
    }

    public void SetTopping(string topping)
    {
        this.topping = topping;
    }

    public override string ToString()
    {
        return $"Pizza{{dough='{dough}', sauce='{sauce}', topping='{topping}'}}";
    }
}

public interface IPizzaBuilder
{
    void BuildDough();
    void BuildSauce();
    void BuildTopping();
    Pizza GetResult();
}

public class HawaiianPizzaBuilder : IPizzaBuilder
{
    private Pizza pizza;

    public HawaiianPizzaBuilder()
    {
        pizza = new Pizza();
    }

    public void BuildDough()
    {
        pizza.SetDough("cross");
    }

    public void BuildSauce()
    {
        pizza.SetSauce("mild");
    }

    public void BuildTopping()
    {
        pizza.SetTopping("ham+pineapple");
    }

    public Pizza GetResult()
    {
        return pizza;
    }
}

public class PizzaDirector
{
    private readonly IPizzaBuilder builder;

    public PizzaDirector(IPizzaBuilder builder)
    {
        this.builder = builder;
    }

    public void ConstructPizza()
    {
        builder.BuildDough();
        builder.BuildSauce();
        builder.BuildTopping();
    }
}

public static void Main(string[] args)
{
    IPizzaBuilder builder = new HawaiianPizzaBuilder();
    PizzaDirector director = new PizzaDirector(builder);

    director.ConstructPizza();
    Pizza pizza = builder.GetResult();

    Console.WriteLine(pizza);
}
```



