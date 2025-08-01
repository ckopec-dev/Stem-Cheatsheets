# Linux Cheatsheet

## Command commands

### End a terminal session
`$ exit`

### Perform release upgrade
~~~
$ sudo apt update
$ sudo apt upgrade
$ sudo apt dist-upgrade
$ sudo do-release-upgrade
~~~

### Use a terminal calculator
~~~
# See https://www.geeksforgeeks.org/linux-unix/bc-command-linux-examples/
$ echo "12+5" | bc
~~~

### Show architecture
`$ uname -m`

### Show kernel version
`$ uname -r`

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
$ ls

# With hidden files in long format
$ ls -la

# Show in a tree format with hidden files and directories
$ tree -a

# Show tree with permissions
$ tree -p
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

### Create a symbolic link
`$ ln -s {target-path} {link-name}`

### Display file type
`$ file {filename}`

### Show unique lines
~~~
# Remove dupes
$ uniq {filename}

# Show dupe counts
$ uniq -c {filename}

# Only show dupes
$ uniq -d {filename}

# Only show unique lines
$ uniq -u {filename}
~~~

### Show word count
~~~
# Single file
$ wc {filename}
# Output is {number-of-lines} {word-count} {byte-count} {filename}| 

# Multiple files
$ wc {filename1} {filename2}
# Output is same as above, with one row per file.

# Line count
$ wc -l {filename}

# Word count
$ wc -w {filename}
~~~

### Search file contents
~~~
# Search for a case-insensitive string
$ grep -i "{search-string}" {filename}

# Count the number of matches
$ grep -c "{search-string}" {filename}

# Display line number of match, along with the line itself
$ grep -n "{search-string}" {filename}

# Find lines that don't match
$ grep -v "{search-string}" {filename}
~~~

### Parse text data using Awk
~~~
# Parse out 2nd column using default delimiter (space)
$ awk '{print $2}' myTextFile.txt

# Parse out 4th column using comma delimiter
$ awk -F, '{print $4}' myTextFile.txt
~~~

### Print first ten lines of a file
`$ head {filename}`

### Print last ten lines of a file
`$ tail {filename}`

### Alias commands
~~~
# List all aliases
$ alias
# OR
$ cat ~/.bash_aliases

# Create an alias
$ alias {alias-name}='{command}'

# Remove an alias
$ unalias {alias-name}

# Refresh bash_aliases without relogging
$ source ~/.bash_aliases
~~~

### Securely copy from one server to another
~~~
# From local to remote
$ scp {filename} {remoteuser}@{remotehost}:{remote-directory}
# E.g. recipes.txt bob@10.0.0.1:/home/bob

# From remote to local
$ scp {remoteuser}@{remotehost}:{remote-file} .
# E.g. carl@10.0.0.2:/home/carl/recipe.txt .

# Use -r flag to copy entire directory recursively.
~~~

### Secure FTP
~~~
# Open a ftp connection
$ ftp-ssl {remote-server}

# List remote directory contents
ftp> dir

# Set local working directory
ftp> lcd {local-directory}

# Get remote file
ftp> get {remote-filename}

# Exit
ftp> quit
$
~~~

### Convert pdf to text

~~~
$ sudo apt install poppler-utils
$ pdftotext input.pdf output.txt

# Alternatively
$ sudo apt install xpdf-utils
$ pdf2text input.pdf output.txt

# With python
$ pip install PyPDF2

import PyPDF2
with open("input.pdf", "rb") as f:
    reader = PyPDF2.PdfReader(f)
    for page in reader.pages:
        print(page.extract_text())
~~~

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

## Hardware

### List usb devices
`$ sudo blkid`

### List mounted drives

~~~
# Everything in a tree format
$ findmnt

# Just ext4 drives
$ sudo df -a -T -h -t ext4
~~~

## File sharing

### Enabling

~~~
# Tested on fresh build of Ubuntu 24.04.

# Install samba
$ sudo apt install samba

# Create folder to share
$ mkdir /home/alice/data

# Edit samba configuration
$ sudo nano /etc/samba/smb.conf

# Add to end of smb.conf
[data]
   comment = Data on Ubuntu
   path = /home/alice/data
   read only = no
   guest ok = no
   browsable = yes
   create mask = 0755

# Restart samba
$ sudo service smbd restart

# Set SMB password
$ sudo smbpasswd -a alice

# Show your configuration
$ testparm

# To map from Windows 10/11 in Explorer:
# Right-click "This PC", select "Map Network Drive...".
# Select a drive letter.
# For Folder, enter "\\{Server-name}\{Share-name}".
# Check both check boxes and click Finish.
~~~

## Monitoring

~~~

# Display real-time system info
$ top
$ htop
$ btop

# Show disk activity
$ sudo iotop

# Show network activity by process
$ sudo nethogs

# Show global network activity
$ sudo iftop

# Measure execution duration of a process
$ time ls -la

# Measure execution duration of a shell script
#!/bin/bash

start_time=$(date +%s.%N) # Get start time with nanosecond precision

# Your command or script goes here
sleep 2

end_time=$(date +%s.%N) # Get end time with nanosecond precision

runtime=$(echo "$end_time - $start_time" | bc) # Calculate difference using 'bc' for floating-point arithmetic
echo "The script took $runtime seconds to execute."

# Get performance analysis metrics of a process
$ sudo perf stat ls -la
~~~

