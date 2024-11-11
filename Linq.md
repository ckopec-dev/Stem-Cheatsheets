
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

## Operators

[Microsoft Learn](https://learn.microsoft.com/en-us/dotnet/csharp/linq/standard-query-operators/filtering-data)
