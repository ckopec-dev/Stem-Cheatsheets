# **Running a C# .NET App as a Service on Ubuntu**

This tutorial shows how to run a C# .NET 7 application as a **systemd service** on Ubuntu, with:

* Automatic startup on boot
* Logging to a dedicated directory
* Log rotation using `logrotate`

We’ll cover both **self-contained** and **framework-dependent** deployment.

---

## **1. Prerequisites**

* Ubuntu 20.04+ or Debian-based system
* .NET 7 SDK installed (for publishing):

```bash
dotnet --version
```

* Non-root user to run the service (recommended: `ubuntu`)
* Your C# project ready to deploy

---

## **2. Publish Your C# App**

### **Option A: Self-contained**

Publishes the app with the runtime included:

```bash
cd /path/to/your/project

dotnet publish -c Release -r linux-x64 --self-contained true -o /opt/myapp
```

> Pros: Can run on systems without .NET installed
> Cons: Larger size

### **Option B: Framework-dependent**

Requires .NET installed on the target system:

```bash
cd /path/to/your/project

dotnet publish -c Release -r linux-x64 --self-contained false -o /opt/myapp
```

> Pros: Smaller size
> Cons: Requires .NET runtime on the server

---

## **3. Create Installation and Log Directories**

```bash
# Create app directory
sudo mkdir -p /opt/myapp

# Copy published files
sudo cp -r /path/to/your/project/bin/Release/net7.0/linux-x64/publish/* /opt/myapp

# Set ownership
sudo chown -R ubuntu:ubuntu /opt/myapp

# Create log directory
sudo mkdir -p /var/log/myapp
sudo chown ubuntu:ubuntu /var/log/myapp
sudo chmod 755 /var/log/myapp
```

---

## **4. Create the systemd Service**

Create a service file:

```bash
sudo nano /etc/systemd/system/myapp.service
```

Example for **self-contained app**:

```ini
[Unit]
Description=My C# App Service
After=network.target

[Service]
WorkingDirectory=/opt/myapp
ExecStart=/opt/myapp/MyApp
Restart=always
RestartSec=5
StandardOutput=append:/var/log/myapp/myapp.log
StandardError=append:/var/log/myapp/myapp-error.log
User=ubuntu
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

Example for **framework-dependent app**:

```ini
ExecStart=/usr/bin/dotnet /opt/myapp/MyApp.dll
```

**Notes:**

* Replace `MyApp` or `MyApp.dll` with your actual app name
* `Restart=always` ensures automatic restart on crash
* `StandardOutput` and `StandardError` redirect logs to `/var/log/myapp/`

---

## **5. Reload systemd and Start the Service**

```bash
# Reload systemd to recognize new service
sudo systemctl daemon-reload

# Start the service
sudo systemctl start myapp

# Enable auto-start on boot
sudo systemctl enable myapp

# Check status
sudo systemctl status myapp
```

---

## **6. Verify Logs**

```bash
# Standard output
tail -f /var/log/myapp/myapp.log

# Error output
tail -f /var/log/myapp/myapp-error.log
```

---

## **7. Set Up Log Rotation**

Create a `logrotate` configuration:

```bash
sudo nano /etc/logrotate.d/myapp
```

Paste the following:

```text
/var/log/myapp/*.log {
    daily
    rotate 14
    compress
    delaycompress
    missingok
    notifempty
    copytruncate
}
```

**Explanation:**

* `daily` → rotate logs daily
* `rotate 14` → keep 14 old logs
* `compress` → compress old logs
* `copytruncate` → keeps service running while rotating logs

Test log rotation:

```bash
sudo logrotate -f /etc/logrotate.d/myapp
```

---

## **8. Optional: Automated Setup Script**

To automate steps 2–7, create `setup_myapp.sh`:

```bash
#!/bin/bash

APP_NAME="MyApp"
APP_PATH="/path/to/your/project"
INSTALL_DIR="/opt/myapp"
SERVICE_USER="ubuntu"
SERVICE_FILE="/etc/systemd/system/myapp.service"
LOG_DIR="/var/log/myapp"

# Publish
dotnet publish "$APP_PATH" -c Release -r linux-x64 --self-contained true -o "$INSTALL_DIR"

# Copy files and set ownership
sudo mkdir -p "$INSTALL_DIR"
sudo cp -r "$APP_PATH/bin/Release/net7.0/linux-x64/publish/"* "$INSTALL_DIR"
sudo chown -R $SERVICE_USER:$SERVICE_USER "$INSTALL_DIR"

# Create log directory
sudo mkdir -p "$LOG_DIR"
sudo chown $SERVICE_USER:$SERVICE_USER "$LOG_DIR"
sudo chmod 755 "$LOG_DIR"

# Create systemd service
sudo bash -c "cat > $SERVICE_FILE <<EOL
[Unit]
Description=$APP_NAME Service
After=network.target

[Service]
WorkingDirectory=$INSTALL_DIR
ExecStart=$INSTALL_DIR/$APP_NAME
Restart=always
RestartSec=5
StandardOutput=append:$LOG_DIR/$APP_NAME.log
StandardError=append:$LOG_DIR/$APP_NAME-error.log
User=$SERVICE_USER
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
EOL"

# Reload systemd and start service
sudo systemctl daemon-reload
sudo systemctl enable myapp
sudo systemctl start myapp
sudo systemctl status myapp

# Logrotate
sudo bash -c "cat > /etc/logrotate.d/myapp <<EOL
$LOG_DIR/*.log {
    daily
    rotate 14
    compress
    delaycompress
    missingok
    notifempty
    copytruncate
}
EOL"

echo "Setup complete! Logs: $LOG_DIR"
```

**Run the script:**

```bash
chmod +x setup_myapp.sh
./setup_myapp.sh
```

---

## ✅ **Summary**

* Published C# app to `/opt/myapp`
* Created a `systemd` service for automatic startup
* Configured logs in `/var/log/myapp/`
* Configured `logrotate` for automatic log management

Your C# .NET application now runs as a **robust service on Ubuntu**.

---
