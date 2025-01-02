# Linux Cheatsheet

## Command commands

### Show current date and time
`$ date`

### Show a calendar of the current month
`$ cal`

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
