
# Virtualization Cheatsheet

## Hyper-v

### How to change the resolution on a linux vm

~~~
$ sudo nano /etc/default/grub

GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video=hyperv_fb:1920x1080"
~~~

## Remote client access

### RDP into a Windows machine from an Ubuntu client

- Install the Remmina package with the Software Manager. It should work immediately, however the following settings should be changed:
  - Main window: toggle dynamic resolution update
  - RDP: edit good quality setting: enable all but composition. Specify good setting in connection
  - Applet: start in tray upon user login

### Configure Ubuntu for remote access

- Settings -> Power -> Screen Blank: Never
- Settings -> Sharing -> Remote Desktop -> Remote Desktop: Enabled
- Settings -> Sharing -> Remote Desktop -> Remote Control: Enabled
- Settings -> Sharing -> Remote Desktop -> User Name: same as regular login
- Settings -> Sharing -> Remote Desktop -> Password: update to regular login password
- Reboot
- When connecting from Windows, make sure 32-bit color is selected. Lower depth may fail due to bug in Window’s RDP client.

