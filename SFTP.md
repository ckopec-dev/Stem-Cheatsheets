# SFTP Tutorial for Ubuntu

## What is SFTP?

SFTP (SSH File Transfer Protocol) is a secure file transfer protocol that runs over SSH. It provides encrypted file transfers between systems, making it much safer than traditional FTP.

## Prerequisites

- Ubuntu system with SSH installed
- Root or sudo access
- Basic command line knowledge

## Part 1: Setting Up SFTP Server

### Install OpenSSH Server

```bash
sudo apt update
sudo apt install openssh-server
```

### Verify SSH is Running

```bash
sudo systemctl status ssh
```

If it's not running, start it:

```bash
sudo systemctl start ssh
sudo systemctl enable ssh
```

### Configure SSH for SFTP

Edit the SSH configuration file:

```bash
sudo nano /etc/ssh/sshd_config
```

Add or modify these lines at the end of the file:

```
Match Group sftp_users
    ChrootDirectory /home/%u
    ForceCommand internal-sftp
    AllowTcpForwarding no
    X11Forwarding no
```

This configuration creates a restricted environment for SFTP users.

### Create SFTP User Group

```bash
sudo groupadd sftp_users
```

### Create an SFTP User

```bash
sudo useradd -m -G sftp_users -s /bin/false sftpuser
sudo passwd sftpuser
```

### Set Up Directory Permissions

The chroot directory must be owned by root:

```bash
sudo chown root:root /home/sftpuser
sudo chmod 755 /home/sftpuser
```

Create an upload directory that the user can write to:

```bash
sudo mkdir /home/sftpuser/uploads
sudo chown sftpuser:sftp_users /home/sftpuser/uploads
sudo chmod 755 /home/sftpuser/uploads
```

### Restart SSH Service

```bash
sudo systemctl restart ssh
```

## Part 2: Using SFTP Client

### Connect to SFTP Server

From another machine or the same machine:

```bash
sftp username@hostname
```

Example:

```bash
sftp sftpuser@192.168.1.100
```

You'll be prompted for a password.

### Basic SFTP Commands

Once connected, you can use these commands:

**Navigate Remote Server:**
- `pwd` - Print working directory
- `ls` - List files
- `cd directory` - Change directory
- `mkdir directory` - Create directory

**Navigate Local Machine:**
- `lpwd` - Print local working directory
- `lls` - List local files
- `lcd directory` - Change local directory

**Transfer Files:**
- `put localfile.txt` - Upload file
- `get remotefile.txt` - Download file
- `put -r localdir` - Upload directory recursively
- `get -r remotedir` - Download directory recursively

**Other Commands:**
- `rm filename` - Delete remote file
- `rmdir dirname` - Remove remote directory
- `rename oldname newname` - Rename remote file
- `exit` or `bye` - Disconnect

### Example Session

```bash
sftp sftpuser@192.168.1.100
sftp> cd uploads
sftp> pwd
sftp> put myfile.txt
sftp> ls
sftp> get remotefile.txt
sftp> exit
```

## Part 3: Advanced Usage

### Using SFTP with SSH Keys (Passwordless Login)

Generate SSH key pair on client:

```bash
ssh-keygen -t rsa -b 4096
```

Copy public key to server:

```bash
ssh-copy-id sftpuser@192.168.1.100
```

Now you can connect without a password:

```bash
sftp sftpuser@192.168.1.100
```

### Batch File Transfers

Create a batch file with commands:

```bash
nano sftp_commands.txt
```

Add commands:

```
cd uploads
put file1.txt
put file2.txt
get remotefile.txt
bye
```

Execute batch file:

```bash
sftp -b sftp_commands.txt sftpuser@192.168.1.100
```

### Using FileZilla (GUI Client)

1. Install FileZilla:
   ```bash
   sudo apt install filezilla
   ```

2. Open FileZilla and enter:
   - Host: `sftp://your-server-ip`
   - Username: your SFTP username
   - Password: your password
   - Port: 22

3. Click "Quickconnect"

## Part 4: Security Best Practices

### Change Default SSH Port

Edit SSH config:

```bash
sudo nano /etc/ssh/sshd_config
```

Change the line:

```
Port 2222
```

Restart SSH:

```bash
sudo systemctl restart ssh
```

### Disable Root Login

In `/etc/ssh/sshd_config`:

```
PermitRootLogin no
```

### Enable Firewall

```bash
sudo ufw allow 22/tcp
sudo ufw enable
```

If you changed the SSH port:

```bash
sudo ufw allow 2222/tcp
```

### Limit User Access

Only allow specific users to SSH:

```
AllowUsers sftpuser user2 user3
```

## Troubleshooting

### Connection Refused

Check if SSH is running:

```bash
sudo systemctl status ssh
```

Check firewall rules:

```bash
sudo ufw status
```

### Permission Denied

Verify directory permissions are correct:

```bash
ls -la /home/sftpuser
```

### Chroot Issues

Ensure the chroot directory and all parent directories are owned by root and not group-writable.

## Summary

You now have a working SFTP setup on Ubuntu. Remember:
- SFTP users are in the `sftp_users` group
- They can only access their home directory
- Files must be uploaded to the `uploads` subdirectory
- Use SSH keys for enhanced security
- Regularly update your system for security patches
