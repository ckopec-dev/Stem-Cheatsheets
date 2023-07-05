
# Pandas Cheatsheet

## Basics

~~~

# Import library
import pandas as pd

# Create a dataframe
df = pd.read_csv('my_file.csv')

# Print the dataframe
print(df)

# Inspect dataframe (displays first 5 rows)
print(df.head())

# Show dataframe info
print(df.info())

# Select a column
first_name = df['first_name']

# Use dot notation to select a column name. Only works if colum name doesn't contain spaces or special characters.
last_name = df.last_name

# Select a column with a logical comparison
expensive = df[df.price > 20]

~~~
