# Shell history usage



## Bash variables
##### HISTCONTROL

Controls how lines are saved:

- ignorespace - lines which begin with a space character are not saved in the history
- ignoredups - lines matching the previous history entry will not be saved
- ignoreboth - same as ignorespace and ignoredups together
- erasedups - like ignoredups, but all previous duplicates of the line will be erased from history, before the most recent is saved

Example:
```
HISTCONTROL=erasedups:ignorespace
```


##### HISTFILE

The file, where the history is saved (defaul `~/.bash_history`.

Example:
```
HISTFILE=~/.config/bash/.bash_history
```

##### HISTSIZE

The number of commands to remember in the command history (`0` means no saving; negative values `-666` e.g. means all cammands are saved).

Example:
```
HISTSIZE=10000
```

⚠ OBACHT

- If and how the history is saved to a file depends other settings like the Bash builtin set options `history` and `histappend`.

##### HISTFILESIZE

The maximum number of lines contained in the history file. Oldest values are being truncated if necessary. The default value equals HISTSIZE, `0` means all lines are being truncated, negative values `-666` e.g. inhibit truncation.

Example:
```
HISTFILESIZE=99999
```

