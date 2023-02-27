# Desktop Environment (DE)

First things first  
```shell
sudo apt update && sudo apt upgrade
```
```shell
sudo apt update
sudo apt upgrade
sudo apt dist-upgrade
sudo reboot
```


## Desktop Environment full installation \(on headless system\)
### 1. Xorg display server installation
```shell
sudo apt install xserver-xorg
```
### 2. Desktop Environment Installation
Choose the environment
#### - LXDE \(Lightweight X11 Desktop Environment\)
One of the lightest DEs
```shell
sudo apt install lxde-core lxappearance
```
#### - XFCE
Almost as light as LXDE
```shell
sudo apt install xfce4 xfce4-terminal
```
#### - PIXEL \(Pi Improved Xwindow Environment Lightweight\)
Raspberry PI Original
```shell
sudo apt install raspberrypi-ui-mods
```
#### - MATE
Desktop PC environment, fork of GNOME 2.
```shell
sudo apt install mate-desktop-environment-core
```
#### - KDE
Classic for PCs.
```shell
sudo apt install kde-plasma-desktop
```
### 3. LightDM display manager
Might already be installed.
```shell
sudo apt install lightdm
```
### 4. Reboot
```shell
sudo reboot
```

--------

# Misc
## Show current Desktop Environment
```shell
echo $XDG_CURRENT_DESKTOP
```

## Show default display manager  
```shell
cat /etc/X11/default-display-manager
# alternative
grep '/usr/s\?bin' /etc/systemd/system/display-manager.service
```  

## Set standard DE
```shell
sudo update-alternatives --config x-session-manager
```
