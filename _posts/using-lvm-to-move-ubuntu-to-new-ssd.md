


# Moving your home folder

```
# create new logical volume on the remaining space
sudo lvcreate -l 100%FREE -n home ubuntu-vg

# make file system
sudo mkfs -t ext4 /dev/ubuntu-vg/home

# mount
sudo mount /dev/ubuntu-vg/home /mnt/home

# Rsync the home folder
sudo rsync -aXS --info=progress2 /home/. /mnt/home/.
```
