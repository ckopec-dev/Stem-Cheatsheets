
# Pandas Cheatsheet

## Basics

### Import pandas
`import pandas as pd`

### Create a dataframe from a csv file. index_col refers to a column used as a row index.
`df = pd.read_csv('my_file.csv', index_col=0)`

### Print the dataframe
`print(df)`

### Inspect dataframe (displays first 5 rows)
`print(df.head())`

### Show dataframe info
`print(df.info())`

### Select a column
`first_name = df['first_name']`

### Use dot notation to select a column name. Only works if colum name doesn't contain spaces or special characters.
`last_name = df.last_name`

### Select a column with a logical comparison
`expensive = df[df.price > 20]`

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

### Select rows and columns by indexes
`df.iloc[[1,2,3], [0,1]]`

### Filter with a comparison operator
`df[df["square_feet"] > 5]`

### Filter with a boolean operator
`df[np.logical_and(df["square_feet"] > 5, df["square_feet] < 15)]`

