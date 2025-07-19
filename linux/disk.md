# Disk

* The commands below are related to disk or boot up

## Fstab

* reference to mount other disk

```
<file system> <mount point>   <type>  <options>       <dump>  <pass>
```

```
UUID=467031ef-c93b-4fb2-a825-0157f3606d45 /mnt/nvme0n1p7 ext4 auto,user,rw 0 0
/mnt/nvme0n1p7/pinloon/dev_ws /home/pinloon/dev_ws ext4 auto,user,rw 0 0
```

## Clone disk using dd

```
$ sudo dd if=/dev/old of=/dev/new status=progress bs=64K conv=noerror,sync
```

## Off Nvidia driver during boot up

* Sometimes during boot up, you may see the error of "ima: error communicating to tpm chip" followed by hanging of screen and the screen would not be able to display properly
* Press "E" while before entering Ubuntu and press "F10" after edit
* find "quite splash" then add "nomodeset" after it

## Mount Shared Drive

* Using default `rclone` may face issues such as

```
sudo apt install rclone
```

```
Failed to copy: failed to open source object: unauthenticated: Unauthenticated
```

[https//www.reddit.com/r/rclone/comments/15s4sis/onedrive\_unauthenticated/?rdt=42577](https://www.reddit.com/r/rclone/comments/15s4sis/onedrive_unauthenticated/?rdt=42577)

```
sudo nano /etc/fuse.conf
Uncomment this line (remove #):
user_allow_other
```

```
rclone mount OneDrive:SharedDocuments /media/user/OneDrive/SharedDocuments/ --vfs-cache-mode full --allow-non-empty  --allow-other --daemon --config /home/user/.config/rclone/rclone.conf
```
