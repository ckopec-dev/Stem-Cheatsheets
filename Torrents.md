# Torrents

## List tracker info

~~~bash
# Install transmission client
$ apt install transmission-cli

# Show info
$ transmission-show filename.torrent
~~~

## Download on linux

~~~bash
# Install aria2 console app
$ apt install aria2

# Download to current directory (may consist of multiple files), then exit
$ aria2c --seed-time=0 filename.torrent
~~~
