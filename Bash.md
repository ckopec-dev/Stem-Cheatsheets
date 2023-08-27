
# Bash Cheatsheet

## Help and misc

### Show bash version

`$ echo $BASH_VERSION`

### Show the manual for a command

`$ man {command}`

### Show history of commands previously entered

`$ history`

The output contains a number followed by the command. To rerun a command, type !{number}

### Open a full screen terminal in Ubuntu

`{ctr-alt-f2}`

### Get a gui back from a full screen console

`{ctr-alt-f7}`

### Get computer name

`$ hostname`

## Printing

### Print from the console

~~~
# For printers in Gnome/Settings/Printers

# Check if a default print is installed
$ lpstat -d

# Install a default printer
# To find the printer name, look in Gnome/Settings/Printers/Printer Details/Name
# When Ubuntu is hosted as a virtual machine, there might be two identical printers listed. Use the one that ends in .local
$ lpadmin -d {printer name}

# Print
$ lpr {filename}

# View print queue
$ lpq
~~~

### Show installed pip packages 
`$ pip list`

## Process management

### Redo last command as root

`$ sudo !!`

### See the processes in your shell

`$ ps`

### Kill a process

`$ kill -{level} {pid}`

Levels (in order of gracefullness):

- 15: sends sigterm (this is the default level)
- 2: sends sigint
- 1: send sighup
- 9: send sigkill

### Suspend foreground process

`ctri-z`

### List jobs

`$ jobs`

### Run a job in the background (append an amperstand)

`$ sort bigfile > bigfile.sorted &`

### Send stopped process to background

`$ bg {jobnum}`

### Bring process to foreground

`$ fg {jobnum}`

### Various terminal "screensavers"

~~~
# Displays various stats related to processes, ram usage, etc.
$ top

# Similar to top but more colorful.
$ sudo apt install htop
$ htop

# Displays vertically scrolling characters.
$ sudo apt install cmatrix
$ cmatrix
~~~

## Disk management

### Create a fast temporary ram disk
`$ mkdir -p /mnt/ram ; mount -t tmpfs tmpfs /mnt/ram -o size=8129M`

### Show disk usage
`$ du -h`

### Show disk usage in current directory, inclusive
`$ du -s`

## File management

The working directory is the current directory. Unless you tell the OS otherwise, all commands apply to your working directory.   
Absolute path: the full path of the file or directory starting with the root directory. E.g. /users/john.  
Relative path: the path relative to the working directory. It never starts with a slash.  
Navigation: to move up the directory tree, use “..”.   
Names that begin with a dot are hidden. All characters are legal in a filename except “/”, and they cannot begin with a “-“.  

### File permissions (as shown with ls -l command): 10 characters {0111222333}

- 0: File type: d for directory, - for file.
- 111: File owner permissions
- 222: File group permissions
- 333: Everybody else

### Permission types

- r(read): For directories, allows listing via ls. For files, allows reading the content.
- w(write): For directories, this gives permission to delete, rename, or add files within the directory. For files, allows changing the file’s contents.
- x(execute): For directories, this gives permission to access it. For files, this gives permission to execute it (i.e. if it isn’t a program, it shouldn’t have execute permission).
- -(none)

### File and directory wildcards

- *: any number of characters
- ?: a single character
- []: surround a group of single characters to match
        
### Show the current directory

`$ pwd`

### List contents of directory

`$ ls {path}`

Use '-R' to list contents recursively.

If the first character of the path starts with a '/', then it's an absolute path. Otherwise, it's relative.  
. means the current directory.  
.. means the parent direcotry of the current directory.  
A path of '/' means the root directory.
~ means your home directory.

### Change current directory

`$ cd {path}`

### Copy a file

`$ cp {original-path} {destination-path}`

### Extract a gz file

`$ tar -xzvf {filename}`

### Copy multiple files into a different directory

`$ cp {file1} {file2} {destination-path}`

### Move files

`$ mv {file1} {file2} {destination-path}`

Similar to a copy and delete.
Can be used to rename files.

### Locate a file

`$ locate {filename}`

### Delete files

`$ rm {file1} {file2}`

### Create directory

`$ mkdir {path}`

### Delete empty directory

`$ rmdir {path}`

### Delete directory and all contents

`$ rm -r {path}`

### Use wildcards

- *: zero or more characters
- ?: a single character
- [...]: any character inside brackets
- {...}: any comma-separated patterns inside the curly brackets, e.g. {*.txt, *.csv}

## Text data manipulation

### View a file's contents

`$ cat {path}`

### View a file's contents with paging

`$ less {path}`

When specifying multiple paths, use ':n' to go to the next file, and ':q' to quit.

### View the start of a file

`$ head {path}`

Shows 10 lines by default. 
Use '-n {lines}' to specify the number of lines.

### View the end of a file

`$ tail {path}`

Similar options as head.

### Select columns from a file

`$ cut -f {field-specifiers}, -d {delimiter} {path}`

Field specifiers are column indexes separated by commas. Can consist of ranges. E.g. 3-5,6,8
Cut is sometimes not the best tool for csv, since it can't handle quoted strings.

### Print counts of characters, words, and lines in a file

`$ wc {path}`

'-c' for characters, '-w' for words, '-l' for lines

### Sort text

`$ sort {path}`

Can pass path from standard input.  

- -r: sort reverse the order
- -b: ignore leading blanks
- -f: case-insensitive

