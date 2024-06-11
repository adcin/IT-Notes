# `shred` - overwrite a file to hide its contents, and optionally delete it

> [!quote] &nbsp;Excerpt from man page - DESCRIPTION
> 
> Overwrite the specified FILE(s) repeatedly, in order to make it harder for even very expensive hardware probing to recover the data.

- [gnu.org - shred](https://www.gnu.org/software/coreutils/shred)

When a file, directory or even a drive is deleted using common tools or commands, then it usually can be recovered quite easily. That's because the physical data is still there. Only the information where the physical data is located gets modified.

Shred is a tool/command which overwrites the physical data, so it gets _destroyed_ before removal.

However, you should always be aware that, with enough effort, the data could possibly still be recovered (at least partially). That's why really sensitive data should always be encrypted and you can even think about destoying the drives physically (rl shredder ^-^), when needed.

## Common options
Source: `man shred`

| Option               | Description                                        |
| :------------------- | :------------------------------------------------- |
| -f, --force          | change permissions to allow writing if necessary   |
| -n, --iterations=N   | overwrite N times instead of the default (3)       |
| --random-source=FILE | get random bytes from FILE                         |
| -u                   | deallocate and remove file after overwriting       |
| -z, --zero           | add a final overwrite with zeros to hide shredding |

## Examples

Fast shred a big file (basic security):
```
shred -fun 1 /data/video_surveillance/Cam1-Recording.avi.old
```
_Command autopsy: `-f` force it, just in case the file is write-protected; `-u` deleted the file after shredding; `-n 1` the data is not very secret, so overwriting them only 1 time to speed up the process should be enough._

Defining a source for the random data:
```
shred -fzun 100 --random-source=/dev/random /data/Secret/lost-security-report_Dallas-1963-11-22.omg
```
_By default `shred` uses an internal pseudo-random generator, with a small amount of entropy. But there are several UNIX built in sources for random data, which can be specified by the `--random-source` option. You will have to check the Documentation of your distro for more details, but here are some common sources: `/dev/urandom`, `/dev/random`, `/dev/arandom`_

Shredding and zeroing a whole drive (first unmount all `/dev/sdc` partitions):
```
sudo shred -z --random-source=/dev/random /dev/sdc
```
_The device `/dev/sdc/` will be overwritten 3 times (default value) with random data from `/dev/random`. Afterwards `shred` will overwrite the whole drive with zeros to obscure the process. This is useful, if someone nosy gets the "cleaned" disk. It is more interesting to analyse a disk with random data, than an empty one._
