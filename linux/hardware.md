# lshw - list hardware

```shell
sudo lshw -short
```

| Option | Description                                     |
|:------:| ----------------------------------------------- |
| -short | Outputs the device tree showing hardware paths | 


</br>

# Block Devices

_A block device is a piece of hardware (Harddisk, USB-Drive, CD, ...) that provides data access in blocks. On the other hand **character devices** (tape drives, serial ports, ...) provide access to the data byte by byte as a stream_

</br>

## Show devices

**lsblk - list block devices**

```shell
lsblk -fp
```

| Option | Description                    |
|:------:| ------------------------------ |
|   -f   | Output info about filesystems. |
|   -p   | Print full device paths.       | 

</br>

**List disk hardware information:**  

```shell
sudo lshw -class disk
```

</br>

## Test devices

**badblocks - check device for bad blocks.**

Non-destructive read-write test:  
```shell
sudo badblocks -s -n -v /dev/mmcblk0
```

Destructive read-write test:  
```shell
sudo badblocks -s -w -v /dev/mmcblk0
```

| Option | Description                                          |
|:------:| ---------------------------------------------------- |
|   -s   | Show progress                                        |
|   -n   | Use non-destructive read-write mode                  |
|   -w   | Use destructive write-mode. This option erases data! |
|   -v   | Verbose mode                                         | 

</br>

**Speed test**

Use dd for a simple and fast speed test. The filesystem must be mounted first.

```shell
# write test
sync && dd if=/dev/zero of=/media/username/mountpoint/testfile bs=100M count=1 oflag=dsync && sync

# read test
sync && dd if=/media/username/mountpoint/testfile of=/dev/null bs=100M count=1 iflag=dsync && sync
```

Example output of testing an old SD-Card:

```text
$ sync && dd if=/dev/zero of=/media/username/mountpoint/testfile bs=100M count=1 oflag=dsync && sync

1+0 records in
1+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 13,0946 s, 8,0 MB/s

$ sync && dd if=/media/username/mountpoint/testfile of=/dev/null bs=100M count=1 iflag=dsync && sync

1+0 records in
1+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 0,0478513 s, 2,2 GB/s
```