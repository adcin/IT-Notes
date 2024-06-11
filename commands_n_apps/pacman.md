> [!warning] Work in progress!
> Stranger - you better should not use these notes!

# `pacman` - package manager utility

> [!quote] &nbsp;Excerpt from man page - DESCRIPTION
> 
> ```
> Pacman is a package management utility that tracks installed packages on a Linux system. It features dependency support, package groups, install and uninstall scripts, and the ability to sync your local machine with a remote repository to automatically upgrade packages. Pacman packages are a zipped tar format.
> 
> Since version 3.0.0, pacman has been the front-end to libalpm(3), the “Arch Linux Package Management” library. This library allows alternative front-ends to be written (for instance, a GUI front-end).
> 
> Invoking pacman involves specifying an operation with any potential options and targets to operate on. A target is usually a package name, file name, URL, or a search string. Targets can be provided as command line arguments. Additionally, if stdin is not from a terminal and a single hyphen (-) is passed as an argument, targets will be read from stdin.
> ```

Upgrade all packages:
```shell
pacman -Syu
```

```shell
pacman-key --init
```

Source: https://bbs.archlinux.org/viewtopic.php?pid=1837082#p1837082
```shell
# rm -R /etc/pacman.d/gnupg/
# rm -R /root/.gnupg/ 
# gpg --refresh-keys
# pacman-key --init && pacman-key --populate archlinux
# pacman-key --refresh-keys
```

## Query package database; Search

Query local database:
```
pacman -Q search-term
```

Query remote packages:
```
pacman -S search-term
```

Use Regular-Expressions:
```
pacman -Qs 'RegEx-Pattern'
pacman -Ss 'RegEx-Pattern'
```

# Install package

```shell
pacman -S package-name
# e.g.
pacman -S man
pacman -S stow vim git curl htop mc inxi tmux ncdu
```