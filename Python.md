

# Python Cheatsheet

## working

Check Installed Python Executables:<br>
```where python```

Windows comes with a Python launcher (```py```) that allows you to specify the version directly when running Python.<br>
To use Python 3.10, run:<br>
```py -3.10 --version```

Similarly, to use Python 3.7, run:<br>
```py -3.7 --version```




## Basics

https://opentechschool.github.io/python-beginners/en/getting_started.html#what-is-python-exactly

### Show installed version

`$ python3 --version`

### Create an interactive session

~~~
# Type "exit()" to exit.
$ python3
~~~

### Create a python script

`$ touch my-script.py`

### Execute the script

`$ python3 my-script.py`

### Make a python script executable

~~~
# Add "#!/usr/bin/python3" to first line of script (it MUST be the first line)
# Make it executable with "chmod + x myscript.py"
# Execute with "./myscript.py"
~~~

### Print a calculation to screen

`print(5 / 8)`

### Math operators

- +: addition
- -: subtraction
- *: multiplication
- /: division
- **: exponentiation
- %: modulo

### Comparison operators

- <: less than
- <=: less than or equal to
- &gt;: greater than
- &gt;=: greater than or equal to
- ==: equal
- !=: not equal

### Boolean operators

- {value1} and {value2}: returns true if both values are true. To use on numpy arrays: np.logical_and({comparison})
- {value1} and {value2}: returns true if either value is true. To use on numpy arrays: np.logical_or({comparison})
- not {value}: returns opposite of value. To use on numpy arrays: np. logical_not({comparison})

### Create a comment

`# This is a comment`

### Use random numbers

~~~
import random
print (random.randrange(1,10))
~~~

## Control statements

### If-then-else

~~~
if condition :
    expression
elif :
    expression
else :
    expression
~~~

### While loops

~~~
while condition :
    expression
~~~

### For loops

~~~
# General syntax
for var in seq :
    expression

# Loop through a range of integers
for x in range(10) :

# To enumerate indexes in an array
for x, y in enumerate(data) :
    print("x: " + str(x) + ", y: " + str(y))

# To loop dictionaries
for key, value in dic.items() :

# To loop 2d numpy arrays
for val in np.nditer(my2d_array) :

# To loop pandas dataframes
for label, row in df.iterrows() :
~~~

## Variables

- Must start with a letter (lowercase is better)
- Remaining letters can be letters, numbers, or underscores
- Case-sensitive

### Create an assign a variable

`{label} = {value}`

### Get the type of a variable

`type({variable})`

## Types

### Primitive types

- float
- int
- string (surround value with single or double quotes)
- bool (True or False)

### Non-primitive types

- Lists
- Tuples
- Dictionaries
- Sets
 
### Explicit type conversion

- float({variable})
- int({variable})
- str({variable})
- bool({variable})

## Lists

- A list is a named collection of values
- A list is a reference type
- Can contain any type
- Can contain different types
- Can contain lists
- Zero-based indexing
- index(): gets the index of the first element that matches input value
- count(): the number of time input value appears in list
- append(): adds an element to the list
- remove(): removes the first element of a list that matches input value
- reverse(): reverses the order of elements

### Indexing

~~~

# Basic example
ages = [2, 19, 34]

# Example with sub-list
names_and_ages = [["mary", 2], ["bob", 19], ["mark", 34]]

# Get item by index. This is the 3rd item in the list.
ages[2]

# Get item by negative index. This is the 3rd item in the list, starting at the last item and moving backwards (the last item is considered zero for this purpose.)
ages[-2]

# Get item from list that contains another list (a.k.a. subsetting.)
names_and_ages[2][0]

# Find index by string value
people = ["bob", "sally"]
index_people = people.index("sally")

~~~

### Slicing

~~~

