
# Windows PE Cheat Sheet

## Disk Administration

### Wipe and format new or used drive

~~~

> diskpart
> list disk
> select disk <disk-number>
> list partition
> select partion <partition-number>
> delete partition
> create partition primary (uses max space up to 2tb)
> exit

reboot

> format c: /fs:ntfs /p:3
> chkdsk c: /f /b /v /r

~~~
