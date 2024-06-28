# `watch` - execute a program periodically, showing output fullscreen

The `watch` command line utility simply repeats a command periodically and displays the output full screen.

> [!quote]- `$ watch --help`
> 
> ```
> Usage:
>  watch [options] command
> 
> Options:
>   -b, --beep             beep if command has a non-zero exit
>   -c, --color            interpret ANSI color and style sequences
>   -d, --differences[=\<permanent>]
>                          highlight changes between updates
>   -e, --errexit          exit if command has a non-zero exit
>   -g, --chgexit          exit when output from command changes
>   -q, --equexit \<cycles>
>                          exit when output from command does not change
>   -n, --interval \<secs>  seconds to wait between updates
>   -p, --precise          attempt run command in precise intervals
>   -r, --no-rerun         do not rerun program on window resize
>   -t, --no-title         turn off header
>   -w, --no-wrap          turn off line wrapping
>   -x, --exec             pass command to exec instead of "sh -c"
> 
>  -h, --help     display this help and exit
>  -v, --version  output version information and exit
> 
> For more details see watch(1).
> ```

Simple example: Show the date and time - refresh every 2 seconds (default refresh rate):
```
$ watch 'echo Currently it is: $(date)'
```
Output:
```
Every 2,0s: echo Currently it is: $(date)           testuser: Sun Jun 16 14:39:18 2024

Currently it is: So 16. Jun 14:39:18 CEST 2024

...
```

Use `Ctrl`+`c` to exit.