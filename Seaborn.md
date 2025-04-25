
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

# Add a trendline
sns.regplot(x="total_bills", y="tip", data=tips, ci=None)
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

### Box plots

~~~

# Shows the distribution of quantitative data
# Colored box represents the 25-75th percentile
# The line inside the box represents the median
# The whiskers represent the spread. They extend to 1.5 times the interquartile range (change by setting whis={multiplier}, or whis=[{lower-percentile},{upper-percentile}])
# Floating points represent outliers (omit with sym="")

# Imports
import seaborn as sns
import matplotlib.pyplot as plt

# Load built-in dataset
tips = sns.load_dataset("tips")

# Create box plot
sns.catplot(x="time", y="total_bill", data=tips, kind="box")
plt.show()

~~~

### Point plots

~~~

# The mean of quantitative variable
# Vertical bars represent 95% confidence intervals

# Imports
import seaborn as sns
import matplotlib.pyplot as plt
import numpy import median

# Data load not shown

# Create point plot
sns.catplot(x="{x-valyes}, y={y-values}, data=df; kind="point")
plt.show()

# Set join=False to remove lines between categories
# To use median instead of mean, set estimator=median
# To add caps to the end of the confidence intervals, set capsize={cap-size}

~~~

### Figure styles

- Changes background and axes, among other things
- Presets: white (the default), dark, whitegrid, darkgrid, ticks
- Set via sns.set_style()

### Palette styles

- Changes the main elements of the plot
- Set via sns.set_palette()
- Diverging palettes provide a transition from one color to another contrasting color
- Sequential palettes are one or two blended colors, moving from light to dark values

### Create a custom palette

~~~

color_names = ["red", etc]
sns.set_palette(color_names)

hex_codes = ['#FBB4AE', etc]
sns.set_palette(hex_codes)

~~~

### Change plot scale

- Changes scale of plot elements and labels
- Set via sns.set_context()
- Presets: paper (the default), notebook, talk, poster

### Seaborn objects

~~~
# Seaborn crates 2 types of objects: FacetGrid (rel and cat plots) and AxesSubplot (box and line plots)
# To view the type:

g = sns.relplot(x="height", y="weight", data=df)
type(g)

# Set the title on a FacetGrid with a custom height (y)
g.fig.suptitle("My Title", y=1.05)

# Set the title on an AxesSubplot
g.set_title("My Title", y=1.05)

# Set the titles on a subplot
g.set_titles("Title {variable_name}")

# Add axis labels
g.set(xlabel="X Label", ylabel="Y Label")

# Rotate x-axis labels
plt.xticks(rotation=90)
~~~
