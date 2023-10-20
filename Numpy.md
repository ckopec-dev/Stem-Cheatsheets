
# Numpy Cheatsheet

## Basics

### Import module
`import numpy as np`

### Create lists

~~~
height = [1.55, 1.67, 1.34]
weight = [154, 134, 164]
~~~

### Create numpy arrays from lists. Numpy arrays can only contain a single type, unlike lists.

~~~

np_height = np.array(height)
np_weight = np.array(weight)

~~~

### Create a new numpy array from existing
`bmi = np_weight / np_height ** 2`

### Returns array of True/False values
`bmi > 23`

### Returns array of values that satisfy the condition
`bmi[bmi > 23]`

### Returns numpy.ndarray
`type(np_height)`

### Create a 2d numpy array
`np_2d = np.array([[1, 2], [3, 4]])`

### Return the number of rows and columns
`np_2d.shape`

### Return an element
`np_2d[0][1]`

### Subset a 2d array

~~~
np_2d[0][2]     // Returns 1st row, 3rd column
np_2d[0, 2]     // Returns same object as above
np_2d[:, 1:3]   // Returns all rows, columns at index 1 through (but not including) 3
np_2d[1, :]     // Returns 2nd row and all columns
~~~

## Random numbers

### Set the random number seed
`np.random.seed(123)`

### Generate a random number
`np.random.rand()`

### Generate a 0 or 1
`np.random.randint(0,2)`
