# SSH Tutorial for Ubuntu

## What is SSH?

SSH (Secure Shell) is a cryptographic network protocol that allows you to securely access and manage remote computers over an unsecured network. It encrypts all traffic between your computer and the remote server, making it safe to send commands, transfer files, and manage systems remotely.

## Installing SSH

### Installing SSH Client

Most Ubuntu systems come with the SSH client pre-installed. To verify or install it:

```bash
sudo apt update
sudo apt install openssh-client
```

### Installing SSH Server

To allow others to connect to your Ubuntu machine, install the SSH server:

```bash
sudo apt update
sudo apt install openssh-server
```

Check if SSH is running:

```bash
sudo systemctl status ssh
```

To start SSH automatically on boot:

```bash
sudo systemctl enable ssh
```

## Basic SSH Usage

### Connecting to a Remote Server

The basic syntax for connecting via SSH is:

```bash
ssh username@hostname_or_ip
```

Example:

```bash
ssh john@192.168.1.100
ssh admin@example.com
```

On first connection, you'll see a message about the host's authenticity. Type `yes` to continue.

### Specifying a Port

If SSH runs on a non-standard port (default is 22):

```bash
ssh -p 2222 username@hostname
```

### Running a Single Command

Execute a command on the remote server without opening an interactive session:

```bash
ssh username@hostname "ls -la /var/www"
```

## SSH Key Authentication

Key-based authentication is more secure than passwords and enables passwordless login.

### Generating SSH Keys

Create a new SSH key pair:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Or for older systems that don't support Ed25519:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Press Enter to accept the default location (`~/.ssh/id_ed25519`) or specify a custom path. You can optionally set a passphrase for extra security.

### Copying Your Public Key to the Server

Use `ssh-copy-id` to automatically copy your public key:

```bash
ssh-copy-id username@hostname
```

Or manually copy the key:

```bash
cat ~/.ssh/id_ed25519.pub | ssh username@hostname "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### Testing Key Authentication

Try connecting again. You should not be prompted for a password:

```bash
ssh username@hostname
```

## SSH Configuration File

Create or edit `~/.ssh/config` to simplify connections:

```
Host myserver
    HostName 192.168.1.100
    User john
    Port 22
    IdentityFile ~/.ssh/id_ed25519

Host production
    HostName example.com
    User admin
    Port 2222
```

Now you can connect simply with:

```bash
ssh myserver
ssh production
```

## Server Configuration

The SSH server configuration file is located at `/etc/ssh/sshd_config`.

### Common Security Settings

Edit the configuration file:

```bash
sudo nano /etc/ssh/sshd_config
```

Recommended security changes:

```
# Disable root login
PermitRootLogin no

# Disable password authentication (after setting up keys)
PasswordAuthentication no

# Change default port (optional)
Port 2222

# Allow only specific users
AllowUsers john admin
```

After making changes, restart SSH:

```bash
sudo systemctl restart ssh
```

## Firewall Configuration

If you're using UFW (Uncomplicated Firewall), allow SSH:

```bash
sudo ufw allow ssh
```

Or for a custom port:

```bash
sudo ufw allow 2222/tcp
```

Enable the firewall:

```bash
sudo ufw enable
```

## File Transfer with SCP

Copy files to a remote server:

```bash
scp file.txt username@hostname:/path/to/destination/
```

Copy files from a remote server:

```bash
scp username@hostname:/path/to/file.txt /local/destination/
```

Copy directories recursively:

```bash
scp -r local_directory username@hostname:/remote/path/
```

## Using SFTP

SFTP provides an interactive file transfer session:

```bash
sftp username@hostname
```

Common SFTP commands:

- `ls` - list remote directory
- `lls` - list local directory
- `cd` - change remote directory
- `lcd` - change local directory
- `get filename` - download file
- `put filename` - upload file
- `exit` - quit SFTP

## SSH Tunneling (Port Forwarding)

### Local Port Forwarding

Access a remote service through your local machine:

```bash
ssh -L 8080:localhost:80 username@hostname
```

Now `localhost:8080` on your machine forwards to port 80 on the remote server.

### Remote Port Forwarding

Allow remote users to access your local service:

```bash
ssh -R 9090:localhost:3000 username@hostname
```

### Dynamic Port Forwarding (SOCKS Proxy)

Create a SOCKS proxy for secure browsing:

```bash
ssh -D 8080 username@hostname
```

## Troubleshooting

### Connection Refused

Check if SSH is running:

```bash
sudo systemctl status ssh
```

Verify firewall rules:

```bash
sudo ufw status
```

### Permission Denied

Ensure correct file permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/authorized_keys
```

### View SSH Logs

Check server logs for connection issues:

```bash
sudo tail -f /var/log/auth.log
```

### Verbose Connection Attempt

Debug connection issues with verbose output:

```bash
ssh -vvv username@hostname
```

## Best Practices

1. **Use key-based authentication** instead of passwords
2. **Disable root login** via SSH
3. **Change the default port** to reduce automated attacks
4. **Use strong passphrases** for SSH keys
5. **Keep SSH updated** with regular system updates
6. **Implement fail2ban** to block repeated failed login attempts
7. **Limit SSH access** to specific users or IP addresses
8. **Enable two-factor authentication** for critical systems

## Useful SSH Options

- `-i` - specify identity file (private key)
- `-p` - specify port number
- `-v` - verbose mode (use `-vvv` for maximum verbosity)
- `-N` - don't execute remote commands (useful for port forwarding)
- `-f` - run SSH in background
- `-X` - enable X11 forwarding (for GUI applications)
- `-C` - compress data

## Conclusion

SSH is an essential tool for securely managing remote Linux systems. Start with basic password authentication, then move to key-based authentication for better security. Always follow security best practices to keep your systems safe.
