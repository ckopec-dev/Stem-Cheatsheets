
# Linq Cheatsheet

## Resources 

- [Microsoft](https://learn.microsoft.com/en-us/dotnet/csharp/linq/)

## Syntax

### Lambda

~~~
string[] words = {"The", "quick", "brown", "fox"};
var shortWords = words.Where(i => i.Length < 4);
~~~

### Query

~~~
string[] words = {"The", "quick", "brown", "fox"};
var shortWords = From i in words where i.Length < 4 select i;
~~~

### Get all files in a directory

```csharp
using System;
using System.IO;
using System.Linq;

class Program
{
    static void Main()
    {
        string path = @"C:\MyFolder";

        var files = Directory.EnumerateFiles(path);

        foreach (var file in files)
        {
            Console.WriteLine(file);
        }
    }
}
```

---

### Filter files by extension

```csharp
var txtFiles = Directory.EnumerateFiles(path, "*.txt")
                        .Select(f => new FileInfo(f))
                        .OrderBy(f => f.Name);

foreach (var file in txtFiles)
{
    Console.WriteLine($"{file.Name} ({file.Length} bytes)");
}
```

---

### Search files containing specific text

```csharp
var filesWithKeyword = Directory.EnumerateFiles(path, "*.txt")
    .Where(file => File.ReadAllText(file).Contains("keyword"));

foreach (var file in filesWithKeyword)
{
    Console.WriteLine(file);
}
```

---

### Get the largest file

```csharp
var largestFile = Directory.EnumerateFiles(path)
    .Select(f => new FileInfo(f))
    .OrderByDescending(f => f.Length)
    .FirstOrDefault();

if (largestFile != null)
{
    Console.WriteLine($"Largest file: {largestFile.Name} ({largestFile.Length} bytes)");
}
```

---

### Group files by extension

```csharp
var grouped = Directory.EnumerateFiles(path)
    .Select(f => new FileInfo(f))
    .GroupBy(f => f.Extension);

foreach (var group in grouped)
{
    Console.WriteLine($"Extension: {group.Key}");
    foreach (var file in group)
    {
        Console.WriteLine($"  {file.Name}");
    }
}
```

## Operators

[Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/filtering-data)

