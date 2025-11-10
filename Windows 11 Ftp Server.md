# Setting Up an FTP Site on Windows 11 with Basic Authentication

## Prerequisites

- Windows 11 Pro, Enterprise, or Education edition
- Administrator access
- A local or domain user account for FTP authentication

## Step 1: Install IIS and FTP Server

1. **Open Windows Features**
   - Press `Win + R`, type `optionalfeatures`, and press Enter
   - Or go to Settings > Apps > Optional Features > More Windows features

2. **Enable IIS and FTP**
   - Scroll down and expand **Internet Information Services**
   - Expand **FTP Server** and check both:
     - FTP Service
     - FTP Extensibility
   - Also expand **Web Management Tools** and check:
     - IIS Management Console
   - Click **OK** and wait for installation to complete

## Step 2: Open IIS Manager

1. Press `Win + S` and search for **Internet Information Services (IIS) Manager**
2. Launch the application
3. You should see your computer name in the left panel under "Connections"

## Step 3: Create an FTP Site

1. **Start the FTP Site Wizard**
   - In IIS Manager, right-click on **Sites** in the left panel
   - Select **Add FTP Site**

2. **Configure Site Information**
   - **FTP site name**: Enter a name (e.g., "My FTP Site")
   - **Physical path**: Click the browse button (...) and select a folder where FTP files will be stored (e.g., `C:\FTPRoot`)
   - Click **Next**

3. **Configure Binding and SSL**
   - **IP Address**: Select **All Unassigned** (or choose a specific IP if needed)
   - **Port**: Leave as **21** (default FTP port)
   - **SSL**: Select **No SSL** for basic setup (or choose "Allow SSL" if you have a certificate)
   - Check **Start FTP site automatically**
   - Click **Next**

4. **Configure Authentication and Authorization**
   - **Authentication**: Check **Basic**
   - **Authorization**: 
     - From the dropdown, select **Specified users**
     - Enter the Windows username(s) that should have access (e.g., `username` or `DOMAIN\username`)
     - **Permissions**: Check **Read** and/or **Write** as needed
   - Click **Finish**

## Step 4: Configure Windows Firewall

1. **Open Windows Defender Firewall**
   - Press `Win + S` and search for "Windows Defender Firewall"
   - Click **Allow an app or feature through Windows Defender Firewall**

2. **Enable FTP Server**
   - Click **Change settings** (requires admin rights)
   - Scroll down and find **FTP Server**
   - Check the boxes for **Private** and/or **Public** networks as needed
   - Click **OK**

## Step 5: Set NTFS Permissions

1. Navigate to your FTP root folder in File Explorer (e.g., `C:\FTPRoot`)
2. Right-click the folder and select **Properties**
3. Go to the **Security** tab
4. Click **Edit** to modify permissions
5. Add your FTP user if not already listed:
   - Click **Add**
   - Enter the username and click **Check Names**
   - Click **OK**
6. Select the user and grant appropriate permissions (Read, Write, Modify)
7. Click **Apply** and **OK**

## Step 6: Test Your FTP Site

1. **Test from Local Machine**
   - Open File Explorer
   - In the address bar, type: `ftp://localhost` or `ftp://127.0.0.1`
   - Enter your username and password when prompted

2. **Test from Another Computer**
   - Find your computer's IP address (run `ipconfig` in Command Prompt)
   - From another computer, use: `ftp://YOUR_IP_ADDRESS`
   - Enter credentials when prompted

3. **Using Command Line**
   `ftp localhost`
   - Enter username and password when prompted
   - Type `dir` to list files
   - Type `quit` to exit

## Troubleshooting Tips

- **Can't connect**: Verify Windows Firewall allows FTP Server
- **Login fails**: Check username/password and ensure the user has NTFS permissions on the FTP folder
- **Passive mode issues**: In IIS Manager, go to FTP Firewall Support and configure the data channel port range (e.g., 5000-5010), then open these ports in the firewall
- **Permission denied**: Verify both IIS authorization rules and NTFS permissions are correctly set

Your FTP site is now configured and ready to use with basic authentication on Windows 11.
