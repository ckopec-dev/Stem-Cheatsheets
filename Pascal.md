
# 🧮 Pascal Programming Language — Comprehensive Tutorial

Pascal is a **procedural, strongly-typed language** created by *Niklaus Wirth* in 1970 to encourage **good programming practices and structured programming**. It became extremely popular in education and later evolved into **Object Pascal** (used by Delphi and FreePascal).

---

# 📚 Table of Contents

1. What is Pascal?
2. Installing a Pascal Compiler
3. Program Structure
4. Variables and Data Types
5. Input and Output
6. Operators
7. Control Flow
8. Procedures and Functions
9. Arrays, Records, and Sets
10. Strings
11. Pointers and Dynamic Memory
12. Files and File I/O
13. Units and Modular Programming
14. Object-Oriented Pascal
15. Error Handling
16. Building Real Programs
17. Best Practices
18. Learning Resources & Project Ideas

---

# 1️⃣ What is Pascal?

Pascal was designed to:

* Encourage **clean, readable code**
* Teach **structured programming**
* Enforce **type safety**

Modern Pascal variants:

* **FreePascal** (open source, cross-platform)
* **Delphi/Object Pascal** (commercial, RAD tools)

---

# 2️⃣ Installing a Pascal Compiler

### ✅ FreePascal (recommended)

Download from:
[https://www.freepascal.org](https://www.freepascal.org)

Verify installation:

```bash
fpc -iV
```

Compile a program:

```bash
fpc hello.pas
```

Run it:

```bash
hello
```

---

# 3️⃣ Pascal Program Structure

Minimal Pascal program:

```pascal
program HelloWorld;

begin
  writeln('Hello, world!');
end.
```

Important parts:

| Element     | Purpose              |
| ----------- | -------------------- |
| program     | Program name         |
| begin / end | Main code block      |
| ;           | Statement terminator |
| .           | End of program       |

---

# 4️⃣ Variables and Data Types

### Common Types

```pascal
var
  age: Integer;
  price: Real;
  letter: Char;
  name: String;
  isActive: Boolean;
```

### Assignments

```pascal
age := 25;
price := 9.99;
```

### Constants

```pascal
const
  Pi = 3.14159;
```

---

# 5️⃣ Input and Output

### Output

```pascal
writeln('Hello');
write('No newline');
```

### Input

```pascal
var name: String;

readln(name);
```

### Example

```pascal
write('Enter age: ');
readln(age);
writeln('You are ', age, ' years old.');
```

---

# 6️⃣ Operators

### Arithmetic

```pascal
+  -  *  /  div  mod
```

### Relational

```pascal
=  <>  <  >  <=  >=
```

### Logical

```pascal
and  or  not
```

---

# 7️⃣ Control Flow

### If Statement

```pascal
if age >= 18 then
  writeln('Adult')
else
  writeln('Minor');
```

### Case Statement

```pascal
case grade of
  'A': writeln('Excellent');
  'B': writeln('Good');
else
  writeln('Unknown');
end;
```

### Loops

#### For

```pascal
for i := 1 to 10 do
  writeln(i);
```

#### While

```pascal
while x < 10 do
  inc(x);
```

#### Repeat-Until

```pascal
repeat
  readln(x);
until x > 0;
```

---

# 8️⃣ Procedures and Functions

### Procedure

```pascal
procedure Greet(name: String);
begin
  writeln('Hello ', name);
end;
```

### Function

```pascal
function Square(x: Integer): Integer;
begin
  Square := x * x;
end;
```

Call:

```pascal
Greet('Chris');
y := Square(5);
```

---

# 9️⃣ Arrays, Records, and Sets

### Arrays

```pascal
var nums: array[1..5] of Integer;
nums[1] := 10;
```

### Records (Structs)

```pascal
type
  Person = record
    name: String;
    age: Integer;
  end;

var p: Person;
p.name := 'Alex';
```

### Sets

```pascal
var letters: set of Char;
letters := ['a','b','c'];
```

---

# 🔟 Strings

```pascal
var s: String;

length(s)
copy(s, 1, 3)
pos('a', s)
concat(s1, s2)
```

Example:

```pascal
s := 'Pascal';
writeln(length(s));
```

---

# 1️⃣1️⃣ Pointers and Dynamic Memory

```pascal
type
  PInt = ^Integer;

var
  p: PInt;

new(p);
p^ := 42;
dispose(p);
```

Dynamic arrays:

```pascal
var arr: array of Integer;
setlength(arr, 10);
```

---

# 1️⃣2️⃣ Files and File I/O

### Text File

```pascal
var f: Text;

assign(f, 'data.txt');
rewrite(f);
writeln(f, 'Hello File');
close(f);
```

### Reading

```pascal
reset(f);
readln(f, s);
```

---

# 1️⃣3️⃣ Units and Modular Programming

### Unit Example

```pascal
unit MathUtils;

interface
function Add(a,b: Integer): Integer;

implementation
function Add(a,b: Integer): Integer;
begin
  Add := a + b;
end;

end.
```

### Use in program

```pascal
uses MathUtils;
```

---

# 1️⃣4️⃣ Object-Oriented Pascal

```pascal
type
  TAnimal = class
    name: String;
    procedure Speak; virtual;
  end;

procedure TAnimal.Speak;
begin
  writeln('Animal sound');
end;
```

Inheritance:

```pascal
type
  TDog = class(TAnimal)
    procedure Speak; override;
  end;
```

---

# 1️⃣5️⃣ Error Handling

```pascal
try
  x := 10 div 0;
except
  writeln('Error occurred');
end;
```

---

# 1️⃣6️⃣ Real Program Example

```pascal
program Calculator;

var a,b: Integer;
    op: Char;

begin
  readln(a);
  readln(op);
  readln(b);

  case op of
    '+': writeln(a+b);
    '-': writeln(a-b);
    '*': writeln(a*b);
    '/': writeln(a/b);
  else
    writeln('Invalid operator');
  end;
end.
```

---

# 1️⃣7️⃣ Best Practices

✅ Use meaningful names
✅ Break programs into units
✅ Prefer functions/procedures
✅ Strong typing
✅ Avoid global variables
✅ Comment intent, not obvious code

---

# 1️⃣8️⃣ Project Ideas

* CLI calculator
* Address book
* File-based database
* Text adventure game
* Compiler/interpreter
* Terminal UI framework
* Retro DOS programs
* Data structure library

---

# 🎯 Summary

You now know:

✔ Pascal syntax
✔ Variables and types
✔ Structured programming
✔ Modular design
✔ OOP in Pascal
✔ File handling
✔ Real-world usage

