
# 🧠 C# Collections Tutorial (Dictionary, HashSet, SortedSet, Queue, LinkedList, Stack)

## 📦 1. Overview of Collection Types

C# collections fall into a few conceptual categories:

| Type         | Purpose                   | Key Feature                  |
| ------------ | ------------------------- | ---------------------------- |
| `Dictionary` | Key → Value lookup        | Fast O(1) access by key      |
| `HashSet`    | Unique items              | No duplicates, fast lookup   |
| `SortedSet`  | Sorted unique items       | Always ordered               |
| `Queue`      | FIFO (First-In-First-Out) | Processing pipelines         |
| `Stack`      | LIFO (Last-In-First-Out)  | Undo/recursion-like behavior |
| `LinkedList` | Node-based list           | Efficient insert/remove      |

---

# 📘 2. Dictionary<TKey, TValue>

### ✅ Use When:

* You need fast lookup by key
* You want to associate values with unique identifiers

### 🔧 Example

```csharp
var users = new Dictionary<int, string>();

users.Add(1, "Alice");
users.Add(2, "Bob");

// Access
Console.WriteLine(users[1]); // Alice

// Safe lookup
if (users.TryGetValue(3, out var name))
{
    Console.WriteLine(name);
}
else
{
    Console.WriteLine("User not found");
}
```

### ⚡ Key Points

* Keys must be unique
* Lookup is very fast (hash-based)
* Throws exception if key doesn’t exist (unless using `TryGetValue`)

---

# 🧩 3. HashSet<T>

### ✅ Use When:

* You need a collection of unique items
* You want fast membership checks

### 🔧 Example

```csharp
var tags = new HashSet<string>();

tags.Add("csharp");
tags.Add("dotnet");
tags.Add("csharp"); // ignored (duplicate)

Console.WriteLine(tags.Contains("csharp")); // true
```

### ⚡ Set Operations

```csharp
var a = new HashSet<int> { 1, 2, 3 };
var b = new HashSet<int> { 3, 4, 5 };

a.UnionWith(b);        // {1,2,3,4,5}
a.IntersectWith(b);    // {3}
a.ExceptWith(b);       // {1,2}
```

---

# 🔢 4. SortedSet<T>

### ✅ Use When:

* You need uniqueness *and* sorted order

### 🔧 Example

```csharp
var numbers = new SortedSet<int> { 5, 1, 3 };

foreach (var n in numbers)
{
    Console.WriteLine(n);
}
// Output: 1, 3, 5
```

### ⚡ Key Points

* Automatically sorted
* Slower than `HashSet` for inserts (tree-based)
* Supports range queries:

```csharp
var range = numbers.GetViewBetween(2, 5);
```

---

# 📬 5. Queue<T> (FIFO)

### ✅ Use When:

* Processing tasks in order received
* Breadth-first traversal

### 🔧 Example

```csharp
var queue = new Queue<string>();

queue.Enqueue("task1");
queue.Enqueue("task2");

Console.WriteLine(queue.Dequeue()); // task1
Console.WriteLine(queue.Peek());    // task2 (still in queue)
```

### ⚡ Key Points

* First in → First out
* `Dequeue()` removes
* `Peek()` inspects without removing

---

# 📚 6. Stack<T> (LIFO)

### ✅ Use When:

* Undo systems
* Backtracking algorithms
* Expression evaluation

### 🔧 Example

```csharp
var stack = new Stack<int>();

stack.Push(1);
stack.Push(2);

Console.WriteLine(stack.Pop());  // 2
Console.WriteLine(stack.Peek()); // 1
```

### ⚡ Key Points

* Last in → First out
* Works like a pile of plates

---

# 🔗 7. LinkedList<T>

### ✅ Use When:

* Frequent insert/remove in the middle
* You need node-level control

### 🔧 Example

```csharp
var list = new LinkedList<string>();

var first = list.AddFirst("A");
var second = list.AddAfter(first, "B");

list.AddLast("C");

// Remove
list.Remove(second);
```

### ⚡ Key Points

* No indexing (`list[0]` doesn’t exist)
* Fast inserts/removals (no shifting like `List<T>`)
* Each element is a node

---

# ⚖️ Choosing the Right Collection

| Scenario                           | Best Choice  |
| ---------------------------------- | ------------ |
| Fast key lookup                    | `Dictionary` |
| Unique items only                  | `HashSet`    |
| Unique + sorted                    | `SortedSet`  |
| FIFO processing                    | `Queue`      |
| LIFO processing                    | `Stack`      |
| Frequent mid-list inserts/removals | `LinkedList` |

---

# 🧪 Real-World Example

```csharp
var visited = new HashSet<string>();
var queue = new Queue<string>();

queue.Enqueue("start");

while (queue.Count > 0)
{
    var current = queue.Dequeue();

    if (!visited.Add(current))
        continue;

    Console.WriteLine($"Processing {current}");

    // Simulate neighbors
    queue.Enqueue(current + "_A");
    queue.Enqueue(current + "_B");
}
```

👉 This combines:

* `Queue` → traversal order
* `HashSet` → avoid duplicates

---

# 🚀 Advanced Tips

### 1. Custom Equality (for HashSet/Dictionary)

```csharp
class PersonComparer : IEqualityComparer<Person>
{
    public bool Equals(Person x, Person y) =>
        x.Id == y.Id;

    public int GetHashCode(Person obj) =>
        obj.Id.GetHashCode();
}
```

---

### 2. SortedSet with Custom Sorting

```csharp
var set = new SortedSet<string>(Comparer<string>.Create(
    (a, b) => a.Length.CompareTo(b.Length)
));
```

---

### 3. When NOT to Use These

* Use `List<T>` when:

  * You need indexing
  * Most operations are sequential

---

# 🧭 Mental Model

* **Dictionary** = lookup table
* **HashSet** = uniqueness filter
* **SortedSet** = ordered uniqueness
* **Queue** = line of people
* **Stack** = stack of plates
* **LinkedList** = chain of nodes

