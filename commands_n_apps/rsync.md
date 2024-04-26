# rsync

Rsync  is  a  fast and extraordinarily versatile file copying tool.  It can copy locally, to/from another host over any remote shell, or to/from a remote rsync daemon. (Source: [rsync man page](https://download.samba.org/pub/rsync/rsync.1))

Basic syntax: 
```shell
rsync options SOURCE DESTINATION
```

## Important slash "`/`" syntax

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

# Common options

|          Option          | Description                                                                                                                                                                                      |
| :----------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|    `-a`, `--archive`     | archive mode (same as `-rlptgoD`) - keeps basically the meta data as in the source.                                                                                                              |
|   `-r`, `--recursive`    | copy directories recursively.                                                                                                                                                                    |
|     `-l`, `--links`      | add  symlinks to the transferred files instead of noisily ignoring them                                                                                                                          |
|     `-p`, `--perms`      | causes the receiving rsync to set the destination permissions to be the same as the source permissions.                                                                                          |
|     `-t`, `--times`      | transfer modification times along with the files and update them on the remote system.                                                                                                           |
|     `-g`, `--group`      | set the group of the destination file to be the same as the source file.                                                                                                                         |
|     `-o`, `--owner`      | set the owner of the destination file to be the same as the source file, but only if the receiving rsync is being run as the super‐user                                                          |
|           `-D`           | equivalent to "`--devices` `--specials`"                                                                                                                                                         |
|       `--devices`        | transfer character and block device files to the remote system to recreate these devices                                                                                                         |
|       `--specials`       | transfer special files, such as named sockets and fifos                                                                                                                                          |
|    `-v`, `--verbose`     | verbose mode (more v's means more verbose `-vv` e.g.)                                                                                                                                            |
|     `-q`, `--quiet`      | quiet mode                                                                                                                                                                                       |
|    `-z`, `--compress`    | compresses data during transfers                                                                                                                                                                 |
| `-h`, `--human-readable` | human readable numbers                                                                                                                                                                           |
|           `-P`           | Same as `--progress` + `--partial`                                                                                                                                                               |
|       `--progress`       | show progress during transfer                                                                                                                                                                    |
|       `--partial`        | Partial files of interrupted transfers are kept and continued, instead of being deleted and restarted.                                                                                           |
|        `--delete`        | delete extraneous files from destination                                                                                                                                                         |
|  `-e`, `--rsh=COMMAND`   | specify the remote shell to use                                                                                                                                                                  |
|    `-n`, `--dry-run`     | perform a trial run with no changes made                                                                                                                                                         |
|  `--exclude-from=FILE`   | read exclude patterns from FILE                                                                                                                                                                  |
|           `-F`           | rsync looks in each directory for a `.rsync‐filter` file and follows the rules defined in this file during the transfer. `-FF` tells rsync not to transfer the `.rsync‐filter` files themselves. |
|   `--delete-excluded`    | remove files which rsync excluded from the transfer on the destination                                                                                                                           |

# Examples
Copy recursively to remote destination (in user's home folder) and show progress. Keep symlinks, permissions and timestamps, don't keep owner/group.
```bash
rsync -rlptzP ~/sourcefolder/ user@server:./destinationfolder
```

Copy the content of a local folder to an other:  
```shell
rsync -rvzh --progress ~/docs/folder1/ ~/docs/folder2
```


Copy multiple sources to a folder:  
```shell
rsync -rvzh --progress ~/downloads/music1/ ~/downloads/music2/ ~/downloads/Pink_Floyd ~/music
```
_Notice: rsync will copy only the content of music1 and music2, but will also copy the folder itself for Pink_Floyd._


Make a backup of a folder (like copy but also deletes files from destination):
```shell
rsync -avzh --progress --delete ~/docs/folder1/ ~/docs/backup_of_folder1
```


Backup local folder to remote server via ssh:
```shell
rsync -avzh --progress --delete -e ssh ~/docs/folder1/ username@192.168.0.69:/home/username/docs/backup_of_folder1
```


Backup local folder to remote server via ssh with keyfile and specified port: 
```shell
rsync -avzh --progress --delete -e "ssh -p 6969 -i ~/.ssh/id_rsa" ~/docs/folder1/ username@192.168.0.69:/home/username/docs/backup_of_folder1
```


Backup remote server folder to local machine via ssh with keyfile and specified port: 
```shell
rsync -avzh --progress --delete -e "ssh -p 6969 -i ~/.ssh/id_rsa" username@192.168.0.69:/home/username/docs/folder1/ ~/docs/backup_of_folder1
```

# !!! THIS PART IS WORK IN PROGRESS !!!
# Advanced backups with rsync
This section describes the basics of filter rules. And remember, that "advanced" does not mean "expert" nor "master". 

⚠ OBACHT: Check your filter rules by a debug dry run, before deploying.

## Filtering (in- and excludes)

Most important is the sequence/order of the filter rules. The every filter-definition overrules the following filters. Examples: 
- If you define as 1st rule to "exclude everything" and as 2rd rule to "include /path/to/source", then rsync will not transfer anything, because the 1st rule (exclude everything) overrules every rule that follows. 
- If you have many "include" rules and as the last rule "exclude everything", then everything defined in the "include" rules will be transferred, but nothing more.
- The typical scenario is to exclude unwanted files like cache or tmp files first, than define the folders which should be transferred. For example first exclude files/folders like ".cache, .thumbnails, .DS_Store", then include folders and files "Documents/, Images/, Project/important/superimportant.pdf", and at the end "exclude everything". In this case rsync will transfer everything from "Documents/, Images/, Project/important/superimportant.pdf", except things which match ".cache, .thumbnails, .DS_Store".

### Using files with filter rules

I like using one file with typical exclude rules and then files with filter rules in the source directory/directories.

Important basics of a rsync include, exclude and filter files:

- One rule per line
- Rules are are processed top-down. When there's a conflict, then the upper one wins (first rule rules them all ^^).
- Blank lines are being ignored, as well as whole-line comments starting with `;` or `#`.
- A line with a filter rule should not contain anything else. Comments in the same line as a rule definition lead to an error!

#### Exclude filter-file

Example rsync exclude filter file (`/root/rsync/exclude.rsync-filter`)
```
# My bysic excludes
# Rsync syntax: --exclude-from=/path/to/file

.cache
/.Trash
.Trash-1*
.local/share/Trash
/Trash
.thumbnails
.thumb
Thumbs.db
.DS_Store
```

These rules should be very specific, so you don't exclude files like "Total Trash - Sonic Youth.mp3", because of the exclude rule "`*Trash*`". Important is to 

Check this repo for home dir excludes: [GitHub - rubo77](https://github.com/rubo77/rsync-homedir-excludes)
Or check gitignore-files for inspiration: [GitHub - gitignore](https://github.com/github/gitignore)

Basic usage example (remember to `--dry-run` before deploying):
```bash
sudo rsync -aP --exclude-from=/root/rsync/exclude.rsync-filter /path/to/source/ /path/to/destination
```

#### Directory based filter-files (`.rsync‐filter`)

The option `-F` makes rsync check directories for a `.rsync‐filter` file and then follow the rules defined in this file. 

Basics of a rsync filter file:

- One rule per line
- Rules are are processed top-down. When there's a conflict, then the upper one wins (first rule rules them all ^^).
- Blank lines are being ignored, as well as whole-line comments starting with `;` or `#`.
- A line with a filter rule should not contain anything else. Comments in the same line as a rule definition lead to an error!
- `+ ` (plus, space) indicates the beginning of an include rule.
- `- ` (minus, space) indicates the beginning of an exclude rule.

Filter rules can be very complex, so check the section "FILTER RULES" on the `rsync` man-pages for more info. Here is an overview of common basic syntax:

| Syntax       | Description                                                                                                                                        |
| :----------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| `+ dir1`     | transfer empty directory `dir1`                                                                                                                    |
| `+ dir*`     | transfers empty directories like: `dir1`, `dir2`, `directory42`, ...                                                                               |
| `+ file*`    | transfers files whose names start with `file`                                                                                                      |
| `+ dir**`    | transfers every path that starts with `dir` like: `dir1/file.txt`, `dir2/bar/ffaa.html`, `director42/everything.god`, ...                          |
| `+ dir***`   | same as `dir**`                                                                                                                                    |
| `+ dir1/*`   | does nothing                                                                                                                                       |
| `+ dir1/**`  | does nothing                                                                                                                                       |
| `+ dir1/***` | transfers the directory `dir1` its content like: `dir1/42.txt`, `dir1/foo.sh`, `dir1/fold/bar.py`, ...                                             |
| `- *`        | exclude everything - very useful as the last rule, so other files and folders than the ones defined above this rule are excluded/ignored by rsync. |

Example of a filter-file (`/path/to/source/.rsync‐filter`):

```
- temp/
+ docs/*.pdf
+ somedir/***
+ megadir/subdir1/***
- *
```

_Explanation_:
- _These rules are applied to the file-hierarchy beginning from the folder where the `.rsync‐filter` file is in - `/path/to/source/` in this case. So the full path of this `dir5/***` rule would be `/path/to/source/dir5/***`._
- _So lets see: Transfer only PDFs from `/path/to/source/docs/` and everything from `/path/to/source/somedir/` and `/path/to/source/megadir/subdir1/`, but ignore any `temp` folder._

Example of the corresponding basic `rsync` backup command (do a `--dry-run` before deploying):
```bash
sudo rsync -aP -F --delete /path/to/source/ /path/to/destination
```

_Explanation_:
- _The `-F` option lets rsync check for `.rsync-filter` files in `/path/to/source/` and its subdirectories. 
    - If you want to use multiple filter options, then place them behind the `-F`, otherwise this pre-scan might fail.
    - Use `-FF` if you don't want to transfer the `.rsync-filter` files themselves._
- _The absolute paths of transferred directories on the destination would be `/path/to/destination/docs/`, `/path/to/destination/somedir/` and `/path/to/destination/megadir/subdir1/`._
- _`--delete` makes rsync delete all files at the destination, which have been deleted at the source. That's commonly used for future updates of the backup._

#### Rsync with the exclude-rules and the directory-based filters from above

- The exclude rules for all backups are now defined in the file: `/root/rsync/exclude.rsync-filter`
- The directory specific filter-rules are in the file: `/path/to/source/.rsync‐filter`

Basic rsync syntax (remember to `--dry-run`):
```bash
sudo rsync --delete -aPF --exclude-from=/root/rsync/exclude.rsync-filter /path/to/source/ /path/to/destination
```

Extended rsync-syntax:
```bash
sudo rsync -rlptgoF --info=progress2 --partial --exclude-from=/root/rsync/exclude.rsync-filter --delete-excluded --delete /path/to/source/ /path/to/destination
```

_Explanation:_
- _The options `-r`, `-l`, `-p`, `-t`, `-g`, `-o` (`-rlptgo`), `--partial` and `--delete` are already explained somewhere above. ^-^_
- `--exclude-from=/root/rsync/exclude.rsync-filter` is explained here: [Exclude filter-file](rsync.md#Exclude%20filter-file)
- `-F` is explained in "[Directory based filter-files (`.rsync‐filter`)](rsync.md#Directory%20based%20filter-files%20(`.rsync‐filter`))".
- `--info=progress2` outputs progress statistics based on the whole transfer, rather than individual files, as the `--progress` or `-P` options do. Don't use the verbose option `-v` together with `progress2` - this would ruin the nice progress output.
- `--delete-excluded` is useful, when you add new rules to the `exclude.rsync-filter` file. Otherwise the newly excluded files/folders might still remain on the destination.

## Testing Filters

https://opensource.com/article/19/5/advanced-rsync
https://unix.stackexchange.com/questions/2161/rsync-filter-copying-one-pattern-only
https://www.baeldung.com/linux/rsync-filtering-files


```bash
sudo rsync --delete --info=progress2,name0 --partial -crlptgo -nF /path/to/source/ /path/to/destination
```

-crlptgo
## Logging
--log‐file=/home/dietpi/logs/rsync_$(date +%F_%H-%M-%S).log

‐‐remote‐option=OPT, ‐M  send OPTION to the remote side only

$(date +%F_%H-%M-%S)

rsync -avP --log-file=/home/dietpi/logs/rsync_$(date +%F_%H-%M-%S).log /mnt/black-ssd/ /mnt/1tb-cold-backup/


rsync -aPF --delete /mnt/extext-active1/ /mnt/500gb-hdd/

Option -F lets rsync check filter rules in /mnt/extext-active1/.rsync-filter

