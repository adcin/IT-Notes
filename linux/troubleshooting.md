# -bash: warning: setlocale: LC_ALL: cannot change locale (de_DE.UTF-8)

```shell
sudo apt install locales
```
```shell
sudo locale-gen de_DE.UTF-8
```
```shell
sudo dpkg-reconfigure locales
```

</br>

# Notification: Pending update of "firefox" snap

1. Close all Firefox windows. Additionally you can execute `sudo killall firefox`.
2. Refresh snap:  
```shell
sudo snap refresh
```

</br>

# Ubuntu server dialogue: "Which service should be restarted ?"

**Cause:** The needrestart command, which is part of the apt-get upgrade process, is set to "interactive" mode by default. This starts a dialogue after every apt upgrade.

**Solution:** Change the needrestart restart mode to automatically.

1. Edit /etc/needrestart/needrestart.conf

```shell
sudo nano /etc/needrestart/needrestart.conf
```

2. Change mode

Find: `#$nrconf{restart} = 'i';`
Change to: `$nrconf{restart} = 'a';`

# Error message: `sh: 0: getcwd() failed: No such file or directory`

This basically means, that the current working directory (cwd) is not valid. You are most likely in the terminal in a directory, which does not exist any more. Maybe it has been moved or deleted by another process.

##### Solution:
Changing the directory (`cd ~/` or `cd /`) should solve the issue.

# KDE - add spell check dictionaries

