# 🧬 C# Inheritance & Polymorphism Tutorial

## 1. What Is Inheritance?

**Inheritance** lets one class reuse and extend another class.

* The base class = **parent**
* The derived class = **child**

### Why use it?

* Avoid code duplication
* Model real-world relationships
* Make code easier to maintain

---

## 2. Basic Inheritance Syntax

```csharp
class Animal
{
    public void Eat()
    {
        Console.WriteLine("Eating...");
    }
}

class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Barking...");
    }
}
```

### Usage

```csharp
Dog dog = new Dog();
dog.Eat();   // inherited
dog.Bark();  // own method
```

---

## 3. Constructors in Inheritance

Base constructors run first.

```csharp
class Animal
{
    public Animal()
    {
        Console.WriteLine("Animal created");
    }
}

class Dog : Animal
{
    public Dog()
    {
        Console.WriteLine("Dog created");
    }
}
```

### Output:

```
Animal created
Dog created
```

---

## 4. Access Modifiers and Inheritance

| Modifier    | Accessible in Derived Class |
| ----------- | --------------------------- |
| `public`    | ✅ Yes                       |
| `protected` | ✅ Yes                       |
| `private`   | ❌ No                        |

### Example

```csharp
class Animal
{
    protected string name = "Unknown";
}

class Dog : Animal
{
    public void ShowName()
    {
        Console.WriteLine(name);
    }
}
```

---

## 5. Method Overriding (Runtime Polymorphism)

To override behavior:

* Base method must be `virtual`
* Derived method must use `override`

```csharp
class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal makes a sound");
    }
}

class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Dog barks");
    }
}
```

---

## 6. Polymorphism Explained

**Polymorphism = “many forms”**

Same method call → different behavior depending on object type.

### Example

```csharp
Animal a = new Dog();
a.Speak();  // Dog barks
```

Even though `a` is typed as `Animal`, it behaves like `Dog`.

---

## 7. Virtual vs Override vs New

### `virtual`

Allows overriding

### `override`

Replaces base implementation

### `new`

Hides base method (not recommended unless intentional)

```csharp
class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal sound");
    }
}

class Cat : Animal
{
    public new void Speak()
    {
        Console.WriteLine("Cat meows");
    }
}
```

⚠️ This is **method hiding**, not true polymorphism.

---

## 8. Abstract Classes

Abstract classes define a base but cannot be instantiated.

```csharp
abstract class Animal
{
    public abstract void Speak();
}
```

Derived class must implement:

```csharp
class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Dog barks");
    }
}
```

---

## 9. Interfaces (Polymorphism Alternative)

Interfaces define contracts.

```csharp
interface IAnimal
{
    void Speak();
}

class Dog : IAnimal
{
    public void Speak()
    {
        Console.WriteLine("Dog barks");
    }
}
```

---

## 10. Real-World Example

```csharp
class Shape
{
    public virtual double Area()
    {
        return 0;
    }
}

class Circle : Shape
{
    public double Radius { get; set; }

    public override double Area()
    {
        return Math.PI * Radius * Radius;
    }
}

class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public override double Area()
    {
        return Width * Height;
    }
}
```

### Polymorphic usage:

```csharp
List<Shape> shapes = new List<Shape>
{
    new Circle { Radius = 2 },
    new Rectangle { Width = 3, Height = 4 }
};

foreach (var shape in shapes)
{
    Console.WriteLine(shape.Area());
}
```

---

## 11. Key Concepts Recap

* **Inheritance** → reuse and extend behavior
* **Polymorphism** → same interface, different behavior
* **virtual/override** → runtime polymorphism
* **abstract** → force implementation
* **interface** → contract-based polymorphism

---

## 12. Common Pitfalls

* Forgetting `virtual` → cannot override
* Using `new` instead of `override`
* Overusing inheritance instead of composition
* Violating “is-a” relationship (Dog *is an* Animal, but Engine is not a Car)

---

## 13. Practice Exercise

Try this:

1. Create a base class `Vehicle`
2. Add method `Drive()`
3. Create derived classes:

   * `Car`
   * `Bike`
4. Override `Drive()` in each
5. Store them in a `List<Vehicle>`
6. Loop and call `Drive()`


