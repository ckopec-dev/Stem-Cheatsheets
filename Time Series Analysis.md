
# Time Series Analysis Cheatsheet

## Overview

Time series data is time-ordered data.

## Useful Pandas tools

### Change a string index to a datetime

`dataframe.index = pandas.to_datetime(dataframe.index)`

### Plot data

`dataframe.plot()`

### Slice data

`dataframe['column']`

### Merge dataframes

`dataframe1.join(dataframe2)`

### Resample data (e.g. convert daily data to weekly data)

`dataframe = dataframe.resample(rule='W').last()`

### Compute percent change

`dataframe['column'].pct_change()`

### Compute differences

`dataframe['column'].diff()`

### Compute correlation

`dataframe['column1'].corr(data['column2'])`

### Compute autocorrelation

`dataframe['column'].autocorr()`

### Example of loading + visualizing Google Trends data

~~~python
import pandas as pd
import matplotlib.pyplot as plt

# Load data into a Panda dataframe (not shown)

# Convert the data index to datetime
df.index = pd.to_datetime(df.index)

# Plot the time series
df.plot(grid=True)
plt.show()
~~~
