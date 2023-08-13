
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

### Variable assignment
`> my_var <- 4`
