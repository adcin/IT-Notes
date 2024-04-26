#  Basic commands

**Print the current working directory path:**  
```shell
pwd
```

**Create folder:**  
```shell
mkdir FOLDER [FOLDER2] [FOL...]
```

Create all non existing parent folders as well:  
```shell
mkdir -p FOLDER [FOLDER2] [FOL...]
```

Create a folder hierarchy with one command:
```
mkdir -p project1/{users/{user1,user2,user3}/{documemts,templates,media},shares/{media,docs,dev}}
```

Result:
<pre style="margin-left: 2em"><code>project1
├── shares
│   ├── dev
│   ├── docs
│   └── media
└── users
   ├── user1
   │   ├── documemts
   │   ├── media
   │   └── templates
   ├── user2
   │   ├── documemts
   │   ├── media
   │   └── templates
   └── user3
       ├── documemts
       ├── media
       └── templates</code></pre>

Create a folder with the permissions 700:
```
mkdir -m 700 my-private-folder
```

**Move/Rename files and folders:**  
```shell
mv /way/to/folder/or/file /new/way/to/folder/or/file
```



**Copy:**  
```shell
cp /way/to/orig/file /way/to/new/copy
```

Copy directory and all subdirectories to folder

```shell
cp -R junk /tmp
```



**delete**

File: 
```shell
rm <FILENAME>
```

Full folder:

```shell
rm -r <FOLDER>
```

Empty folder: 

```shell
rmdir FOLDER
```



Overwrite a file to hide its contents, then deallocate and remove it:  

```shell
shred -u <FILENAME>
```



Overwrite a whole partition to delete all files safely:  

```shell
sudo shred -fvn 3 /dev/sdb1
```

| Option             | Description                                      |
|:------------------ | ------------------------------------------------ |
| -u                 | deallocate and remove file after overwriting     |
| -f, --force        | change permissions to allow writing if necessary |
| -v, --verbose      | show progress                                    |
| -n, --iterations=N | overwrite N times instead of the default (3)     |





Show file system disk space usage:

```shell
df -h
```



Show Inode usage:

```shell
df -i
```

_Simplified - amount of available (used/free) files._




Count all files (not folders) in a directory and subdirectories:

```shell
find /path/to/directory -type f | wc -l
# or for the current directory
find ./ -type f | wc -l 
```



---------------------



# Mount



## mnt vs media

| Path   | Description                                     |
|:------ | ----------------------------------------------- |
| /media | Linux mounts media like CDs or USB-Drives here. | 
| /mnt   | You should mount manually here.                 |



## Show mounted file-systems:

```shell
cat /proc/mounts
less /proc/mounts
```

```shell
cat /etc/mtab
less /etc/mtab
```



## sshfs mount

```shell
sudo apt update && sudo apt install sshfs libfuse2 -y
```



Add remote host to known_hosts:

```shell
ssh-keyscan -H REMOTE_HOST_ADDRESS >> ~/.ssh/known_hosts
```



Create local mount directory (if necessary):

```shell
mkdir /home/USER_NAME/MOUNT/HERE
```



1st quick mount as test and for adding host key to known hosts (important):

```shell
sshfs REMOTE_USER_NAME@REMOTE_HOST_ADDRESS:/PATH/TO/REMOTE/DIRECTORY /home/LOCAL_USER_NAME/MOUNT/HERE
```



Mount without interactive password request (method: unsecure, quick and dirty):

```shell
sshfs -o reconnect,password_stdin REMOTE_USER_NAME@REMOTE_HOST_ADDRESS:/PATH/TO/REMOTE/DIRECTORY /home/LOCAL_USER_NAME/MOUNT/TO/HERE <<< 'REMOTE_USER_PASSWORD'
```



Mount with ssh authentication key `/home/LOCAL_USER_NAME/.ssh/ID_KEYFILE` (method: more secure, needs to be supported and set up on remote host first):

