
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

### Working with Pandas example

~~~

# Import libraries
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Load dataframe
df = pd.read_csv("MyCsv.csv")

# Plot data
sns.countplot(x="columnA", data=df)
plt.show()

~~~

### Working with 3d data

~~~

# Import libraries
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Load built-in dataset
tips = sns.load_dataset("tips")

# Create a dictionary to control hue colors. See documentation for available colors.
hue_colors = {"Yes": "black", "No: "red"}

# Plot data
sns.scatterplot(x="total_bill", y="tip", data=tips, hue="smoker", hue_order=["Yes","No"], palette=hue_colors)
plt.show()

~~~
