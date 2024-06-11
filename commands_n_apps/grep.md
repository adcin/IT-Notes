# Command: `grep`

> [!quote] &nbsp;Excerpt from man page:
> 
> ```
> NAME
> grep, egrep, fgrep, rgrep - print lines that match patterns
> 
> SYNOPSIS
> grep [OPTION...] PATTERNS [FILE...]
> grep [OPTION...] -e PATTERNS ... [FILE...]
> grep [OPTION...] -f PATTERN_FILE ... [FILE...]
> 
> DESCRIPTION
> grep searches for PATTERNS in each FILE. PATTERNS is one or more patterns separated by newline characters, and grep prints each line that matches a pattern. Typically PATTERNS should be quoted when grep is used in a shell command.
> ```

## Synonyms

| Command | `grep` equivalent | Description                                                   |
| ------- | ----------------- | ------------------------------------------------------------- |
| `egrep` | `grep -E`         | Interpret pattern as extended RegEx                           |
| `fgrep` | `grep -F`         | Interpret PATTERNS as fixed strings, not regular expressions. |
| `rgrep` | `grep -r`         | Read all files recursively.                                   |

## Common options

|                            Option                            | Description                                                                                                                                                                                                                                                           |
| :----------------------------------------------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|                             `-E`                             | Use extended RegEx (ERE)                                                                                                                                                                                                                                              |
|                             `-F`                             | Interpret PATTERNS as fixed strings, not RegEx.                                                                                                                                                                                                                       |
|                        `-e PATTERNS`                         | Useful when searching multiple PATTERNS or to protect a PATTERN which begins with a "`-`" symbol.                                                                                                                                                                     |
|                             `-i`                             | `--ignore-case` - Ignore case distinctions in patterns and input data.                                                                                                                                                                                                |
|                             `-v`                             | `--invert-match` - Invert the sense of matching, to select non-matching lines.                                                                                                                                                                                        |
|                             `-c`                             | `--count` - Print only a number of matching lines for each input file.                                                                                                                                                                                                |
|                             `-l`                             | Suppress normal output; instead print the name of each input file from which output would normally have been printed.<br>Very useful, when searching in many large files, because scanning each input file stops upon first match.                                    |
|                             `-q`                             | `--quiet` - do not write anything to standard output. Exit immediately with zero status if any match is found, even if an error was detected.                                                                                                                         |
|                             `-s`                             | Suppress error messages about nonexistent or unreadable files.                                                                                                                                                                                                        |
|                             `-n`                             | `--line-number` - Prefix each line of output with the line number within its input file.                                                                                                                                                                              |
| `--include=GLOB`<br>`--exclude=GLOB`<br>`--exclude-dir=GLOB` | Include/Exclude files/directories which match the pattern `GLOB`, using wildcard matching: `*`, `?`, `[...]` and `\` to quote a wildcard or backslash character literally.<br>If contradictory --include and --exclude options are given, the last matching one wins. |
|                             `-R`                             | Read all files under each directory, recursively. Follow all symbolic links, unlike `-r`.                                                                                                                                                                             |

## Useful aliasses

Display the matched PATTERN in color:
```
alias grep='grep --color=auto'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias rgrep='rgrep --color=auto'
```