
# Windows 10 Cheatsheet

## System administration

### Clean free disk space

Open cmd prompt as admin

`> cipher /w:<drive-letter>`

## IIS

### Create a secure dev site and access from Chrome

1. Add a new IP address to your PC.
2. Open notepad.exe as an administrator. Open "c:\windows\system32\drivers\etc\hosts". Add an entry for your new IP, e.g. "192.168.0.10  mysite.internal". Save.
3. Open PowerShell as an administrator.
`PS C:\Windows\system32> New-SelfSignedCertificate -DnsName "demo.internal" -CertStoreLocation "Cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(10) -Type SSLServerAuthentication`
4. Open mmc.exe. Add the snap-in "Certificates -> Computer account".
5. Go to "Personal -> Certificates". Select the cert you just created. Right-click -> All tasks -> Export (do not export private key).
6. From Chrome, go to chrome://certificate-manager/. Select Custom -> Import -> choose the file you just created.
7. In IIS, select your site. Go to Bindings. Add new binding. Choose the IP address you created. Choose the SSL certificate you created. Restart site.
8. It may be necessary to close all instances of Chrome.