# Slice from index 0.
ages[{start-index-inclusive}:{end-index-exclusive]

Returns first four items.
ages[:4]

# Slice to end. Returns everything from 4th item to last item.
ages[4:]

~~~

### Updating lists

~~~

# Basic example.
ages[0] = 3

# Updating with a slice.
ages[0:2] = [3, 20]

# Appending to a list.
names_and_ages + ["Sally", 14]
names_and_ages.append(some_other_list)

# Deleting from a list. Deletes item with index 2.
del(names_and_ages[2])

~~~

### Create a new list from an existing list

~~~

# This value-copies items from x into y. I.e. they don't share the same reference. Slicing also achieves this.
y = list(x)

# This retains the reference to the same memory location, so updates to y will be reflected in x.
y = x

~~~

## Dictionaries

- Dictionaries are like lists of key-value pairs
- Keys should be unique and immuatable
  
### Examples

~~~

# Example of names + ages
people = { "bob":34, "alice":14, "mark": 24 }

# Get bob's age
people["bob"]

# Get all the keys
people.keys()

# Update a value
people["mark"] = 25

# Check if a key exists. Returns true/false.
"bob" in people

# Add a key while assigning a value.
people["jeff"] = 64

# Remove a key.
del(people["jeff"])

~~~

## Tuples

- Similar to lists
- Immutable: values can't be modified

~~~

even_nums = (2, 4, 6)
a, b, c = even_nums
print(even_nums[1])

# Combine tubles using zip(), resulting in a iterator of tuples
odd_nums = {1, 3, 5}
z = zip(even_nums, odd_nums)

~~~

## Modules

### Examples of commonly used modules

- matplotlib: plotting
- pandas: loads data
- scikit-learn: ML
- scipy: stats
- nltk: manipulates text data
- statsmodels: stats
- seaborn: visualization
- numpy: maths
- math: maths

### Import modules

~~~

# Import pandas with an alias
import pandas as pd

# Selective import syntax
from math import pi

from matplotlib import pyplot as plt

# Pandas loads data
df = pd.read_csv('data.csv')

# Matplotlib plots it
plt.plot(df.columnA, df.columnB)
plt.show()

~~~

### Install a package

`$ pip3 install numpy`

### List installed packages

`$ pip3 list`

### Uninstall a package

`$ pip3 uninstall numpy`

## Functions

- Positional arguments are ordinary arguments, assigned by their positional location.
- Keyword arguments come after positional arguments, and are assigned with their label (e.g. arg='value')

### Some useful built-in functions

- help(): display help for another built-in function, e.g. help(max)
- print(): prints value to stdout
- type(): returns type of value
- len(): length of value
- max(): maximum value
- pow(base, exp): power
- upper(): convert to uppercase

### Docstrings

Describe a function

~~~
# Example
def do_something(argA, argB):
    """Explains what the function does.

    Args:
        argA (DataFrame): Explains argA.
        argB (iterable of str): Explains argB.

    Returns:
        Explains what is returned, if anything.

    Raises:
        Describes any intentionally returned error types.

    Notes:
        Additional notes.

    """
    The function code
~~~

### Custom functions

~~~

# Define the function
def raise_to_power(num, power):
    # Docstrings describe what the function does. 
    """Returns the power of a number."""

    new_value = num ** power
    return new_value

# Call the function
num = raise_to_power(4, 2)

# Return multiple values
def my_func():
    # Body code

    # Create a tuple to return multiple values
    return my_tuple

~~~

### Lambda functions

~~~

# Lambdas are "quick and dirty" functions

# Example
raise_to_power = lambda x, y: x ** y

# Call it
raise_to_power(2, 3)

~~~

### Scope

- Global: defined in main body
- Local: defined inside a function
- Built-in: in pre-defined built-ins module

## Error handling

~~~

# Explicitly raise an exception
raise ValueError("explanation here")

# Try to execute some code
try:
    {do something}

# To catch specific types of exceptions
except TypeError:
    {code that executes for a TypeError exception}

# To catch general exceptions
except:
    {code that executes if something went wrong}

~~~

## Iterators

- An object is iterable, it has an iter() method
- Examples: lists, strings, dictionaries, file connections

~~~

# Iterate over a list
employees = ["Bob", "Mark", "Sam"]
for employee in employees:
    print(employee)

# Iterate with a loop
for letter in "HelloWorld":
    print(letter)

# Iterate over a range of numbers
for number in range(5):
    print(number)

~~~