```shell
sshfs -o reconnect,IdentityFile=/home/LOCAL_USER_NAME/.ssh/ID_KEYFILE REMOTE_USER_NAME@REMOTE_HOST_ADDRESS:/PATH/TO/REMOTE/DIRECTORY /home/LOCAL_USER_NAME/MOUNT/TO/HERE
```

**Options**

|    Option     | Description                                                                                                                                                      |
|:-------------:| ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -o idmap=user | Convert file owner between local and remote user IDs. The idmap=user option translates the UID of the connecting user to the remote user (GID remains unchanged) |
| -o reconnect  | automatically  reconnect  to  server if connection is interrupted                                                                                                |



## Mount automatically using crontab

Create a script:

```shell
nano ~/my_mount_script.sh
```

Simple example for sshfs mount:

```shell
#!/bin/bash

## Insert your credentials!

REMOTE_USER=""
REMOTE_HOST=""
REMOTE_DIRECTORY=""
AUTH_KEY=""
LOCAL_DESINATION_DIRECTORY=""

# Wait 15s for network connection to establish
sleep 15

# Mount the remote drive
sshfs -o reconnect,IdentityFile=$AUTH_KEY $REMOTE_USER@$REMOTE_HOST:$REMOTE_DIRECTORY $LOCAL_DESINATION_DIRECTORY
```



Create a crontab instruction:

```shell
crontab -e
```

Insert the line at the end of the file:

```shell
@reboot /home/USER_NAME/my_mount_script.sh
```



_Done - the network drive should now be automatically mounted on next reboot._



## Unmount

```shell
umount /home/LOCAL_USER_NAME/MOUNT/HERE
```

---------------------



# Symbolic links (symlinks)



Create a hardlink:
```shell
ln [/PATH/TO/TARGET] [/PATH/TO/SYMLINK]
``` 
_Hardlinks basically define the location of a file on the hardware (disc). If the "source" is deleted, the file is still on the disk and the hardlink works fine. Only when all hardlinks are deleted, also the file on the disk will be removed.  
Hardlinks work only on the same device and filesystem.  
Hardlinks work with files, not with folders._



Create a softlink:  
```shell
ln -s [/PATH/TO/TARGET] [/PATH/TO/SYMLINK]
```  

_Softlinks refer to the path in the filesystem, not the location on the hardware/disc. When the softlink is removed, the file stays intact. If the original file is deleted, the soft symlink is still there, but it doesn't work any more.  
Softlinks can be used for files and folders, and work regardless of different disks or partitions._  



Remove a symlink:
```shell
rm </PATH/TO/SYMLINK>
# or
unlink </PATH/TO/SYMLINK>
```

<br>

Remove broken symlinks from the current dir and all subdirs:
```shell
find -xtype l -delete
```

---------------------



# Navigate along a path

