# du - estimate file space usage
Description (excerpt from `man` page): 
> Summarize disk usage of the set of FILEs, recursively for directories.

- [gnu.org - du manual](https://www.gnu.org/software/coreutils/manual/html_node/du-invocation.html)
- [man7.org - du(1) — Linux manual page](https://man7.org/linux/man-pages/man1/du.1.html)

Help/Info/Manual:  
```shell
du --help
man du
info du
```


</br>

Show folder size (`./` - current folder):

```shell
du -sh ./
```


Show size of each folder that's in the current directory:  
```shell
du -m -d 1
```


| Option               | Description                                                                                      |
|:-------------------- |:------------------------------------------------------------------------------------------------ |
| -s, --summarize      | Display an entry for each specified file                                                         |
| -h, --human-readable | “Human-readable” output, like MB,GB,TB...                                                        |
| -c, --total          | Display a grand total                                                                            |
| -m                   | like --block-size=1M                                                                             |
| -d, --max-depth=N    | print the total for a directory only if it is N or fewer levels below the command line argument. |
| --exclude=PATTERN    | exclude files that match PATTERN. PATTERN is a shell pattern (not a regular expression).         | 
