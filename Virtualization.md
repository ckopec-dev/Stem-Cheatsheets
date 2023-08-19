
# Virtualization Cheatsheet

## Hyper-v

### How to change the resolution on a linux vm

~~~

$ sudo nano /etc/default/grub

GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video=hyperv_fb:1920x1080"

~~~
