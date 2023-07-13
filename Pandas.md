
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

df = pd.DataFrame(data)
print(df)

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

### From CSV

~~~

import pandas as pd

df = pd.read_csv('data.csv')

# This will print the first 5 and last 5 rows by default
# To override, set pd.options.display.max_rows = n
print(df)

# This will print everything
print(df.to_string())

~~~

### From JSON

~~~

import pandas as pd

df = pd.read_json('data.json')
print(df)

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
df["ColumnA"]fillna(x, inplace = True)

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




### Sort with single column
`df.sort_values("columnA", ascending=False)`

### Sort with multiple columns
`df.sort_values(["columnA", "columnB"], ascending=[True, False])`

### Sort by index values
`df.sort_index(level=["columnA", "columnB"], ascending=[True, False])`

## Subsetting

### Select a column
`first_name = df["first_name"]`

### Select multiple columns
`df[["first_name", "last_name"]]`

### Use dot notation to select a column name. Only works if colum name doesn't contain spaces or special characters.
`last_name = df.last_name`

### Select a column with a logical comparison
`expensive = df["price"] > 20`

### Subset with a logical comparison to return all affected rows
`df[df["price"] > 20]`

### Subset based on a date column
`df[df["dob"] < "2000-01-01"]`

### Subset using isin()
`df[df["color"].isin(["Red", "Blue"])]`

### Setting a column as the index
`index = df.set_index("columnA")`

### Setting multiple columns as the index
`index = df.set_index(["columnA", "columnB"])`

### Removing an index. drop=True discards it.
`index.reset_index(drop=True)`

### Create a dataframe from a dictionary
`df = pd.DataFrame({dictionary})`

### Return a dataframe by selecting keys from an existing dataframe
`df = my_dataframe[["my_key1", "my_key2"]]`

### Slice a dataframe to return only certain rows
`df[1:4]`

### Select rows by named indexes
`df.loc[["my_index1", "my_index2"]]`

### Select rows by named indexes and specify columns
`df.loc[["my_index1", "my_index2"], ["col1", "col2"]]`

### Select rows with tuples
`df.loc[[("value1", "value2"), ("value3", "value4")]]`

### Select rows and columns by indexes
`df.iloc[[1,2,3], [0,1]]`

### Filter with a comparison operator
`df[df["square_feet"] > 5]`

### Filter with a boolean operator
`df[np.logical_and(df["square_feet"] > 5, df["square_feet] < 15)]`

## Feature engineering

### Adding a new column
`dogs["height_m"] = dogs["height_cm"] / 100`

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

~~~

# General syntax
df.groupby("columnA")["columnB"].mean()`

# Examples
dogs.groupby("color")["weight"].agg([min, max, sum])
dogs.groupby(["color", "breed"])["weight"].mean()

~~~

### Pivot tables

~~~

# By default, aggregation is by mean
dogs.pivot_table(values="weight", index="color")

# Specify an aggregation
dogs.pivot_table(values="weight", index="color", aggfunc=np.median)

# Specify multiple aggregations
dogs.pivot_table(values="weight", index="color", aggfunc=[np.median, np.mean])

# Pivot by two variables.
# fill_value prevents NaN results
# margins creates a final column containing the mean of values for given row (not including 0 values)
dogs.pivot_table(values="weight", index="color", columns="breed", fill_value=0, margins=True)

~~~


