# Patterns Cheatsheet

## Builder

The Builder Pattern in software design is a creational design pattern used to construct complex objects step-by-step, separating the construction process from the representation of the object.

~~~
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

~~~
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
