
# Pandas Cheatsheet

Pandas is a python library for working with data (analyzing, cleaning, importing/exporting, and manipulating.)

## Basics

### Installation
`$ pip3 install pandas`

### Import pandas
`import pandas as pd`

### Check version
`print(pd.__version__)`

## Series

A Pandas Series is analogous to a column in a database table.
It is a one-dimensional array consisting of a single type.

~~~
import pandas as pd

s = [1, 3, 5]
my_series = pd.Series(s)
print(my_series)
~~~

### The values are labeled with their zero-based index number, unless specified.  
`print(my_series[0])`

### To customize the labels

~~~
import pandas as pd

s = [1, 3, 5]
my_series = pd.Series(s, index = ["a", "b", "c"])
print(my_series)
~~~

### Create a series from a dictionary (the keys become the labels)

~~~
import pandas as pd

s = {"a": 1, "b": 3, "c": 5}
my_series = pd.Series(s)
print(my_series)
~~~

### Create a series from a dictionary using only select keys

~~~
import pandas as pd

s = {"a": 1, "b": 3, "c": 5}
my_series = pd.Series(s, index = ["a", "b"])
print(my_series)
~~~

## DataFrames

A Pandas DataFrame is analogous to a database table, i.e. a 2d structure with rows and columns.

~~~
import pandas as pd

data = {
    "height": [45, 56, 43],
    "weight": [12, 45, 23]
}

# Create DataFrame from a dictionary
df = pd.DataFrame(data)
print(df)

