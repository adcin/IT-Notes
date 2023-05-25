#  Basic commands

**Print the current working directory path:**  
```shell
pwd
```

</br>

**Create folder:**  
```shell
mkdir FOLDER [FOLDER2] [FOL...]
```
Create all non existing parent folders as well:
```shell
mkdir -p FOLDER [FOLDER2] [FOL...]
```

</br>

**Move/Rename files and folders:**  
```shell
mv /way/to/folder/or/file /new/way/to/folder/or/file
```

</br>

**Copy:**  
```shell
cp /way/to/orig/file /way/to/new/copy
```

Copy directory and all subdirectories to folder

```shell
cp -R junk /tmp
```

</br>

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

</br>

Overwrite a file to hide its contents, then deallocate and remove it:  

```shell
shred -u <FILENAME>
```

</br>

Overwrite a whole partition to delete all files safely:  

```shell
sudo shred -vn 1 /dev/sdb1
```

</br>

Show folder size:

```shell
du -shc <FOLDER1> <FOLDER2> <...>
```
| Option | Description |
|:----:|:------|
| -s, --summarize | Display an entry for each specified file |
| -h, --human-readable | “Human-readable” output, like MB,GB,TB... |
| -c, --total | Display a grand total |

</br>

Show file system disk space usage:

```shell
df -h
```

</br>

Show Inode usage:

```shell
df -i
```

_Simplified - amount of available (used/free) files._

---------------------

</br>

# Mount

Show mounted filesystems:

```shell
cat /proc/mounts
less /proc/mounts
```

```shell
cat /etc/mtab
less /etc/mtab
```

</br>

## sshfs mount

```shell
sudo apt update && sudo apt install sshfs libfuse2 -y
```

</br>

Add remote host to known_hosts:

```shell
ssh-keyscan -H REMOTE_HOST_ADDRESS >> ~/.ssh/known_hosts
```

</br>

Create local mount directory (if necessary):

```shell
mkdir /home/USER_NAME/MOUNT/HERE
```

</br>

1st quick mount as test and for adding host key to known hosts (important):

```shell
sshfs REMOTE_USER_NAME@REMOTE_HOST_ADDRESS:/PATH/TO/REMOTE/DIRECTORY /home/LOCAL_USER_NAME/MOUNT/HERE
```

</br>

Mount without interactive password request (method: unsecure, quick and dirty):

```shell
sshfs -o reconnect,password_stdin REMOTE_USER_NAME@REMOTE_HOST_ADDRESS:/PATH/TO/REMOTE/DIRECTORY /home/LOCAL_USER_NAME/MOUNT/TO/HERE <<< 'REMOTE_USER_PASSWORD'
```

</br>

Mount with ssh authentication key `/home/LOCAL_USER_NAME/.ssh/ID_KEYFILE` (method: more secure, needs to be supported and set up on remote host first):

```shell
sshfs -o reconnect,IdentityFile=/home/LOCAL_USER_NAME/.ssh/ID_KEYFILE REMOTE_USER_NAME@REMOTE_HOST_ADDRESS:/PATH/TO/REMOTE/DIRECTORY /home/LOCAL_USER_NAME/MOUNT/TO/HERE
```

</br>

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

</br>

Create a crontab instruction:

```shell
crontab -e
```

Insert the line at the end of the file:

```shell
@reboot /home/USER_NAME/my_mount_script.sh
```

</br>

_Done - the network drive should now be automatically mounted on next reboot._

</br>

## Unmount

```shell
umount /home/LOCAL_USER_NAME/MOUNT/HERE
```

---------------------

</br>

# Symbolic links (symlinks)

Create a hardlink:
```shell
ln [/PATH/TO/FILE] [/DESTINATION/OF/SYMLINK]
``` 
_Hardlinks basically define the location of a file on the hardware (disc). If the "source" is deleted, the file is still on the disk and the hardlink works fine. Only when all hardlinks are deleted, also the file on the disk will be removed.  
Hardlinks work only on the same device and filesystem.  
Hardlinks work with files, not with folders._

Create a softlink:  
```shell
ln -s [/PATH/TO/FILE/OR/FOLDER] [/DESTINATION/OF/SYMLINK]
```  

_Softlinks refer to the path in the filesystem, not the location on the hardware/disc. When the softlink is removed, the file stays intact. If the original file is deleted, the soft symlink is still there, but it doesn't work any more.  
Softlinks can be used for files and folders, and work regardless of different disks or partitions._  


---------------------

</br>

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

Download to ~/download/subdirectory/:

```shell
wget http://example.com/file.tar -O ~/download/subdirectory/file.tar
```

Alternative:

