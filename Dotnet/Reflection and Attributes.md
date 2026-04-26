# 🧠 C# Reflection & Attributes Tutorial

## 🔍 What is Reflection?

**Reflection** lets your program inspect and interact with its own code at runtime.

You can:

* Discover types, methods, properties
* Invoke methods dynamically
* Read/write properties
* Inspect custom metadata (attributes)

### ⚠️ Reality check

Reflection is slower and less safe than normal code. Use it when:

* You need **extensibility** (plugins, frameworks)
* You don’t know types at compile time
* You’re building tooling (ORMs, serializers, DI containers)

Avoid it for core hot paths.

---

## 🧱 Basic Reflection Example

```csharp
using System;
using System.Reflection;

public class Person
{
    public string Name { get; set; }

    public void SayHello()
    {
        Console.WriteLine($"Hello, my name is {Name}");
    }
}

class Program
{
    static void Main()
    {
        Type type = typeof(Person);

        Console.WriteLine("Methods:");
        foreach (var method in type.GetMethods())
        {
            Console.WriteLine(method.Name);
        }

        Console.WriteLine("\nProperties:");
        foreach (var prop in type.GetProperties())
        {
            Console.WriteLine(prop.Name);
        }
    }
}
```

---

## ⚙️ Creating Instances Dynamically

```csharp
Type type = typeof(Person);

// Create instance
object obj = Activator.CreateInstance(type);

// Set property
PropertyInfo prop = type.GetProperty("Name");
prop.SetValue(obj, "Chris");

// Call method
MethodInfo method = type.GetMethod("SayHello");
method.Invoke(obj, null);
```

---

## 🏷️ What are Attributes?

**Attributes** are metadata you attach to code.

Examples you’ve seen:

```csharp
[Obsolete]
[Serializable]
[HttpGet]
```

They don’t *do* anything by themselves—you read them using reflection.

---

## 🧪 Creating a Custom Attribute

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method)]
public class InfoAttribute : Attribute
{
    public string Description { get; }

    public InfoAttribute(string description)
    {
        Description = description;
    }
}
```

---

## 🧩 Using the Attribute

```csharp
[Info("This is a test class")]
public class MyClass
{
    [Info("This method prints something")]
    public void DoWork()
    {
        Console.WriteLine("Working...");
    }
}
```

---

## 🔎 Reading Attributes with Reflection

```csharp
Type type = typeof(MyClass);

// Get class attribute
var classAttr = type.GetCustomAttribute<InfoAttribute>();
Console.WriteLine($"Class: {classAttr.Description}");

// Get method attribute
foreach (var method in type.GetMethods())
{
    var attr = method.GetCustomAttribute<InfoAttribute>();
    if (attr != null)
    {
        Console.WriteLine($"{method.Name}: {attr.Description}");
    }
}
```

---

## 🧠 Real-World Pattern: Attribute-Driven Behavior

This is where things get interesting.

### Example: Simple Command System

#### Step 1: Define Attribute

```csharp
[AttributeUsage(AttributeTargets.Method)]
public class CommandAttribute : Attribute
{
    public string Name { get; }

    public CommandAttribute(string name)
    {
        Name = name;
    }
}
```

#### Step 2: Use It

```csharp
public class Commands
{
    [Command("greet")]
    public void Greet()
    {
        Console.WriteLine("Hello!");
    }

    [Command("exit")]
    public void Exit()
    {
        Console.WriteLine("Exiting...");
    }
}
```

#### Step 3: Discover & Execute

```csharp
using System.Linq;

var commands = new Commands();
var methods = typeof(Commands).GetMethods();

var commandMap = methods
    .Select(m => new
    {
        Method = m,
        Attr = m.GetCustomAttribute<CommandAttribute>()
    })
    .Where(x => x.Attr != null)
    .ToDictionary(x => x.Attr.Name, x => x.Method);

// Simulate input
string input = "greet";

if (commandMap.TryGetValue(input, out var method))
{
    method.Invoke(commands, null);
}
```

---

## ⚡ Advanced: Assembly Scanning

You can scan entire assemblies for types:

```csharp
var types = Assembly.GetExecutingAssembly().GetTypes();

foreach (var type in types)
{
    Console.WriteLine(type.Name);
}
```

Used heavily in:

* Dependency Injection frameworks
* ORMs (like Entity Framework)
* Serialization libraries

---

## ⚠️ Pitfalls & Best Practices

### ❌ Avoid

* Reflection in tight loops
* Using string names everywhere (fragile)
* Replacing normal polymorphism with reflection

### ✅ Prefer

* Cache reflection results
* Use `nameof()` instead of raw strings
* Combine with interfaces when possible

---

## 🧩 When to Use Reflection + Attributes

Use them when you need:

| Scenario             | Why                     |
| -------------------- | ----------------------- |
| Plugin systems       | Discover unknown types  |
| Dependency Injection | Auto-wire services      |
| Serialization        | Map objects dynamically |
| CLI tools            | Map commands to methods |
| Testing frameworks   | Discover test methods   |

---

## 🧪 Mini Exercise

Try building:

* A `[Route("/api/users")]` system
* A `[Validate]` attribute for input validation
* A mini test runner using `[Test]`

---

## 🚀 Final Takeaway

* **Reflection = runtime introspection**
* **Attributes = metadata**
* Together → **declarative, extensible systems**

But don’t overuse them—most problems are better solved with:

* Interfaces
* Generics
* Dependency Injection


