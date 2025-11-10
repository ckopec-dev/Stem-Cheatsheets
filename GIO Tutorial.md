# Mounting Windows Shares on Linux with GIO

## What is GIO?

GIO (GNOME I/O) is part of GLib and provides a modern, virtual filesystem layer for GNOME desktop environments. It uses GVFS (GNOME Virtual File System) in the background to handle network shares, including Windows SMB/CIFS shares, without requiring root permissions or manual `/etc/fstab` modifications.

## Prerequisites

- Linux distribution with GNOME or a desktop environment using GVFS
- `gvfs` and `gvfs-backends` packages installed
- Network connectivity to the Windows share
- Valid credentials for the Windows share

Install the required packages if needed:

```bash
# Ubuntu/Debian
sudo apt install gvfs gvfs-backends

# Fedora
sudo dnf install gvfs gvfs-smb

# Arch Linux
sudo pacman -S gvfs gvfs-smb
```

## Using GIO Command Line

### Basic Syntax

```bash
gio mount smb://[username@]server/share
```

### Mounting a Windows Share

**Example 1: Mount with username prompt**
```bash
gio mount smb://192.168.1.100/SharedFolder
```

**Example 2: Mount with username specified**
```bash
gio mount smb://john@192.168.1.100/SharedFolder
```

**Example 3: Mount with domain**
```bash
gio mount smb://DOMAIN\;john@192.168.1.100/SharedFolder
```

You'll be prompted for a password. The share will be mounted at:
```
/run/user/$UID/gvfs/smb-share:server=192.168.1.100,share=SharedFolder
```

### Listing Mounted Shares

```bash
gio mount -l
```

This displays all currently mounted locations, including Windows shares.

### Unmounting a Share

```bash
gio mount -u smb://192.168.1.100/SharedFolder
```

Or unmount by path:
```bash
gio mount -u /run/user/$UID/gvfs/smb-share:server=192.168.1.100,share=SharedFolder
```

## Using the File Manager

Most GNOME-based file managers (Nautilus, Nemo, Caja) integrate with GIO automatically.

1. Open your file manager
2. Look for "Other Locations" or "Network" in the sidebar
3. At the bottom, find "Connect to Server" or type directly in the address bar
4. Enter: `smb://server/share`
5. Press Enter and provide credentials when prompted

The share will appear in your file manager sidebar and remain mounted until you unmount it or log out.

## Accessing Mounted Shares

### From Command Line

```bash
# Navigate to the mount point
cd /run/user/$(id -u)/gvfs/

# List available mounts
ls -la

# Access a specific share
cd smb-share:server=192.168.1.100,share=SharedFolder/
```

### Creating Symbolic Links

For easier access, create a symbolic link:

```bash
ln -s /run/user/$(id -u)/gvfs/smb-share:server=192.168.1.100,share=SharedFolder ~/WindowsShare
```

## Advanced Usage

### Non-Interactive Mounting (Scripts)

For scripting, you can store credentials in GNOME Keyring or use environment variables:

```bash
# Using expect for automation (install expect first)
expect << EOF
spawn gio mount smb://192.168.1.100/SharedFolder
expect "User*"
send "username\r"
expect "Password*"
send "password\r"
expect eof
EOF
```

### Checking Mount Status

```bash
# Check if a share is mounted
gio mount -l | grep "192.168.1.100"

# Get detailed info
gio info smb://192.168.1.100/SharedFolder
```

### Working with Files

Once mounted, use standard commands:

```bash
# Copy files
cp ~/document.pdf /run/user/$(id -u)/gvfs/smb-share:.../

# List files
ls -la /run/user/$(id -u)/gvfs/smb-share:.../

# Use with other tools
rsync -av ~/backup/ /run/user/$(id -u)/gvfs/smb-share:.../
```

## Troubleshooting

### Share Not Visible

```bash
# Check GVFS daemon is running
systemctl --user status gvfs-daemon

# Restart if needed
systemctl --user restart gvfs-daemon
```

### Permission Denied

Ensure you're using the correct username, domain, and password. For domain users:
```bash
gio mount smb://DOMAIN\;username@server/share
```

### Connection Timeout

Check network connectivity and firewall rules:
```bash
# Test SMB connectivity
smbclient -L //192.168.1.100 -U username
```

### Finding the Exact Mount Path

```bash
# Use gio to find the mount point
gio mount -l | grep -A 3 "192.168.1.100"
```

## Benefits of GIO over Traditional Mount

- **No root required**: User-level mounting without sudo
- **GUI integration**: Seamless integration with file managers
- **Credential management**: Uses GNOME Keyring for secure password storage
- **Automatic discovery**: Browse network shares easily
- **Clean unmounting**: Proper cleanup when logging out

## Persistence

GIO mounts don't persist across reboots by default. For persistent mounts, consider:

1. **Using GNOME startup applications** to run mount commands
2. **Creating a systemd user service**
3. **Traditional `/etc/fstab` entries** for system-wide mounts (requires root)

For most use cases, the manual mounting approach with GIO is sufficient and more secure than storing credentials in fstab.
