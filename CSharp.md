
# C# Cheatsheet

## Classes

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
