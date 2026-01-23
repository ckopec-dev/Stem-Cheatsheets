
# ✅ F# Programming Language – Comprehensive Tutorial

F# is a **functional-first, strongly typed language** on the .NET platform. It blends:

* Functional programming
* Object-oriented programming
* Imperative programming

It excels at **correctness, conciseness, data modeling, async workflows, finance, scientific computing, compilers, and web services.**

---

# 📦 1. Installing F#

### Option A: Install .NET SDK (recommended)

[https://dotnet.microsoft.com/download](https://dotnet.microsoft.com/download)

Verify:

```bash
dotnet --version
```

Create an F# project:

```bash
dotnet new console -lang F#
cd myapp
dotnet run
```

### Option B: Interactive REPL

```bash
dotnet fsi
```

This launches **F# Interactive (FSI)** — great for experimentation.

---

# ✨ 2. Your First F# Program

```fsharp
printfn "Hello, F#!"
```

Key notes:

* No semicolons
* `printfn` prints with newline
* Type inference everywhere

---

# 🧠 3. Core Language Concepts

## Values (Immutable by default)

```fsharp
let x = 10
let name = "Chris"
let pi = 3.14159
```

Once defined, they cannot change.

Mutable value:

```fsharp
let mutable counter = 0
counter <- counter + 1
```

---

## Functions

```fsharp
let add a b = a + b
let square x = x * x
```

Calling:

```fsharp
add 2 3
square 5
```

Functions are **first-class values**.

```fsharp
let apply f x = f x
apply square 4
```

---

## Type Annotations

Usually optional:

```fsharp
let multiply (a:int) (b:int) : int = a * b
```

---

# 🔢 4. Basic Data Types

```fsharp
int      float      bool
string   char       unit
```

Examples:

```fsharp
let a = 10
let b = 3.14
let c = true
let d = "hello"
let e = ()
```

---

# 🔀 5. Control Flow

## If expression

```fsharp
let sign x =
    if x > 0 then "positive"
    elif x < 0 then "negative"
    else "zero"
```

(Everything returns a value.)

---

## For loops

```fsharp
for i = 1 to 5 do
    printfn "%d" i
```

---

## While loops

```fsharp
let mutable n = 5
while n > 0 do
    printfn "%d" n
    n <- n - 1
```

---

# 📚 6. Collections

## Lists (immutable linked lists)

```fsharp
let numbers = [1; 2; 3; 4]
let names = ["Alice"; "Bob"]

let newList = 0 :: numbers
```

Iterating:

```fsharp
numbers |> List.iter printfn
```

Mapping:

```fsharp
numbers |> List.map (fun x -> x * 2)
```

Filtering:

```fsharp
numbers |> List.filter (fun x -> x % 2 = 0)
```

---

## Arrays (mutable)

```fsharp
let arr = [|1;2;3|]
arr[0] <- 99
```

---

## Sequences (lazy)

```fsharp
let seqNums = seq { 1..100 }
```

---

## Tuples

```fsharp
let person = ("Chris", 35)

let name, age = person
```

---

# 🧱 7. Records (Data Structures)

```fsharp
type Person = {
    Name : string
    Age : int
}

let p = { Name = "Chris"; Age = 30 }
```

Update immutably:

```fsharp
let p2 = { p with Age = 31 }
```

---

# 🔀 8. Discriminated Unions (Power Feature)

```fsharp
type Shape =
    | Circle of float
    | Rectangle of float * float
    | Square of float
```

Pattern matching:

```fsharp
let area shape =
    match shape with
    | Circle r -> System.Math.PI * r * r
    | Rectangle (w,h) -> w * h
    | Square s -> s * s
```

This is central to **domain modeling** in F#.

---

# 🧩 9. Pattern Matching

```fsharp
let describe x =
    match x with
    | 0 -> "zero"
    | 1 -> "one"
    | _ -> "many"
```

With tuples:

```fsharp
let addPair (a,b) = a + b
```

With records:

```fsharp
let greet { Name = n } = "Hello " + n
```

---

# ⚙️ 10. Pipelines and Composition

Pipeline operator:

```fsharp
numbers
|> List.filter (fun x -> x > 2)
|> List.map (fun x -> x * 2)
|> List.sum
```

Function composition:

```fsharp
let add1 x = x + 1
let double x = x * 2

let addThenDouble = add1 >> double
```

---

# 🧪 11. Option and Result Types

## Option (avoid null)

```fsharp
let tryDivide a b =
    if b = 0 then None
    else Some (a / b)
```

Use:

```fsharp
match tryDivide 10 2 with
| Some v -> printfn "%d" v
| None -> printfn "Invalid"
```

---

## Result (error handling)

```fsharp
let parseInt s =
    match System.Int32.TryParse s with
    | true, v -> Ok v
    | _ -> Error "Not a number"
```

---

# 🧵 12. Recursion

```fsharp
let rec factorial n =
    if n <= 1 then 1
    else n * factorial (n-1)
```

Tail recursion:

```fsharp
let factorial n =
    let rec loop acc n =
        if n <= 1 then acc
        else loop (acc*n) (n-1)
    loop 1 n
```

---

# 🏛 13. Object-Oriented F#

## Classes

```fsharp
type Car(make:string, year:int) =
    member _.Make = make
    member _.Year = year
    member _.Info() = $"{make} ({year})"
```

---

## Interfaces

```fsharp
type ILogger =
    abstract Log : string -> unit

type ConsoleLogger() =
    interface ILogger with
        member _.Log msg = printfn "%s" msg
```

---

# 🔌 14. .NET Interop

Use any .NET library:

```fsharp
open System.IO

File.WriteAllText("test.txt", "Hello")
let text = File.ReadAllText("test.txt")
```

LINQ works:

```fsharp
open System.Linq
numbers.Where(fun x -> x > 2)
```

---

# ⚡ 15. Async and Parallelism

## Async workflows

```fsharp
open System.Net.Http

let download url =
    async {
        use client = new HttpClient()
        return! client.GetStringAsync(url) |> Async.AwaitTask
    }
```

Run:

```fsharp
download "https://example.com"
|> Async.RunSynchronously
```

---

## Parallel

```fsharp
[1..10]
|> List.map (fun x -> async { return x * x })
|> Async.Parallel
|> Async.RunSynchronously
```

---

# 🧰 16. Modules and Files

```fsharp
module MathTools

let add a b = a + b
```

Use:

```fsharp
open MathTools
add 2 3
```

---

# 🧪 17. Unit Testing

```bash
dotnet new xunit -lang F#
```

Example:

```fsharp
[<Fact>]
let ``addition works`` () =
    Assert.Equal(4, 2 + 2)
```

---

# 🌐 18. Building Real Applications

F# is excellent for:

### ✔ Web APIs

* ASP.NET Core
* Giraffe
* Saturn

### ✔ Desktop apps

* Avalonia
* WPF

### ✔ Data / ML

* FSharp.Data
* ML.NET
* Math.NET

### ✔ Finance & simulations

* Heavy functional modeling

---

# 🧠 19. Functional Design Patterns

* Immutability
* Expression-oriented code
* Railway-oriented programming
* Domain-driven design with DUs
* Computation expressions
* Type-driven development

Example pipeline:

```fsharp
let process input =
    input
    |> validate
    |> transform
    |> persist
```

---

# 🧾 20. Mini Practical Example

```fsharp
type BankAccount = {
    Owner : string
    Balance : decimal
}

let deposit amount acc =
    { acc with Balance = acc.Balance + amount }

let withdraw amount acc =
    if amount > acc.Balance then None
    else Some { acc with Balance = acc.Balance - amount }

let account = { Owner = "Chris"; Balance = 100m }

let updated =
    account
    |> deposit 50m
    |> withdraw 30m
```

---

# 📘 21. Recommended Learning Path

1. Syntax + immutability
2. Lists + pipelines
3. Records + unions
4. Pattern matching
5. Async workflows
6. Domain modeling
7. .NET ecosystem

---

# 🚀 22. What Makes F# Powerful

* Type safety without verbosity
* Concise expressive code
* Built-in concurrency model
* Perfect for DSLs and modeling
* Best of FP + .NET
