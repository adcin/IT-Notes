# Collection of useful stuff

</br>

## `Xev` - prints content of X events

> Xev creates a window and then asks the X server to send it events whenever anything happens to the window (such as it being moved, resized, typed in, clicked in, etc.).

Useful if your keyboard or mouse do not work properly and you want to check, what the x server does when you press a key or move the mouse.

</br>

### Installation 

**Xev is part of x11-utils:**
```shell
sudo apt-get install x11-utils 
```

</br>

### Usage

After executing xev a window will appear. When interacting with this window, xev will print the corresponding X server events to stdout. To stop xev you have to close this window.

</br>

**Execute without options:**

```shell
xev
```

_Usually not useful because of the sheer amount of output._

</br>

**Get the keycode of keyboard keys:**

```shell
xev | grep keycode
```

</br>

**Get information about the mouse buttons:**

```shell
xev | grep button
```



</br>

## ImageMagick

- [ImageMagick Website](https://imagemagick.org)
- [ImageMagick Mogrify - Inline Image Modification](https://imagemagick.org/script/mogrify.php)

</br>

### Remove metadata from images with `mogrify`

[Description from official website](https://imagemagick.org/script/command-line-options.php#strip):
> -strip
>
>Strip the image of any profiles, comments or these PNG chunks: bKGD,cHRM,EXIF,gAMA,iCCP,iTXt,sRGB,tEXt,zCCP,zTXt,date. To remove the orientation chunk, orNT, set the orientation to undefined, e.g., -orient Undefined.

</br>

**Example - remove metadata from all jpg files in current folder:**
```shell
mogrify -strip ./*.jpg
```