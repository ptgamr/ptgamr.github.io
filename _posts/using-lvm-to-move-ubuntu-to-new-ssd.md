


# Moving your home folder to new SSD

```
# create new logical volume on the remaining space
sudo lvcreate -l 100%FREE -n home ubuntu-vg

# make file system
sudo mkfs -t ext4 /dev/ubuntu-vg/home

# mount
sudo mount /dev/ubuntu-vg/home /mnt/home

# RSync the home folder
sudo rsync -aXS --info=progress2 /home/. /mnt/home/.

# Backup the current /home
cd /
sudo mv /home /home_backup

# Mount /home to the new location
sudo mkdir /home

# Change fstab
sudo vim /etc/fstab

# Add following line:
UUID=<your_partition_uui>   /home   ext4    nodev,nosuid  0 2

# ( To get the UUID you can use: $ sudo blkid )

# Logout and login again to check if it works
# If everything is find, delete the backup folder
sudo rm -rf /home_backup

# You might shrink your root partition
sudo lvresize -L 20G --resizefs /dev/ubuntu-vg/root

# To extend /home to take up all the free space
sudo lvresize -l +100%FREE --resizefs /dev/ubuntu-vg/home
```

