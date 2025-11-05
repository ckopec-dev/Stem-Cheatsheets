# Patterns Cheatsheet

## Builder

The Builder Pattern in software design is a creational design pattern used to construct complex objects step-by-step, separating the construction process from the representation of the object.

~~~csharp
using System;

// Immutable Product
public sealed class House
{
    public string Walls { get; }
    public string Roof { get; }
    public int Windows { get; }

    public House(string walls, string roof, int windows)
    {
        Walls = walls;
        Roof = roof;
        Windows = windows;
    }

    public override string ToString()
    {
        return $"House with {Walls} walls, {Roof} roof, and {Windows} windows.";
    }
}

// Immutable Builder
public sealed class HouseBuilder
{
    public string Walls { get; }
    public string Roof { get; }
    public int Windows { get; }

    // Private constructor for internal use
    private HouseBuilder(string walls, string roof, int windows)
    {
        Walls = walls;
        Roof = roof;
        Windows = windows;
    }

    // Default constructor
    public HouseBuilder() : this("Unknown", "Unknown", 0) { }

    public HouseBuilder WithWalls(string walls) =>
        new HouseBuilder(walls, Roof, Windows);

    public HouseBuilder WithRoof(string roof) =>
        new HouseBuilder(Walls, roof, Windows);

    public HouseBuilder WithWindows(int count) =>
        new HouseBuilder(Walls, Roof, count);

    public House Build() =>
        new House(Walls, Roof, Windows);
}

// Usage
class Program
{
    static void Main()
    {
        var builder = new HouseBuilder()
            .WithWalls("Brick")
            .WithRoof("Gable")
            .WithWindows(4);

        House house = builder.Build();

        Console.WriteLine(house);

        // Thread-safe usage example
        var builder1 = builder.WithWalls("Wood");
        var builder2 = builder.WithWalls("Concrete");

        // No shared state is mutated; safe for concurrent builds
        Console.WriteLine(builder1.Build());
        Console.WriteLine(builder2.Build());
    }
}
~~~

## Factory

The Factory Pattern (often called the Factory Method Pattern or Factory Design Pattern) is a creational software design pattern that provides a way to create objects without exposing the creation logic to the client and instead lets subclasses or a dedicated method decide which object to instantiate.

~~~csharp
using System;

// Product interface
public interface IVehicle
{
    void Drive();
}

// Concrete Products
public class SportsCar : IVehicle
{
    public void Drive() => Console.WriteLine("Driving a fast sports car!");
}

public class PickupTruck : IVehicle
{
    public void Drive() => Console.WriteLine("Hauling cargo in a pickup truck!");
}

// Creator (abstract)
public abstract class VehicleFactory
{
    // Factory Method
    public abstract IVehicle CreateVehicle();

    // Common logic for all factories
    public void TestDrive()
    {
        var vehicle = CreateVehicle();
        Console.WriteLine("Test driving vehicle:");
        vehicle.Drive();
    }
}

// Concrete Creators
public class SportsCarFactory : VehicleFactory
{
    public override IVehicle CreateVehicle() => new SportsCar();
}

public class PickupTruckFactory : VehicleFactory
{
    public override IVehicle CreateVehicle() => new PickupTruck();
}

// Client
class Program
{
    static void Main()
    {
        VehicleFactory factory1 = new SportsCarFactory();
        factory1.TestDrive();  // Uses SportsCar

        VehicleFactory factory2 = new PickupTruckFactory();
        factory2.TestDrive();  // Uses PickupTruck
    }
}
~~~

## Singleton

The Singleton Pattern is a creational software design pattern that ensures a class has exactly one instance in the entire application, and it provides a global point of access to that 
instance.

~~~csharp
using System;

public sealed class Logger
{
    // Lazy<T> ensures thread-safe lazy initialization
    private static readonly Lazy<Logger> _instance =
        new Lazy<Logger>(() => new Logger());

    // Private constructor prevents external instantiation
    private Logger() { }

    // Public accessor for the single instance
    public static Logger Instance => _instance.Value;

    // Example method
    public void Log(string message)
    {
        Console.WriteLine($"[{DateTime.Now:HH:mm:ss}] {message}");
    }
}

// Usage
class Program
{
    static void Main()
    {
        Logger.Instance.Log("Application started");
        Logger.Instance.Log("Doing some work...");
        Logger.Instance.Log("Application finished");
    }
}
~~~

