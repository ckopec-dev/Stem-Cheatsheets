
# Numpy Cheatsheet

## Basics

~~~

# Import module
import numpy as np

# Create lists
height = [1.55, 1.67, 1.34]
weight = [154, 134, 164]

# Create numpy arrays from lists
np_height = np.array(height)
np_weight = np.array(weight)

# Create a new numpy array from existing
bmi = np_weight / np_height ** 2

# Numpy arrays can only contain a single type.

# Returns array of True/False values
bmi > 23

# Returns array of values that satisfy the condition
bmi[bmi > 23]

# Returns numpy.ndarray
type(np_height)

# Create a 2d numpy array
np_2d = np.array([[1, 2], [3, 4]])

# Return the number of rows and columns
np_2d.shape

# Return an element
np_2d[0][1]

~~~

