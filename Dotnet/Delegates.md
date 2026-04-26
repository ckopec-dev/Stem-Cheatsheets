
# 🧠 1. Why Delegates Matter for Composability

A **delegate** is a type that represents a method. Think of it as a **function pointer with type safety**.

Instead of hardcoding behavior, you can *inject it*, *combine it*, or *swap it at runtime*.

That’s the foundation of:

* Composability → combine small behaviors into bigger ones
* Extensibility → add new behavior without modifying existing code

---

# 🔧 2. Basic Delegate

```csharp
public delegate int Transformer(int x);

class Program
{
    static int Square(int x) => x * x;

    static void Main()
    {
        Transformer t = Square;
        Console.WriteLine(t(5)); // 25
    }
}
```

---

# 🔗 3. Composing Delegates

Delegates can be **combined** using `+`.

```csharp
public delegate void Logger(string message);

static void LogToConsole(string msg) => Console.WriteLine($"Console: {msg}");
static void LogToFile(string msg) => Console.WriteLine($"File: {msg}");

static void Main()
{
    Logger logger = LogToConsole;
    logger += LogToFile;

    logger("Hello!");
}
```

👉 Output:

```
Console: Hello!
File: Hello!
```

This is **multicast delegation** — one call, multiple behaviors.

---

# 🧰 4. Generic Delegates (Built-in)

Instead of defining your own delegates, C# gives you:

### ✅ `Action<T>` → no return

### ✅ `Func<T, TResult>` → returns a value

### ✅ `Predicate<T>` → returns `bool`

---

## 🔹 4.1 Action<T>

```csharp
Action<string> printer = msg => Console.WriteLine(msg);

printer("Hello Action!");
```

Multiple parameters:

```csharp
Action<string, int> repeat = (msg, times) =>
{
    for (int i = 0; i < times; i++)
        Console.WriteLine(msg);
};
```

---

## 🔹 4.2 Func<T, TResult>

```csharp
Func<int, int> square = x => x * x;

Console.WriteLine(square(4)); // 16
```

Multiple inputs:

```csharp
Func<int, int, int> add = (a, b) => a + b;
```

---

## 🔹 4.3 Predicate<T>

Used for filtering:

```csharp
Predicate<int> isEven = x => x % 2 == 0;

Console.WriteLine(isEven(4)); // True
```

---

# 🧩 5. Anonymous Methods

Before lambdas, C# used `delegate` keyword:

```csharp
Action<string> printer = delegate(string msg)
{
    Console.WriteLine(msg);
};

printer("Hello from anonymous method");
```

Still useful when:

* You want a quick inline method
* You need access to outer variables (closures)

---

# 🔄 6. Real Composability Example

Let’s build a **data processing pipeline**.

```csharp
class Pipeline<T>
{
    private readonly List<Func<T, T>> _steps = new();

    public void AddStep(Func<T, T> step)
    {
        _steps.Add(step);
    }

    public T Execute(T input)
    {
        T result = input;

        foreach (var step in _steps)
            result = step(result);

        return result;
    }
}
```

---

## Usage

```csharp
var pipeline = new Pipeline<int>();

pipeline.AddStep(x => x + 2);
pipeline.AddStep(x => x * 3);
pipeline.AddStep(x => x - 1);

int result = pipeline.Execute(5);

Console.WriteLine(result); // ((5 + 2) * 3) - 1 = 20
```

👉 You can **extend behavior without modifying Pipeline**

---

# 🔍 7. Filtering with Predicate<T>

```csharp
List<int> numbers = new() { 1, 2, 3, 4, 5 };

List<int> Filter(List<int> list, Predicate<int> predicate)
{
    var result = new List<int>();

    foreach (var item in list)
        if (predicate(item))
            result.Add(item);

    return result;
}
```

---

## Usage

```csharp
var evens = Filter(numbers, x => x % 2 == 0);
```

👉 You can plug in any rule:

* even numbers
* prime numbers
* > 100
* custom logic

---

# 🧱 8. Extensibility Pattern: Behavior Injection

Instead of:

```csharp
class ReportGenerator
{
    public void Generate()
    {
        Console.WriteLine("Generating report...");
        // hardcoded logic
    }
}
```

---

## Make it extensible:

```csharp
class ReportGenerator
{
    private readonly Action _before;
    private readonly Action _after;

    public ReportGenerator(Action before, Action after)
    {
        _before = before;
        _after = after;
    }

    public void Generate()
    {
        _before?.Invoke();

        Console.WriteLine("Generating report...");

        _after?.Invoke();
    }
}
```

---

## Usage

```csharp
var report = new ReportGenerator(
    () => Console.WriteLine("Start logging"),
    () => Console.WriteLine("End logging")
);

report.Generate();
```

👉 You just extended behavior **without changing the class**

---

# 🔗 9. Delegate Chaining for Pipelines

You can also compose functions dynamically:

```csharp
Func<int, int> pipeline =
    x => x + 1;

pipeline += x => x * 2; // ⚠ only last return is used!
```

⚠ Important:

* Multicast delegates **do not chain return values**
* Only the **last return value is preserved**

---

## Correct functional composition:

```csharp
Func<int, int> step1 = x => x + 1;
Func<int, int> step2 = x => x * 2;

Func<int, int> composed = x => step2(step1(x));

Console.WriteLine(composed(5)); // 12
```

---

# 🧠 10. Closures (Powerful Feature)

Delegates capture variables:

```csharp
int factor = 10;

Func<int, int> multiply = x => x * factor;

Console.WriteLine(multiply(5)); // 50
```

👉 `factor` is captured — this enables dynamic behavior.

---

# 🏗️ 11. Putting It All Together

A flexible system might use:

* `Func<T, T>` → transformations
* `Predicate<T>` → rules
* `Action<T>` → side effects (logging, output)
* Anonymous methods → quick inline extensions

---

# ⚖️ Best Practices

✔ Prefer built-in delegates (`Action`, `Func`, `Predicate`)
✔ Use lambdas over anonymous methods (cleaner)
✔ Use delegates to:

* decouple logic
* inject behavior
* build pipelines

⚠ Avoid:

* overusing multicast delegates for return values
* making delegate chains too hard to trace
