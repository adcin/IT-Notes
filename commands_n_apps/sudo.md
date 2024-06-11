# sudo

> [!quote] &nbsp;Excerpt from man page:
> 
> DESCRIPTION
> 
> Sudo allows a permitted user to execute a command as the superuser or another user, as specified by the security policy.  The invoking user’s real (not effective) user‐ID is used to determine the user name with which to query the security policy.


# Installation
You need to have root permissions
```bash
apt install sudo
```

# Add user to sudo group

```bash
usermod -aG sudo username
```

# Configuration

```bash
sudo visudo
```

> [!quote]- &nbsp;/etc/sudoers
> 
> ```bash
> #
> # This file MUST be edited with the 'visudo' command as root.
> #
> # Please consider adding local content in /etc/sudoers.d/ instead of
> # directly modifying this file.
> #
> # See the man page for details on how to write a sudoers file.
> #
> Defaults        env_reset
> Defaults        mail_badpass
> Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
> 
> # This fixes CVE-2005-4890 and possibly breaks some versions of kdesu
> # (#1011624, https://bugs.kde.org/show_bug.cgi?id=452532)
> Defaults        use_pty
> 
> # This preserves proxy settings from user environments of root
> # equivalent users (group sudo)
> #Defaults:%sudo env_keep += "http_proxy https_proxy ftp_proxy all_proxy no_proxy"
> 
> # This allows running arbitrary commands, but so does ALL, and it means
> # different sudoers have their choice of editor respected.
> #Defaults:%sudo env_keep += "EDITOR"
> 
> # Completely harmless preservation of a user preference.
> #Defaults:%sudo env_keep += "GREP_COLOR"
> 
> # While you shouldn't normally run git as root, you need to with etckeeper
> #Defaults:%sudo env_keep += "GIT_AUTHOR_* GIT_COMMITTER_*"
> 
> # Per-user preferences; root won't have sensible values for them.
> #Defaults:%sudo env_keep += "EMAIL DEBEMAIL DEBFULLNAME"
> 
> # "sudo scp" or "sudo rsync" should be able to use your SSH agent.
> #Defaults:%sudo env_keep += "SSH_AGENT_PID SSH_AUTH_SOCK"
> 
> # Ditto for GPG agent
> #Defaults:%sudo env_keep += "GPG_AGENT_INFO"
> 
> # Host alias specification
> 
> # User alias specification
> 
> # Cmnd alias specification
> 
> # User privilege specification
> root    ALL=(ALL:ALL) ALL
> 
> # Allow members of group sudo to execute any command
> %sudo   ALL=(ALL:ALL) ALL
> 
> # See sudoers(5) for more information on "@include" directives:
> 
> @includedir /etc/sudoers.d
> ```

Save custom user configuration files to `/etc/sudoers.d/`.

# Disable sudo password query for a specific user
The basic syntax is (replace `username`):
```
username ALL=(ALL:ALL) NOPASSWD: ALL
```

Method 1 - Insert the prior described line via `sudo visudo` to the `/etc/sudoers` in the "User privilege specification" section:
```
# User privilege specification
root    ALL=(ALL:ALL) ALL
username ALL=(ALL:ALL) NOPASSWD: ALL
```

Method 2 (preferred) - Add a custom user config file to `/etc/sudoers.d/`:
```
echo 'username ALL=(ALL:ALL) NOPASSWD: ALL' | sudo tee /etc/sudoers.d/username
```
_Check "@include" in `man 5 sudoers` for further details._

# Link collection
Beware, I may not have checked theses yet, but just saved for later.

- https://karandeepsingh.ca/post/sudo-mastery-and-best-practices/
- https://github.com/fcaviggia/hardening-script-el6/blob/master/config/sudoers
- https://networklogician.com/2022/01/05/best-practices-for-etc-sudoers/
- https://www.digitalocean.com/community/questions/mini-tutorial-restricting-sudo-users-to-only-a-handful-commands
- https://ostechnix.com/restrict-sudo-users-run-specific-commands/