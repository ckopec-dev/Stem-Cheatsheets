
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

~~~ 

## Labels and legends

~~~

plt.xlabel("This is the x axis label")
plt.ylabel("This is the y axis label")
plt.title("This is the title", fontsize=20)

# Add arbitrary text
plt.text(xcoord, ycoord, "Some text")

~~~

## Styles

~~~

# Add before any other plotting code
plt.style.use('seaborn')

# Show all available styles
print(plt.style.available)

~~~
