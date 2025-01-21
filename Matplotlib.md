
# Matplotlib Cheatsheet

## Basics

~~~
# Import library
from matplotlib import pyplot as plt

# Create plot
plt.plot(x_values, y_values, label="Dataset M", color="tomato", linewidth=1, linestyle="-", marker="x")

# Add 2nd set of values
plt.plot(a_values, b_values, label="Dataset N", color="seagreen", linewidth=1)

# Show it. All specifications need to come before show()
plt.legend(color="green")
plt.show()

# Clear plot
# plt.clf()
~~~ 

## Windows 10 backend for rendering on Windows from code running on WSL

`$ sudo pip install tk`

## Specify backend

~~~
import matplotlib
matplotlib.use('TkAgg')
import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4])
plt.show()
~~~

## Labels and legends

~~~
plt.xlabel("This is the x axis label")
plt.ylabel("This is the y axis label")
plt.title("This is the title", fontsize=20)

# Add arbitrary text
plt.text(xcoord, ycoord, "Some text")

# Customize ticks. First param is the tick values, 2nd param is the tick labels.
plt.yticks([0, 2, 4, 6, 8, 10], ['0', '2B', '4B', '6B', '8B', '10B'])
~~~

## Styles

~~~
# Add before any other plotting code
plt.style.use('seaborn')

# Show all available styles
print(plt.style.available)
~~~

## Types of plots

### Scatter plots

Good for an unordered collection of 2d data.

~~~
# Give markers transparency by setting alpha. 
plt.scatter(x_values, y_values, alpha=0.1)
~~~

### Bar charts

Good for a comparison of categorical data.

~~~
# For vertical bar chart
plt.bar(x_values, y_values)

# For horizontal bar chart
plt.barh(x_values, y_values)

# Show error bars (for example, from standard deviation of data)
plt.bar(x_values, y_values, yerr=df.error)
~~~

### Stacked bar charts

~~~
plt.bar(x_values, y_values)
plt.bar(x_values, z_values, bottom=y_values)
~~~

### Histograms

Visualizes a distribution of values in a dataset.

~~~
# By default, 10 bins are created.
plt.hist(y_values, bins=40)

# Limit values used for creating the histogram.
plt.hist(y_values, range=(5, 15))

# Normalization reduces the height of each bar by a constant factor.
# Useful for comparing two histograms with different ranges.
plt.hist(y.values, density=True)
plt.hist(z.values, density=True)
~~~
