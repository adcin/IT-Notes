# Printenv - print all or part of environment

- [GNU.org - prinzenv](https://www.gnu.org/software/coreutils/printenv)

> [!quote] &nbsp;Excerpt from man page:
> 
> SYNOPSIS
> printenv [OPTION]... [VARIABLE]...
> 
> DESCRIPTION
> Print the values of the specified environment VARIABLE(s).  If no VARIABLE is specified, print name and value pairs for them all.

## Examples

Show the current environment variables:
```bash
printenv
```
- _Alternative command: `env`_

Show the value of the EDITOR variable:
```bash
printenv EDITOR
```
- Many alternatives, like: `echo $EDITOR`

Show all LC_ variables:
```bash
printenv | grep LC_
```

> [!quote]- &nbsp;Output (stdout)
> 
> ```
> LC_ADDRESS=de_DE.UTF-8
> LC_NAME=de_DE.UTF-8
> LC_MONETARY=de_DE.UTF-8
> LC_PAPER=de_DE.UTF-8
> LC_IDENTIFICATION=de_DE.UTF-8
> LC_TELEPHONE=de_DE.UTF-8
> LC_MEASUREMENT=de_DE.UTF-8
> LC_TIME=de_DE.UTF-8
> LC_NUMERIC=de_DE.UTF-8
> ```


> [!important]
> The printed environment variables are from the current terminal session. So there may be different values in your Desktop Environment, than in the shell session, or they may change if you switch to another shell or open a different terminal emulator.

