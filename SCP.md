# SCP Command Tutorial

## What is SCP?

SCP (Secure Copy Protocol) is a command-line utility that allows you to securely transfer files and directories between hosts over an SSH connection. It encrypts both the files and passwords during transfer, making it a safe choice for moving data across networks.

## Basic Syntax

```bash
scp [options] source destination
```

## Common Use Cases

### 1. Copy a Local File to a Remote Server

```bash
scp /path/to/local/file.txt username@remote_host:/path/to/destination/
```

**Example:**
```bash
scp document.pdf john@192.168.1.100:/home/john/documents/
```

### 2. Copy a Remote File to Your Local Machine

```bash
scp username@remote_host:/path/to/remote/file.txt /path/to/local/destination/
```

**Example:**
```bash
scp john@192.168.1.100:/home/john/report.pdf ~/Downloads/
```

### 3. Copy a Directory Recursively

Use the `-r` flag to copy entire directories:

```bash
scp -r /path/to/local/directory username@remote_host:/path/to/destination/
```

**Example:**
```bash
scp -r ~/projects/myapp john@192.168.1.100:/var/www/
```

### 4. Copy Between Two Remote Hosts

```bash
scp username1@host1:/path/to/file username2@host2:/path/to/destination/
```

### 5. Copy Multiple Files

```bash
scp file1.txt file2.txt file3.txt username@remote_host:/destination/
```

## Important Options

| Option | Description |
|--------|-------------|
| `-r` | Recursively copy entire directories |
| `-P port` | Specify SSH port (note: uppercase P) |
| `-p` | Preserve modification times, access times, and modes |
| `-q` | Quiet mode (suppress progress meter and warnings) |
| `-v` | Verbose mode (show detailed debugging information) |
| `-C` | Enable compression during transfer |
| `-i identity_file` | Specify SSH key file for authentication |
| `-l limit` | Limit bandwidth in Kbit/s |

## Practical Examples

### Using a Custom SSH Port

```bash
scp -P 2222 file.txt username@remote_host:/destination/
```

### Using an SSH Key

```bash
scp -i ~/.ssh/my_private_key file.txt username@remote_host:/destination/
```

### Copy with Compression (Good for Slow Connections)

```bash
scp -C largefile.zip username@remote_host:/destination/
```

### Preserve File Attributes

```bash
scp -p important_file.txt username@remote_host:/destination/
```

### Limit Bandwidth to 1000 Kbit/s

```bash
scp -l 1000 bigfile.iso username@remote_host:/destination/
```

## Tips and Best Practices

1. **Always use absolute paths** on remote systems to avoid confusion about the working directory.

2. **Use tab completion** when typing local file paths to avoid typos.

3. **Test with a small file first** when transferring to a new host to verify connectivity and permissions.

4. **Use wildcards carefully:**
   ```bash
   scp username@remote_host:"/path/to/files/*.txt" ./local_directory/
   ```
   Note the quotes around the remote path to prevent local shell expansion.

5. **Check available disk space** on the destination before transferring large files.

6. **For frequent transfers to the same host**, consider setting up SSH config (`~/.ssh/config`) with aliases:
   ```
   Host myserver
       HostName 192.168.1.100
       User john
       Port 2222
       IdentityFile ~/.ssh/my_key
   ```
   Then you can simply use:
   ```bash
   scp file.txt myserver:/destination/
   ```

## Troubleshooting

### Permission Denied

- Check that you have read permissions on the source file
- Verify write permissions on the destination directory
- Ensure your SSH key or password is correct

### Connection Refused

- Verify the remote host is running an SSH server
- Check that you're using the correct port
- Ensure firewall rules allow SSH connections

### Host Key Verification Failed

This happens when the remote host's key has changed. If you trust the host:
```bash
ssh-keygen -R hostname
```
Then try the scp command again.

## Alternatives to Consider

- **`rsync`**: Better for syncing directories and resuming interrupted transfers
- **`sftp`**: Interactive file transfer with more features
- **`sshfs`**: Mount remote directories as local filesystems

## Security Notes

- SCP encrypts data during transfer, making it secure for sensitive files
- Always verify the host key fingerprint on first connection
- Use SSH keys instead of passwords when possible
- Keep your SSH client software up to date
