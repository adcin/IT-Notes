# tldr - Simplified and community-driven man pages

- [tldr.sh - Official website](https://tldr.sh/)
- [github.com - wiki](https://github.com/tldr-pages/tldr/wiki)
- [github.com - list of clients](https://github.com/tldr-pages/tldr/wiki/tldr-pages-clients)

---------------------------

</br>

# Installation

</br>

Debian: 
```shell
sudo apt install tldr
```

-----------------

</br>

# Syntax

</br>

```shell
tldr COMMAND[-SUBCOMMAND]
```


</br>

Update local cache:  
```shell
tldr -u
```

----------------------

</br>

# Example

Command: 
```shell
tldr printf
```

Output: 
```shell
printf

Format and print text.
More information: https://www.gnu.org/software/coreutils/printf.

 - Print a text message:
   printf "%s\n" "Hello world"

 - Print an integer in bold blue:
   printf "\e[1;34m%.3d\e[0m\n" 42

 - Print a float number with the Unicode Euro sign:
   printf "\u20AC %.2f\n" 123.4

 - Print a text message composed with environment variables:
   printf "var1: %s\tvar2: %s\n" "$VAR1" "$VAR2"

 - Store a formatted message in a variable (does not work on zsh):
   printf -v myvar "This is %s = %d\n" "a year" 2016

```