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
