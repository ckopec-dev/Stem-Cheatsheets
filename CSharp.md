
# C# Cheatsheet

## Variables

### Value types

- bool: true/false
- byte: 8-bit unsigned int (0 to 255)
- char: character
- decimal: numerical type (useful for financial data)
- double: double precision floating point
- float: single precision floating point
- int: integer (-2147483658 to 2147483647)
- long: long integer (-9223372036854775808 to 9223372036854775807)
- sbyte: 8-bit signed int (-128 to 127)
- short: short integer (-32768 to 32767)
- uint: unsigned int (0 to 4294967295)
- ulong: unsigned long (0 to 18446744073709551615)
- ushort: unsigned short (0 to 65535)

### Assign a variable with scientific notation

~~~
float f = 5e20F;  
double d = 6e21D;  
~~~

### Casting types

- Implicit: happens automatically going from a smaller type to a larger type. E.g. char -> int -> long -> float -> double
- Explicit: put the type in parens in front of the variable, or by using Convert method.

### Create and assign members to an array

~~~
// Single-dimensional
string[] cars = new string[4];
string[] cars = new string[4] {"Volvo", "BMW", "Ford", "Mazda"};
string[] cars = new string[] {"Volvo", "BMW", "Ford", "Mazda"};
string[] cars = {"Volvo", "BMW", "Ford", "Mazda"};

// Multi-dimensional
int[,] table = new int[5, 5];
table[3, 4] = 19;

// Jagged (array of arrays)
int[][] j = new int[3,4];
j[0] = new int[2];
~~~

### Declare and assign multiple variables of the same type in the same statement

`int x = 5, y = 6, z = 7;`

## Control statements

~~~
// If then
if(condition)
  statement;
else if (condition)
  statement;
...
else (condition)
  statement;

// ?
bool expression ? true expression2 : false expression;

// Switch
switch(expression)
{
  case constant1:
    statement sequence
    break;
  case constant2:
    statement sequence
    break;
  default
    statement sequence
    break;
}

// For loop
for(initialization; condition; iteration)
{
  statement sequence
}

// While loop
while (condition) statement;

// Do-while loop
do
{
  statements;
} while (condition);

// Break out of a loop
break;

// Skip to next iteration of a loop
continue;

// Unconditional jump
goto label;
label:
  statements;
~~~
        
## Classes

### Declaration

Header consists of:
- The attributes and modifiers of the class
- The name of the class
- The base class (when inheriting from a base class)
- The interfaces implemented by the class
    
Body consists of:
- A list of member declarations written between the delimiters { and }.

~~~
// Simple delcaration example
public class Point
{
    public int X { get; }
    public int Y { get; }
    
    public Point(int x, int y) => (X, Y) = (x, y);
}

// Create a new instance of the Point class.
var p1 = new Point(0, 0);

// Create a factory class for generating many instances of the Point class.
public class PointFactory(int numberOfPoints)
{
    public IEnumerable<Point> CreatePoints()
    {
        var generator = new Random();
        for (int i = 0; i < numberOfPoints; i++)
        {
            yield return new Point(generator.Next(), generator.Next());
        }
    }
}

// Use the factory class.
var factory = new PointFactory(10);
foreach (var point in factory.CreatePoints())
{
    Console.WriteLine($"({point.X}, {point.Y})");
}
~~~

### Access modifiers

- Public: The code is accessible for all classes.
- Private: The code is only accessible within the same class.
- Protected: The code is accessible within the same class, or in a class that is inherited from that class. 
- Internal: The code is accessible within its own assembly, but not from another assembly. 

## Formatting

### How to Use placeholders in strings with string interpolation

~~~
string firstName = "John";
string lastName = "Doe";
string name = $"My full name is: {firstName} {lastName}";
~~~

## Compilation

### How to access command line args anywhere in a console app

`Environment.GetCommandLineArgs`

### How to use directives

