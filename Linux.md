# Linux Cheatsheet

## Command commands

### End a terminal session
`$ exit`

### Print a message
`$ echo "Hello, world!"`

### Show current date and time
`$ date`

### Show a calendar
~~~
# The current month
$ cal

# The current year
$ cal -y

# A specific month
$ cal 10 2012

# A specific year
$ cal 2018

# Previous, current, and next month
$ cal -3

# Use julian format (days do not reset on 1st of month)
$ cal -j
~~~

### Show current logged in user
`$ whoami`

### Show all logged in users
`$ who`

### Show last time system was booted
`$ who -b`

### Show your working directory
`$ pwd`

### Change your working directory
~~~
# Absolute path
$ cd /users/bob
# Relative path
$ cd spreadsheets
~~~

### List directory contents
~~~
# Basic list
$ ls`
# With hidden files in long format
$ ls -la
~~~

### Show file contents
`$ less {filename}`

### File and directory wildcards
- *: match any characters
- ?: match a single character
- []: match any characters inside brackets

### Create a directory
`$ mkdir {directory-name}`

### View file contents
`$ cat {filename}`

### Copy a file
`$ cp {old} {new}`

### Move a file (also works to rename)
`$ mv {old} {new}`

### Delete a file
`$ rm {filename}`

## Security

### Change permissions
`$ chmod {permissions} {file(s)}`

### Show groups
`$ groups`

## Printing

### Print a file
`$ lp {filename}`

### Show printer queue
`$ lpq`

### Cancel active job
`$ lprm`
