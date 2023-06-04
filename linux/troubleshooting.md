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
# Notification: Pending update of "firefox" snap

1. Close all Firefox windows. Additionally you can execute `sudo killall firefox`.
2. Refresh snap:  
```shell
sudo snap refresh
```

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