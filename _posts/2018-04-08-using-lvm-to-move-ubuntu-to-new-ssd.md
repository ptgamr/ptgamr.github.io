### Moving old LVM Partition to the new SSD

After you have installed the SSD, in my case, it is located at `/dev/nvme0n1`, and let say your current Ubuntu is installed at `/dev/sdOLD`

```bash
# Make it a LVM Physical volume
sudo pvcreate /dev/nvme0n1

# Verify it
sudo lvmdiskscan

# My result:
#  /dev/nvme0n1          [     232.89 GiB] LVM physical volume#
#  /dev/ubuntu-vg/root   [      123.31 GiB]
#  .....
#  /dev/sdOLD   [      123.31 GiB] LVM physical volume#
# 

# Add the new LVM volume to existing ubuntu-vg LVM group
sudo vgextend ubuntu-vg /dev/nvme0n1

# Moving the old partion to the new 
sudo pvmove /dev/sdOLD

# Removing the old volume from ubunut-vg group 
sudo vgreduce ubuntu-vg /dev/sdOLD

# Remove the physical logical volume (if you wish)
sudo pvremove /dev/sdOLD

# Updating grub
sudo update-grub
sudo grub-install /dev/nvme0n1
```

### Moving your home folder to new SSD

```bash
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

### References

- Good introduction about LVM: https://www.digitalocean.com/community/tutorials/how-to-use-lvm-to-manage-storage-devices-on-ubuntu-16-04

- The original idea is taken from the answer in this thread: https://askubuntu.com/questions/161279/how-do-i-move-my-lvm-250-gb-root-partition-to-a-new-120gb-hard-disk

