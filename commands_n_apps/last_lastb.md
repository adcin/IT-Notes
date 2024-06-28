# `last`, `lastb` - show a listing of last logged in users

`last` - information about successful logins and -outs (`/var/log/wtmp`)
`lastb` - information about BAD login attemts (`/var/log/btmp`)

> [!quote] &nbsp;Excerpt from man page 
> 
> `DESCRIPTION`
> 
> ```
> last searches back through the /var/log/wtmp file (or the file designated by the -f option) and displays a list of all users logged in (and out) since that file was created. One or more usernames and/or ttys can be given, in which case last will show only the entries matching those arguments. Names of ttys can be abbreviated, thus last 0 is the same as last tty0.
> ...
> lastb is the same as last, except that by default it shows a log of the /var/log/btmp file, which contains all the bad login attempts.
> ```

> [!quote]- Click here for the `last --help` output.
> 
> ```
> Usage:
>  last [options] [\<username\>...] [\<tty\>...]
> 
> Show a listing of last logged in users.
> 
> Options:
>  -\<number\>            how many lines to show
>  -a, --hostlast       display hostnames in the last column
>  -d, --dns            translate the IP number back into a hostname
>  -f, --file \<file\>    use a specific file instead of /var/log/wtmp
>  -F, --fulltimes      print full login and logout times and dates
>  -i, --ip             display IP numbers in numbers-and-dots notation
>  -n, --limit \<number\> how many lines to show
>  -R, --nohostname     don't display the hostname field
>  -s, --since \<time\>   display the lines since the specified time
>  -t, --until \<time\>   display the lines until the specified time
>  -T, --tab-separated    use tabs as delimiters
>  -p, --present \<time\> display who were present at the specified time
>  -w, --fullnames      display full user and domain names
>  -x, --system         display system shutdown entries and run level changes
>      --time-format \<format\>  show timestamps in the specified \<format\>:
>                                notime|short|full|iso
> 
>  -h, --help           display this help
>  -V, --version        display version
> 
> For more details see last(1).
> ```

## Common use cases

List all logged in users at a specific time:
```shell
last -p '2024-06-24 13:00'
# or
last --present '2024-06-24 13:00'
```

List all logged in users in a specific time period:
```shell
last -s '2024-06-24 05:00' -t '2024-06-24 07:00'
# or
last --since '2024-06-24 05:00' --until '2024-06-24 07:00'
```

List today's logged in users:
```shell
last -s today
```

List all bad login attempts from yesterday:
```shell
sudo lastb -s yesterday -t today
```

Common syntax for the time (check man page for more):
- `now`
- `today`
- `yesterday`
- `YYYY-MM-DD hh:mm`