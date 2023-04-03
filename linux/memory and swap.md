# `free` - Display amount of free and used memory in the system

```shell
free -h
```

```shell
free -m
```

| Option | Description            |
|:------:| ---------------------- |
|   -h   | Human readable         |
|   -m   | Show value in Mebibyte | 

Output example:

|       | total | used  | free  | shared | buff/cache | available |
| ----- | ----- | ----- | ----- | ------ | ---------- | --------- |
| Mem:  | 15Gi  | 3,0Gi | 8,0Gi | 983Mi  | 4,5Gi      | 11Gi      |
| Swap: | 1,9Gi | 0B    | 1,9Gi |        |            |           |

_Linux uses "unused" memory to increase performance. So the important value is in the **available**  column, as if you want to check your system having enough memory._

# swap

## swappiness

Swappiness is the factor how likely the system uses the swap partition/file. The value can be from 0 (almost no swap use at all) to 100 (very aggressive swap usage) - 60 is usually the default value.

Show current swappiness:
```shell
cat /proc/sys/vm/swappiness
```

Change swappiness to 10:
```shell
sudo sysctl vm.swappiness=10
```

Edit `/etc/sysctl.conf` as root to make swappiness changes permanent:

```shell
sudo nano /etc/sysctl.conf
```

Enter or edit the following option:

```shell
vm.swappiness = 10
```

## Clear swap (disable/enable)

Write swap content to memory, then disable swap (can take some time):
```shell
sudo swapoff -av
```

Enable swap again:
```shell
sudo swapon -av
```

| Option | Description                                                                                                                                                        |
|:------:| ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|   -a   | All devices marked as in /etc/fstab are made available, except for those with the noauto option. Devices that are already being used as swap are silently skipped. | 
|   -v   | verbose                                                                                                                                                            |

**When a swap-file is used (eg. Raspberry Pi) other commands might have to be used:**

Disable swap (can take some time):
```shell
sudo dphys-swapfile swapoff
```

Enable swap again:
```shell
sudo dphys-swapfile swapon
```