## Proxy

The Proxy Pattern is a structural design pattern in software development that provides a surrogate or placeholder object which controls access to another object — usually called the real subject.

~~~csharp
using System;

// Subject interface
public interface IDocument
{
    void Display();
}

// Real Subject
public class ConfidentialDocument : IDocument
{
    private readonly string _title;

    public ConfidentialDocument(string title)
    {
        _title = title;
    }

    public void Display()
    {
        Console.WriteLine($"Displaying confidential document: {_title}");
    }
}

// Proxy
public class DocumentProxy : IDocument
{
    private readonly string _userRole;
    private ConfidentialDocument _realDocument;

    public DocumentProxy(string userRole, string title)
    {
        _userRole = userRole;
        _realDocument = new ConfidentialDocument(title);
    }

    public void Display()
    {
        if (_userRole == "Admin")
        {
            _realDocument.Display();
        }
        else
        {
            Console.WriteLine("Access denied. You do not have permission to view this document.");
        }
    }
}

// Client
class Program
{
    static void Main()
    {
        IDocument adminDoc = new DocumentProxy("Admin", "Company Strategy 2025");
        adminDoc.Display();  // Allowed

        IDocument guestDoc = new DocumentProxy("Guest", "Company Strategy 2025");
        guestDoc.Display();  // Access denied
    }
}
~~~

## Command

The Command Pattern is a behavioral design pattern in software programming that turns a request into a standalone object containing all the information needed to perform an action at a later time.

~~~csharp
using System;

// Command interface
public interface ICommand
{
    void Execute();
    void Undo();
}

// Receiver
public class Light
{
    public void On() => Console.WriteLine("Light is ON");
    public void Off() => Console.WriteLine("Light is OFF");
}

// Concrete Commands
public class LightOnCommand : ICommand
{
    private readonly Light _light;
    public LightOnCommand(Light light) => _light = light;
    public void Execute() => _light.On();
    public void Undo() => _light.Off();
}

public class LightOffCommand : ICommand
{
    private readonly Light _light;
    public LightOffCommand(Light light) => _light = light;
    public void Execute() => _light.Off();
    public void Undo() => _light.On();
}

// Invoker
public class RemoteControl
{
    private ICommand _command;
    public void SetCommand(ICommand command) => _command = command;
    public void PressButton() => _command.Execute();
    public void PressUndo() => _command.Undo();
}

// Client
class Program
{
    static void Main()
    {
        var light = new Light();
        var remote = new RemoteControl();

        remote.SetCommand(new LightOnCommand(light));
        remote.PressButton(); // Light is ON
        remote.PressUndo();   // Light is OFF

        remote.SetCommand(new LightOffCommand(light));
        remote.PressButton(); // Light is OFF
        remote.PressUndo();   // Light is ON
    }
}
~~~

## Publisher/Subscriber

The Publisher–Subscriber Pattern (often shortened to Pub/Sub) is a messaging and communication pattern in software development where senders (publishers) of messages do not send them directly to specific receivers.
Instead, messages are published to a channel or topic, and subscribers that have expressed interest in that channel receive them.

It’s commonly used in event-driven architectures, message brokers, and UI frameworks.

~~~csharp
using System;
using System.Collections.Generic;

// Broker (Event Bus)
public class EventBus
{
    private readonly Dictionary<string, List<Action<string>>> _subscribers = new();

    public void Subscribe(string topic, Action<string> handler)
    {
        if (!_subscribers.ContainsKey(topic))
            _subscribers[topic] = new List<Action<string>>();
        _subscribers[topic].Add(handler);
    }

    public void Publish(string topic, string message)
    {
        if (_subscribers.ContainsKey(topic))
        {
            foreach (var handler in _subscribers[topic])
            {
                handler(message);
            }
        }
    }
}

// Usage
class Program
{
    static void Main()
    {
        var bus = new EventBus();

        // Subscriber 1
        bus.Subscribe("news", msg => Console.WriteLine($"[Subscriber 1] Received news: {msg}"));

        // Subscriber 2
        bus.Subscribe("news", msg => Console.WriteLine($"[Subscriber 2] Got breaking news: {msg}"));

        // Publisher sends message
        bus.Publish("news", "New product launch tomorrow!");
    }
}
~~~
