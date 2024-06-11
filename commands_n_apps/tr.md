# Command: `tr`

> [!quote] &nbsp;Excerpt from the man page:
> 
> NAME
> tr - translate or delete characters
> 
> SYNOPSIS
> `tr [OPTION]... SET1 [SET2]`
> 
> DESCRIPTION
> Translate, squeeze, and/or delete characters from standard input, writing to standard output.

| Option | Description                                 |
| :----: | ------------------------------------------- |
|  `-d`  | delete characters in SET1, do not translate |
|  `-c`  | use the complement of SET1                  |

## Examples

Create a string of 20 random alphanumeric characters (letters and digits):
```shell
tr -dc A-Za-z0-9 </dev/urandom | head -c 13; echo
```
Command autopsy:

| Section | Description          |
| :------ | -------------------- |
| -dc     | You should know this |
