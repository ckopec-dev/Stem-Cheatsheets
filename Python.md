
# Python Cheatsheet

## Basics

### Create an interactive session

~~~

# Type "exit()" to exit.
$ python3

~~~

### Create a python script

`$ touch my-script.py`

### Execute the script

`$ python3 my-script.py`

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
- >: greater than
- >=: greater than or equal to
- ==: equal
- !=: not equal

### Boolean operators

- {value1} and {value2}: returns true if both values are true. To use on numpy arrays: np.logical_and({comparison})
- {value1} and {value2}: returns true if either value is true. To use on numpy arrays: np.logical_or({comparison})
- not {value}: returns opposite of value. To use on numpy arrays: np. logical_not({comparison})

### Create a comment

`# This is a comment`

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

# To enumerate indexes in an array
for x, y in enumerate(data) :
    print("x: " + str(x) + ", y: " + str(y))

# To loop dictionaries
for key, value in dic.items() :

# To loop 2d numpy arrays
for val in np.nditer(my2d_array) :

~~~

## Variables

- Must start with a letter (lowercase is better)
- Remaining letters can be letters, numbers, or underscores
- Case-sensitive

### Create an assign a variable

`{label} = {value}`

### Get the type of a variable

`type({variable})`

### Some common types

- float
- int
- string (surround value with single or double quotes)
- bool (True or False)

### Type conversion

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

