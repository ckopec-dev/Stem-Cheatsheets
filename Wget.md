
# Wget Cheatsheet

## Basics

### See if wget is installed

`$ man wget`

### Install wget

`$ sudo apt install wget`

### Basic syntax

`$ wget [option flags] [URL]`

### Useful flags

- -b: go to background after startup
- -q: quiet. turn off output
- -c: continue. resume previously halted transfer

### Download multiple files from a text file which lists one url per line

`$ wget -i {file-list.txt}`

### Throttle downloads (useful for large files)

`$ wget --limit-rate=200k -i {file-list.txt}`

### Wait between downloads (useful for lots of small files)

`$ wget --wait={seconds} -i {file-list.txt}`
