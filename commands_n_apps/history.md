# Shell history usage

Display the shells history:
```
history
```

Display the last 10 entries of the shells history:
```
history 10
```

List specific commands from history (e.g. ssh):
```
history | grep ssh
```

Execute/Repeat a command from history by its history-ID:
```
!<ID>
# e.g.
!265
```

Execute/Repeat the fourth last command from hostory:
```
!-4
```

Remove a specific command from history:
```
history -d 666
```

Clear the entire history:
```
history -c
```
##### Modify the last command

_Very useful, when you have a typo in the last command._

\1. Display/Print the last command (it will not be executed):
```
!:p
```

\2. Change foo to bar in the last command from history and execute the modified command:
```
^foo^bar^
```

Example:
```
$ printf "\n%15s\n\n" "Hello wold!"

    Hello wold!

$ ^wold^World^
printf "\n%15s\n\n" "Hello World!"

   Hello World!
   
```

# `fc` - Find/Fix command
`fc` is a shell builtin command. It lists, or edits and re‐executes, commands previously entered to an interactive shell. So it has similar functions as history, but it has 2 very nice extras:

- You can edit and then execute multiple commands at once.
- You can use your favorite editor to edit the commands.

Display last 10 commands:
```
fc -l
```

List commands 120 to 125:
```
fc -l 120 125
```

List all commands since the last one beginning with `sud` (will find `sudo`, but not `su`):
```
fc -l sud
```

Edit the last command with vim and then execute it:
```
fc -e vim
```

_The command will be executed immediately after you exit the editor. If you saved the changes, then the new command will be executed. If you did not save the changes, then the old command will be repeated. So the best way to cancel the execution of the command is to delete it in the editor, then save and exit._

Edit the 2nd last command in nano:
```
fc -e nano -2
```

Edit commands 120 to 122 in vi:
```
fc -e vi 120 122
```

# History variables
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

