# Tcpdump Cheatsheet

## Basic usage

~~~
# List interfaces available to scan on
$ sudo tcpdump -D

# Scan tcp on interface 3
$ sudo tcpdump -i 3

# Scan udp on interface 3
$ sudo tcpdump -i 3 udp		

# Ignore port 22 (for example, if using from a remote ssh connection)
$ sudo tcpdump -i 3 port not 22 
~~~