~~~
static void Main(string[] args)
{
    for (int i = 0; i < 10; i++)
    {
        // Do something 

        #if BANANA

        Console.WriteLine("Banana number {0}", i);
        #warning Banana defined!

        #endif

        #if DEBUG // Special value that is defined by visual studio during compilation. No need to define it manually.

        Console.WriteLine("Processing i = {0}", i);

        #elif PRODUCTION // This value DOES need to be defined.

        // Do something

        #else

        // Do something

        #endif

        #if DEBUG && PRODUCTION

        // They shouldn't both be defined at the same time, so throw an error.
        #error Compiled with both DEBUG and PRODUCTION. Define only one or the other.

        #endif
    }
}
~~~

## IO

General process for working with a file
1. Open/create the file.
2. Set up a stream to to/from the file.
3. Read/write the stream.
4. Close the stream/file.
   
### Read/write text data

~~~
public static void Read()
{
    string buffer;
    StreamReader myFile = File.OpenText("text.txt");

    while ((buffer = myFile.ReadLine()) != null)
    {
        Console.WriteLine(buffer);
    }

    myFile.Close();
}

public static void ReadAll()
{
    string text = System.IO.File.ReadAllText("text.txt");
}

public static void Write()
{
    StreamWriter myFile = File.CreateText("text.txt");

    for (int i = 0; i < 10; i++)
    {
        myFile.WriteLine("{0}", i);
    }

    myFile.Close();
}

public static void WriteAll()
{
    System.IO.File.WriteAllText("filename.txt", “some text”);
}
~~~

### Read/write binary data

~~~
public static void Read()
{
    FileStream myFile = new FileStream("binary.dat", FileMode.Open);
    BinaryReader brFile = new BinaryReader(myFile);

    while (brFile.PeekChar() != -1)
    {
        Console.Write("<{0}>", brFile.ReadInt32());
    }

    brFile.Close();
    myFile.Close();
}

public static void Write()
{
    FileStream myFile = new FileStream("binary.dat", FileMode.Create);
    BinaryWriter bwFile = new BinaryWriter(myFile);

    for (int i = 0; i < 100; i++)
    {
        bwFile.Write(i);
    }

    bwFile.Close();
    myFile.Close();
}
~~~

## Functional techniques

### How to use relational patterns in switch statements

~~~
string WaterState(int tempInFahrenheit) =>
    tempInFahrenheit switch
    {
        (> 32) and (< 212) => "liquid",
        < 32 => "solid",
        > 212 => "gas",
        32 => "solid/liquid transition",
        212 => "liquid / gas transition",
    };

decimal CalculateDiscount(Order order) =>
    order switch
    {
        { Items: > 10, Cost: > 1000.00m } => 0.10m,
        { Items: > 5, Cost: > 500.00m } => 0.05m,
        { Cost: > 250.00m } => 0.02m,
        null => throw new ArgumentNullException(nameof(order), "Can't calculate discount on null order"),
        var someObject => 0m,
    };
~~~

## Multithreading

### Async and await

~~~
public class MultithreadingDemo
{
    static void Main()
    {
        Demo();
        Console.ReadLine();
    }

    public static void Demo()
    {
        // Wait for stove to heat before cooking.
        HeatStove().Wait();

        var task2 = CookBacon();
        var task3 = CookEggs();

        Task.WaitAll(task2, task3);
    }

    public static async Task HeatStove()
    {
        await Task.Run(() =>
        {
            Thread.Sleep(7500);
            Console.WriteLine("Stove heated.");
        });
    }

    public static async Task CookEggs()
    {
        await Task.Run(() =>
        {
            Thread.Sleep(3000);
            Console.WriteLine("Eggs cooked.");
        });
    }

    public static async Task CookBacon()
    {
        await Task.Run(() =>
        {
            Thread.Sleep(4000);
            Console.WriteLine("Bacon cooked.");
        });
    }
}
~~~

## Entity Framework Core

### Installing the ef extension

`$ dotnet tool install --global dotnet-if`

### Scaffold an existing database

`$ dotnet ef dbcontext scaffold {Connection_String} --project {Project_Name} Microsoft.EntityFrameworkCore.SqlServer --use-database-names --output-dir {Output_Directory_Name} --context {Context_Name} --verbose --force`
