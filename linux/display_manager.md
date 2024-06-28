# Display manager

- [baeldung.com - display managers install uninstall](https://www.baeldung.com/linux/display-managers-install-uninstall)
- [Wikipedia.org - X display manager](https://en.wikipedia.org/wiki/X_display_manager)

The purpose of the display manager is to control the login-screen. It passes the login attempt to the Linux system for verification and starts then the chosen display server and desktop environment. The Linux display manager packages use the X Display Manager Control Protocol (XDMCP) for the graphical login process.

It can be tricky to find out which display manager is in use. Each Linux flavor seems to have it's own implementation, which change from version to version. I recommend using external tools, if you struggle:

- systemd: `systemctl status display-manager` or `sudo systemctl -i | grep -i display`
- inxi (external tool): `inxi -Sxx`
- For the most common DMs: `ps -e | grep -E 'gdm|sddm|lightdm|xdm'`

Common display managers:

- XDM - The X Display Manager (XDM) is the default, bare-bone display manager for the X window system. 
    - The location of its config is usually `/etc/X11/xdm/xdm-config`. 
    - [XDM -Repo](https://gitlab.freedesktop.org/xorg/app/xdm)
- KDM - Was the default KDE display manager ([KDE.org](https://kde.org)). It's been replaced with SDDM.
- SDDM - The Simple Desktop Display Manager is currently the default KDE display manager.
    - [GitHub.com - SDDM](https://github.com/sddm/sddm)
- GDM - GNOME Display Manager
    - [GNOME.org - GDM Repo](https://gitlab.gnome.org/GNOME/gdm)
    - [GDM Wiki](https://wiki.gnome.org/Projects/GDM/)
- LightDM: A lightweight display manager, which is developed independent of desktop environments.
    - [GitHub.com - LightDM Repo](https://github.com/canonical/lightdm)

## LXDM
- Display Manager from [LXDE (Lightweight X11 Desktop Environment)](https://www.lxde.org/)
- [Arch-Wiki LXDM](https://wiki.archlinux.org/title/LXDM)
- Location of config-files: `/etc/lxdm/`; `/etc/lxdm/lxdm.conf`

### LXDM - setup autologin user

I have created the user `displayer`. The purpose is to auto-start a browser in kiosk-mode directly after bootup. System: fedora 40 with LXDE as desktop environment.

Edit `/etc/lxdm/lxdm.conf`:
```shell
sudo vim /etc/lxdm/lxdm.conf
```

Add this:
```
autologin=displayer
session=/usr/bin/startlxde
```