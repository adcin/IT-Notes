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