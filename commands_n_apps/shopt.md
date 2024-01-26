# Shopt 
`Shopt` is a builtin command of the Bash shell. It's used to enable or disable various optional behavior settings of the Bash shell.

</br>

Manual:  
```shell
shopt --help
# or
help shopt
```

</br>

List all options and their current status:
```shell
shopt
```

</br>

Enable (set) OPTION:
```shell
shopt -s OPTION
```

</br>

Disable (unset) OPTION:
```shell
shopt -u OPTION
```

</br>


# Additional info

`Shopt` is very similar to the builtin `set` command. `Set` is a Bourne shell `/bin/sh` builtin and it is POSIX 7 complaint. `Shopt` emerges from the `set` command and is Bash specific. 
Source: [stackexchange.com - set and shopt why two](https://unix.stackexchange.com/questions/32409/set-and-shopt-why-two)