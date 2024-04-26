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

# `jhead` - Digicam JPEG Exif header manipulation tool

##### DESCRIPTION

jhead is used to display and manipulate data contained in the Exif header of JPEG images from digital cameras. By default, jhead displays the more useful camera settings from the file in a user‐friendly format.

jhead can also be used to manipulate some aspects of the image relating to JPEG and Exif headers, such as changing the internal timestamps, removing the thumbnail, or transferring Exif headers back into edited images after graphical editors deleted the Exif header. jhead can also be used to launch other programs, similar in style to the UNIX find command, but much simpler.

# `ncdu` - NCurses Disk Usage

##### DESCRIPTION
ncdu (NCurses Disk Usage) is a curses‐based version of the well‐known `du`, and provides a fast way to see what directories are using yourdisk space.

Very useful, when you suddenly don't have space left and need to delete something.