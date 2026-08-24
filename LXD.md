
# LXD

A lightweight VM native to Ubuntu.

[Documentation](https://canonical.com/lxd/docs/default/)

## Install

~~~bash

# Install
sudo snap install lxd

# Update
sudo snap refresh lxd

# Set groups
sudo usermod -aG lxd "$USER"
newgrp lxd

# Initialize
lxd init --minimal
~~~

## Basic examples

### Create VMs

~~~bash
# Show versions
lxc version

# One liner with defaults
lxc launch ubuntu:24.04 vm1 --vm
lxc info vm1

# Create a VM with custom resources
lxc init ubuntu:24.04 vm2 --vm
lxc config set vm2 limits.cpu 4
lxc config set vm2 limits.memory 8GiB
lxc config device override vm2 root size=50GiB
lxc start vm2
lxc info vm2
~~~

### Show VMs

~~~bash
# List
lxc list
~~~

### Destroy VMs

~~~bash
# Delete without shutting them down first since these are just examples
lxc delete vm1 --force
lxc delete vm2 --force
~~~
