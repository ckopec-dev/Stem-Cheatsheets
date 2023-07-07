
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

### Relational scatter plots

Similar to scatter plot, except with the ability to create subplots

~~~

# Imports
import seaborn as sns
import matplotlib.pyplot as plt

# Load built-in dataset
tips = sns.load_dataset("tips")

# Plot data. Creates two plots next to each other (in columns). To arrange in rows, use row param.
sns.relplot(x="total_bill", y="tip", data=tips, kind="scatter", col="smoker", col_wrap=2, col_order=["Thur","Fri","Sat","Sun"])
plt.show()

# Use size and hue.
sns.relplot(x="total_bill", y="tip", data=tips, kind="scatter", size="size", hue="size")
plt.show()

# Use point style.
sns.relplot(x="total_bill", y="tip", data=tips, kind="scatter", hue="smoker", style="smoker")
plt.show()

# Set transparency. 0 = 100% transparent, 1 = opaque.
sns.relplot(x="total_bill", y="tip", data=tips, kind="scatter", alpha=0.4)
plt.show()

~~~

### Relational line plots

~~~

# Similar to scatter plots. Use kind="line".

sns.relplot(x="hour", y="brightness", data=sunlight, kind="line", hue="location", markers=True, dashes=False)

# If there are multiple records per x value, the y values are aggregated into a mean and a confidence interval is displayed. To show the standard deviation, set ci="sd". To turn it off, set ci=None.

~~~

### Categorical plots 

~~~

# Involve a categorical variable
# Used to compare groups

# Imports
import seaborn as sns
import matplotlib.pyplot as plt

# Data load not shown

# Create list to specify category order
category_order = [4, 6, 8, 12]

# Create count plot
sns.catplot(x="cylinders", data=cars, kind="count", order=category_order)
plt.show()

# To create a category bar plot, set kind="bar" and set y={y-values}.
# Bar plots display mean of quantitative variable per category (95% confidence interval).

~~~

