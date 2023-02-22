# grub configuration

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
