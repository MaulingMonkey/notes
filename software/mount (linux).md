# Setup Commands

```sh
# Depending on distro:
sudo apt-get install cifs-utils                     # /sbin/mount.smb3 (to enable fstab)
sudo pacman -S cifs-utils                           # /usr/bin/mount.smb3

sudo mkdir              /mnt/nas1
sudo vim                /etc/fstab.nas1.credentials
#sudo chown root:root   /etc/fstab.nas1.credentials # already owned by root
sudo chmod 600          /etc/fstab.nas1.credentials # might not play nice with non-sudo mounting?
sudo vim                /etc/fstab

sudo systemctl daemon-reload                        # or `mount` will whine/warn on SteamOS?
sudo mount              /mnt/nas1
```



# /etc/fstab.nas1.credentials

```text
username=user
domain=WORKGROUP
password=[redacted]
```



# /etc/fstab

```text
# Using on Steam Machine running SteamOS
//nas1/all /mnt/nas1 smb3 async,noauto,nofail,_netdev,x-systemd.automount,vers=3.0,uid=1000,gid=1000,credentials=/etc/fstab.nas1.credentials 0 0
# async:                allow batching multiple I/O requests together?
# noauto:               don't automount at boot (optimization)
# nofail:               allow successful boot even if it can't mount
# _netdev:              hint: wait for network services before mounting
# x-systemd.autmount:   auto-mount on demand

# Previously used on System76 Laptop running PopOS
//nas1/all /mnt/nas1 smb                    vers=3.0,uid=1000,gid=1000,credentials=/etc/fstab.nas1.credentials 0 0
```

`uid`/`gid` correspond to `user:user` per `/etc/passwd` and `/etc/group`



# Notes

Can't setup through `Files` etc. as those attempt to negotiate SMB1 whereas Synology NAS by default demands SMB2+ (for security?)



# Related commands
-   `mount | grep "..."`
-   `findmnt`
-   `systemctl daemon-reload`
-   `systemctl list-units --type=mount`
-   <code>systemctl cat <span style="opacity: 25%">*unit*</span></code>
