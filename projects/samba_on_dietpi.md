# Goal

Run a simple fileserver on an old Raspberry Pi 2 Model B Rev 1.1 with a 500GB USB HDD attached.

Official homepage: https://www.samba.org/

--------------------

# Preconditions

Hardware:
- Raspberry Pi 2 Model B Rev 1.1
- 500GB ext HDD

Software:
- DietPi headless installed and updated [DietPi Homepage](https://dietpi.com/)

Notice: Most commands might have to be run as root. If you get an error like _"Root privileges required"_, just execute `sudo !!`. This will repeat the last command as sudo.

--------------------

# Installation and setup

## 1. Install Samba server

```shell
sudo dietpi-software
```

Search for Samba and install the _Samba Server: Feature-rich SMB/CIFS server_.  

## 2. Mount the external HDD

```shell
sudo dietpi-drive_manager
```

My desired mount point is _/mnt/exthdd500a_ (exthdd500a, because I have labels on my external drives, to identify them easier). The DietPi Drive Manager will create the directory, if it doesn't already exist.

## 3. Edit the Samba configuration file.

- [samba.org - smb.conf documentation](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html)

Edit the _\[dietpi\]_ part in the _smb.conf_.

```shell
sudo nano /etc/samba/smb.conf
```

Original:  
```shell
[dietpi]
        comment = DietPi Share
        path = /mnt/dietpi_userdata
        browseable = yes
        create mask = 0664
        directory mask = 0775
        valid users = dietpi
        writeable = yes
max connections = 8
```

Edited:  
```shell
[exthdd500a]
        comment = 500 GB USB HDD
        path = /mnt/exthdd500a
        browseable = yes
        create mask = 0664
        directory mask = 0775
        valid users = marcin
        writeable = yes
max connections = 8
```

## 4. Add the _valid user_ to Samba

The _valid users_ from smb.conf (in my case "marcin") need to be added to Samba. Notice that they don't have to match the system users of your DietPi!

```shell
sudo smbpasswd -a marcin
```

If you need multiple users just add them comma separated to the _valid users_ in _smb.conf_: `valid users = marcin, foo, bar`
Then add them to Samba `smbpasswd -a`.

## 5. Restart the server

```shell
sudo systemctl restart nmbd smbd
```

# Useful Samba commands

</br>

## pdbedit

- [samba.org - pdbedit documentation](https://www.samba.org/samba/docs/current/man-html/pdbedit.8.html)  

List valid Samba users:  
```shell
sudo pdbedit -L -v
```



</br>

# Connection to the drive

Depends on client:

```shell
\\192.168.0.123\exthdd500a
```

```shell
smb://192.168.0.123/exthdd500a
```

**Mount on Linux CLI:**

Install cifs-utils:

```shell
sudo apt update && sudo apt install cifs-utils -y
```

Create share directory:

```shell
sudo mkdir -p /mnt/smb_share
```

Mount SMB drive:

```shell
sudo mount -t cifs -o username=marcin //192.168.0.123/exthdd500a /mnt/smb_share
```
