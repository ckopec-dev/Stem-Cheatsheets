
# R Cheatsheet

## Installation on Ubuntu 22.04

~~~
$ wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo gpg --dearmor -o /usr/share/keyrings/r-project.gpg
$ echo "deb [signed-by=/usr/share/keyrings/r-project.gpg] https://cloud.r-project.org/bin/linux/ubuntu jammy-cran40/" | sudo tee -a /etc/apt/sources.list.d/r-project.list
$ sudo apt update
$ sudo apt install --no-install-recommends r-base
~~~

## Basics

### Start interactive session
`$ R`

### Print to console
`> print("Hello World!)`

### Comments
`# This is a comment`

### Loading packages

~~~
> library(gapminder)
> library(dplyr)
~~~

### Data types

- numerics: decimals
- integers: whole numbers
- logical: boolean, TRUE or FALSE
- characters: text, in double quotes


### Show type

`> class(my_variable)`

### Math operators

- +: addition
- -: subtraction
- *: multiplication
- /: division
- ^: exponentiation
- %%: modulo

### Comparison operators

- <: less than
- &gt;: greater than
- <=: less than or equal to
- &gt;=: greater than or equal to
- ==: equal to
- !=: not equal to

### Variable assignment

`> my_var <- 4`

## Vectors

Vectors are one-dimensional arrays that can store numeric, character, or boolean data.

### Create a vector

~~~
numeric_vector <- c(1, 2, 3)
character_vector <- c("a", "b", "c")
boolean_vector <- c(TRUE, FALSE, TRUE)
~~~

### Assign names to vector elements

~~~
my_vector <- c("John Doe", "Programmer")
names(my_vector) <- c("Name", "Profession")
~~~

### Adding vectors
`> vector3 <- vector1 + vector2`

### Summing a vector
`> vector_sum <- sum(my_vector)`

### Averaging a vector
`> vector_avg <- mean(my_vector)`

### Selecting vector elements

~~~
# Select first element (vectors are 1-based, not 0)
print(my_vector[1])

# Select first and third element
print(my_vector[c(1,3)]

# Select range of 2-4 elements (inclusive on both ends)
print(my_vector[2:4]

# Select by names
print(my_vector[c("Monday", "Tuesday")]
~~~

### Vector comparisons

~~~

# Comparison operators can be used on vectors
my_vector(4, 5, 6) > 5
# Result: FALSE FALSE TRUE
~~~

### Summarize a vector
`> summary(my_vector)`

## Matrices

Matrices are two-dimensional arrays that can store numeric, character, or boolean data.

### Create a matrix

~~~
matrix(1:9, byrow=TRUE, nrow=3)

# 1st arg: collection of elements that fill the matrix
# byrow: fill by rows. To fill by columns, set byrow = FALSE
# nrow: number of rows in the matrix
~~~

### Sum rows
`> rowSums(my_matrix)`

### Merge matrices and/or vectors by column
`> my_matrix <- cbind(matrix1, matrix2, vector1)`

### Merge matrices and/or vectors by row
`> my_matrix <- rbind(matrix1, matrix2, vector1)`

## Factors

A factor is a data type used to store categorical data.

### Create a factor from a vector
`> my_factor <- factor(my_vector)`

### Change factor names
`> levels(my_geneder_vector) <- c("Female", "Male")`

## Data frames

A data frame has the variables of a dataset as columns and the observations as rows.

### Print the first few rows of a data frame
`> head(my_dataframe)`

### Print the last few rows of a data frame
`> tail(my_dataframe)`

### Show the structure of a data frame
`> str(my_dataframe)`

### Create a data frame
`> my_dataframe <- data.frame(vector_1, vector_2, vector_n)`

### Select rows and columns
`> my_dataframe[1:3,2:4] # Selects rows 1,2,3 and cols 2,3,4`

### Select rows and columns by name
`> my_dataframe[1:4,"my_col"]`

### Subset a data frame
`> subset(my_dataframe, subset = radius < 1)`

## Lists

A list stores a variety of objects in a single container.  

### Create a list
`> my_list <- list(my_vector, my_matrix)`

### Create a list with named elements
`> my_list <- list("name1" = my_vector, "name2" = my_matrix)`

### Name elements on existing list
`> names(my_list) <- c("name1", "name2")`

### Select the nth element of a list
`> my_list[[n]]`

### Select a named element of a list
`> my_list$my_element`

