
# 🚀 Scala Programming Language — Comprehensive Tutorial

Scala (“Scalable Language”) is a modern, high-level language that blends **object-oriented** and **functional programming**. It runs on the **JVM**, integrates seamlessly with Java, and is widely used for **backend systems, distributed computing, and big data (Apache Spark)**.

---

# 📚 Table of Contents

1. What is Scala?
2. Installing Scala
3. Your First Scala Program
4. Variables, Types, and Values
5. Control Flow
6. Functions
7. Object-Oriented Programming
8. Case Classes & Pattern Matching
9. Collections
10. Functional Programming in Scala
11. Error Handling
12. Traits & Advanced OOP
13. Generics & Type System
14. Concurrency (Futures)
15. Build Tools (sbt)
16. Interoperability with Java
17. Testing
18. Scala for Big Data (Spark)
19. Project Structure
20. Learning Path & Best Practices

---

# 1️⃣ What is Scala?

Scala is:

* ✅ Statically typed
* ✅ Functional + Object-Oriented
* ✅ JVM-based
* ✅ Concise, expressive, safe

Used by companies like Twitter, Netflix, LinkedIn, Airbnb.

---

# 2️⃣ Installing Scala

### Option A — Install directly

* Install **JDK 17+**
* Install Scala: [https://scala-lang.org/download/](https://scala-lang.org/download/)

### Option B — Using Coursier (recommended)

```bash
winget install coursier
cs setup
```

Verify:

```bash
scala -version
scalac -version
```

---

# 3️⃣ Your First Scala Program

### Hello.scala

```scala
object Hello {
  def main(args: Array[String]): Unit = {
    println("Hello, Scala!")
  }
}
```

Compile & run:

```bash
scalac Hello.scala
scala Hello
```

Or use Scala REPL:

```bash
scala
```

---

# 4️⃣ Variables, Types, and Values

### Immutable by default

```scala
val x: Int = 10
var y: Int = 5
y = 20
```

### Type inference

```scala
val name = "Chris"
val pi = 3.14159
```

### Basic types

```scala
Int, Long, Double, Float
Boolean, Char, String
Unit, Nothing, Any
```

---

# 5️⃣ Control Flow

### If expression

```scala
val result = if (x > 0) "positive" else "negative"
```

### Loops

```scala
for (i <- 1 to 5) println(i)

while (x < 10) {
  println(x)
}
```

### For-comprehension

```scala
for {
  i <- 1 to 3
  j <- 1 to 2
} println(s"$i $j")
```

---

# 6️⃣ Functions

```scala
def add(a: Int, b: Int): Int = {
  a + b
}
```

Short form:

```scala
def square(x: Int) = x * x
```

### Default & named parameters

```scala
def greet(name: String = "World") =
  s"Hello $name"

greet()
greet(name = "Scala")
```

### Anonymous functions (lambdas)

```scala
val double = (x: Int) => x * 2
```

---

# 7️⃣ Object-Oriented Programming

### Classes

```scala
class Person(val name: String, var age: Int) {
  def greet(): String = s"Hi, I'm $name"
}
```

```scala
val p = new Person("Alex", 30)
println(p.greet())
```

### Objects (singletons)

```scala
object MathUtil {
  def square(x: Int) = x * x
}
```

---

# 8️⃣ Case Classes & Pattern Matching

### Case class

```scala
case class User(name: String, age: Int)
```

Auto-creates:

* constructor
* equals/hashCode
* copy()
* toString
* pattern matching support

```scala
val u = User("Sam", 25)
val u2 = u.copy(age = 30)
```

### Pattern matching

```scala
def describe(x: Any): String = x match {
  case 0 => "zero"
  case i: Int => "an int"
  case s: String => s"a string: $s"
  case _ => "something else"
}
```

---

# 9️⃣ Collections

### Immutable collections (preferred)

```scala
val nums = List(1,2,3)
val set = Set(1,2,3)
val map = Map("a" -> 1, "b" -> 2)
```

### Common operations

```scala
nums.map(_ * 2)
nums.filter(_ % 2 == 0)
nums.reduce(_ + _)
nums.sum
```

### Mutable collections

```scala
import scala.collection.mutable

val buf = mutable.ArrayBuffer(1,2,3)
buf += 4
```

---

# 🔟 Functional Programming in Scala

### Higher-order functions

```scala
def applyTwice(f: Int => Int, x: Int) =
  f(f(x))
```

### Immutability

```scala
val list2 = nums :+ 4
```

### Option (null safety)

```scala
def find(id: Int): Option[String] =
  if (id == 1) Some("Chris") else None
```

```scala
find(1).getOrElse("Not found")
```

### Either

```scala
def divide(a: Int, b: Int): Either[String, Int] =
  if (b == 0) Left("Division by zero")
  else Right(a / b)
```

---

# 1️⃣1️⃣ Error Handling

### Try / Catch

```scala
import scala.util.Try

val result = Try(10 / 0)
```

```scala
result match {
  case scala.util.Success(v) => println(v)
  case scala.util.Failure(e) => println(e.getMessage)
}
```

---

# 1️⃣2️⃣ Traits & Advanced OOP

### Traits (like interfaces + mixins)

```scala
trait Logger {
  def log(msg: String): Unit =
    println(s"[LOG] $msg")
}
```

```scala
class Service extends Logger {
  def run() = log("Service running")
}
```

### Multiple inheritance

```scala
class App extends Service with Serializable
```

---

# 1️⃣3️⃣ Generics & Type System

```scala
class Box[T](val value: T)
```

```scala
val intBox = new Box
```

### Bounded types

```scala
def printAll[T <: AnyVal](x: T) = println(x)
```

### Variance

```scala
class Covariant[+A]
class Contravariant[-A]
```

---

# 1️⃣4️⃣ Concurrency (Futures)

```scala
import scala.concurrent._
import ExecutionContext.Implicits.global

val f = Future {
  Thread.sleep(1000)
  42
}
```

```scala
f.onComplete {
  case Success(v) => println(v)
  case Failure(e) => e.printStackTrace()
}
```

### Await (blocking)

```scala
Await.result(f, duration.Duration.Inf)
```

---

# 1️⃣5️⃣ Build Tools (sbt)

### Create project

```bash
sbt new scala/scala3.g8
```

### build.sbt

```scala
name := "MyProject"
scalaVersion := "3.3.1"

libraryDependencies += "org.scalatest" %% "scalatest" % "3.2.18" % Test
```

### Commands

```bash
sbt compile
sbt run
sbt test
```

---

# 1️⃣6️⃣ Interoperability with Java

Scala can directly use Java libraries:

```scala
val list = new java.util.ArrayList[String]()
list.add("Hello")
```

Java can use Scala classes too.

---

# 1️⃣7️⃣ Testing

### ScalaTest example

```scala
class MathTest extends org.scalatest.funsuite.AnyFunSuite {
  test("addition") {
    assert(2 + 2 == 4)
  }
}
```

Run:

```bash
sbt test
```

---

# 1️⃣8️⃣ Scala for Big Data (Spark)

```scala
import org.apache.spark.sql.SparkSession

val spark = SparkSession.builder()
  .appName("Demo")
  .master("local[*]")
  .getOrCreate()

val data = spark.read.csv("data.csv")
data.show()
```

Scala is Spark’s **native language** and performs best.

---

# 1️⃣9️⃣ Typical Project Structure

```
project/
  build.sbt
  src/
    main/scala/
    test/scala/
```

---

# 2️⃣0️⃣ Best Practices & Learning Path

### Best practices

* Prefer `val` over `var`
* Favor immutability
* Use `Option` instead of null
* Prefer pattern matching over if-else
* Small pure functions
* Use sbt and tests early

### Learning path

1. Core syntax
2. Collections & FP
3. OOP + traits
4. Type system & generics
5. Concurrency
6. Frameworks (Akka, ZIO, Cats Effect)
7. Big data (Spark)

---

# 🎯 What Scala is Best At

* High-performance backend systems
* Distributed systems
* Big data pipelines
* DSLs and compilers
* Functional application design

---

# 📦 Example Mini-Project

```scala
case class Task(title: String, done: Boolean = false)

object TodoApp extends App {
  var tasks = List[Task]()

  def add(title: String) =
    tasks = tasks :+ Task(title)

  def complete(title: String) =
    tasks = tasks.map(t =>
      if (t.title == title) t.copy(done = true) else t
    )

  add("Learn Scala")
  add("Build project")
  complete("Learn Scala")

  tasks.foreach(println)
}
```

---

# 🧠 Summary

Scala gives you:

* JVM power
* Functional safety
* Object-oriented structure
* Massive ecosystem
* Enterprise + big data readiness


