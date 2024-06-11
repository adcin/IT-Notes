# Sed - stream editor for filtering and transforming text
- [GNU.org - sed homepage](https://www.gnu.org/software/sed)

## Description (excerpt from man page):
> Sed is a stream editor.  A stream editor is used to perform basic text transformations on an input stream (a file or input from a pipeline).


## Links

- [sourceforge.io - sed project page](https://sed.sourceforge.io/)
- [GNU.org - sed manual](https://www.gnu.org/software/sed/manual/html_node/index.html)
- [The seder's grab bag](https://sed.sourceforge.io/grabbag)

## Syntax

`sed` is a very powerful tool. I recommend using the tutorials an manuals listed above for further information.

##### Basic `sed` structure

```
sed -f <file>
```
- Execute instructions from a file (script).

```
sed -e <instructions>
```
- Execute the instructions directly from command line. Multiple `-e` parameters are possible.

##### Basic structure of an `<instruction>`

```
[adress]<command>[options]
```

- `[adress]` - defines on which line(s) the `command` should be executed. Examples with the command "`d`" (delete).
    - "`d`" - if no `adress` is defined, then the `instruction` is executed on all lines. In this case all lines are being deleted ... not very useful. ;)
    - `"4d"` - delete 4th line
    - `"5,9d"` - delete lines 5 to 9
    - `"/^#/d"` - delete all line matching the RegEx (RE) pattern between the slashes "`/<RegEx>/`". So, delete `d` all lines beginning `^` with a hashtag `#`.
    - `"2,/ERRORS:$/d"` - delete all lines in the range from line "`2`" until "`,`" the first line which matches the RegEx "`//`": Line ends "`$`" with the pattern "`ERRORS:`"
- `<command>[options]` - `sed` uses one character commands. Here are some common examples:
    - `a TEXT` - Append a new line with `TEXT` after a line
    - `c TEXT` - Replace (change) lines with `TEXT`
    - `d` - Delete the pattern space.
    - `i TEXT` - insert `TEXT` before a line
    - `q[EXIT-CODE]` - (quit) Exit ‘sed’ without processing any more commands or input.
        - `Q[EXIT-CODE]` - Same as `q`, but will not print the contents of pattern space.
    - `s/REGEXP/REPLACEMENT/[FLAGS]` - (substitute) Search `REGEXP` and change the result to `REPLACEMENT`.
        - Check out `info sed -n 'The "s" Command'` and the examples below.
    - `w FILENAME` - Write the pattern space to FILENAME.
        - `W FILENAME` - Same as `w FILENAME`, but writes also the rest of the line.
    - `p` - Print the current pattern space. Useful with the option `-n` to suppress unwanted output.
    - `=` - Print the current input line number
    - Check out `info sed 'sed scripts'` for details.

##### Basic structure with multiple instructions

```
sed -e [adress]<command>[options] -e [adress]<command>[options]
```

For some `commands` a semicolon "`;`" can be used as a separator as well: `sed -e [adress]<command>[options];[adress]<command>[options]`

> [!example] &nbsp;Example - Display the content of /etc/services except empty lines and comment lines:
> 
> ```bash
> sed -e "/^\s*$/d;/^#/d" /etc/services
> ```
>
> > [!info] The RegEx `^\s*$` matches all empty lines or lines containing only whitespaces (spaces, tabs etc.):
> > - `^\s*$`
> >   - `^` beginning of line 
> >   - `\s` whitespace
> >   - `*` multiplier: zero or more (whitespaces)
> >   - `$` end of line

## Options

|             Option              | Description                                           |
| :-----------------------------: | ----------------------------------------------------- |
|              `-i`               | Edit files in place.                                  |
|          `-i[SUFFIX]`           | Create a backup, then edit the file.                  |
| `-E`, `-r`, `--regexp-extended` | Use [extended regular expressions](../misc/RegEx.md)  |
|   `-n`, `--quiet`, `--silent`   | Suppress automatic printing. Useful with the flag `p` |

# Examples

> [!example] &nbsp;Example - Substitution
> 
> Change "foo" to "bar" from example.txt:
> ```shell
> sed 's/foo/bar/' example.txt
> ```
>
> > [!info]
> >
> > - The changes will be printed to stdout, but the file itself stays the same.
> > - Delimiter: The first character **after** the `s` is the delimiter - in this case it's a slash `/`. But other characters can be used as well - for example: `%` `@`
> > - `foo` can be a string or a regular expression.
> > - Only the first `foo` in each line will be changed. The syntax for all appearances of `foo` is `'s/foo/bar/g'` (`g` for global).
> 
> Change every "foo" to "bar" from example.txt and write the changes directly into the file itself:
> ```shell
> sed -i 's/foo/bar/g' example.txt
> # or create a backup example.txt.bak, then change original
> sed -i.bak 's/foo/bar/g' example.txt
> ```

> [!example] &nbsp;Combine sed with find
> 
> Replace all `foo` with `bar` in every `*.txt` file in the current directory and all subdirectories:  
> ```shell
> find ./ -type f -iname *.txt -print0 | xargs -0 sed -i 's/foo/bar/g'
> ```
> 
> Command autopsy:
> 
> | Section                | Description                                                                                                                                                   |
> | :--------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
> | `find ./ -type f`      | Search all files in the current directory and all subdirectories.                                                                                             |
> | `-iname *.txt`         | Find only files with the suffix `.txt` and search case insensitive `-iname`                                                                                   |
> | `-print0 \|`           | Print the full file name, followed by a null character (instead of the newline character) and pipe the output to the next command.                            |
> | `xargs`                | Build and execute the following command from the piped input.                                                                                                 |
> | `-0`                   | Input items are terminated by a null character instead of a whitespace or newline. Quotes and backslash are not special (every character is taken literally). |
> | `sed -i 's/foo/bar/g'` | Read each file piped by `find`, substitute all occurrences of foo with bar `s/foo/bar/g` and edit the file in place `-i`.                                     |
>
> > [!important]
> > 
> > The combination of the `-print0` and `-0` option ensures, that files with whitespaces, quotes or backslashes in their name are processed correctly.


> [!example] &nbsp;Display only the line number x.
> 
> Display the hex values of all chars in line 5 of example.sh:
> ```
> sed '5!d;q' example.sh | od -A x -t x1z -v
> ```
>
> > [!note]
> > Very useful for debugging, when you have an error caused by a wrong symbol. Examples:
> > - `-` hyphen-minus (ASCII 2D, Unicode 002D)
> > - `–` en dash (Unicode 2013)
> > - `—` em dash (Unicode 2014)
> > - `−` minus (Unicode 2212)

# Useful aliases

Display `/etc/services` without comment lines or empty lines:
```
alias ports='sed -e "/^\s*$/d;/^#/d" /etc/services | less'
```

