
# Windows 10 Cheatsheet

## System administration

### Clean free disk space

Open cmd prompt as admin

`> cipher /w:<drive-letter>`

## IIS

### Create a secure dev site and access from Chrome

- Open PowerShell as an administrator.
`PS C:\Windows\system32> New-SelfSignedCertificate -CertStoreLocation cert:\LocalMachine\My -DnsName {computer name} -NotAfter (Get-Date).AddYears(10) -FriendlyName '{cert name}'`
- Open mmc.exe. Add the snap-in "Certificates -> Computer account".
- Go to "Personal -> Certificates". Select the cert you just created. Right-click -> All tasks -> Export (do not export private key).
- From Chrome, go to chrome://certificate-manager/. Select Custom -> Import -> choose the file you just created.
- In IIS, select your site. Go to Bindings. Add new binding. Choose the SSL certificate you just created. Restart site.
- It may be necessary to close all instances of Chrome.


