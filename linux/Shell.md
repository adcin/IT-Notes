# Shell in general

Show current shell:

```shell
ps -p $$
#or
echo "$0"
```

Show available shells:

```shell
cat /etc/shells
```

Show path of a specific shell:

```shell
type -a bash
```

Change shell to bash:

```shell
bash
```

Change standard shell to bash (current user):

```shell
chsh -s /bin/bash
```

Change standard shell of a specific user to bash:

```shell
sudo chsh -s /bin/bash username
```

Reload Bash configuration `~/.bashrc` (works for other shells as well):  
```shell
source ~/.bashrc
# or in short syntax
. ~/.bashrc
```


--------------------

# Bash

## Bash aliases

The Bash alias is a shortcut for a command. It's often used to automatically execute a command with your standard options, so you don't have to type them every time.  

Syntax: `alias [alias_name]="[command]"`

You can insert the aliases into your _~/.bashrc_ file, or maybe as a better way create a separate ~/.bash_aliases file.  

```shell
nano ~/.bash_aliases
```  

Example ~/.bash_aliases file:

```text
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them directly in ~/.bashrc.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.
# To make shure .bash_aliases will be loaded, you can insert the following code into your ~/.bashrc:
#================================
# if [ -f ~/.bash_aliases ]; then
#     . ~/.bash_aliases
# fi
#================================

eval "$(dircolors -b)"

alias ls="ls --color=auto"
alias l="ls -lahF --time-style=long-iso --color=auto"
alias dir="dir --color=auto"
alias vdir="vdir --color=auto"

alias grep="grep --color=auto"
alias fgrep="fgrep --color=auto"
alias egrep="egrep --color=auto"

alias rm="rm -I"
alias cp="cp -i"
alias mv="mv -i"

alias cls="echo -ne '\033c'"
```  

List existing aliases:

```shell
alias -p
```

Unset alias:

```shell
unalias [ALIAS_NAME]
# example
uanlias ll
# unset all aliases
unalias -a
```  

## Bash shell colours

> [!warning]
> What I wrote here was a little naive, but it worked for me. Check out this Link for better and deeper information:
> - [gist.github.com - fnky: ANSI Escape Sequences](https://gist.github.com/fnky/458719343aabd01cfb17a3a4f7296797)

- https://wiki.ubuntuusers.de/Bash/Prompt/
- https://michurin.github.io/xterm256-color-picker/
- https://misc.flogisoft.com/bash/tip_colors_and_formatting

Important Bash prompt control sequences for color settings:
- `\[` - begin a sequence of non-printing characters (e.g. control sequence or comment)
- `\]` - end a sequence of non-printing characters
- `\033[` - (or `\e[`) beginning of a color control sequence
- `m` - end of a color control sequence

Examples:
- `\[\033[36m\]` - full control sequence for cyan text color
- `\[\033[00m\]` - full control sequence for resetting terminal colors to default

##### Basic foreground/text color codes:

| Code | Effect |
| :--: | ------ |
|  30  | Black  |
|  34  | Blue   |
|  36  | Cyan   |
|  32  | Green  |
|  35  | Purple |
|  31  | Red    |
|  37  | White  |
|  33  | Yellow |

##### Basic background color codes:

| Code | Effect |
| :--: | ------ |
|  40  | Black  |
|  44  | Blue   |
|  46  | Cyan   |
|  42  | Green  |
|  45  | Purple |
|  41  | Red    |
|  47  | White  |
|  43  | Yellow |

##### Attribute codes
The syntax is `<attribute>;<color>` (e.g. `4;32` for underlined green foreground/text). So the attribute first, followed by semicolon and color-code.

| Code | Effect                                           |
| :--: | ------------------------------------------------ |
|  0   | normal                                           |
|  1   | bold or light (depends on the terminal emulator) |
|  2   | dim                                              |
|  4   | underlined                                       |
|  5   | blinking (ignored by most terminal emulators)    |
|  7   | inverts the foreground and background colors     |
|  8   | hidden                                           |

Examples:
`\[\033[1;31;47m\]` - red and bold/light text on white background
`\[\033[0;32;40m\]` - normal green text on black background
`\[\033[00m\033[34m\]` - reset color settings and then make text blue

Remember:
Define at the end of the prompt settings your standard terminal text color or reset the color-setting to default (`\[\033[00m\]`).

Bash colour chart:  
![Bash Colour Chart](../zz_rss/bash-color-chart.png)

xterm-256color chart: 
![xterm-256color chart](https://upload.wikimedia.org/wikipedia/commons/1/15/Xterm_256color_chart.svg)


```shell
nano ~/.bashrc
```  

| Objective | find | change to |
|:---------|:---------|:----------|
| Activate shell colours | `#force_color_prompt=yes` | `force_color_prompt=yes`|
| Change username to red | `\[\033[01;32m\]\u@` | `\[\033[0;31m\]\u@` |

My standard user setting:  
```shell
PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u \[\033[00;00m\]@ \[\033[01;37m\]\h\[\033[00m\]:\[\033[01;34m\]\w \$\[\033[00m\] '
``` 

My standard root setting:  
```shell
PS1='${debian_chroot:+($debian_chroot)}\[\033[00;31m\]\u \[\033[00;00m\]@ \[\033[01;37m\]\h\[\033[00m\]:\[\033[01;34m\]\w \$\[\033[00m\] '
``` 

--------------------

# Change tty shortcuts 

> [!warning]
> This works, but you should better simply learn to use `[Ctrl]`+`[Shift]`+`[C]` and `[Ctrl]`+`[Shift]`+`[V]` - believe me.

Command: [stty](https://manpages.debian.org/bullseye/coreutils/stty.1.en.html) - change and print terminal line settings

List tty settings:  
```shell
stty -a
```  

Change intr (interrupt foreground process) setting to `ctrl`+`G`:  
```shell
stty intr ^G
```  

Delete (undef) lnext (enter the next character quoted) setting:  
```shell
stty lnext ^-
```  

Make the changes persistent by adding commands to `~/.bashrc`:  
```shell
echo -e "\n# remove ctrl-c and ctrl-v binding in tty\nstty intr ^G\nstty lnext ^-" >> ~/.bashrc
```  
