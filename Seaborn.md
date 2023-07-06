
# Seaborn Cheatsheet

Seaborn is a Python data visualization library built on top of matplotlib.

## Installation and configuration

### Install

`$ pip3 install seaborn`

## Basic example

~~~

# Import libraries
import seaborn as sns
import matplotlib.pyplot as plt

# Create data
heights = [62, 66, 75]
weights = [140, 122, 163]

# Plot data
sns.scatterplot(x=heights, y=weights)
plt.show()

~~~

### Count plot example

~~~

# Import libraries
import seaborn as sns
import matplotlib.pyplot as plt

# Create data
gender = ["F", "F", "M", "F", "M]

# Plot data
sns.countplot(x=gender)
plt.show()

~~~


