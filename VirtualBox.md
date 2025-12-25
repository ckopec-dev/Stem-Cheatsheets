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

# Managing VirtualBox from the Terminal

VirtualBox provides a powerful command-line interface called `vboxmanage` that lets you control virtual machines without opening the GUI. This tutorial covers the essential commands for managing VMs from the terminal.

## Prerequisites

- VirtualBox installed on your system
- Terminal/command prompt access
- Basic familiarity with command-line interfaces

## Getting Started

### Verify VBoxManage Installation

First, confirm VBoxManage is available:

```bash
vboxmanage --version
```

This should display your VirtualBox version.

## Essential Commands

### Listing Virtual Machines

View all registered VMs:

```bash
vboxmanage list vms
```

View running VMs only:

```bash
vboxmanage list runningvms
```

Get detailed information about a specific VM:

```bash
vboxmanage showvminfo "VM_Name"
```

### Starting Virtual Machines

Start a VM in headless mode (no GUI):

```bash
vboxmanage startvm "VM_Name" --type headless
```

Start with GUI:

```bash
vboxmanage startvm "VM_Name" --type gui
```

Start in detachable GUI mode:

```bash
vboxmanage startvm "VM_Name" --type separate
```

### Controlling Running VMs

Pause a running VM:

```bash
vboxmanage controlvm "VM_Name" pause
```

Resume a paused VM:

```bash
vboxmanage controlvm "VM_Name" resume
```

Gracefully shut down (requires Guest Additions):

```bash
vboxmanage controlvm "VM_Name" acpipowerbutton
```

Force power off (like pulling the plug):

```bash
vboxmanage controlvm "VM_Name" poweroff
```

Save the VM state:

```bash
vboxmanage controlvm "VM_Name" savestate
```

### Creating a New Virtual Machine

Create a new VM:

```bash
vboxmanage createvm --name "Ubuntu-Server" --ostype "Ubuntu_64" --register
```

Common OS types include: `Ubuntu_64`, `Debian_64`, `Windows10_64`, `Fedora_64`. List all types:

```bash
vboxmanage list ostypes
```

### Configuring Virtual Machines

Set memory (RAM) in MB:

```bash
vboxmanage modifyvm "VM_Name" --memory 2048
```

Set number of CPUs:

```bash
vboxmanage modifyvm "VM_Name" --cpus 2
```

Enable IOAPIC (required for multicore):

```bash
vboxmanage modifyvm "VM_Name" --ioapic on
```

Configure network adapter:

```bash
vboxmanage modifyvm "VM_Name" --nic1 nat
vboxmanage modifyvm "VM_Name" --nic2 bridged --bridgeadapter2 eth0
```

### Managing Storage

Create a virtual hard disk:

```bash
vboxmanage createhd --filename "/path/to/disk.vdi" --size 20480
```

Add a storage controller:

```bash
vboxmanage storagectl "VM_Name" --name "SATA Controller" --add sata --controller IntelAhci
```

Attach a hard disk:

```bash
vboxmanage storageattach "VM_Name" --storagectl "SATA Controller" --port 0 --device 0 --type hdd --medium "/path/to/disk.vdi"
```

Attach an ISO file:

```bash
vboxmanage storageattach "VM_Name" --storagectl "SATA Controller" --port 1 --device 0 --type dvddrive --medium "/path/to/ubuntu.iso"
```

Remove media:

```bash
vboxmanage storageattach "VM_Name" --storagectl "SATA Controller" --port 1 --device 0 --type dvddrive --medium none
```

### Snapshots

Create a snapshot:

```bash
vboxmanage snapshot "VM_Name" take "Snapshot_Name" --description "Clean install"
```

List snapshots:

```bash
vboxmanage snapshot "VM_Name" list
```

Restore a snapshot:

```bash
vboxmanage snapshot "VM_Name" restore "Snapshot_Name"
```

Delete a snapshot:

```bash
vboxmanage snapshot "VM_Name" delete "Snapshot_Name"
```

### Cloning VMs

Clone a VM:

```bash
vboxmanage clonevm "Original_VM" --name "Cloned_VM" --register
```

Create a linked clone (saves disk space):

```bash
vboxmanage clonevm "Original_VM" --name "Linked_Clone" --snapshot "Snapshot_Name" --options link --register
```

### Importing and Exporting

Export a VM to OVA format:

```bash
vboxmanage export "VM_Name" --output "/path/to/vm.ova"
```

Import an OVA/OVF file:

```bash
vboxmanage import "/path/to/vm.ova"
```

### Deleting VMs

Unregister a VM (keeps files):

```bash
vboxmanage unregistervm "VM_Name"
```

Delete a VM and all its files:

```bash
vboxmanage unregistervm "VM_Name" --delete
```

## Practical Example: Creating and Starting a VM

Here's a complete workflow to create and start a basic Ubuntu VM:

```bash
# Create the VM
vboxmanage createvm --name "Ubuntu-Test" --ostype "Ubuntu_64" --register

# Configure basic settings
vboxmanage modifyvm "Ubuntu-Test" --memory 2048 --cpus 2 --ioapic on

# Create and attach storage
vboxmanage createhd --filename "$HOME/VirtualBox VMs/Ubuntu-Test/Ubuntu-Test.vdi" --size 25600

vboxmanage storagectl "Ubuntu-Test" --name "SATA Controller" --add sata --controller IntelAhci

vboxmanage storageattach "Ubuntu-Test" --storagectl "SATA Controller" --port 0 --device 0 --type hdd --medium "$HOME/VirtualBox VMs/Ubuntu-Test/Ubuntu-Test.vdi"

# Attach Ubuntu ISO
vboxmanage storageattach "Ubuntu-Test" --storagectl "SATA Controller" --port 1 --device 0 --type dvddrive --medium "$HOME/Downloads/ubuntu-22.04.iso"

# Configure network
vboxmanage modifyvm "Ubuntu-Test" --nic1 nat

# Start the VM
vboxmanage startvm "Ubuntu-Test" --type gui
```

## Tips and Best Practices

- Always use quotes around VM names that contain spaces
- Use `--type headless` for servers that don't need a GUI
- Take snapshots before major changes
- Use `vboxmanage list` commands to verify your configurations
- Check the exit status of commands in scripts: `$?` (bash) contains the return code
- For automation, consider using the `--wait` flag with certain commands

## Getting Help

View all available commands:

```bash
vboxmanage --help
```

Get help for a specific command:

```bash
vboxmanage startvm --help
```

This tutorial covers the most common VBoxManage operations. The command-line interface provides even more advanced options for networking, USB devices, shared folders, and remote management.
