# Grub configuration

- [Debian Wiki](https://wiki.debian.org/Grub)
- [Debian Administrator's Handbook](https://debian-handbook.info/browse/stable/sect.config-bootloader.html)


System info (`neofetch distro kernel shel`):

| distro | Debian GNU/Linux 11 (bullseye) x86_64 |
| ------ | ------------------------------------- |
| kernel | 5.10.0-21-amd64                       | 
| shell  | bash 5.1.4                            |

Login as root (`su -`).

Backup original:  
```shell
cp /etc/default/grub /etc/default/grub.bak
```  

Change timeout to 0 s (GRUB_TIMEOUT=5 to GRUB_TIMEOUT=0):  

```shell
nano /etc/default/grub
```  

Update the bootloader:  
```shell
update-grub
```  

# Efibootmgr

Manipulate the UEFI Boot Manager.

Show current settings:

```shell
sudo efibootmgr
sudo efibootmgr -v
```

Output example:

```shell 
BootCurrent: 0005  
Timeout: 0 seconds  
BootOrder: 0001,0005,0000,2001,2002,2003  
Boot0000* HDD2:    
Boot0001* Windows Boot Manager  
Boot0005* ubuntu  
Boot2001* EFI USB Device  
Boot2002* EFI DVD/CDROM  
Boot2003* EFI Network
```

Change boot order example:

```shell
sudo efibootmgr -o 5,1,2001,2002
```

New output:

```shell
BootCurrent: 0005  
Timeout: 0 seconds  
BootOrder: 0005,0001,2001,2002
Boot0000* HDD2:    
Boot0001* Windows Boot Manager  
Boot0005* ubuntu  
Boot2001* EFI USB Device  
Boot2002* EFI DVD/CDROM  
Boot2003* EFI Network
```  
