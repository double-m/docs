### How to make an USB key with an Ext4 filesystem witable to a specific user on a Debian GNU/Linux

*Compatibility: works with version 2.2.0 of the Debian package `usb-modeswitch`;
it doesn't with version 1.2.3.*

Insert the USB key and get its UUID, for example:

```
me@linuxbox:~$ /sbin/blkid
/dev/sda1: LABEL="MYLABEL" UUID="wwww-wwww" TYPE="vfat"
/dev/sda5: TYPE="swap" UUID="xxxxxxxx-xxxx-xxxx-xxxx"
/dev/sda6: UUID="yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy" TYPE="ext3"
/dev/sdb1: UUID="zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz" TYPE="ext4"
```

The next steps must be taken as root or sudoer.
Create a mount point with the privileges of the specific user:

```
linuxbox:~# mkdir -p /media/me/myusb
linuxbox:~# chown me:me /media/me/myusb
```

As root or sudoer, add a line similar to the following to your `/etc/fstab`:

```
UUID=zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz /media/me/myusb ext4 defaults,user,noauto 0 0
```

Now eject the USB key and insert it again; it will be mounted on
`/media/me/myusb` but the permssion on that directory will be
overridden, so let's change them according to our needs (we will do
the same as before, but with the USB key mouted):

```
linuxbox:~# ls -ld /media/me/myusb
drwxr-xr-x 2 root root 4096 Feb 21 10:37 /media/marcello/my/
linuxbox:~# chown me:me /media/me/myusb
linuxbox:~# ls -ld /media/me/myusb
drwxr-xr-x 2 me:me 4096 Feb 21 10:37 /media/marcello/my/
```

Done! Now, eject the USB key and insert it again: it will be still mounted on
`/media/me/myusb`, this time writable to the user `me`.

