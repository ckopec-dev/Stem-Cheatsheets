
# Dotnet Cheatsheet

## Installing on Ubuntu 22.04

~~~
# Accurate as of 9/7/24

$ wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh

$ chmod +x ./dotnet-install.sh

$ ./dotnet-install.sh --version latest

# Add the following line to the bottom of .bashrc:

export PATH="$PATH:/home/<!username!>/.dotnet

# Relog
~~~

## Using with VS Code on Ubuntu 22.04

- Make sure dotnet is installed via: $ dotnet --version
- Install c# dev kit extension