- [`cd` Debian Manpage](https://manpages.debian.org/bullseye/tcl8.6-doc/cd.3tcl.en.html)
- [Path resolution](https://manpages.debian.org/bullseye/manpages/path_resolution.7.en.html)

Change working directory:  
```shell
cd <DESTINATION>
```

_The process starts the lookup at current directory, then looks for the next destination (the destinations are separated with a `/` slash). If found the lookup is set to this directory. Now the process looks for the next destination (seperated by `/`) and so on, until end of \<DESTINATION\> is reached._

| Destination | Refers to                                                                                               |
| ----------- | ------------------------------------------------------------------------------------------------------- |
| .           | the current directory itself                                                                            |
| ..          | parent directory                                                                                        |
| \/          | root directory when at the beginning of \<DESTINATION\> and is ignored if at the end of \<DESTINATION\> |
| \~          | current users home directory                                                                            |
| \~\<USER\>  | \<USER\> home directory                                                                                 | 


**Examples:**  

Change to my home directory: 
```shell
cd ~
# or
cd /home/$USER
```

Change to "Pictures" in my home directory:
```shell
cd ~/Pictures
```

Change to parent directory:
```shell
cd ..
```

Change to parent and then to "same_level" directory:
```shell
cd ../same_level
```

Change to parent of parent and then to "logs" directory:
```shell
cd ../../logs
```

---------------------

# Transfer over network

## Download files from internet

Download to current directory:

```shell
wget http://example.com/file.tar
```

Download to specified directory:

```shell
wget -P ~/tmp http://example.com/file.tar
```

Download to specified file:

```shell
wget -O ~/download/subdirectory/my_file.tar http://example.com/what_ever.tar
```

Alternative:

```shell
curl http://example.com/file.tar -o /path/to/dir/file.tar
```

## scp - ssh copy

> scp copies files between hosts on a network.

Scp is not resumable, if the connection drops during a large transfer, you will need to restart the entire operation. It's recommended to use rsync.

Copy file from server to client:  
```shell
scp <USER>@<SOURCE_HOST>:/path/to/source/file /path/to/destination/file
``` 

Copy file from client to server:  
```shell
scp /path/to/source/file <USER>@<DESTINATION_HOST>:/path/to/destination/file
``` 

Example IPv6 client to server with authentication key:
```shell
scp -6 -i ~/.ssh/megasecret.rsa -P 31337 cinhub@\[2001:6666::abcd\]:/path/to/source/file /path/to/destination/file
```

Copy folder recursively from server to client:  
```shell
scp -r <USER>@<SOURCE_HOST>:/path/to/source/folder /path/to/destination/directory
``` 


# Permissions

## `chmod`- change file or folder permissions

**Read, write and execute only by owner:**

```shell
chmod 700 <FILE>
#or
chmod u=rwx <FILE>
```

**Make file executable (for Owner, Group and Everyone):**

```shell
chmod +x <FILE>
# same as
chmod a+x <FILE>
```

**Make file executable (for Owner):**

```shell
chmod u+x <FILE>
```

**Remove write permissions for group and others:**

```shell
chmod go-w <FILE>
```

**Letter users notation:**

| Letter | Description                        |
|:------:| ---------------------------------- |
|   u    | owner                              |
|   g    | group                              |
|   o    | other (not owner, nor group users) |
|   a    | all (default option)               | 

**Numerical permission notation:**

| #   | Sum                | rwx | Permission              |
| --- | ------------------ | --- | ----------------------- |
| 7   | 4(r) + 2(w) + 1(x) | rwx | read, write and execute |
| 6   | 4(r) + 2(w)        | rw- | read and write          |
| 5   | 4(r) + 1(x)        | r-x | read and execute        |
| 4   | 4(r)               | r-- | read only               |
| 3   | 2(w) + 1(x)        | -wx | write and execute       |
| 2   | 2(w)               | -w- | write only              |
| 1   | 1(x)               | --x | execute only            |
| 0   | 0                  | --- | none                    |

### Change execute mode only for directories

The upper-case **`X`** means, the change to the execute bit will only take effect on directories. Other files execute bit will stay untouched.  

Example:  
```shell
sudo chmod -R ug+rwX,o+rX-w /path/to/folder
```

| Cmd part        | Description                                                                                                                                         |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| sudo chmod -R   | Change mode recursively as root user                                                                                                                |
| ug+rwX          | User and group (`ug`) are allowed (`+`) to read, to write (`rw`) and execute directories, but don't change the execute mode of files (`X`)             |
| o+rX-w          | Others (`o`) are allowed (`+`) to read and to execute directories, but don't change the execute mode of files (`X`). They are not allowed (`-`) to write (`w`) |
| /path/to/folder | Path to directory, which modes will be changed                                                                                                      | 

## `umask` - change the default file and folder permissions

Show actual default mask settings for the current folder:

```shell
umask
```  

Alternatively displays the current mask as a symbolic value:
```shell
umask -S
```  

**Calculating `umask`:** Umask works kind of opposite to chmod. You define witch permissions should not be set as default.  

For example if you want the standard mask: Owner is allowed everything (7), Group is allowed to read (4), Everyone has no rights (0) -> 740

So you don't want: Owner to have no rights (0), Group to write or execute (3), Everyone to read, write or execute (7) -> 037

777 - 740 = 037 -> `umask 037`

For delicate files and folders you might want to have very strickt settings, so just the owner has access:  
```shell
umask 077
```

## `chown`  - change ownership 

```shell 
chown <USER>:<GROUP> <FILE OR FOLDER>
```

---------------------

# Search/Find files

locate - fast search the file sshd_config: 
```shell
locate sshd_config
```



# Archives


## tar (tarball) archives

- [TecMint tar examples](https://www.tecmint.com/tar-command-examples-linux/)



Extract:
```shell
tar vxf archiv.tar
```



Extract to a defined directory:
```shell
tar vxf archiv.tar -C $HOME/extract/here
```



Extract to a defined directory, but without the first 2 leading parent directories. Very useful, if you want to get rid of the `/home/user` directory.

```shell
tar vxf archiv.tar -C $HOME/extract/here --strip-components=2
```



Extract only a specific file/folder from within the tarball. Specify destination directory and strip 2 leading parent directories.

```shell
tar vxf archiv.tar -C $HOME/extract/here home/user/Documents/ --strip-components=2
```



Create new archive:
```shell
tar cfvp archiv.tar file1 file2 file3
```



Create new archive and compress with gzip:
```shell
tar cfzvp archiv.tar.gz file1 file2 file3
```



Archive all files from inside a folder (not the folder itself)

```shell
tar cfzvp $HOME/destination/of/my/backup.tar.gz -C /path/to/source/folder ./
```



List the contents of an archive:
```shell
tar tfv archiv.tar
```



Options:

|          Option           | Description                                                    |
|:-------------------------:|:-------------------------------------------------------------- |
|             c             | create new tar archive                                         |
|             v             | verbosely  list  files processed                               |
|             f             | archive filename                                               |
|             z             | compress with gzip                                             |
|             x             | extract archive                                                |
|             t             | show content of the archive                                    |
|             p             | preserve  file permissions (default for superuser)             |
|             C             | Change to DIR before performing any following operations.      |
| --strip-components=NUMBER | Strip NUMBER leading components from file names on extraction. |



## gzip .gz archives

Compress file.txt:
```shell
gzip file.txt
```
Output: `file.gz`

Extract file.gz:
```shell
gunzip file.gz
```
Output: `file.txt`



## zip/unzip .zip archives

Create folder.zip of folder and all subfolders:
```shell
zip -r folder.zip folder
```



Extract file.zip to current folder:
```shell
unzip file.zip
```



## xzcat .xz archives

Extract file.xz to current folder and delete the input file.

```shell
unxz file.xz
```



Command aliases: 
- unxz is equivalent to xz --decompress.
- xzcat is equivalent to xz --decompress --stdout.
- lzma is equivalent to xz --format=lzma.
- unlzma is equivalent to xz --format=lzma --decompress.
- lzcat is equivalent to xz --format=lzma --decompress --stdout.

------------------------



# Compare files and directories

- [baeldung.com - benchmark of comparison methods](https://www.baeldung.com/linux/fastest-check-files-same-content)

## diff

### Quote [man diff](https://man7.org/linux/man-pages/man1/diff.1.html):

Description:  
> Compare FILES line by line.

Synopsis:    
> `diff [OPTION]... FILES`



Compare 2 directories, show the differences:

```shell
diff -q -r ~/Music /mnt/ext_ssd/music
```


| Option | Description                                  |
|:------:| -------------------------------------------- |
|   -q   | report only when files differ                |
|   -r   | recursively compare any subdirectories found |



## cmp

### Quote man cmp:  

Description:
> Compare two files byte by byte.

Synopsis: 
> `cmp [OPTION]... FILE1 [FILE2 [SKIP1 [SKIP2]]]`

## stat

### Quote man stat:  

Description:
> display file or file system status

Synopsis: 
> `stat [OPTION]... FILE...`