### Remove duplicate adjacent rows

`$ uniq {path}`

## Grep searches

### Basic grep usage

`$ grep {search-text} {file1} {file2}`

Returns all lines from files which contain search-text.

### Common flags

| Flag | Description |
| ---- | ----------- |
| -c | prints count of matching lines |
| -h | don't print the names of files when searching multiple files |
| -i | ignore case |
| -l | print the names of the files that contain the matches (instead of the matches) |
| -n | print line numbers for matching lines |
| -v | only show lines that don't match |

## Redirection and related controls

### Redirect output to a file

`$ ls > {path}`

Outputs the directory listing and redirects it to specified file.

### Redirect output to the input of another command

`$ cat {path} | sort`

Sends output of cat to sort.

### Chain commands together. Each command is executed sequentially.

`$ cat file1.txt; cat file2.txt`

### Chain commands together. Each command is executed sequentially, ONLY if the preceeding command succeeded.

`$ cat file1.txt && cat file2.txt`

## Shell variables

### Common environment variables

- HOME: user's home directory
- PWD: present working directory
- SHELL: which shell is being used
- USER: user's Id

Available everywhere

### Create and assign a shell (local) variable

`$ {variable-name}={variable-value}`

### Print a variable value

`$ echo ${variable-name}`

## Creating and using scripts

### Create a Hello World script

~~~

#!/bin/bash

echo "Hello World!"

~~~

### Pass arguments to a script

~~~

#!/bin/bash

# Based on position of arguments that are passed in.
echo "Username: $1";
echo "Age: $2";
echo "Full Name: $3";chg

# $@ is a special expression that means "all of the arguments"
# $* also means "all of the arguments"
# $# gives the number of arguments


~~~

### Execute a script

`$ bash my_script.sh`

or

`$ ./my_script.sh`

## Control statements

### Basic IF statement syntax

~~~

if [ CONDITION ]; then
      # CODE TO EXECUTE IF TRUE
else 
      # CODE TO EXECUTE IF FALSE
fi

# For numerical comparisons, there are two options for declaring the condition:
if ((CONDITION)); then # This option uses standard numerical comparison operators, e.g. '>'
if [ CONDITION ]; then # This option requires comparison operator flags, e.g. '-gt'

# Commands can be executed in the condition by excluding the brackets, e.g.:
if grep -q SomeText MyFile.txt; then

# Execute via shell-within-a-shell technique. Produces same result as above:
if $(grep -q SomeText MyFile.txt); then

~~~

### Multiple condition examples

~~~

if [ CONDITION A ] && [ CONDITION B ]; then # A AND B
if [ CONDITION A && CONDITION B ]; then # A AND B (same as above)
if [ CONDITION A ] || [ CONDITION B ]; then # A OR B
if [ CONDITION A || CONDITION B ]; then # A OR B (same as above)

~~~

### Special conditional flags

| Flag | Description |
| ---- | ----------- |
| -e | if the file exists |
| -s | if the file exists and the the size is greater than 0 |
| -r | if the file exists and is readable |
| -w | if the file exists and is writable |

### Arithmetic conditional flags
| Flag | Description |
| ---- | ----------- |
| -eq | equal to |
| -ne | not equal to |
| -lt | less than |
| -le | less than or equal to |
| -gt | greater than |
| -ge | greater than or equal to |

### Basic FOR statement syntax

~~~

for ITEM in ITEMS
do
      # Do something
done

# Numeric iteration
for i in {START..STOP..INCREMENT} # Start defaults to 1 if not specified.

# Three expression syntax example (very similar to c syntax):
for i in ((i=2;i<=4;i++))

# File iteration example:
for logfile in logfiles/*

# Shell-in-a-shell example:
for loggile in $(ls logfiles/ | grep -i 'ftp')

~~~

### Basic WHILE statement syntax

~~~

while [ CONDITION ]; # Syntax is similar to IF condtition syntax.
do
      # Do something
done

~~~

### Basic CASE statement syntax

~~~

case STRING OR VARIABLE in
      PATTERN1)
      COMMAND1;;
      PATTERN2)
      COMMAND2;;
      *)
      DEFAULT COMMAND;;
esac

~~~

## Process management

### Stop a running command

`CTRL-C`

## Windows shares

### Accessing a Windows share from Linux

~~~

*** IMPORTANT! cifs is not secure, so this example isn't either. Use only in test/isolated environments.

# Pre-req installs
$ sudo apt-get install nfs-common
$ sudo apt install cifs-utils

# To test connectivity/mounting
$ sudo mkdir {local mount location, e.g. /mnt/windowsshare}
$ sudo mount -t cifs -o username={username},password={password} {remote share, e.g. //192.168.0.10/WindowsShare} {local mount location, e.g. /mnt/windowsshare}
$ ls {local mount location, e.g. /mnt/windowsshare}

# To mount at boot:
$ sudo nano /etc/sambapasswords
        username={windows username}
        password={windows password}

$ sudo chown 0.0 /etc/sambapasswords # So root owns
$ sudo chmod 600 /etc/sambapasswords # So only root can access

$ sudo nano /etc/fstab
        add a line: {remote share, e.g. //192.168.0.10/WindowsShare} {local mount location, e.g. /mnt/windowsshare} cifs auto,gid=users,credentials=/etc/sambapasswords,file_mode=0777,dir_mode=0777 0 0
$ sudo shutdown -r now

~~~

