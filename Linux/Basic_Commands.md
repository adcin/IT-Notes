# Show Linux version  
```shell
lsb_release -a
```

# Show kernel version  
```shell
uname -r
```

# Execute last command as root/sudo  
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

# echo - print text



# Shutdown/Reboot

**Shutdown system in 1 minute:**  
```shell
sudo shutdown -h +1
```

**Shutdown and turn power off now:**  
```shell
sudo shutdown -P now
```

**Reboot:**  
```shell
sudo reboot
sudo shutdown -r now
```

**Show currently logged in users:**  
```shell
w
who
users
finger
```

**Show user login history:**  
```shell
last
```

Show failed (bad) logins:
```shell
lastb
```


# Hardware info/config

## lshw - list hardware

**Output a short overview:**   
```shell
sudo lshw -short
```

**Output detailed info about a hardware class:**  
```shell
sudo lshw -C <class>
```
_The "class" can be found by using `sudo lshw -short`._

## Harddrives  

**List all bulk devices and their file systems:**  
```shell
lsblk -f
```

## USB

**Show basic info:**  
```shell
lsusb
```
  
**Show detailed info:**  
```shell
usb-devices
```

## WiFi  

- [`iw` Debian Manpage](https://manpages.debian.org/bullseye/iw/iw.8.en.html)

**Show detailed WiFi adaptor information:**  
```shell
iw list
```

# Mail

- `mail` [Debian Manpage](https://manpages.debian.org/bullseye/mailutils/mail.1.en.html)  
- [GNU Mailutils manuals](https://mailutils.org/manual/)

**Open the system mailbox for the invoking user (works only if you have mail in the spool):**  
```shell
mail
# or
mail.mailutils
```

**Open other Mailbox:**  
```shell
? fold & #Open current user’s personal mailbox
? fold % #Open current user’s system mailbox
? fold %<USER> #Open <USER> system mailbox
```
_`file`, `fi`, `folder` or `fold` when used without argument, prints the information about the current mailbox. Otherwise, closes the current mailbox and opens the mailbox named by the [argument](https://mailutils.org/manual/mailutils.html#Changing-mailbox_002fdirectory). When closing the current mailbox, its messages are processed according to their [state](https://mailutils.org/manual/mailutils.html#mail-message-states)._  

**List headers of messages in current inbox:  **
```shell
? headers
# or
? z.
```

All the following commands are are finally executed, only if you use `quit` to exit the mailbox.

**To read a mail type the given ordinal number in the mailbox:**    
```shell
? 2 #Read mail number 2
```

**Mailbox summary:**  
```shell
? su
```

**Delete mails:**  
```shell
? d2 #Delete mail number 2
? d* # Delete all mails in the mailbox
```

**Exit mailbox:**  
```shell
? exit #Exit discarding all changes
? quit #Execute changes (process messages according to their state) and exit mailbox
```
`strg`+`D` is equivalent to `quit`.  
