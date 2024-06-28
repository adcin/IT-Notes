# `date` - print or set the system date and time

**Description (excerpt from `man` page):**

> Display date and time in the given FORMAT.  With -s, or with \[MMDDhhmm\[\[CC]YY]\[.ss]], set the date and time.

**Synopsis:**

`date [option]… [+format]`

# Examples

Print current date in format: YYYY-MM-DD hh:mm:ss
```shell
date '+%Y-%m-%d %X'
```
Output:
```text
2024-01-07 20:03:13
```

## Using `date` for timestamps

If you generate multiple files (for example with a cronjob) then it's a good idea to use a timestamp in the filename. Here are 2 methods I currently use.

##### Method 1 - the quick and dirty
This method uses the Unix time feature of the `date` command. In computing the epoch ([wikipedia.org - Epoch (computing)](https://en.wikipedia.org/wiki/Epoch_(computing))) is a fixed date and time which is used as a reference to measure time. The Unix and POSIX epoch is: **1 January 1970 00:00:00 UT**

And the Unix time ([wikipedia.org - Unix time](https://en.wikipedia.org/wiki/Unix_time)) is the number of seconds that passed since the Unix epoch.

Print the current Unix time:
```shell
$ date +%s
1718376559
```

Convert the previous Unix time to the default format:
```shell
$ date --date='@1718376559'
Fr 14. Jun 16:49:19 CEST 2024
```
_I'm in germany, so my current time zone is CEST._

Let's use this as a timestamp for the file name:
```shell
$ touch test-$(date +%s).txt
$ ls
test-1718378605.txt
```

If you need to get more precise, you can add nanoseconds:
```shell
$ touch test-$(date +%s%N).txt
$ ls
test-1718379232608643670.txt
```

##### Method 2 - the human readable
- To get a good result with alphabetical sorting, I like to use the following order:
    - `YEAD - MONTH - DAY - HOUR - MINUTE - SECOND`
- I also try to avoid characters, which may cause trouble - like: space, tab, slash, colon, dot

Print the date and time in an human readable and IT friendly format:
```shell
$ date +%FT%H-%M-%S
2024-06-14T17-47-05
```

Usually used as a variable in a script:
```shell
TIMESTAMP=$(date +%FT%H-%M-%S)
```



# Format sequences

| Sequence | Description                                                         |
| :------: | ------------------------------------------------------------------- |
|   `%%`   | a literal %                                                         |
|   `%a`   | locale’s abbreviated weekday name (e.g., Sun)                       |
|   `%A`   | locale’s full weekday name (e.g., Sunday)                           |
|   `%b`   | locale’s abbreviated month name (e.g., Jan)                         |
|   `%B`   | locale’s full month name (e.g., January)                            |
|   `%c`   | locale’s date and time (e.g., Thu Mar  3 23:05:25 2005)             |
|   `%C`   | century; like %Y, except omit last two digits (e.g., 20)            |
|   `%d`   | day of month (e.g., 01)                                             |
|   `%D`   | date; same as %m/%d/%y                                              |
|   `%e`   | day of month, space padded; same as %_d                             |
|   `%F`   | full date; like %+4Y-%m-%d                                          |
|   `%g`   | last two digits of year of ISO week number (see %G)                 |
|   `%G`   | year of ISO week number (see %V); normally useful only with %V      |
|   `%h`   | same as %b                                                          |
|   `%H`   | hour (00..23)                                                       |
|   `%I`   | hour (01..12)                                                       |
|   `%j`   | day of year (001..366)                                              |
|   `%k`   | hour, space padded ( 0..23); same as %_H                            |
|   `%l`   | hour, space padded ( 1..12); same as %_I                            |
|   `%m`   | month (01..12)                                                      |
|   `%M`   | minute (00..59)                                                     |
|   `%n`   | a newline                                                           |
|   `%N`   | nanoseconds (000000000..999999999)                                  |
|   `%p`   | locale’s equivalent of either AM or PM; blank if not known          |
|   `%P`   | like %p, but lower case                                             |
|   `%q`   | quarter of year (1..4)                                              |
|   `%r`   | locale’s 12-hour clock time (e.g., 11:11:04 PM)                     |
|   `%R`   | 24-hour hour and minute; same as %H:%M                              |
|   `%s`   | seconds since the Epoch (1970-01-01 00:00 UTC)                      |
|   `%S`   | second (00..60)                                                     |
|   `%t`   | a tab                                                               |
|   `%T`   | time; same as %H:%M:%S                                              |
|   `%u`   | day of week (1..7); 1 is Monday                                     |
|   `%U`   | week number of year, with Sunday as first day of week (00..53)      |
|   `%V`   | ISO week number, with Monday as first day of week (01..53)          |
|   `%w`   | day of week (0..6); 0 is Sunday                                     |
|   `%W`   | week number of year, with Monday as first day of week (00..53)      |
|   `%x`   | locale’s date representation (e.g., 12/31/99)                       |
|   `%X`   | locale’s time representation (e.g., 23:13:48)                       |
|   `%y`   | last two digits of year (00..99)                                    |
|   `%Y`   | year                                                                |
|   `%z`   | +hhmm numeric time zone (e.g., -0400)                               |
|  `%:z`   | +hh:mm numeric time zone (e.g., -04:00)                             |
|  `%::z`  | +hh:mm:ss numeric time zone (e.g., -04:00:00)                       |
| `%:::z`  | numeric time zone with : to necessary precision (e.g., -04, +05:30) |
|   `%Z`   | alphabetic time zone abbreviation (e.g., EDT)                       |


By default, date pads numeric fields with zeroes.  The following optional flags may follow ’%’:

| Flag | Description                                                    |
|:----:| -------------------------------------------------------------- |
| `-`  | (hyphen) do not pad the field                                  |
| `_`  | (underscore) pad with spaces                                   |
| `0`  | (zero) pad with zeros                                          |
| `+`  | pad with zeros, and put ’+’ before future years with >4 digits |
| `^`  | use upper case if possible                                     |
| `#`  | use opposite case if possible                                  |
