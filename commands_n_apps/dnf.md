# DNF - Dandified YUM package manager

- [dnf.readthedocs.io](https://dnf.readthedocs.io)
- [github.com - rpm-software-management: dnf](https://github.com/rpm-software-management/dnf)
- [wikipedia.org - dnf](https://en.wikipedia.org/wiki/DNF_(software))
- [github.io - A Blog of The DNF Team](https://rpm-software-management.github.io)

> [!quote] &nbsp;Excerpt from man page - DESCRIPTION
> 
> DNF is the next upcoming major version of YUM, a package manager for RPM-based Linux distributions. It roughly maintains CLI compatibility with YUM and defines a strict API for extensions andplugins.

## DNF - search, install, uninstall

Search for patterns (keywords) and display the result sorted from the most relevant to the least.:
```
dnf search [--all] <patterns>
```
- _Lists packages, which match all patterns (AND operation). Searches in package names and summaries._
- _`--all` option - Lists packages, which match at least one of the patterns (OR operation). Searches in package names, summaries, descriptions and URLs._

Display package information:
```
dnf info <package>
```

Install package:
```
sudo dnf install <package>
```

Remove package:
```
sudo dnf remove <package>
```

## DNF - maintenance

Check for available updates:
```
dnf check-update [--refresh]
```
_`--refresh` - Forces the metadata synchronization of all enabled repositories. By default dnf doesn't sync the data every time, to reduce unnecessary traffic._

Upgrade packages:
```
sudo dnf upgrade [--downloadonly] [-y]
```
_`--downloadonly` - Does what it says. The upgrade packages are downloaded but not yet installed._
`-y`, `--assumeyes` - Automatically answer yes for all questions.

Display packages installed by the user:
```
dnf repoquery --userinstalled
```

Display package that owns the given file `/usr/bin/vim`:
```
dnf repoquery --installed --file /usr/bin/vim [--info]
```
_`--info` -  Shows detailed information about the package._

Downgrade a package by one version:
```
sudo dnf downgrade <package>
```

Show the history of dnf transactions - the most recent as last (better when working in terminal):
```
dnf history --reverse
```

Undo all transactions performed after the transaction with `<ID>`:
```
sudo dnf history rollback <ID>
```

Undo a specific transaction:
```
sudo dnf history undo <ID>
```

## DNF - repositories

### Add EPEL (Extra Packages for Enterprise Linux) repos to rocky linux

- [wiki.rockylinux.org - Rocky Linux Repositories](https://wiki.rockylinux.org/rocky/repo/)

View currently enabled repositories:
```shell
sudo dnf repolist
```
_Use `sudo dnf repolist --all` to display all repos._

Install the EPEL package:
```shell
sudo dnf install epel-release
```

Verify installation:
```shell
sudo dnf repolist
```
