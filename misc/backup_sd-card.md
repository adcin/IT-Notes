# Backup methods for Raspberry PI


## Mac OS - backup SD-Card to Image

### 1. Determine the device name of your SD-Card

Insert the SD-Card into a card reader.

```shell
diskutil list
```
If you're not sure, run the command before inserting the SD-Card. Then once again after inserting it and compare the results.

My Results:
```shell
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *251.0 GB   disk0
   1:             Apple_APFS_ISC Container disk1         524.3 MB   disk0s1
   2:                 Apple_APFS Container disk3         245.1 GB   disk0s2
   3:        Apple_APFS_Recovery Container disk2         5.4 GB     disk0s3
  
...
...
...
  
/dev/disk6 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *63.9 GB    disk6
   1:             Windows_FAT_32 boot                    268.4 MB   disk6s1
   2:                      Linux                         63.6 GB    disk6s2
```
In this case "/dev/disk6" is my 64 GB SD-Card.

Alternatives:

```shell
lsblk -f
```

```shell
sudo lshw -class disk
```

```shell
sudo fdisk -l
```

```shell
ls -l /dev/disk/
ls -l /dev/disk/by-id
```

### 2. Create an image of the whole SD-Card

The prozess can take quite long - even one hour or more.

#### Method 1: Simple 1 to 1 copy.

```shell
sudo dd status=progress if=/dev/disk6 of=wifi-pi.img
```

| Command | Description       |
| ------- | ----------------- |
| dd      | convert and copy  |
| if=     | input/source file |
| of=     | output file       |
| status=progress | show progress of dd process |

My method 1 output:

```shell
124735488+0 records in
124735488+0 records out
63864569856 bytes transferred in 1978.393303 secs (32281028 bytes/sec)
```

This backup of a fresh installed Raspberry OS Lite on a 64 GB SD-Card took ~33 min. The size of the compressed image is ~64 GB.

#### Method 2: Faster copy + compress

```shell
sudo dd status=progress if=/dev/rdisk6 bs=1M | gzip > ~/backups/wifi-pi.gz
```
| Command | Description |
|-----|-----|
| /dev/rdisk6 | rdisk (instead of disk) triggers the raw read option |
| bs=1m | read and write up to 1 MB at a time |
| \| gzip | pipes the output into gzip to compress the file |
| > ~/... | destination of the image file in home directory of current user |

My method 2 output:
```shell
60906+0 records in
60906+0 records out
63864569856 bytes transferred in 1599.367785 secs (39931134 bytes/sec)
```
This backup of a fresh installed Raspberry OS Lite on a 64 GB SD-Card took ~27 min. The size of the compressed image is ~760 MB.

## Mac OS - restore SD-Card from Image

Insert the SD-Card into a card reader.

### 1. Determine the device name...

See Mac OS backup - [part 1](#1-determine-the-device-name-of-your-sd-card "Determine the device name of your SD-Card")

### 2. Unmount SD-Card

Unmout but don't remove it!

```shell
diskutil unmountDisk /dev/disk6
```

### 3. Write image to SD-Card

#### Method 1: Uncompressed image

```shell
sudo dd status=progress if=wifi-pi.img of=/dev/disk6
```

#### Method 2: Compressed image

```shell
sudo gzip -dcv /backups/wifi-pi.gz | sudo dd status=progress of=/dev/rdisk6 bs=1M
```
| Command        | Description                          |
| -------------- | ------------------------------------ |
| -d             | decompress                           |
| -c             | write to stdout, keep original files |
| -v             | verbose-mode                         |
| \| sudo dd of= | pipe output to `dd` command          |

## Backup and shrink an ssd drive

Find the name of the device you want to backup:
```bash
lsblk -f
```
_Mine is: `/dev/sda`_

Check the sector size of the device:
```bash
sudo blockdev --getss /dev/sda
512
```
_If the sector size is not 512, you may later have to add the option `--sector-size` to `losetup`. Check the `losetup` manual for further info._

Create the backup image:
```bash
sudo dd status=progress if=/dev/sda of=/home/marcin/backups/ssd-backup.img
```

Associate a loop devices with backup-image:
```bash
sudo losetup --show --find --partscan /home/marcin/backups/ssd-backup.img
/dev/loop25
```

> [!tip]
> 
> Show which loop device is associated to a file:
> ```bash
> losetup --associated /home/marcin/backups/ssd-backup.img
> ```

Mount it:
```bash
sudo mount -m /dev/loop24p2 /mnt/tmp
```

Shrink the image:
```bash
sudo fstrim -v /mnt/tmp
```

Unmount an detach the image file:
```bash
sudo umount /mnt/tmp && sudo losetup --detach /dev/loop24
```

Optionally compress the image:
```bash
sudo gzip ssd-backup.img
```