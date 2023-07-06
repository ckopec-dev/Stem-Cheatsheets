
# Csvkit Cheatsheet

## Installation and configuration

### Help

For most csvkit commands, use "{command} -h" for help.

### Install 

`$ pip3 install csvkit`

### Upgrade

`$ pip3 install --upgrade csvkit`

## Conversions and manipulations

### Convert Excel spreadsheet into csv

`$ in2csv MyWorkbook.xlsx > MyCsv.csv`

### Print all sheet names in workbook

`$ in2csv -n MyWorkbook.xlsx`

### Specify worksheet to be converted (1st will be used by default)

`$ in2csv MyWorkbook.xlsx --sheet "MyWorksheet" > MyCsv.csv`

### Preview csv data in Markdown-compatible format

`$ csvlook MyCsv.csv`

### Show descriptive stats of csv data

`$ csvstat MyCsv.csv`

### Print all column names in csv data. Returns position index (1-based) and name.

`$ csvcut -n MyCsv.csv`

### Return data by column position

~~~

# By index
$ csvcut -c {position-index} MyCsv.csv

# By name
$ csvcut -c "{column-name}" MyCsv.csv

# Multiple columns
$ csvcut -c 2,3 MyCsv.csv
$ csvcut -c "ColA","ColB" MyCsv.csv

~~~

### Return filtered rows

csvgrep: must be paired with one of 3 flags:   
  
- -m: exact row value (for grabbing a row by Id, for example)
- -r: regex pattern  
- -f: file path  

### Csvgrep example

~~~

# Returns entire row with given Id
$ csvgrep -c "Id" -m 123 MyCsv.csv

~~~

### Stack multiple csv files with the same schema

~~~

# Basic example
$ csvstack MyCsv1.csv MyCsv2.csv > MyCsvCombined.csv

# Creates a new column called group with values indicating the source of the row
$ csvstack -g "MyCsv1","MyCsv2" MyCsv1.csv MyCsv2.csv > MyCsvCombined.csv

# Override default group name
$ csvstack -g "MyCsv1","MyCsv2" -n "source" MyCsv1.csv MyCsv2.csv > MyCsvCombined.csv

~~~

### Create a csv file from sql data

~~~

# Basic structure of command
$ sql2csv --db "{connection-string}" \
        --query "SELECT * FROM Some_Table" \
        > MyCsv.csv

# To connect to a MS Sql Server database, need to install pymssql
$ pip3 install pymssql

# Since the database isn't in the connection string, it needs to be specified in the query
# If the password contains a '@', it needs to be escaped with '%40'
$ sql2csv --db "mssql+pymssql://{username}:{password}@{server-address}/?charset=utf8" \
        --query "SELECT * FROM MyDatabase.dbo.Some_Table" \
        > MyCsv.csv

~~~

### Execute sql against a csv file

~~~

# Since this creates an in-memory instance of a sql database from the entire csv file, it isn't practical for large files.
$ csvsql --query "SELECT * FROM MyTable LIMIT 10" MyCsv.csv

# Can be used to query multiple files using joins. See documentation for details.

~~~

### Insert data into sql from csv

~~~

$ csvsql --db "{connection-string}" --insert MyCsv.csv

# Useful flags: 
# --no-inference: disables type inference when parsing input
# --no-constraints: generates a schema without length limits or null checks

~~~

