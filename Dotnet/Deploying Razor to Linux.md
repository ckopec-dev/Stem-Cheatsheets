# Deploying a .NET Razor Application to Linux Server

This tutorial covers deploying an ASP.NET Core Razor Pages application to a Linux server using Ubuntu as the example distribution.

## Prerequisites

- A Linux server (Ubuntu 20.04+ recommended) with SSH access
- A .NET Razor Pages application ready for deployment
- Basic familiarity with terminal/command line

## Step 1: Prepare Your Local Application

First, publish your application locally to ensure it builds correctly:

```bash
dotnet publish -c Release -o ./publish
```

This creates optimized production files in the `./publish` directory.

## Step 2: Set Up the Linux Server

### Install .NET Runtime

SSH into your server and install the .NET runtime:

```bash
# Add Microsoft package repository
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

# Install .NET runtime
sudo apt-get update
sudo apt-get install -y aspnetcore-runtime-8.0
```

Replace `8.0` with your .NET version (e.g., `6.0`, `7.0`).

### Install Nginx (Reverse Proxy)

```bash
sudo apt-get install -y nginx
```

## Step 3: Transfer Application Files

From your local machine, copy the published files to the server:

```bash
scp -r ./publish/* username@your-server-ip:/var/www/myapp/
```

Or use an FTP client like FileZilla for a GUI approach.

On the server, set proper permissions:

```bash
sudo chown -R www-data:www-data /var/www/myapp
sudo chmod -R 755 /var/www/myapp
```

## Step 4: Create a Systemd Service

Create a service file to manage your application:

```bash
sudo nano /etc/systemd/system/myapp.service
```

Add the following configuration:

```ini
[Unit]
Description=My .NET Razor Application
After=network.target

[Service]
WorkingDirectory=/var/www/myapp
ExecStart=/usr/bin/dotnet /var/www/myapp/YourApp.dll
Restart=always
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=myapp
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Replace `YourApp.dll` with your actual DLL filename.

Enable and start the service:

```bash
sudo systemctl enable myapp.service
sudo systemctl start myapp.service
sudo systemctl status myapp.service
```

## Step 5: Configure Nginx as Reverse Proxy

Create an Nginx configuration file:

```bash
sudo nano /etc/nginx/sites-available/myapp
```

Add this configuration:

```nginx
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;
    
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Enable the site and restart Nginx:

```bash
sudo ln -s /etc/nginx/sites-available/myapp /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

## Step 6: Configure Firewall

Allow HTTP and HTTPS traffic:

```bash
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
sudo ufw enable
```

## Step 7: Set Up SSL with Let's Encrypt (Optional but Recommended)

Install Certbot:

```bash
sudo apt-get install -y certbot python3-certbot-nginx
```

Obtain and install certificate:

```bash
sudo certbot --nginx -d your-domain.com -d www.your-domain.com
```

Follow the prompts. Certbot will automatically configure Nginx for HTTPS.

## Step 8: Configure Application Settings

Create or modify `appsettings.Production.json` on the server:

```bash
sudo nano /var/www/myapp/appsettings.Production.json
```

Example configuration:

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "DefaultConnection": "your-production-connection-string"
  }
}
```

Restart your application after changes:

```bash
sudo systemctl restart myapp.service
```

## Troubleshooting

### View Application Logs

```bash
sudo journalctl -fu myapp.service
```

### Check Nginx Logs

```bash
sudo tail -f /var/log/nginx/error.log
sudo tail -f /var/log/nginx/access.log
```

### Common Issues

**Port 5000 in use**: Check your application's port configuration in `Program.cs` or use environment variables.

**Permission denied**: Ensure www-data user has access to application files.

**502 Bad Gateway**: Application isn't running. Check service status with `sudo systemctl status myapp.service`.

## Updating Your Application

To deploy updates:

1. Stop the service: `sudo systemctl stop myapp.service`
2. Upload new files to `/var/www/myapp`
3. Start the service: `sudo systemctl start myapp.service`

## Additional Considerations

- **Database**: Set up your database server (PostgreSQL, MySQL, etc.) and update connection strings
- **Environment Variables**: Store sensitive data in environment variables or Azure Key Vault
- **Monitoring**: Consider using tools like Prometheus, Grafana, or Application Insights
- **Backups**: Implement regular backup strategies for your application and database
- **CI/CD**: Automate deployments using GitHub Actions, Azure DevOps, or Jenkins

Your .NET Razor application should now be running on your Linux server!
