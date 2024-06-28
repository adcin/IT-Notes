# `cryptsetup` - manage plain dm-crypt, LUKS, and other encrypted volumes

- [linuxconfig.org - How to use a file as a LUKS device key](https://linuxconfig.org/how-to-use-a-file-as-a-luks-device-key)
- [golinuxcloud.com - How to auto mount LUKS device (encrypted partition) using fstab in Linux](https://www.golinuxcloud.com/mount-luks-encrypted-disk-partition-linux/)

# Add LUKS keyfile to an encrypted device

Create a keyfile with 4 MiB random data:
```shell
dd if=/dev/urandom of=/path/to/key-file bs=512 count=8
```

The encrypted device is `/dev/sdb1`.

Add key to a key-slot of `/dev/sdb1`:
```shell
sudo cryptsetup luksAddKey /dev/sdb1 /path/to/key-file
```

Map/Open the encrypted device with key-file:
```shell
sudo cryptsetup open /dev/sdb1 sdb1-opened --key-file=/path/to/key-file
```

Mount the device as usual:
```shell
sudo mount /dev/mapper/sdb1-opened /mnt/sdb1
```

##### Unlock LUKS device automatically at boot with the key-file

The device needs to be specified in `/etc/crypttab`. Check out `man 5 crypttab` for details.

Add a line to `/etc/crypttab`:
```
sdb1-opened /dev/sdb1 /path/to/key-file luks
```

It is better to use the UUID:
```
sdb1-opened UUID=2505567a-9e27-4efe-a4d5-15ad146c258b /path/to/key-file luks
```

_The device will be automatically unlocked during the next boot. But it still needs to be mounted properly!_

Add mount point to `/etc/fstb`:
```
/dev/mapper/sdb1-opened    /mnt/sdb1    btrfs    defaults    0 0
```

# LUKS encrypted container (file)

## Create and encrypt container

Create a file and preallocate the needed space:
```bash
fallocate -l 1GB /home/username/my-luks-container
```

Initializes a LUKS partition and sets the initial passphrase (for key-slot 0):
```bash
cryptsetup --type luks2 luksFormat /home/username/my-luks-container
```
_Read and follow the instructions carefully._

Opens the LUKS container and set up the mapping:
```bash
cryptsetup luksOpen /home/username/my-luks-container my-luks-container
```
_You can list the mappings:_ `ls -l /dev/mapper`

Format the mapped container:
```bash
mkfs -t ext4 /dev/mapper/my-luks-container
```

The container is now prepared and can be used. To use it right away skip the next step and mount mapped container.

Close the mapped LUKS container:
```bash
cryptsetup luksClose my-luks-container
```

## Map and mount a LUKS encrypted container file

Open the LUKS container and set up the mapping:
```bash
cryptsetup luksOpen /home/username/my-luks-container my-luks-container
```

Mount the mapped container:
```bash
mount -m /dev/mapper/my-luks-container /home/username/mnt/my-luks-container
```

## Unmount and close the container

Unmount the container:
```bash
umount /home/username/mnt/my-luks-container
```

Close the mapped device:
```bash
cryptsetup close my-luks-container
```

# VeraCrypt (TrueCrypt)

TrueCrypt is not maintained any more since 2014. But these commands should work with most TrueCrypt encrypted devices/containers as well. Check `man cryptsetup` for further details.

## Mount a VeraCrypt container

Open and map the VeraCrypt container:
```bash
cryptsetup open --type tcrypt /home/username/my-veracrypt-container.hc my-veracrypt-container
```

Mount the mapped container:
```bash
mount -m /dev/mapper/my-veracrypt-container /home/username/mnt/my-veracrypt-container
```

## Unmount and close the container

```bash
umount /home/username/mnt/my-veracrypt-container && cryptsetup close my-veracrypt-container
```
