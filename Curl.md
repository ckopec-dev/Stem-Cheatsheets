

# Curl cheatsheet

## Basics

### See if curl is installed

`$ man curl`

### Install curl

`$ sudo apt install curl`

### Basic syntax

`$ curl [option flags] [URL]`

### Save file with original name

`$ curl -O {url}`

### Save file with new name

`$ curl -o {new-name} {url}`

### Use wildcards to download multiple files

`$ curl -O https://site.com/file*.txt`

### Use globbing parser to download multiple files

`$ curl -O https://site.com/file[001-100].txt`

### Useful flags

- -L: redirects 300 error code
- -C: resumes previously timed out transfer
