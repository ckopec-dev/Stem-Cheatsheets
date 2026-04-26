
# 🧵 C# Multithreading Tutorial

## 1. What is Multithreading?

Multithreading lets your program run **multiple operations concurrently**, improving responsiveness and performance.

### Why use it?

* Keep UI responsive
* Perform background work (I/O, computation)
* Utilize multiple CPU cores

---

## 2. Threads Basics (`System.Threading`)

The most direct (but lowest-level) way is using `Thread`.

```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        Thread thread = new Thread(DoWork);
        thread.Start();

        Console.WriteLine("Main thread continues...");
    }

    static void DoWork()
    {
        Console.WriteLine("Worker thread running...");
        Thread.Sleep(2000);
        Console.WriteLine("Worker thread done.");
    }
}
```

### Key Points:

* `Thread.Start()` begins execution
* `Thread.Sleep()` pauses execution
* Threads are expensive → avoid overusing

---

## 3. Thread Pool (Better than raw threads)

Instead of manually creating threads, use the **Thread Pool**:

```csharp
using System;
using System.Threading;

class Program
{
    static void Main()
    {
        ThreadPool.QueueUserWorkItem(_ =>
        {
            Console.WriteLine("Running in thread pool");
        });

        Console.ReadLine();
    }
}
```

### Why ThreadPool?

* Reuses threads
* More efficient than creating new ones

---

## 4. Tasks (Modern Standard) ✅

The **Task Parallel Library (TPL)** is the preferred approach.

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static void Main()
    {
        Task task = Task.Run(() =>
        {
            Console.WriteLine("Task running...");
        });

        task.Wait(); // Wait for completion
    }
}
```

### Returning values

```csharp
Task<int> task = Task.Run(() =>
{
    return 42;
});

Console.WriteLine(task.Result);
```

---

## 5. Async / Await (High-Level Concurrency)

Best for **I/O-bound operations** (network, file, DB).

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        int result = await DoWorkAsync();
        Console.WriteLine(result);
    }

    static async Task<int> DoWorkAsync()
    {
        await Task.Delay(2000);
        return 100;
    }
}
```

### Key Benefits:

* Non-blocking
* Cleaner than callbacks
* Scales better

---

## 6. Parallel Programming (`Parallel` class)

Used for CPU-bound work across cores.

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static void Main()
    {
        Parallel.For(0, 5, i =>
        {
            Console.WriteLine($"Iteration {i}");
        });
    }
}
```

### Also available:

* `Parallel.ForEach`
* `Parallel.Invoke`

---

## 7. Race Conditions ⚠️

When multiple threads access shared data → problems occur.

```csharp
int counter = 0;

Parallel.For(0, 1000, _ =>
{
    counter++; // NOT SAFE
});
```

### Fix with locking

```csharp
object lockObj = new object();

Parallel.For(0, 1000, _ =>
{
    lock (lockObj)
    {
        counter++;
    }
});
```

---

## 8. Thread-Safe Alternatives

### `Interlocked` (fast atomic operations)

```csharp
using System.Threading;

int counter = 0;

Parallel.For(0, 1000, _ =>
{
    Interlocked.Increment(ref counter);
});
```

---

### Concurrent Collections

```csharp
using System.Collections.Concurrent;

ConcurrentBag<int> bag = new ConcurrentBag<int>();

Parallel.For(0, 10, i =>
{
    bag.Add(i);
});
```

Other types:

* `ConcurrentQueue<T>`
* `ConcurrentDictionary<TKey, TValue>`

---

## 9. Cancellation

Gracefully stop threads using `CancellationToken`.

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        var cts = new CancellationTokenSource();

        var task = Task.Run(() =>
        {
            while (!cts.Token.IsCancellationRequested)
            {
                Console.WriteLine("Working...");
                Thread.Sleep(500);
            }
        }, cts.Token);

        await Task.Delay(2000);
        cts.Cancel();

        await task;
        Console.WriteLine("Cancelled.");
    }
}
```

---

## 10. Synchronization Primitives

### `lock` (Monitor)

* Simple mutual exclusion

### `SemaphoreSlim`

* Limit concurrent access

```csharp
SemaphoreSlim semaphore = new SemaphoreSlim(2);

await semaphore.WaitAsync();
try
{
    Console.WriteLine("Inside critical section");
}
finally
{
    semaphore.Release();
}
```

---

### `Mutex`

* Cross-process locking (heavier)

### `ReaderWriterLockSlim`

* Multiple readers, single writer

---

## 11. Deadlocks ⚠️

Occurs when threads wait on each other forever.

### Example (bad):

```csharp
lock (lockA)
{
    lock (lockB)
    {
    }
}

lock (lockB)
{
    lock (lockA)
    {
    }
}
```

### Avoid by:

* Consistent lock ordering
* Minimizing lock scope
* Using async instead of blocking

---

## 12. Best Practices ✅

* Prefer **Task + async/await** over raw threads
* Use **Parallel** only for CPU-bound work
* Avoid shared mutable state
* Use thread-safe collections
* Keep critical sections small
* Always handle exceptions in threads

---

## 13. When to Use What

| Scenario             | Tool                  |
| -------------------- | --------------------- |
| UI responsiveness    | `async/await`         |
| CPU parallel work    | `Parallel`            |
| Background jobs      | `Task.Run`            |
| Fine control threads | `Thread` (rare)       |
| Shared data safety   | `lock`, `Interlocked` |

---

## 14. Real-World Example

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        var tasks = new[]
        {
            Task.Run(() => Work(1)),
            Task.Run(() => Work(2)),
            Task.Run(() => Work(3))
        };

        await Task.WhenAll(tasks);

        Console.WriteLine("All tasks complete.");
    }

    static void Work(int id)
    {
        Console.WriteLine($"Task {id} starting");
        Task.Delay(1000).Wait();
        Console.WriteLine($"Task {id} done");
    }
}
```

---

## Final Thoughts

If you take one thing away:

👉 Use **`async/await` + `Task` for most cases**
👉 Use **`Parallel` for CPU-heavy work**
👉 Avoid raw threads unless absolutely necessary


