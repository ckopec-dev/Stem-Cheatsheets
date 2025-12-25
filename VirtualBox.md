# VirtualBox on Ubuntu: Installation and Management Tutorial

## What is VirtualBox?

VirtualBox is a free, open-source virtualization software that allows you to run multiple operating systems on your Ubuntu machine simultaneously. You can use it to test different Linux distributions, run Windows applications, or create isolated development environments.

## Installing VirtualBox

### Method 1: Install from Ubuntu Repositories (Easiest)

Open a terminal and run:

```bash
sudo apt update
sudo apt install virtualbox
```

This installs the version available in Ubuntu's official repositories, which is stable but may not be the latest release.

### Method 2: Install from Oracle's Repository (Latest Version)

For the newest version with the latest features:

1. **Import Oracle's GPG key:**
```bash
wget -O- https://www.virtualbox.org/download/oracle_vbox_2016.asc | sudo gpg --dearmor --yes --output /usr/share/keyrings/oracle-virtualbox-2016.gpg
```

2. **Add the VirtualBox repository:**
```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] https://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib" | sudo tee /etc/apt/sources.list.d/virtualbox.list
```

3. **Install VirtualBox:**
```bash
sudo apt update
sudo apt install virtualbox-7.0
```

### Install the Extension Pack (Optional but Recommended)

The Extension Pack adds USB 2.0/3.0 support, remote desktop, and other features. Download it from the [VirtualBox downloads page](https://www.virtualbox.org/wiki/Downloads) and install by double-clicking the file, or:

```bash
sudo apt install virtualbox-ext-pack
```

## Basic Configuration

### Add Your User to the vboxusers Group

This allows you to use USB devices in your virtual machines:

```bash
sudo usermod -aG vboxusers $USER
```

Log out and log back in for this change to take effect.

## Creating Your First Virtual Machine

1. **Launch VirtualBox** from your applications menu or by typing `virtualbox` in the terminal.

2. **Click "New"** to create a new virtual machine.

3. **Configure the basics:**
   - Name your VM (e.g., "Windows 10" or "Ubuntu Test")
   - Select the operating system type and version
   - Click "Next"

4. **Allocate memory (RAM):**
   - Use the slider to assign RAM (stay in the green zone)
   - Recommended: 2-4 GB for most desktop OSes
   - Click "Next"

5. **Create a virtual hard disk:**
   - Select "Create a virtual hard disk now"
   - Choose VDI (VirtualBox Disk Image)
   - Select "Dynamically allocated" for better disk space management
   - Set the size (20-50 GB is typical)
   - Click "Create"

6. **Mount an installation ISO:**
   - Select your new VM and click "Settings"
   - Go to "Storage"
   - Click the empty CD icon under "Controller: IDE"
   - Click the CD icon on the right and choose "Choose a disk file"
   - Select your OS installation ISO file

7. **Start the VM** by clicking the green "Start" arrow and follow the OS installation process.

## Managing Virtual Machines

### Starting and Stopping VMs

- **Start:** Select the VM and click "Start"
- **Pause:** Click "Machine" → "Pause"
- **Save state:** Close the VM window and choose "Save the machine state" (like hibernation)
- **Shutdown:** Close the VM window and choose "Send the shutdown signal" or shut down from within the guest OS
- **Power off:** Close the VM window and choose "Power off the machine" (like pulling the plug)

### Taking Snapshots

Snapshots save your VM's current state so you can restore it later:

1. With the VM selected, click "Machine" → "Take Snapshot"
2. Name your snapshot and add a description
3. To restore: Click "Machine" → "Restore Snapshot"

This is invaluable before making system changes or testing software.

### Configuring VM Settings

Right-click a VM and select "Settings" to modify:

- **System:** Adjust RAM, CPU cores, boot order
- **Display:** Increase video memory, enable 3D acceleration
- **Storage:** Add more virtual hard drives or CD/DVD drives
- **Network:** Change network adapter settings (NAT, Bridged, Host-only)
- **Shared Folders:** Share directories between host and guest
- **USB:** Configure USB device access

### Installing Guest Additions

Guest Additions improve performance and enable features like shared folders and better screen resolution:

1. Start your VM
2. Click "Devices" → "Insert Guest Additions CD image"
3. In the guest OS, open the CD drive and run the installer
4. For Linux guests: `sudo sh /media/cdrom/VBoxLinuxAdditions.run`
5. For Windows guests: Run VBoxWindowsAdditions.exe
6. Reboot the guest OS

## Useful Management Commands

Check VirtualBox version:
```bash
vboxmanage --version
```

List all VMs:
```bash
vboxmanage list vms
```

Start a VM in headless mode (no window):
```bash
vboxmanage startvm "VM Name" --type headless
```

Clone a VM:
```bash
vboxmanage clonevm "Original VM" --name "Cloned VM" --register
```

## Troubleshooting Common Issues

**VMs won't start or are very slow:** Check if hardware virtualization (VT-x/AMD-V) is enabled in your BIOS/UEFI settings.

**Kernel driver not installed:** Run `sudo /sbin/vboxconfig` to rebuild kernel modules.

**USB devices not working:** Make sure you've added your user to the vboxusers group and logged out/in.

**Screen resolution issues:** Install Guest Additions and select "View" → "Auto-resize Guest Display".

## Uninstalling VirtualBox

If installed from Ubuntu repositories:
```bash
sudo apt remove virtualbox
sudo apt autoremove
```

If installed from Oracle's repository:
```bash
sudo apt remove virtualbox-7.0
sudo apt autoremove
```

To remove configuration files and VMs:
```bash
rm -rf ~/.config/VirtualBox
rm -rf ~/VirtualBox\ VMs
```

## Next Steps

Now that you have VirtualBox set up, you can experiment with different operating systems, create development environments, or test software in isolation. The snapshot feature makes it easy to experiment without consequences, and shared folders let you move files between your host and guest systems seamlessly.
