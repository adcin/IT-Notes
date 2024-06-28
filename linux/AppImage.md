# AppImage

> [!quote] Quote from appimage.org:
> AppImage is a format for packaging and running Linux applications without installation or root rights. Learn how to use AppImageKit to create your own AppImages or download and try some popular applications in AppImage format.

- [AppImage.org - Official website](https://appimage.org)
- [AppImage.org - Documentation](https://docs.appimage.org)
- [appimage.github.io - List of applications](https://appimage.github.io/apps/)
- Desktop integration:
    - [AppImage.org - Documentation - Integrating AppImages into the desktop](https://docs.appimage.org/user-guide/run-appimages.html#integrating-appimages-into-the-desktop)
    - [GitHub.com - go-appimage (appimaged)](https://github.com/probonopd/go-appimage)
    - [GitHub.com - AppImageLauncher](https://github.com/TheAssassin/AppImageLauncher)

## Manual desktop integration

My system:

- OS: Fedora 40
- Kernel: x86_64 Linux 6.9.5-200.fc40.x86_64
- Shell: bash 5.2.26
- DE: KDE
- WM: KWin

On fedora 40 I've had some errors when using the appimaged deamon, so I integrated the desktop entries manually. In this example I'm using the textosaurus AppImage(v0.9.13):

- https://github.com/martinrotter/textosaurus
- https://github.com/martinrotter/textosaurus/releases
- https://github.com/martinrotter/textosaurus/releases/download/0.9.13/textosaurus-0.9.13-9cb7064-linux64.AppImage

##### 1. Download the Appimage

```
~$ cd Downloads
```

```
~/Downloads$ wget https://github.com/martinrotter/textosaurus/releases/download/0.9.13/textosaurus-0.9.13-9cb7064-linux64.AppImage
```

Make it executable:
```
~/Downloads$ chmod +x textosaurus-0.9.13-9cb7064-linux64.AppImage
```

##### 2. Extract the AppImage

```
~/Downloads$ ./textosaurus-0.9.13-9cb7064-linux64.AppImage --appimage-extract
```

_Now there should be a new directory `~/Downloads/squashfs-root/`._

##### 3. Move the AppImage to its final location

```
~/Downloads$ mv textosaurus-0.9.13-9cb7064-linux64.AppImage ~/.local/bin/
```

Display the full path. You will need it later:
```
~/Downloads$ realpath ~/.local/bin/textosaurus-0.9.13-9cb7064-linux64.AppImage
/home/someusername/.local/bin/textosaurus-0.9.13-9cb7064-linux64.AppImage
```

##### 4. Edit the extracted .desktop file

First check out what we got:
```
~/Downloads$ ls -la squashfs-root
total 164
drwx------. 1 someusername someusername   148 Jun 28 12:11 .
drwxr-xr-x. 1 someusername someusername   336 Jun 28 12:12 ..
-rwxrwxr-x. 1 someusername someusername 14016 Jun 28 12:11 AppRun
-rw-rw-r--. 1 someusername someusername 69639 Jun 28 12:11 .DirIcon
-rw-rw-r--. 1 someusername someusername   295 Jun 28 12:11 io.github.martinrotter.textosaurus.desktop
-rw-rw-r--. 1 someusername someusername 69639 Jun 28 12:11 textosaurus.png
drwx------. 1 someusername someusername    66 Jun 28 12:11 usr
```
_`io.github.martinrotter.textosaurus.desktop` is our `*.desktop` file._

Lets see what's inside:
```
~/Downloads$ cat ./squashfs-root/io.github.martinrotter.textosaurus.desktop
[Desktop Entry]
Type=Application
Version=1.0
Exec=textosaurus %F
Name=Textosaurus
GenericName=Text editor
Comment=Simple cross-platform text editor based on Qt and QScintilla
Icon=textosaurus
Terminal=false
Categories=Office;Development;
MimeType=text/plain;text/xml;
X-AppImage-Version=9cb7064
```

We need to change the `Exec` entry from
- `Exec=textosaurus %F`
to
- `Exec=/home/someusername/.local/bin/textosaurus-0.9.13-9cb7064-linux64.AppImage %F`

_`/home/someusername/.local/bin/textosaurus-0.9.13-9cb7064-linux64.AppImage` is the path we got at the end of the last step with the `realpath` command._

Lets edit the file:
```
~/Downloads$ vim ./squashfs-root/io.github.martinrotter.textosaurus.desktop
```

This should work:
```
~/Downloads$ cat ./squashfs-root/io.github.martinrotter.textosaurus.desktop
[Desktop Entry]
Type=Application
Version=1.0
Exec=/home/someusername/.local/bin/textosaurus-0.9.13-9cb7064-linux64.AppImage %F
Name=Textosaurus
GenericName=Text editor
Comment=Simple cross-platform text editor based on Qt and QScintilla
Icon=textosaurus
Terminal=false
Categories=Office;Development;
MimeType=text/plain;text/xml;
X-AppImage-Version=9cb7064
```

##### 5. Put everything in place

To find the correct icon image, I like using the `tree` command: `tree -f squashfs-root/usr/share/icons/`

But `ls -R` works as well:
```
~/Downloads$ ls -R ./squashfs-root/usr/share/icons/
./squashfs-root/usr/share/icons/:
hicolor

./squashfs-root/usr/share/icons/hicolor:
512x512

./squashfs-root/usr/share/icons/hicolor/512x512:
apps

./squashfs-root/usr/share/icons/hicolor/512x512/apps:
textosaurus.png
```

_Here we have it: `./squashfs-root/usr/share/icons/hicolor/512x512/apps/textosaurus.png`_

Now there are a few ways to manage the icons. Textsaurus has only one icon image, so we'll put in in the local users icon folder:
```
~/Downloads$ mv ./squashfs-root/usr/share/icons/hicolor/512x512/apps/textosaurus.png ~/.local/share/icons/
```

> [!tip]
> Often apps have multiple icon images like for example Obsidian does:
> 
> - `./squashfs-root/usr/share/icons/hicolor/128x128/apps/obsidian.png`
> - `./squashfs-root/usr/share/icons/hicolor/16x16/apps/obsidian.png`
> - `./squashfs-root/usr/share/icons/hicolor/256x256/apps/obsidian.png`
> - `./squashfs-root/usr/share/icons/hicolor/32x32/apps/obsidian.png`
> - `./squashfs-root/usr/share/icons/hicolor/48x48/apps/obsidian.png`
> - `./squashfs-root/usr/share/icons/hicolor/512x512/apps/obsidian.png`
>
> In this case I recommend copying them to the corresponding "/usr/share/icons/..." directories like so:
> 
> ```
> sudo cp ./squashfs-root/usr/share/icons/hicolor/128x128/apps/obsidian.png /usr/share/icons/hicolor/128x128/apps/obsidian.png
> ```
> 
> _It's simply the same path, but without the "./squashfs-root" part._

Now we need to move our modified .desktop entry to it's final location:
```
~/Downloads$ mv ./squashfs-root/io.github.martinrotter.textosaurus.desktop ~/.local/share/applications/
```

Sometimes you'll need to relogg or update the desktop database to see the new entry:
```
~/Downloads$ update-desktop-database ~/.local/share/applications
```

And voila ... this should be it.

Okok ... let's cleanup ^-^:
```
~/Downloads$ rm -r squashfs-root
```

