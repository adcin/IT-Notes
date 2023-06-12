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

</br>

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