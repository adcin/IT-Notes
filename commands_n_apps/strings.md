# Strings - Print the sequences of printable characters in files

> [!quote] &nbsp;Excerpt from man page:
> 
> DESCRIPTION
> 
> For each file given, GNU strings prints the printable character sequences that are at least 4 characters long (or the number given with the options below) and are followed by an unprintable character.
> 
> ...
> 
> strings is mainly useful for determining the contents of non‐text files.

## Examples

 Print all strings of /bin/cat:
 ```bash
strings /bin/cat
```

Print strings with a length of at least 10 characters and the name of the file before each string:
```bash
strings -f -n 10 /bin/cat
```

## Common options

|     Option      | Description                                                                                                                                                                                                                                                                                                                                                                    |
| :-------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|       -f        | Print the name of the file before each string.                                                                                                                                                                                                                                                                                                                                 |
| -n _min‐length_ | Print sequences of at least _min‐length_ characters.                                                                                                                                                                                                                                                                                                                           |
|   -t _radix_    | Print the offset within the file before each string (_radix_: o for octal, x for hexadecimal, d for decimal).                                                                                                                                                                                                                                                                  |
|  -e _encoding_  | Character encoding of the strings that are to be found. Possible values:<br><br>&nbsp;&nbsp;s = single-7-bit-byte characters (default)<br>&nbsp;&nbsp;S = single-8-bit-byte characters<br>&nbsp;&nbsp;b = 16-bit bigendian<br>&nbsp;&nbsp;l = 16-bit littleendian<br>&nbsp;&nbsp;B = 32-bit bigendian<br>&nbsp;&nbsp;L = 32-bit littleendian<br><br>Useful for finding wide character strings (l and b apply to, for example, Unicode UTF-16/UCS-2 encodings). |

rtfm for more ^-^

> [!abstract] Diagram demonstrating big- versus little-endianness
> ![700](_attachments-media/32bit-Endianess.svg)
> By [Aeroid](http://commons.wikimedia.org/wiki/User:Aeroid) - Own work, [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0), [Wikimedia-Link](https://commons.wikimedia.org/w/index.php?curid=137790829)
