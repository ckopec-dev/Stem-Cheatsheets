# 📘 Comprehensive Kotlin Programming Language Tutorial

Kotlin is a modern, statically typed language developed by JetBrains. It runs on the JVM, Android, JavaScript, and Native platforms, and is fully interoperable with Java.

---

# 1. What is Kotlin?

Kotlin is designed to be:

* ✅ Concise
* ✅ Safe (null-safety built in)
* ✅ Interoperable with Java
* ✅ Multi-platform (JVM, Android, JS, Native)

Common uses:

* Android development
* Backend services (Spring, Ktor)
* Desktop apps
* Multiplatform shared code
* Native tools

---

# 2. Installing Kotlin

## Option A – JVM / CLI

Install via SDKMAN (recommended):

```bash
curl -s https://get.sdkman.io | bash
sdk install kotlin
```

Check install:

```bash
kotlinc -version
```

Run a file:

```bash
kotlinc hello.kt -include-runtime -d hello.jar
java -jar hello.jar
```

## Option B – IntelliJ IDEA

* Download IntelliJ IDEA
* New Project → Kotlin → JVM / Android / Multiplatform

---

# 3. Your First Kotlin Program

```kotlin
fun main() {
    println("Hello, Kotlin!")
}
```

Kotlin programs start from `fun main()`.

---

# 4. Variables and Types

```kotlin
val name = "Chris"     // immutable
var age = 30           // mutable

val score: Int = 100
val pi: Double = 3.14
val active: Boolean = true
```

Type inference is automatic.

---

# 5. Basic Data Types

| Type    | Example |
| ------- | ------- |
| Int     | 42      |
| Long    | 42L     |
| Double  | 3.14    |
| Float   | 3.14f   |
| Boolean | true    |
| Char    | 'A'     |
| String  | "Hello" |

Strings:

```kotlin
val s = "Kotlin"
println("Hello, $s")
println("Length: ${s.length}")
```

---

# 6. Null Safety (Core Kotlin Feature)

```kotlin
var name: String? = null
```

Safe access:

```kotlin
println(name?.length)
```

Elvis operator:

```kotlin
val len = name?.length ?: 0
```

Not-null assertion (dangerous):

```kotlin
println(name!!.length)
```

Safe cast:

```kotlin
val x: Any = "Hello"
val y: String? = x as? String
```

---

# 7. Control Flow

## If Expression

```kotlin
val max = if (a > b) a else b
```

## When Expression

```kotlin
when (x) {
    0 -> println("Zero")
    1, 2 -> println("One or Two")
    in 3..10 -> println("Range")
    is String -> println("String")
    else -> println("Other")
}
```

## Loops

```kotlin
for (i in 1..5) println(i)

for (i in 10 downTo 1 step 2) println(i)

while (x > 0) x--
```

---

# 8. Functions

```kotlin
fun add(a: Int, b: Int): Int {
    return a + b
}
```

Single-expression:

```kotlin
fun multiply(a: Int, b: Int) = a * b
```

Default arguments:

```kotlin
fun greet(name: String = "Guest") {
    println("Hello $name")
}
```

Named arguments:

```kotlin
greet(name = "Chris")
```

---

# 9. Classes and Objects

```kotlin
class Person(val name: String, var age: Int) {
    fun introduce() {
        println("I'm $name and I'm $age years old")
    }
}
```

Usage:

```kotlin
val p = Person("Chris", 30)
p.introduce()
```

---

# 10. Data Classes

```kotlin
data class User(val id: Int, val email: String)
```

Auto-generates:

* toString()
* equals()
* hashCode()
* copy()

```kotlin
val u2 = u1.copy(email = "new@mail.com")
```

---

# 11. Inheritance and Interfaces

```kotlin
open class Animal {
    open fun speak() = println("Animal sound")
}

class Dog : Animal() {
    override fun speak() = println("Bark")
}
```

Interfaces:

```kotlin
interface Flyable {
    fun fly()
}
```

---

# 12. Visibility Modifiers

* public (default)
* private
* protected
* internal

```kotlin
internal class Engine
```

---

# 13. Collections

