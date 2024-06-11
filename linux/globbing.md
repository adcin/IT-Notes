# Glob - globbing pathnames

Source: `man 7 glob`

## Wildcards:

| Expression | Description |
|---|---|
| `?` | any single character |
| `*` | any string, including an empty string |
| `[...]` | any single character matching the definition between the brackets |
| `[!..]` | any single character NOT matching the definition between the brackets |

## Bracket expression `[...]`

### Basics

- Bracket expressions cannot be empty
- To match a `]` it must be the first char: `[]]`
- To match a `-` it must be the first or last char: `[x-]` or `[-x]`
- To match a `!` it must not be the first char: `[.!,]`
- A slash `/` cannot be matched by `*`, `?` or ranges like `[.-0]`
- Filenames starting with a dot `.` are protected and must be matched explicitly. So `rm *` would not remove dot-files (e.g. .mysuper-dotfile.conf).
- Example: `[][!-]` matches `]`, `[`, `!` and `-`
- "`[:` and `:]`" with a string in between is a named character class. Char classes still need to be put in extra brackets (e.g. `[[:alpha:]]` or `[[:alpha:][:punct:]]`)

### Character classes and ranges

| Expression               | Description                                                                            |
| ------------------------ | -------------------------------------------------------------------------------------- |
| `[agVf97!,.-]`           | matches `a`, `g`, `V`, `f`, `9`, `7`, `!`, `,`, `.` or `-`                             |
| `[a-fA-F0-9]`            | matches any single lower or upper case hexadecimal character                           |
| `[!.]`                   | any single character except dots `.`                                                   |
| `[[:alpha:]]`            | letters                                                                                |
| `[[:alnum:]]`            | letters and digits                                                                     |
| `[[:lower:]]`            | lower-case letters                                                                     |
| `[[:upper:]]`            | upper-case letters                                                                     |
| `[[:digit:]]`            | digits                                                                                 |
| `[[:xdigit:]]`           | hexadecimal digits (0-9A-Fa-f)                                                         |
| `[[:punct:]]`            | punctuation marks                                                                      |
| `[[:graph:]]`            | printable characters excluding Space                                                   |
| `[[:print:]]`            | all printable characters                                                               |
| `[[:blank:]]`            | usually Space and TAB                                                                  |
| `[[:space:]]`            | whitespace characters                                                                  |
| `[[:cntrl:]]`            | all control characters                                                                 |
| `[[:lower:][:digit:].-]` | matches any single lower-case letter (`a-z`), digit (`0-9`), dot (`.`) or minus (`-`). |
| `[![:digit:]]`           | any single character, except digits                                                    |

### Expert knowledge:

- Named character classes are defined by the LC_CTYPE category in the current locale. So \[:alpha:] matches also the chars `ö`, `ü`, `ä`, `ß`, ..., when LC_CTYPE="de_DE.UTF-8".
- Although ranges like A-Z also comprise locale characters (as defined by LC_COLLATE), you may have trouble with alphabets, which have characters before `A` or after `Z` - for example the danish alphabet: ..., X, Y, Z, Æ, Ø, Å. \[A-Z] would miss these characters, but \[\[:upper:]] would match the whole alphabet.
- The string between `[.` and `.]` is a collating element, as defined for the current locale. Examples: `[.a‐acute.]` represents an `á`; `[.a-umlaut.]` represents an `ä`
- The string between `[=` and `=]` is any collating element from its equivalence class, as defined for the current locale. Example: `[[=a=]]` might be equivalent to `[aáàäâ]`
- This means `[[=a=]]` might be equivalent to `[aáàäâ]` as well as `[a[.a‐acute.][.a‐grave.][.a‐umlaut.][.a‐circum‐flex.]]`.