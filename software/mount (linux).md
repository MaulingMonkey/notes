# Setup Commands

```sh
sudo apt-get install cifs-utils         # /sbin/mount.smb3 (to enable fstab)
sudo vim                /etc/fstab.nas1
#sudo chown root:root   /etc/fstab.nas1
sudo chmod 600          /etc/fstab.nas1
sudo vim                /etc/fstab
```



# /etc/fstab.nas1

```
username=user
domain=WORKGROUP
password=[redacted]
```



# /etc/fstab

```
//nas1/all /mnt/nas1 smb vers=3.0,uid=1000,gid=1000,credentials=/etc/fstab.nas1 0 0
```

`uid`/`gid` correspond to `user:user` per `/etc/passwd` and `/etc/group`



# Notes

Can't setup through `Files` etc. as those attempt to negotiate SMB1 whereas Synology NAS by default demands SMB2+ (for security?)
