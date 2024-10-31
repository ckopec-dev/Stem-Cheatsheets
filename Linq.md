
# Linq Cheatsheet

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
