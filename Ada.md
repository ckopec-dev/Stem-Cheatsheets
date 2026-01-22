
# ✅ Ada Programming Language – Comprehensive Tutorial

---

# 1. What Is Ada?

**Ada** is a strongly typed, compiled programming language designed for:

* Safety-critical systems (avionics, medical, rail, defense)
* Real-time and embedded systems
* Large, long-lived software projects

Key goals of Ada:

* Reliability
* Maintainability
* Readability
* Concurrency support
* Compile-time error detection

Ada was originally commissioned by the U.S. Department of Defense and is now standardized as **ISO Ada** (latest major revision: Ada 2012 / Ada 2022).

---

# 2. Installing Ada

The most common compiler is **GNAT** (GNU Ada).

### Windows / Linux / macOS

Install via:

* **GNAT Community Edition**
  [https://www.adacore.com/download](https://www.adacore.com/download)

Or on Linux:

```bash
sudo apt install gnat
```

Verify:

```bash
gnatmake -v
```

---

# 3. Your First Ada Program

Create a file: `hello.adb`

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Hello is
begin
   Put_Line ("Hello, Ada!");
end Hello;
```

Compile and run:

```bash
gnatmake hello.adb
./hello
```

---

# 4. Program Structure

Every executable Ada program has:

* `with` → imports a package
* `procedure` → main entry point
* `begin ... end` → executable section

Example layout:

```ada
with Ada.Text_IO;

procedure Main is
   -- declarations
begin
   -- statements
end Main;
```

File name must match the main procedure:
`main.adb` → `procedure Main`

---

# 5. Variables and Types

Ada is **strongly typed**.

```ada
Age    : Integer := 25;
Price  : Float   := 19.99;
Letter : Character := 'A';
Name   : String := "Chris";
```

Constants:

```ada
Pi : constant Float := 3.14159;
```

Ada will not allow unsafe conversions:

```ada
X : Integer := 5;
Y : Float := Float(X);  -- explicit conversion required
```

---

# 6. Input and Output

```ada
with Ada.Text_IO; use Ada.Text_IO;
with Ada.Integer_Text_IO; use Ada.Integer_Text_IO;

procedure IO_Demo is
   Age : Integer;
begin
   Put ("Enter age: ");
   Get (Age);
   Put_Line ("You entered: " & Integer'Image(Age));
end IO_Demo;
```

---

# 7. Control Flow

### If

```ada
if Age >= 18 then
   Put_Line ("Adult");
elsif Age >= 13 then
   Put_Line ("Teen");
else
   Put_Line ("Child");
end if;
```

### Case

```ada
case Day is
   when 1 => Put_Line ("Monday");
   when 2 => Put_Line ("Tuesday");
   when others => Put_Line ("Unknown");
end case;
```

### Loops

```ada
for I in 1 .. 10 loop
   Put_Line (Integer'Image(I));
end loop;
```

```ada
while X < 10 loop
   X := X + 1;
end loop;
```

```ada
loop
   exit when Done;
end loop;
```

---

# 8. Procedures and Functions

### Procedure (no return)

```ada
procedure Greet (Name : String) is
begin
   Put_Line ("Hello " & Name);
end Greet;
```

### Function (returns value)

```ada
function Square (X : Integer) return Integer is
begin
   return X * X;
end Square;
```

Use:

```ada
Result := Square(5);
```

---

# 9. Packages (Ada’s Modular System)

Ada uses **packages** instead of classes or modules.

### Specification (interface)

`math_utils.ads`

```ada
package Math_Utils is
   function Square (X : Integer) return Integer;
end Math_Utils;
```

### Body (implementation)

`math_utils.adb`

```ada
package body Math_Utils is
   function Square (X : Integer) return Integer is
   begin
      return X * X;
   end Square;
end Math_Utils;
```

Use it:

```ada
with Math_Utils;

X := Math_Utils.Square(4);
```

---

# 10. Strong Typing and Subtypes

```ada
subtype Age is Integer range 0 .. 130;

My_Age : Age := 42;  -- compiler enforces range
```

Custom types:

```ada
type Meters is new Float;
type Seconds is new Float;

Speed : Meters := 10.0; -- Seconds incompatible
```

This prevents whole classes of bugs.

---

# 11. Records (Structs)

```ada
type Person is record
   Name : String(1..20);
   Age  : Integer;
end record;

P : Person := (Name => "Chris               ", Age => 35);
```

Access:

```ada
Put_Line(P.Name);
```

---

# 12. Arrays

```ada
type Int_Array is array (1 .. 5) of Integer;
A : Int_Array := (1,2,3,4,5);
```

Multidimensional:

```ada
type Matrix is array (1..3, 1..3) of Integer;
```

---

# 13. Enumerations

```ada
type Color is (Red, Green, Blue);

C : Color := Red;

if C = Red then
   Put_Line("Stop");
end if;
```

---

# 14. Pointers (Access Types)

```ada
type Int_Ptr is access Integer;

P : Int_Ptr := new Integer'(42);
Put_Line(Integer'Image(P.all));
```

Ada forces explicit memory access and deallocation.

---

# 15. Error Handling (Exceptions)

```ada
begin
   X := A / B;
exception
   when Constraint_Error =>
      Put_Line("Division by zero");
end;
```

Custom exceptions:

```ada
My_Error : exception;
raise My_Error;
```

---

# 16. Concurrency (Tasks)

Ada has **built-in concurrency**, not libraries.

```ada
task Printer;

task body Printer is
begin
   for I in 1..5 loop
      Put_Line("Running");
      delay 1.0;
   end loop;
end Printer;
```

Tasks start automatically.

---

# 17. Protected Objects (Thread-Safe Data)

```ada
protected Counter is
   procedure Increment;
   function Value return Integer;
private
   C : Integer := 0;
end Counter;
```

```ada
protected body Counter is
   procedure Increment is
   begin
      C := C + 1;
   end Increment;

   function Value return Integer is
   begin
      return C;
   end Value;
end Counter;
```

This is Ada’s built-in mutex system.

---

# 18. Real-Time Programming

Ada supports real-time features:

```ada
delay 2.5;
```

```ada
pragma Priority(10);
```

```ada
with Ada.Real_Time;
```

Used heavily in embedded and safety systems.

---

# 19. Generics (Templates)

```ada
generic
   type T is private;
function Swap (A, B : in out T);
```

```ada
function Swap (A, B : in out T) is
   Temp : T := A;
begin
   A := B;
   B := Temp;
end Swap;
```

Instantiate:

```ada
function Int_Swap is new Swap(Integer);
```

---

# 20. Object-Oriented Ada

Ada supports OOP using tagged types.

```ada
type Animal is tagged record
   Age : Integer;
end record;

procedure Speak (A : Animal);
```

Inheritance:

```ada
type Dog is new Animal with record
   Breed : String(1..10);
end record;
```

Polymorphism supported through dispatching.

---

# 21. Building Larger Projects

GNAT project files:

```bash
gnatmake main.adb
```

For professional projects:

```bash
gprbuild project.gpr
```

Ada encourages:

* Clean package boundaries
* Strict contracts
* Long-term maintainability

---

# 22. Why Ada Is Still Used

Ada dominates in domains where failure is unacceptable:

* ✈️ Avionics
* 🚄 Rail control
* 🏥 Medical devices
* 🚀 Space systems
* 🔐 Defense and cryptography

Key advantages:

* Compile-time correctness
* Strong concurrency model
* Formal verification support
* Predictable real-time behavior

---

# 23. Example: Complete Program

```ada
with Ada.Text_IO; use Ada.Text_IO;

procedure Counter_App is
   Count : Integer := 0;
begin
   for I in 1..10 loop
      Count := Count + 1;
      Put_Line("Count:" & Integer'Image(Count));
   end loop;
end Counter_App;
```

---

# 24. Where to Go Next

To truly master Ada:

* Data representation clauses
* SPARK Ada (formally verified Ada)
* Embedded Ravenscar profile
* Distributed systems (Ada DSA)
* Real-time scheduling
* GNAT project system

---

# 25. Summary

You now understand:

* Ada syntax and structure
* Strong typing philosophy
* Modular package system
* Concurrency and safety
* Real-time and embedded features
* Object-oriented Ada

Ada is less about writing code fast — and more about writing **correct systems that last decades**.

