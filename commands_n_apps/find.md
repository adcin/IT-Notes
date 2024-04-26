# Find - search for files in a directory hierarchy

Search the file sshd_config: 
```shell
sudo find / -name "sshd_config" -print
```

Search files, folders and links:
```shell
find /path/to/file/ -type d,f,l -iname filename
```

Search files (not folders):
```shell
find /path/to/file/ -iname filename
```

Show all files (not folders) in current directory:

```shell
find .
```

Show all files (not folders) named software in current directory:

```shell
find . software
```

Search in home folder of current user:

```shell
find ~/ -iname filename
```

Search in the whole system:

```shell
find / -iname filename
```

Show files with foo at the beginning of their name:

```shell
find /path/to/file/ -iname foo*
```

Show files which have been modified up till 2 days ago:

```shell
find /path/to/file/ -mtime -2
```

Show files which have been modified up till 24 hours ago:

```shell
find $HOME -mtime 0
```

Show file which have been accessed up till 2 days ago:

```shell
find /path/to/file/ –atime -2
```

Show file which have been changed up till 2 days ago:

```shell
find /path/to/file/ –ctime -2
```

Show files bigger then 5 MB:

```shell
find /path/to/file/ –size +5M
```

Delete all ini files in the current directory and all sub-directories:

```shell
cd /media/some_usb_drive/directory/
find . -name 'desktop.ini' -type f -delete
```
