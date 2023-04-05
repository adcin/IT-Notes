# Sources

## Debian

### Source List

#### Format
- `deb [ option1=value1 option2=value2 ] uri suite [component1] [component2] [...]`
- `deb-src [ option1=value1 option2=value2 ] uri suite [component1] [component2] [...]`

#### Files
- /etc/apt/sources.list
- /etc/apt/sources.list.d/

#### Show standard source list:

```shell
cat /etc/apt/sources.list
```

Output on Debian GNU/Linux 11 (bullseye) aarch64:

```
deb http://deb.debian.org/debian bullseye main contrib non-free
deb http://security.debian.org/debian-security bullseye-security main contrib non-free
deb http://deb.debian.org/debian bullseye-updates main contrib non-free
# Uncomment deb-src lines below then 'apt-get update' to enable 'apt-get source'
#deb-src http://deb.debian.org/debian bullseye main contrib non-free
#deb-src http://security.debian.org/debian-security bullseye-security main contrib non-free
#deb-src http://deb.debian.org/debian bullseye-updates main contrib non-free
```

#### Add custom sources

Create separate `*.list` files in `/etc/apt/sources.list.d/`.


#### Resources:
- Debian manual: [Chapter 2. Debian package management](https://www.debian.org/doc/manuals/debian-reference/ch02.en.html)
- Debian man page: [SOURCES.LIST(5)](https://manpages.debian.org/bullseye/apt/sources.list.5.en.html)

-------------------

# Package Management

## Dpkg - apt, aptitude, synaptic, etc.

Dpkg is the base package manager for Debian. Apt, Aptitude, Synaptic etc. are tools and interfaces for Package Management, with additional features to dpkg.

[Dpkg - Homepage](https://www.dpkg.org/)
[Dpkg - Manpage](https://manpages.debian.org/dpkg.1.en.html)

### apt

Command line interface for package management.

Download package information from all configured sources:
```shell
sudo apt update
```
- `apt list --upgradable`

Update packages which are installed from the sources:
```shell
sudo apt upgrade
```

Update and upgrade:
```shell
sudo apt update && sudo apt upgrade -y
```

Search for packages:
```shell
apt search <PATTERN>
```

Install packages:
```shell
sudo apt install <PACKAGE_NAME>
```

Install a local deb package file:
```shell
sudo apt install ./STRATGE_APPLICATION.deb
```

Remove packages (keep the configuration files):
```shell
sudo apt remove <PACKAGE_NAME>
```

Remove packages (delete the configuration files):
```shell
sudo apt purge <PACKAGE_NAME>
```

### aptitude

Text-based user interface for package management.

Start the interface:
```shell
sudo aptitude
```

| Short-key  | Function  |
|:----------:| --------- |
| `Ctrl`+`T` | Open Menu |
|    `?`     | Help      |
|    `q`     | Quit      | 

### synaptic

Graphical management of software packages.

```shell
sudo apt install synaptic
```

### snap

Quote [Canonical - About Snaps](https://snapcraft.io/about) (Mrz 2023):
Snaps are app packages for desktop, cloud and IoT that are easy to install, secure, cross‐platform and dependency‐free. Snaps are discoverable and installable from the Snap Store

-------------------
# Software  

## General

### ClamAV - Anti Virus

- [ClamAV - Homepage](https://www.clamav.net)
- [ClamAV - Documentation](https://docs.clamav.net)

**Installation on Kubuntu**

1. RAR Support  
```shell
sudo apt install libclamunrar9
```  
2. Install ClamAV Packages  
```shell
sudo apt install clamav clamav-base clamav-daemon clamav-docs clamav-freshclam clamav-milter
```  
3. Check if Daemon is running  
```shell
clamdtop
```  
4. If you get errors, you might want reconfigure the Daemon  
```shell
sudo dpkg-reconfigure clamav-daemon
```  

### VirtualBox  

- [Download and installation manual](https://www.virtualbox.org/wiki/Linux_Downloads)  

System info: `neofetch distro kernel shell de wm`  

| distro | Kubuntu 22.04.1 LTS x86_64 |
| ------ | -------------------------- |
| kernel | 5.15.0-60-generic          |
| shell  | bash 5.1.16                |
| de     | Plasma 5.24.7              |
| wm     | KWin                       |

1. Add VirtualBox repository: 

```shell
sudo cat << EOF | sudo tee /etc/apt/sources.list.d/virtualbox.list.tmp  
deb [arch=amd64 signed-by=/usr/share/keyrings/oracle-virtualbox-2016.gpg] https://download.virtualbox.org/virtualbox/debian jammy contrib  
EOF
```  
2. Download and register the corresponding key:  
```shell
wget -O- https://www.virtualbox.org/download/oracle_vbox_2016.asc | sudo gpg --dearmor --yes --output /usr/share/keyrings/oracle-virtualbox-2016.gpg
```  
3. Update package information:  
```shell
sudo apt update
```  
4. Install VirtualBox:  
```shell
sudo apt install virtualbox virtualbox-dkms virtualbox-ext-pack
```  

## Command Line Software

### nano

A very common FOSS editor, inspired by Pico.

- [Official GNU nano homepage](https://www.nano-editor.org/)
- [Official Documentation](https://www.nano-editor.org/docs.php)
- [Documentation - nanorc \(config file\)](https://www.nano-editor.org/dist/latest/nanorc.5.html)

```shell
sudo apt install nano
```

Configuration file locations:
- `/etc/nanorc`
- `~/.nanorc`
- `$XDG_CONFIG_HOME/nano/nanorc`
- `~/.config/nano/nanorc`

| Option           | Description                |
|:---------------- | -------------------------- |
| set tabsize 4    | Tabsize value (0-8)        |
| set tabstospaces | Use Spaces instead of Tabs | 

### neofetch

A fast, highly customizable system info script.  

- [Offizial GitHub repository](https://github.com/dylanaraps/neofetch)  
- [Neofetch Debian manpage](https://manpages.debian.org/bullseye/neofetch/neofetch.1.en.html)

```shell
sudo apt install neofetch
```

### iftop

Display live bandwidth usage on an interface by host.

- [Iftop Debian manpage](https://manpages.debian.org/bullseye/iftop/iftop.8.en.html)

```shell
sudo apt install iftop
```

### screen

With screen you can run a session inside your current ssh session. For example detach it so it keeps running in the background, and reattach it later. This way you can have multiple sessions in one terminal or close the terminal without loosing the screen session.  

- [Screen Debian manpage](https://manpages.debian.org/bullseye/screen/screen.1.en.html)

Check if screen is installed:
```shell
which screen
```

Install Screen:  
```shell
sudo apt install screen
```

Start Screen session:  
```shell
screen -S <SESSION_NAME>
```
It's recommended to use the `-S` option to name the session, so you can identify it, when several screen session are running.  

Now your Screen session is started, so use it :)

Detach Screen session:  
`Ctrl+A` `Ctrl+D`

List active Screen sessions:  
```shell
screen -ls
```

Reattach Screen session:  
```shell
screen -r <SESSION_NAME>
```

To close the session use exit in an attached screen session:  
```shell
exit
```


## Desktop Software

### Synaptic - GUI package manager
```shell
sudo apt install synaptic
```

### Thunderbird - Mail + Kalender
```shell
sudo apt install thunderbird thunderbird-locale-de
```

### Filezilla - FTP-Client
```shell
sudo apt install filezilla 
```

### GParted - Partition manager

```shell
sudo apt install gparted
```

Driver for NTFS partitions:
```shell
sudo apt install ntfs-3g
```

### Krusader - file manager

Powerful, Total Commander and Midnight Commander like file manager. Homepage: https://krusader.org/

```shell
sudo apt install krusader
```

### Kate / Kwrite - Texteditor

- [Kate Website](https://kate-editor.org/)
- [Syntax Highlight](https://kate-editor.org/syntax/)
- [Alerts Syntax](https://kate-editor.org/syntax/data/html/test-alerts.dark.html)

Kate is a powerful text editor - focused on development.

```shell
sudo apt install kate markdownpart svgpart
```

KWrite is a simpler Kate editor.

```shell
sudo apt install kwrite
```

## Server Software

### fail2ban

[Fail2ban Debian manpage](https://manpages.debian.org/bullseye/fail2ban/fail2ban.1.en.html)

```shell
sudo apt install fail2ban
```

#### Configuration

The jail.local configuration is given priority by fail2ban - jail.conf shouldn't be changed.

1. create jail.local by copying the jail.conf  
```shell
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

2. customise the configuration as preferred 

```shell
nano /etc/fail2ban/jail.local
```

Useful changes:
```shell
# Dauer des IP-Bans
bantime  = 24h
# Dauer in der die Anzahl (maxretry) der Fehlversuche stattfinden müssen
findtime  = 1h
# Anzahl der Fehlversuche bis zum Bann binnen findtime
maxretry = 3
```

```shell
service fail2ban restart
```

#### Status of the service
```shell
fail2ban-client status
```

Output: 
```shell
Number of jail:      1
Jail list:   sshd
```


Info from sshd jail list:
```shell
fail2ban-client status sshd
```

#### Resources:
- https://www.the-art-of-web.com/system/fail2ban-log/
- https://www.youtube.com/watch?v=GpxAjl58qRU
