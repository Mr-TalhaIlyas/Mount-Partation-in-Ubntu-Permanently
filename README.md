[![Linux](https://svgshare.com/i/Zhy.svg)](https://svgshare.com/i/Zhy.svg)  [![made-with-bash](https://img.shields.io/badge/Made%20with-Bash-1f425f.svg)](https://www.gnu.org/software/bash/) [![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fgithub.com%2FMr-TalhaIlyas%2FMount-Partation-in-Ubntu-Permanently&count_bg=%2379C83D&title_bg=%23555555&icon=&icon_color=%23E7E7E7&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)
# Mount-Partation-in-Ubntu-Permanently

## Mount Drives <= 2TB
## Mount Drives > 2TB 
## Make mount permanent

### Mount Drives <= 2TB
------
1. Type command 
```
lsblk
```
to see all the available storage devices available in your system.

![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s1.png)

## Partation using `Fdisk` command


Run the following command to list all existing partitions:
```
sudo fdisk -l
```
![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s2.png)

Select the storage disk you want to create partitions on by running the following command:
```
sudo fdisk /dev/sda
```
![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s3.png)

Create a New Partition;

1. press `n'
2. then just press `enter` to select default value -> this will set partation number.
3. then again press `enter` to select a default value -> this will define the 1st sector of paration.
4. enter the value you want to mount on the disk, e.g. in my case I want to mount all the disk memory and my disk is 1TB so I'll type `+1000GB` and press `enter`. -> this will define the last sector of paration

![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s4.png)

Note: if you are asked to remove signature  then just enter `yes` to proceed.

The system created the partition, but the changes are not written on the disk.

To write the changes on disk, run the `w` command:

![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s5.png)

Verify that the partition is created by running the following command:

```
sudo fdisk -l
```
![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s6.png)

or by 
```
lsblk
```
![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s7.png)

Once a partition has been created with the parted of fdisk command, format it before using it.

Format the partition by running the following command: (out mounted partation is sda1 as shonw in image)
```
sudo mkfs -t ext4 /dev/sda1
```
![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s8.png)

To begin interacting with the disk, create a mount point and mount the partition to it.

1. Create a mount point by running the following command:
```
sudo mkdir -p /data_hdd
```
After that, mount the partition by entering:
```
sudo mount -t auto /dev/sda1 /data_hdd
```
These lines wont output anything, check the mount point by typing `lsblk`
![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s9.png)

## Make Mount Permanent 

type 
```
sudo blkid
```
to get the UUID of all the storage devices on your system. copy the UUID of the newly mounted drive. We will use it while editing `fstab`

![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s10.png)

You need ot edit `/etc/fstab` to make the partation mount permanently.
first open it by
```
sudo gedit /etc/fstab
```
and then enter the follwoign things in the file
UUID=<enter your unique UUID> <mount point> <file system> <options> <dump> <fsck>
in my case as shown in above images it'll look something like

![alt text](https://github.com/Mr-TalhaIlyas/Mount-Partation-in-Ubntu-Permanently/blob/main/Pictures/s11.png)
  
A simple /etc/fstab, using file system UUIDs: (more on this [here](https://wiki.archlinux.org/title/Fstab))

`/etc/fstab`
```
# <device>                                <dir> <type> <options> <dump> <fsck>
UUID=0a3407de-014b-458b-b5c1-848e92a327a3 /     ext4   noatime   0      1
UUID=f9fe0b69-a280-415d-a03a-a32752370dee none  swap   defaults  0      0
UUID=b411dc99-f0a0-4c87-9e05-184977be8539 /home ext4   noatime   0      2
```
    <device> describes the block special device or remote file system to be mounted; see #Identifying file systems.
    <dir> describes the mount directory.
    <type> the file system type.
    <options> the associated mount options; see mount(8) § FILESYSTEM-INDEPENDENT_MOUNT_OPTIONS and ext4(5) § MOUNT_OPTIONS.
    <dump> is checked by the dump(8) utility. This field is usually set to 0, which disables the check.
    <fsck> sets the order for file system checks at boot time; see fsck(8). For the root device it should be 1. For other partitions it should be 2, or 0 to disable checking.
      
### Reference 
1. [Phoenixnap](https://phoenixnap.com/kb/linux-create-partition)  Doesn't involve instructions for permanent mounting.
2. [AliBabaCloud](https://www.alibabacloud.com/help/en/elastic-compute-service/latest/partition-and-format-a-data-disk-larger-than-2-tib-in-size)
