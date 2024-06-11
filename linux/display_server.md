# Display Server

## Determine which display server is running

##### Using `echo`
```
echo $WAYLAND_DISPLAY
```
_If you get no output, then your current session is using most likely X11. Otherwise, it's probably wayland._

##### Using `loginctl` (systemd)

\1. Get your current session ID:
```
# or
loginctl list-sessions
```

Example output - Session ID: `2`
```
SESSION  UID USER   SEAT  TTY  STATE  IDLE SINCE  
      2 1000 test01 seat0 tty2 active no   -
```

\2. Print the type of the session with ID `2`:
```shell
loginctl show-session 2 -p Type
```
Output example:
```
Type=wayland
```

# X Window System

Currently the X.Org foundation leads this project. There is a wide variety of names concerning the Linux X display server, e.g.: X.org-X11, X, X11 or X.Org

## X11 Architecture

Nowadays X11 creates the session configuration on the fly using runtime auto-detection of the hardware involved with each GUI’s session. However, if manual settings are required here are some hints.

⚠ OBACHT: This is just a collection of my notes from various sources. I probably did not test these myself.

Not used any more - the primary configuration file: `/etc/X11/xorg.conf` or `/etc/xorg.conf`
Application settings location: `/etc/X11/xorg.conf.d/`

### Manual creation of X11 config files

##### 1. Shutdown the X-Server:
```
sudo telinit 3
```
_This usually works on SysVinit and systemd systems - `telinit` translates the request to the according command._

##### 2. Generate the config file:
```
sudo Xorg -configure
```
_The file `xorg.conf.new` will be created._

##### 3. Open the file in an editor. Make the necessary changes.

A `xorg.conf` file is divided in sections.

| Section | Description |
|-----|-----|
| Files | File pathnames |
| ServerFlags | Server flags |
| Module | Dynamic module loading |
| Extensions | Extension enabling |
| InputDevice | Input device description |
| InputClass | Input class description |
| OutputClass | Output class description |
| Device | Graphics device description |
| VideoAdaptor | Xv video adaptor description |
| Monitor | Monitor description |
| Modes | Video modes descriptions |
| Screen | Screen configuration |
| ServerLayout | Overall layout |
| DRI | DRI-specific configuration |
| Vendor | Vendor-specific configuration |

For more details check out:
```
man 5 xorg.conf
```

##### 4. Rename the file and move the file to its proper location.

##### 5. Restart the X-Server:
```
sudo telinit 5
```

## Troubleshooting X.Org

##### If something goes wrong, then X.Org should generate an error-log text file:
```
~/.xsession-errors
```

##### Print information about the X.Org server:
```
xdpyinfo
```

##### Print window information:
```
xwininfo
```
_OBACHT: `xwininfo` may hang, if run on wayland. Use `Ctrl`+`c` to exit the process._

##### If a X.Org session freezes, try `Ctrl`+`Alt`+`Backspace` to restart the X-Server.

# Wayland

- [https://wayland.freedesktop.org](https://wayland.freedesktop.org)
- [Weston Composer](https://gitlab.freedesktop.org/wayland/weston)

## Troubleshooting Wayland

Some tools and commands do not work on Wayland (e.g. `gnome-shell --replace`). So if you have issues only with a specific app, check first if it supports Wayland at all.

### Check if problem exists on X11 as well
When X11 is available as well on the distro, try switching the Display Server and check if the problem still exists.

1. Log out of the sessiom.
2. Pick a Desktop Environment without Wayland
3. Log back in

### Wayland on Gnome

1. Edit the /etc/gdm3/custom.conf
    - Find the line `#WaylandEnable=false` and uncomment it.
2. Reboot and check if the problem is gone

### Check graphics card wayland support

There are some graphics cards, which support only X11. Check the vendor's website or other online sources.

### Change the composer

Switch the composer to the original Wayland Weston composer: [https://gitlab.freedesktop.org/wayland/weston](https://gitlab.freedesktop.org/wayland/weston)