```kotlin
val nums = listOf(1, 2, 3)
val mutable = mutableListOf(1, 2, 3)

val map = mapOf("a" to 1, "b" to 2)
val set = setOf(1, 2, 3)
```

Iteration:

```kotlin
for (n in nums) println(n)
```

---

# 14. Functional Programming

```kotlin
val nums = listOf(1, 2, 3, 4)

val squares = nums.map { it * it }
val evens = nums.filter { it % 2 == 0 }
val sum = nums.reduce { a, b -> a + b }
```

Chaining:

```kotlin
nums.filter { it > 2 }.map { it * 10 }
```

---

# 15. Lambdas and Higher-Order Functions

```kotlin
val add: (Int, Int) -> Int = { a, b -> a + b }

fun operate(a: Int, b: Int, op: (Int, Int) -> Int): Int {
    return op(a, b)
}
```

---

# 16. Extension Functions

```kotlin
fun String.reverseText(): String {
    return this.reversed()
}

println("Kotlin".reverseText())
```

---

# 17. Generics

```kotlin
class Box<T>(val value: T)

val b = Box(10)
val s = Box("Hello")
```

Constraints:

```kotlin
fun <T : Number> square(x: T): Double {
    return x.toDouble() * x.toDouble()
}
```

---

# 18. Sealed Classes and Enums

```kotlin
sealed class Result {
    data class Success(val data: String) : Result()
    data class Error(val msg: String) : Result()
}
```

```kotlin
enum class Direction {
    NORTH, SOUTH, EAST, WEST
}
```

---

# 19. Object, Companion, and Singleton

```kotlin
object Database {
    fun connect() {}
}
```

```kotlin
class User {
    companion object {
        fun create() = User()
    }
}
```

---

# 20. Exceptions

```kotlin
try {
    val x = 10 / 0
} catch (e: Exception) {
    println(e.message)
} finally {
    println("Done")
}
```

Nothing throws by default (no checked exceptions).

---

# 21. File I/O

```kotlin
import java.io.File

val text = File("data.txt").readText()
File("out.txt").writeText("Hello")
```

---

# 22. Coroutines (Kotlin’s Killer Feature)

Add dependency:

```
org.jetbrains.kotlinx:kotlinx-coroutines-core
```

Basic coroutine:

```kotlin
runBlocking {
    launch {
        delay(1000)
        println("World")
    }
    println("Hello")
}
```

Async:

```kotlin
val result = async { heavyTask() }
println(result.await())
```

Suspend function:

```kotlin
suspend fun fetchData(): String {
    delay(1000)
    return "Data"
}
```

---

# 23. Kotlin + Java Interoperability

Call Java from Kotlin:

```kotlin
val list = ArrayList<String>()
```

Call Kotlin from Java:

```java
MyKt.myFunction();
```

Kotlin generates standard JVM bytecode.

---

# 24. Android Kotlin Basics

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

View binding, coroutines, and Jetpack Compose are Kotlin-first.

---

# 25. Multiplatform Kotlin

Targets:

* JVM
* Android
* JS
* Native (Windows/Linux/macOS/iOS)

Shared module example:

```kotlin
expect fun platformName(): String
```

---

# 26. Build Tools

Gradle Kotlin DSL:

```kotlin
plugins {
    kotlin("jvm") version "1.9.0"
}
```

Run:

```bash
./gradlew build
```

---

# 27. Best Practices

* Prefer `val` over `var`
* Use data classes heavily
* Avoid `!!`
* Use coroutines instead of threads
* Use sealed classes for state
* Favor immutability

---

# 28. Learning Path

Beginner:

* Syntax, null-safety, collections, classes

Intermediate:

* Coroutines, generics, FP, DSLs

Advanced:

* Multiplatform
* Compiler plugins
* Compose
* Performance tuning

---

# 29. Mini Project Ideas

* CLI task manager
* REST API with Ktor
* Android note app
* Multiplatform crypto tracker
* Coroutine-based web crawler
* Kotlin DSL builder

---

# 30. Kotlin vs Java (Quick Comparison)

| Kotlin              | Java            |
| ------------------- | --------------- |
| Null-safe           | Null-prone      |
| Coroutines          | Threads         |
| Concise             | Verbose         |
| Data classes        | Boilerplate     |
| Extension functions | Utility classes |