```shell
curl http://example.com/file.tar -o /path/to/dir/file.tar
```

## Transfer over ssh `scp`

Copy from server to client:  
```shell
scp <USER>@<SOURCE_HOST>:/path/to/source/file /path/to/destination/file
``` 

Copy from client to server:  
```shell
scp /path/to/source/file <USER>@<DESTINATION_HOST>:/path/to/destination/file
``` 

Example IPv6 client to server with authentication key:
```shell
scp -6 -i ~/.ssh/megasecret.rsa -P 31337 cinhub@\[2001:6666::abcd\]:/path/to/source/file /path/to/destination/file
```

## rsync

Rsync  is  a  fast and extraordinarily versatile file copying tool.  It can copy locally, to/from another host over any remote shell, or to/from a remote rsync daemon. (Source: [rsync man page](https://download.samba.org/pub/rsync/rsync.1))

Basic syntax: 
```shell
rsync options SOURCE DESTINATION
```
</br>

### Important slash "`/`" syntax

Copy the content of `folder1`into `folder2`:
```shell
rsync -a ~/docs/folder1/ ~/docs/folder2
```

Copy the `folder1` itself and its content into `folder2`:
```shell
rsync -a ~/docs/folder1 ~/docs/folder2
```

_Notice the missing `/` at the end of the source in the 2nd example_

This means these two commands do the same: 
```shell
rsync -a ~/docs/folder1/ ~/docs/folder2/folder1
rsync -a ~/docs/folder1 ~/docs/folder2
```
</br> 

### Common options

|   Option   | Description                                                                       |
|:----------:| --------------------------------------------------------------------------------- |
|     -a     | archive mode (same as -rlptgoD) - keeps basically the meta data as in the source. |
|     -v     | verbose mode                                                                      |
|     -q     | quiet mode                                                                        |
|     -z     | compresses data during transfers                                                  |
|     -h     | human readable numbers                                                            |
| --progress | show progress during transfer                                                     |
|  --delete  | delete extraneous files from destination                                          |
|     -e     | specify the remote shell to use                                                   |
|     -n     | perform a trial run with no changes made                                          |
</br>

### Examples

Copy the content of a local folder to an other:  
```shell
rsync -rvzh --progress ~/docs/folder1/ ~/docs/folder2
```
</br>

Copy multiple sources to a folder:  
```shell
rsync -rvzh --progress ~/downloads/music1/ ~/downloads/music2/ ~/downloads/Pink_Floyd ~/music
```
_Notice: rsync will copy only the content of music1 and music2, but will also copy the folder itself for Pink_Floyd._
</br>

Make a backup of a folder (like copy but also deletes files from destination):
```shell
rsync -avzh --progress --delete ~/docs/folder1/ ~/docs/backup_of_folder1
```
</br>

Backup local folder to remote server via ssh:
```shell
rsync -avzh --progress --delete -e ssh ~/docs/folder1/ username@192.168.0.69:home/username/docs/backup_of_folder1
```
</br>

Backup local folder to remote server via ssh with keyfile and specified port: 
```shell
rsync -avzh --progress --delete -e "ssh -p 6969 -i ~/.ssh/id_rsa" ~/docs/folder1/ username@192.168.0.69:home/username/docs/backup_of_folder1
```
</br>

Backup remote server folder to local machine via ssh with keyfile and specified port: 
```shell
rsync -avzh --progress --delete -e "ssh -p 6969 -i ~/.ssh/id_rsa" username@192.168.0.69:home/username/docs/folder1/ ~/docs/backup_of_folder1
```

---------------------

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

Plocate - fast search the file sshd_config: 
```shell
plocate sshd_config
```

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

Show file which have been accesed up till 2 days ago:

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

------------------------

# Archives


## GNU  tar .tar archives

Extract:
```shell
tar xfv archiv.tar
```

Extract to a defined directory:
```shell
tar xfv archiv.tar -C $HOME/extrace/here
```

Create new archive:
```shell
tar cfvp archiv.tar file1 file2 file3
```

Create new archive and compress with gzip:
```shell
tar cfzvp archiv.tar file1 file2 file3
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

| Option | Description                                               |
|:------:|:--------------------------------------------------------- |
|   c    | create new tar archive                                    |
|   v    | verbosely  list  files processed                          |
|   f    | archive filename                                          |
|   z    | compress with gzip                                        |
|   x    | extract archive                                           |
|   t    | show content of the archive                               |
|   p    | preserve  file permissions (default for superuser)        |
|   C    | Change to DIR before performing any following operations. | 

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

## unzip .zip archives

Extract file.zip to current folder:
```shell
unzip file.zip
```