## Useful Links:
- [Spell Checker Docs](https://docs.kde.org/stable5/en/plasma-desktop/kcontrol/spellchecking/index.html)
- [Hunspell Project Website](http://hunspell.github.io/)
- [Hunspell Languages](https://hosted.weblate.org/projects/hunspell/#languages)
- [GNU Aspell](http://aspell.net/)
- [GNU Aspell Dictionaries](https://ftp.gnu.org/gnu/aspell/dict/0index.html)


## Prerequisites

**Executed on:**

| distro | Kubuntu 23.04 x86_64 | 
| ------ | -------------------- |
| kernel | 6.2.0-20-generic     |
| shell  | bash 5.2.15          |
| de     | Plasma 5.27.4        |
| wm     | KWin                 |

## Procedure

1. Find the Aspell and Hunspell dictionaries you need. You can use the dictionary links above or simply try your country code.

</br>

Example for German dictionaries:

```shell
apt-cache search "hunspell-de|aspell-de" --names-only
# alternative
apt search "hunspell-de|aspell-de"
```

Stdout of `apt-cache search "hunspell-de|aspell-de" --names-only`:

```shell
hunspell-de-at - Austrian (German) dictionary for hunspell  
hunspell-de-at-frami - German (Austria) dictionary for hunspell ("frami" version)  
hunspell-de-ch - Swiss (German) dictionary for hunspell  
hunspell-de-ch-frami - German (Switzerland) dictionary for hunspell ("frami" version)  
hunspell-de-de - German dictionary for hunspell  
hunspell-de-de-frami - German dictionary for hunspell ("frami" version)  
libaspell-dev - Development files for applications with GNU Aspell support  
libhunspell-dev - spell checker and morphological analyzer (development)  
aspell-de - German dictionary for aspell  
aspell-de-1901 - Traditional German dictionary for aspell  
hunspell-de-med - German medical dictionary for hunspell
```

</br>

2. Install the dictionaries

</br>

Example for my German dictionaries:

```shell
sudo apt install hunspell-de-de-frami aspell-de
```

</br>

3. Activate new dictionaries in the KDE System Settings GUI

App:  System Settings
Manu: Regional Settings ▷ Spell Check

# MacBook 11,1 - WIFI not working after upgrade fedora 39 to 40

> [!tip]
> I connected my android phone via usb to the notebook and activated usb tathering on the phone. This way I could go online without a working internal networking device or a dedicated usb network adapter.

Source of solution: https://discussion.fedoraproject.org/t/macbook-11-1-wifi-driver-broken-after-update/80276

Output of `lspci -k`:
```
03:00.0 Network controller: Broadcom Inc. and subsidiaries BCM4360 802.11ac Dual Band Wireless Network Adapter (rev 03)
        Subsystem: Apple Inc. Device 0117
        Kernel driver in use: wl
        Kernel modules: bcma, wl
```

The following commands solved the problem for me:
```
sudo dnf install broadcom-wl
rpm -q -a akmod-wl kmod-wl\*
rpm -V -a akmod-wl kmod-wl\*
rpm -q -l -a akmod-wl kmod-wl\*
ls -l /usr/src/akmods/wl-kmod*
sudo akmods --force --akmod wl
akmodsbuild /usr/src/akmods/wl-kmod.latest
sudo rpm --force -i ~/kmod-wl-*.rpm
sudo modprobe wl
```

> [!quote]- &nbsp;Full list of commands and output:
> 
> ```
> $ sudo dnf install broadcom-wl
> [sudo] password for marcin:
> Last metadata expiration check: 1:18:01 ago on Mo 20 Mai 2024 17:35:18 CEST.
> Package broadcom-wl-6.30.223.271-23.fc40.noarch is already installed.
> Dependencies resolved.
> Nothing to do.
> Complete!
> 
> $ rpm -q -a akmod-wl kmod-wl\*
> kmod-wl-6.8.8-200.fc39.x86_64-6.30.223.271-49.fc39.x86_64
> kmod-wl-6.8.9-200.fc39.x86_64-6.30.223.271-49.fc39.x86_64
> akmod-wl-6.30.223.271-51.fc40.x86_64
> kmod-wl-6.30.223.271-51.fc40.x86_64
> kmod-wl-6.8.9-300.fc40.x86_64-6.30.223.271-51.fc40.x86_64
> 
> $ rpm -V -a akmod-wl kmod-wl\*
> 
> $ rpm -q -l -a akmod-wl kmod-wl\*
> /lib/modules/6.8.8-200.fc39.x86_64/extra
> /lib/modules/6.8.8-200.fc39.x86_64/extra/wl
> /lib/modules/6.8.8-200.fc39.x86_64/extra/wl/wl.ko.xz
> /lib/modules/6.8.9-200.fc39.x86_64/extra
> /lib/modules/6.8.9-200.fc39.x86_64/extra/wl
> /lib/modules/6.8.9-200.fc39.x86_64/extra/wl/wl.ko.xz
> /usr/src/akmods/wl-kmod-6.30.223.271-51.fc40.src.rpm
> /usr/src/akmods/wl-kmod.latest
> (contains no files)
> /lib/modules/6.8.9-300.fc40.x86_64/extra
> /lib/modules/6.8.9-300.fc40.x86_64/extra/wl
> /lib/modules/6.8.9-300.fc40.x86_64/extra/wl/wl.ko.xz
> 
> $ ls -l /usr/src/akmods/wl-kmod*
> -rw-r--r--. 1 root root 5808348 Feb  4 01:00 /usr/src/akmods/wl-kmod-6.30.223.271-51.fc40.src.rpm
> lrwxrwxrwx. 1 root root      36 Feb  4 01:00 /usr/src/akmods/wl-kmod.latest -> wl-kmod-6.30.223.271-51.fc40.src.rpm
> 
> $ sudo akmods --force --akmod wl
> Checking kmods exist for 6.8.9-300.fc40.x86_64 [  OK  ]
> 
> $ akmodsbuild /usr/src/akmods/wl-kmod.latest
> * Rebuilding /usr/src/akmods/wl-kmod.latest for kernel(s) 6.8.9-300.fc40.x86_64: prep build install clean; Successfull; Saved kmod-> wl-6.8.9-300.fc40.x86_64-6.30.223.271-51.fc40.x86_64.rpm in /home/marcin/
> 
> $ sudo rpm --force -i ~/kmod-wl-*.rpm
> ```

