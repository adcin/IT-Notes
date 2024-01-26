# tmux - the terminal multiplexer

Description from the man page:

> tmux is a terminal multiplexer: it enables a number of terminals to be created, accessed, and controlled from a single screen.  tmux may be detached from a screen and continue running in the background, then later reattached.

<br>

# Install

```shell
sudo apt install tmux
```

<br>

# Start tmux session

```shell
tmux
```

<br>

# List running sessions

```shell
tmux ls
```

<br>

# Attach to a running session

Attach to the most recently used session:  
```shell
tmux a
```

Attach to a specific (Target) session (target session 2 in this example):  
```shell
tmux a -t 2
```

<br>

# Shortcuts / tmux commands

To execute a tmux command like "split the current pane" you need to use the prefix shortcut first and then the command shortcut. Common standard commands are:

|   `Strg` + `B`   | Prefix key for tmux commands                  |
|:----------------:| --------------------------------------------- |
|       `D`        | Detach the current client                     |
|       `%`        | Split pane vertically                         |
|       `"`        | Split pane horizontally                       |
| Arrows `↑ ↓ ← →` | Change to another pane                        |
|       `X`        | Kill the current pane                         |
|       `C`        | Create a new window (tab)                     |
|       `P`        | Change to previous window                     |
|       `N`        | Change to next window                         |
| `0` `1` .... `9` | Change to window 0 up to 9                    |
|       `w`        | Window and pane overview (great shortcut!)    | 
|       `&`        | Kill the current window                       |
|       `,`        | Rename current window                         |
|       `$`        | Rename current session                        |
|       `S`        | Interactive session select menu with previews |
|       `:`        | Start the tmux command prompt                 |

<br>

# configuration and customization

Default tmux config file locations: 

```shell
/etc/tmux.conf
~/.tmux.conf
$XDG_CONFIG_HOME/tmux/tmux.conf
~/.config/tmux/tmux.conf
```

I'm going to use `~/.config/tmux/tmux.conf`.

Create empty config file:  
```shell
mkdir -p ~/.config/tmux
touch ~/.config/tmux/tmux.conf
```

Edit the file:  
```shell
nano ~/.config/tmux/tmux.conf
```

<br>

# Mouse select and copy to desktop clipboard

- [github.com - tmux wiki - clipboard](https://github.com/tmux/tmux/wiki/Clipboard)

Copying from clipboard into a tmux session in usually not a problem. The other way around might be an issue.

Install xclip:  
```shell
sudo apt install xclip
```

Insert into `tmux.conf`:  
```shell
# Mouse Mode
set -g mouse on

# Mouse copy to clipboard - vi mode
bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "xclip -selection clipboard -i"
# Mouse copy to clipboard - emacs mode
bind-key -T copy-mode MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "xclip -selection clipboard -i"

```

Restart the tmux session.

<br>

# Autostart tmux in terminal

## Alacritty

Start Alacritty with zsh shell and tmux - attach to existing tmux session, or otherwise create a new one.

Attach to `~/.alacritty.yml`:  
```shell
shell:
  program: /bin/zsh
  args:
    - -l
    - -c
    - "tmux attach || tmux"
```

<br>

Or simply start Alacritty with this option:  
```shell
alacritty -e tmux
```

<br>

# Tips

- Some recommend setting the prefix key to `Ctrl + A`. I do not recommend this, because this is the prefix key of the cli tool **screen**. So you might have trouble when using both, tmux and screen.

<br>

# Links

- [tmux on github](https://github.com/tmux/tmux)
- [tmuxcheatsheet.com](https://tmuxcheatsheet.com/)
- [YouTube - Learn Linux TV - tmux playlist](https://youtube.com/playlist?list=PLT98CRl2KxKGiyV1u6wHDV8VwcQdzfuKe&feature=shared)