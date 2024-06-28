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

Search for a specific file/command. For example - which packages provide the `newaliases` command:
```shell
dnf provides */newaliases
```

> [!quote]- Output example: `$ dnf provides */newaliases`
> ```
> determining the fastest mirror (177 hosts).. done.        ] ---  B/s |   0  B     --:-- ETA
> Fedora 40 - x86_64                                          1.8 MB/s |  56 MB     00:31
> Fedora 40 openh264 (From Cisco) - x86_64                    1.1 kB/s | 2.1 kB     00:01
> Fedora 40 - x86_64 - Updates                                2.2 MB/s |  31 MB     00:14
> determining the fastest mirror (21 hosts).. done.         ] ---  B/s |   0  B     --:-- ETA
> RPM Fusion for Fedora 40 - Free                             511 kB/s | 603 kB     00:01
> RPM Fusion for Fedora 40 - Free - Updates                   269 kB/s | 177 kB     00:00
> RPM Fusion for Fedora 40 - Nonfree                          394 kB/s | 261 kB     00:00
> determining the fastest mirror (1 hosts).. done.          ] ---  B/s |   0  B     --:-- ETA
> RPM Fusion for Fedora 40 - Nonfree - NVIDIA Driver           17 kB/s |  15 kB     00:00
> RPM Fusion for Fedora 40 - Nonfree - Steam                  2.6 kB/s | 2.1 kB     00:00
> RPM Fusion for Fedora 40 - Nonfree - Updates                131 kB/s |  86 kB     00:00
> exim-4.97.1-5.fc40.x86_64 : The exim mail transfer agent
> Repo        : fedora
> Matched from:
> Filename    : /usr/bin/newaliases
> 
> opensmtpd-6.8.0p2-14.fc40.x86_64 : Free implementation of the server-side SMTP protocol as defined by RFC 5321
> Repo        : fedora
> Matched from:
> Filename    : /usr/bin/newaliases
> 
> opensmtpd-7.5.0p0-1.fc40.x86_64 : Free implementation of the server-side SMTP protocol as defined by RFC 5321
> Repo        : updates
> Matched from:
> Filename    : /usr/bin/newaliases
> 
> postfix-2:3.8.5-4.fc40.x86_64 : Postfix Mail Transport Agent
> Repo        : fedora
> Matched from:
> Filename    : /usr/bin/newaliases
> 
> sendmail-8.18.1-1.fc40.x86_64 : A widely used Mail Transport Agent (MTA)
> Repo        : fedora
> Matched from:
> Filename    : /usr/bin/newaliases
> 
> ssmtp-2.64-35.fc40.x86_64 : Extremely simple MTA to get mail off the system to a Mailhub
> Repo        : fedora
> Matched from:
> Filename    : /usr/bin/newaliases
> ```
> 
> OK ... so the following packages provide the `newaliases` command on my fedora 40 workstation:
> - `exim` - The exim mail transfer agent
> - `opensmtpd` - Free implementation of the server-side SMTP protocol as defined by RFC 5321
> - `postfix` - Postfix Mail Transport Agent
> - `sendmail` - A widely used Mail Transport Agent (MTA)
> - `ssmtp` - Extremely simple MTA to get mail off the system to a Mailhub

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
