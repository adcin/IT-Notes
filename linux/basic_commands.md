# Show Linux version  

```shell
lsb_release -a
```

# Show kernel version  

```shell
uname -r
```

# Execute last command as sudo

```shell
sudo !!
```

# Locate a program file

```shell
which <PROGRAMM NAME>
```

Example - show all (`-a`) locations of rsync:  

```shell
which -a rsync
```

Locate the binary, source, and manual page files for a command:

```shell
whereis <COMMAND>
```

# Command chains

Execute automatically multiple commands one by one.

`&&` Stops whole chain when an error occurs:
```shell
sudo apt update && sudo apt upgrade
sudo apt this_is_a_typo && sudo apt upgrade
```

`;` Proceeds to the next command **regardless if an error occurred** in previous command:
```shell
sudo apt update; sudo apt upgrade
sudo apt this_is_a_typo; sudo apt upgrade
```

`||` Proceeds to the next command **only if an error occures**:
```shell
htop || sudo apt install htop && htop
```

# echo - print text

Usually quotation marks are the easiest way to use `echo`: 

```shell
echo "Hello World!"
```

The option `-e` enables backslash escapes:  

```shell
echo -e "Hello\nWorld!\a"
```

```shell
duration=45
echo -e "Duration: $duration \bm"
```

| Option | Description                      |
|:------:| -------------------------------- |
|  \\n   | new line                         |
|  \\b   | backspace (useful for variables) |
|  \\\\  | actual backslash                 |
|  \\a   | alert (BEL)                      | 

Write the echo output into a file _\(If the file already exists one `>` would delete all content\)_:  

```shell
echo "Line No 1" > file.txt
```

Two `>>` will add the text to the end of the file:  

```shell
echo "Line No 2" >> file.txt
```

Update 2023-08-28:
> Meanwhile I've learned about POSIX compliance. See this post on stackoverflow: [Why is printf better than echo?](https://unix.stackexchange.com/a/65819)

# Read files

Print whole file: 

```shell
cat <FILENAME>
```

Print one page at a time:  

```shell
less <FILENAME>
```

Navigate: Arrow → Scroll 1 row/col; Space Bar → Scroll 1 Page
Search forward (starts at the top of current display): \/\<PATTERN\>; `n` → next; `N` → previous


# Shutdown/Reboot

Shutdown system in 1 minute:

```shell
sudo shutdown -h +1
```

Shutdown and turn power off now:

```shell
sudo shutdown -P now
```

Reboot:

```shell
sudo reboot
sudo shutdown -r now
```

Show currently logged in users:

```shell
w
who
users
finger
```

Show user login history:

```shell
last
```

Show failed (bad) logins:

```shell
lastb
```


# Hardware info/config

## lshw - list hardware

Output a short overview:

```shell
sudo lshw -short
```

Output detailed info about a hardware class:

```shell
sudo lshw -C <class>
```

_The "class" can be found by using `sudo lshw -short`._

## lspci - list PCI devices

Short list sorted by the device address:

```shell
lspci
```

List with a more verbose information:

```shell
lspci -v
lspci -vvx
lspci -vvxxx
```


## Harddrives  

List all bulk devices and their file systems:  

```shell
lsblk -f
```

## USB

Show basic info:

```shell
lsusb
```
  
Show detailed info:

```shell
usb-devices
```

## WiFi  

- [`iw` Debian Manpage](https://manpages.debian.org/bullseye/iw/iw.8.en.html)

Show detailed WiFi adaptor information:

```shell
iw list
```

# Mail

- `mail` [Debian Manpage](https://manpages.debian.org/bullseye/mailutils/mail.1.en.html)  
- [GNU Mailutils manuals](https://mailutils.org/manual/)

Open the system mailbox for the invoking user (works only if you have mail in the spool):

```shell
mail
# or
mail.mailutils
```

Open other Mailbox:

```shell
? fold & #Open current user’s personal mailbox
? fold % #Open current user’s system mailbox
? fold %<USER> #Open <USER> system mailbox
```

_`file`, `fi`, `folder` or `fold` when used without argument, prints the information about the current mailbox. Otherwise, closes the current mailbox and opens the mailbox named by the [argument](https://mailutils.org/manual/mailutils.html#Changing-mailbox_002fdirectory). When closing the current mailbox, its messages are processed according to their [state](https://mailutils.org/manual/mailutils.html#mail-message-states)._  

List headers of messages in current inbox:

```shell
? headers
# or
? z.
```

All the following commands are are finally executed, only if you use `quit` to exit the mailbox.

To read a mail type the given ordinal number in the mailbox:

```shell
? 2 #Read mail number 2
```

Mailbox summary:

```shell
? su
```

Delete mails:

```shell
? d2 #Delete mail number 2
? d* # Delete all mails in the mailbox
```

Exit mailbox:

```shell
? exit #Exit discarding all changes
? quit #Execute changes (process messages according to their state) and exit mailbox
```

_`strg`+`D` is equivalent to `quit`_
