
# 🧱 1. Classes vs Structs — The Core Idea

In C#, both **classes** and **structs** define custom types, but they behave very differently under the hood.

### Classes

* **Reference types**
* Stored on the **heap**
* Variables hold a **reference (pointer)**

### Structs

* **Value types**
* Stored **inline** (stack or inside another object)
* Variables hold the **actual data**

---

# 🧠 2. Mental Model (Important)

```csharp
class Person { public string Name; }

struct Point { public int X, Y; }
```

### Class behavior:

```csharp
var p1 = new Person { Name = "Alice" };
var p2 = p1;

p2.Name = "Bob";

Console.WriteLine(p1.Name); // Bob (same object!)
```

### Struct behavior:

```csharp
var pt1 = new Point { X = 1, Y = 2 };
var pt2 = pt1;

pt2.X = 100;

Console.WriteLine(pt1.X); // 1 (copied!)
```

👉 **Key takeaway:**

* Class = shared reference
* Struct = copied value

---

# 🏗️ 3. Defining a Class

```csharp
public class Car
{
    public string Make;
    public string Model;
    public int Year;

    public void Drive()
    {
        Console.WriteLine($"{Make} {Model} is driving.");
    }
}
```

### Using it:

```csharp
var car = new Car
{
    Make = "Toyota",
    Model = "Camry",
    Year = 2022
};

car.Drive();
```

---

# 🧱 4. Defining a Struct

```csharp
public struct Vector2
{
    public float X;
    public float Y;

    public float Length()
    {
        return MathF.Sqrt(X * X + Y * Y);
    }
}
```

### Using it:

```csharp
var v = new Vector2 { X = 3, Y = 4 };
Console.WriteLine(v.Length()); // 5
```

---

# ⚙️ 5. Constructors

## Class constructors

```csharp
public class Person
{
    public string Name;

    public Person(string name)
    {
        Name = name;
    }
}
```

## Struct constructors (rules!)

```csharp
public struct Point
{
    public int X;
    public int Y;

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }
}
```

👉 Struct rules:

* Must initialize **all fields**
* Cannot define a **parameterless constructor** (before C# 10)
* Always has an implicit default (zeroed)

---

# 🔒 6. Properties (Recommended over fields)

```csharp
public class User
{
    public string Name { get; set; }
    public int Age { get; set; }
}
```

With validation:

```csharp
private int _age;

public int Age
{
    get => _age;
    set => _age = value < 0 ? 0 : value;
}
```

---

# 🧬 7. Methods & Behavior

Both classes and structs support methods:

```csharp
public struct Rectangle
{
    public int Width;
    public int Height;

    public int Area() => Width * Height;
}
```

---

# 🔁 8. Mutability (Huge Difference in Practice)

## Mutable struct (dangerous)

```csharp
public struct Counter
{
    public int Value;

    public void Increment()
    {
        Value++;
    }
}
```

Problem:

```csharp
var list = new List<Counter>();
list.Add(new Counter());

list[0].Increment(); // modifies a COPY, not the list item!
```

👉 Result: No change in list.

---

## Immutable struct (best practice)

```csharp
public readonly struct Counter
{
    public int Value { get; }

    public Counter(int value)
    {
        Value = value;
    }

    public Counter Increment() => new Counter(Value + 1);
}
```

---

# 🧩 9. Inheritance

## Classes support inheritance:

```csharp
public class Animal
{
    public void Eat() { }
}

public class Dog : Animal
{
    public void Bark() { }
}
```

## Structs DO NOT:

```csharp
// ❌ Not allowed
struct Dog : Animal { }
```

👉 Structs:

* Cannot inherit from other structs/classes
* Implicitly inherit from `System.ValueType`

---

# 🧪 10. Boxing & Unboxing

When a struct is treated like an object:

```csharp
int x = 42;
object obj = x; // boxing

int y = (int)obj; // unboxing
```

⚠️ Boxing:

* Allocates memory
* Slower
* Avoid in performance-critical code

---

# ⚡ 11. Performance Considerations

## Structs are faster when:

* Small (≤ 16 bytes is a good rule)
* Immutable
* Frequently allocated
* Short-lived

## Classes are better when:

* Large objects
* Need inheritance
* Shared state
* Nullable reference semantics

---

# 📏 12. Passing to Methods

## Class (reference)

```csharp
void Modify(Person p)
{
    p.Name = "Changed";
}
```

## Struct (value copy)

```csharp
void Modify(Point p)
{
    p.X = 100; // modifies copy
}
```

### Use `ref` to avoid copying:

```csharp
void Modify(ref Point p)
{
    p.X = 100;
}
```

---

# 🧵 13. Readonly Structs

```csharp
public readonly struct Money
{
    public decimal Amount { get; }

    public Money(decimal amount)
    {
        Amount = amount;
    }
}
```

Benefits:

* Prevents accidental mutation
* Better performance (no defensive copying)

---

# 🧰 14. Record Types (Modern Alternative)

## Record class:

```csharp
public record Person(string Name, int Age);
```

## Record struct:

```csharp
public readonly record struct Point(int X, int Y);
```

👉 Features:

* Built-in equality
* Immutable by default
* Cleaner syntax

---

# ⚖️ 15. When to Use What

## Use a **class** when:

* You need inheritance
* Object is large or complex
* You want shared mutable state
* Null should be allowed

## Use a **struct** when:

* Small, simple data
* Value semantics make sense
* Immutable design
* High-performance scenarios

---

# 🚨 16. Common Pitfalls

### ❌ Mutable structs in collections

Leads to silent bugs

### ❌ Large structs

Copying becomes expensive

### ❌ Boxing overhead

Hidden performance cost

### ❌ Forgetting value semantics

Leads to confusing behavior

---

# 🧪 17. Real-World Examples

### Struct use cases:

* `DateTime`
* `TimeSpan`
* `Vector2`, `Vector3`
* Coordinates, colors

### Class use cases:

* Services
* Entities (User, Order)
* Controllers
* Business logic

---

# 🧭 Final Rule of Thumb

If you're unsure:

👉 **Start with a class**

Switch to a struct only when:

* You understand the trade-offs
* You need the performance or value semantics
