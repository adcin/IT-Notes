# `zsh` - the Z shell

- https://www.zsh.org/
- https://zsh.sourceforge.io/
- https://ohmyz.sh/
- https://www.bash2zsh.com/
- [Configuring Zsh Without Dependencies](https://thevaluable.dev/zsh-install-configure-mouseless/)

> [!quote] &nbsp;Excerpt from man page
> 
> DESCRIPTION
>
> Zsh is a UNIX command interpreter (shell) usable as an interactive login shell and as a shell script command processor. Of the standard shells, zsh most closely resembles ksh but includes many enhancements. It does not provide compatibility with POSIX or other shells in its default operating mode...

# Installation

List currently available shells:
```shell
cat /etc/shells
```

Install zsh:
```shell
sudo apt install zsh
sudo dnf install zsh
# and so on
```

Stdout after initial wizard

```shell
Copied old '~/.zshrc' to '~/.zshrc.zni'.  
  
The function will not be run in future, but you can run  
it yourself as follows:  
 autoload -Uz zsh-newuser-install  
 zsh-newuser-install -f  
  
The code added to ~/.zshrc is marked by the lines  
# Lines configured by zsh-newuser-install  
# End of lines configured by zsh-newuser-install  
You should not edit anything between these lines if you intend to  
run zsh-newuser-install again.  You may, however, edit any other part  
of the file.
```

# Prompt

In zsh you can test your prompt by using the `print` command:
```shell
print -P '%F{green}%n@%m%f %F{blue}%~%f %# '
```