# Create DataFrame from a dictionary of lists
dict_of_lists = {
    "name": ["Sam", "Bob"],
    "age": [45, 32]
}
df = pd.DataFrame((dict_of_Lists)
~~~

### Locate a row by row index
`print(df.loc[0])`

When using single brackets, the result is a Series.

### Locate multiple rows by row indexes
`print(df.loc[[0, 1, 2]])`

When using double brackets, the result is a DataFrame.

### Add a custom index to give each row an indentifier/name.

~~~
import pandas as pd

data = {
    "height": [45, 56, 43],
    "weight": [12, 45, 23]
}

df = pd.DataFrame(data, index  = ["bob", "mary", "juan"])
print(df)
~~~

### Return a row by index name
`print(df.loc["bob"])`

## Reading file data

### From/To CSV

~~~
import pandas as pd

df = pd.read_csv('data.csv')

# This will print the first 5 and last 5 rows by default
# To override, set pd.options.display.max_rows = n
print(df)

# This will print everything
print(df.to_string())

# Export
df.to_csv("MyCsv.csv")

# To load in chunks (if the file is too big to load into memory, for example)
for chunk in pd.read_csv('data.csv', chunksize=1000):
~~~

### From JSON

~~~
import pandas as pd

df = pd.read_json('data.json')
print(df)
~~~

## Disply the content

### Display the full content of each column, regardless of its width.
~~~
pd.set_option("display.max_colwidth", None)
~~~

## Analyzing data

### Show first n rows. If n is not specified, it will return 5 by default.
`print(df.head(n))`

### Show last n rows. If n is not specified, it will return 5 by default.
`print(df.tail(n))`

### Show dataframe info
`print(df.info())`

### Print the number of rows and columns
`print(df.shape)`

### Show summary stats
`print(df.describe())`

### Get 2d numpy array of values
`df.values`

### Get index of column names
`df.columns`

### Get index of rows
`df.index`

## Data cleaning

### Show missing values

~~~
import pandas as pd

df = pd.read_csv("data.csv")

# Show a boolean for every cell (not very helpful)
print(df.isna())

# Show a boolean for each column. True if there are any missing values in that column.
print(df.isna().any())

# Show a count of missing values for each column.
print(df.isna().sum())
~~~

### Remove rows with one or more empty cells 

~~~
import pandas as pd

df = pd.read_csv("data.csv")

# The dropna method returns a new dataframe
new_df = df.dropna()

# To update the dataframe in place and return null
df.dropna(inplace = True)
~~~

### Replace empty values with new value

~~~
import pandas as pd

df = pd.read_csv("data.csv")

# Replace null values with 123
df.fillna(123, inplace = True)

# To only replace the null values in a specific column
df["ColumnA"].fillna(123, inplace = True)

# To replace the null values with the mean of all the values in given column
m = df["ColumnA"].mean()
df["ColumnA"]fillna(m, inplace = True)
~~~

### Replacing data in the wrong format

~~~
import pandas as pd

df = pd.read_csv("data.csv")
df["Date"] = pd.to_datetime(df["Date"])

# Now remove rows with a null date
df.dropna(subset=["Date"], inplace = True)
~~~

### Replace data in a specific row and column
`df.loc[15, "ColumnA"] = 123`

### To loop through all rows and replace or delete them with a rule

~~~
for i in df.index:
    if df.loc[i, "ColumnA"] > 100:

        # Replace
        df.loc[i, "ColumnA"] = 100

        # Delete
        df.drop(i, inplace = True)
~~~

### Deduplicate rows

~~~
import pandas as pd

df = pd.read_csv("data.csv")

# Show duplicates (True or False for every row)
print(df.duplicated())

# Remove them
df.drop_duplicates(inplace = True)
~~~

## Show correlations between columns

Result varies between -1 (perfect negative correlation) to 1 (perfect positive correlation).
A "good" correlation is >= 0.6 or <= -0.6.

`print(df.corr())`

## Sorting DataFrames

### Sort a single column

~~~
import pandas as pd

df = pd.read_csv("data.csv")

# Ascending (the default)
df.sort_values(by=["ColumnA"], inplace=True)

# Descending
df.sort_values(by=["ColumnA"], inplace=True, ascending=False)
~~~

### Sort by multiple columns

~~~
import pandas as pd

df = pd.read_csv("data.csv")

# All ascending
df.sort_values(by=["ColumnA", "ColumnB"], inplace=True)

# Mixed
df.sort_values(by=["ColumnA", "ColumnB"], inplace=True, ascending=[False, True])
~~~

### Sort by index

~~~
import pandas as pd

data = {
    "height": [45, 56, 43],
    "weight": [12, 45, 23]
}

df = pd.DataFrame(data, index  = ["bob", "mary", "juan"])

# Ascending (the default)
df.sort_index()

# Descending
df.sort_index(ascending=False)
~~~

## Aggregation

### Show various stats of a column

- mean()
- median()
- mode()
- min()
- max()
- var()
- std()
- sum()
- quantile()
- agg({custom-function-name}): this allows custom aggregate functions to be used like native functions
- cumsum(): cummulative sum - returns a number for each row of the dataframe, cumulatively summed
- cummax()
- cummin()
- cumprod()

### Count 

`df.["columnA"].value_counts(sort=True)`

### Count proportions of total

`df.["columnA"].value_counts(normalize=True)`

### Group summaries
`df.groupby("columnA")["columnB"].mean()`

### Pivot tables

~~~
# By default, aggregation is by mean
df.pivot_table(values="columnA", index="columnB")

# Specify an aggregation
df.pivot_table(values="columnA", index="columnB", aggfunc=np.median)

# Specify multiple aggregations
dogs.pivot_table(values="columnA", index="columnB", aggfunc=[np.median, np.mean])

# Pivot by two variables.
# fill_value prevents NaN results
# margins creates a final column containing the mean of values for given row (not including 0 values)
dogs.pivot_table(values="columnA", index="columnB", columns="columnC", fill_value=0, margins=True)
~~~

## Feature engineering

### Adding a new derived column
`df["ColumnA"] = df["ColumnB"] / 100`

## Subsetting

### Create a new DataFrame with selected columns of an existing DataFrame
`new_df = df[["ColumnA", "ColumnB"]]`

### Create a new DataFrame with filtered rows of an existing DataFrame
`new_df = df[df["ColumnA"] > 100]`

### Create a new DataFrame with specific columns and filtered rows of an existing DataFrame
`new_df = df.loc[df["ColumnA"] > 100, "ColumnB"]`

### Create a new DataFrame with specific row and column indexes
`new_df = df.iloc[9:25, 2:5]`

## Data visualization

### Histograms

Creates buckets of variable counts.

~~~
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("data.csv")

# Use defaults
df["ColumnA"].hist()

# Change number of bins
df["ColumnA"].hist(bins=5)
plt.show()
~~~

### Bar plots

Show relationship between a categorical variable and a numeric variable.

~~~
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("data.csv")

# Use defaults
df.plot(kind="bar")

# Add a title
df.plot(kind="bar", title="title")
plt.show()
~~~

### Line plots

For visualizing changes in numeric variables over time.

~~~
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("data.csv")

# Use defaults
df.plot(x="columnA", y="columnB", kind="line")

# Rotate x-axis labels to make them easier to read
df.plot(x="columnA", y="columnB", kind="line", rot=45)
plt.show()
~~~

### Scatter plots

For visualizing relationships between two numeric variables.

~~~
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("data.csv")

# Use defaults
df.plot(x="columnA", y="columnB", kind="scatter")
plt.show()
~~~

### Stacking plots

~~~
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv("data.csv")

# 1st plot. Make semi-transparent with alpha
df["ColumnA"].hist(alpha=0.7)

# 2nd plot
df["ColumnB"].hist(alpha=0.7)

# Legend to distinguish the two
plt.legend(["Plot1", "Plot2"])

plt.show()
~~~

## Merging data

### Combining data from dataframes with different columns

~~~
# This combines the wards DF with the census DF, joining on the ward column in a 1-1 relationship.
# If the same column appears in both DFs, each will be given a suffix to distinguish them.
wards_census = wards.merge(census, on="ward")

# Specify the suffixes
wards_census = wards.merge(census, on="ward", suffixes=("_ward", "_cen"))

# For a 1-n relationship, same general syntax applies, but now rows with duplicate data will appear.

# Example with a left join
movies_taglines = movies.merge(taglines, on="id", how="left")

# Example with a right join, and key columns that are not named identially
tv_movies = movies.merge(tv_genre, how="right", left_on="id", right_on="movie_id")

# Example of a self join
original_sequels = sequels.merge(sequels, left_on="sequel", right_on="id", suffixes=("_org", "_seq"))
~~~

### Combining multiple dataframes

~~~
# Merge on two columns
grants.merge(licenses, on=["address", "zip"])

# Merge multiple dfs
grants_licenses_ward = grants.merge(licenses, on["address", "zip").merge(wards, on="ward", suffixes("_bus", "_ward"))
~~~

### Join types

- Inner: returns all rows that have matching key vales in both tables (dataframes)
- Left: returns all rows in left table, and rows in the right table that have matching key records
- Right: returns all rows in right table, and rows in the left table that have matching key records
- Outer: returns all rows from both left and right tables
- Self: merges a table into itself
- Semi: similar to an inner join, but returns only columns from the left table, and no duplicates
- Anti:  returns all rows in left table that do not have matching rows in the right table

### Vertical concatenation

~~~
# Basic exmaple of combining multiple dataframes vertically
df = pd.concat([inv_jan, inv_feb, inv_mar])

# Ignore the index if it contains no valuable information
df = pd.concat([inv_jan, inv_feb, inv_mar], ignore_index=True)

# Set label to original dataframes
df = pd.concat([inv_jan, inv_feb, inv_mar], ignore_index=False, keys=["jan", "feb", "mar"])

# Ignore columns that don't exist in all dataframes
df = pd.concat([inv_jan, inv_feb], join="inner")
~~~

### Merging time series data

- merge_ordered() method is very similar to merge(), however default join type is outer
- merge_ordered() is called by passing both dataframes as params
- merge_asof() method is similar to merge_ordered(), however matches on nearest key and not exact matches
- merge_asof() requires "on" columns to be pre-sorted

~~~
# Merge apple + microsoft stock history
pd.merge_ordered(appl_stock, msft_stock, on="date", suffixes=("_appl", "_msft"))

# Fill missing data by populating with the previous value (forward fill)
pd.merge_ordered(appl_stock, msft_stock, on="date", suffixes=("_appl", "_msft"), fill_method="ffill")
~~~

## Querying data

- query('{selection statement}'): similar to SQL WHERE clause

~~~
# Select rows where Apple close price is greater than or equal to 60
stocks.query('stock=="appl" and close_price >= 60)
~~~

## Melting data

- melt(): converts wide format data to long format data
    - wide format: each column specifies an attribute of the record (this is the usual way data is stored in sql)
    - long format: data about each record spans multiple rows (for example, with attribute/value columns)

~~~
# This keeps columns A + B, and turns data from columns C + D into variable/value columns named year and dollars.
df_melted = df.melt(id_vars=["ColumnA", "ColumnB"], value_vars=["ColumnC", "ColumnD"], var_name=["year"], value_name="dollars")
~~~
    
## Date and time series functionality

- Data types are for date + time info, and represent points in time or periods in time
- Attributes + methods reflect time-related details

### Manually create a date

~~~
import pandas as pd
from datetime import datetime
time_stamp = pd.Timestamp(datetime(2023, 9, 1))
~~~
