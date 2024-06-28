# GIT Basics

- [Git - Official Website](https://git-scm.com/)
- [gittutorial - A tutorial introduction to Git](https://git-scm.com/docs/gittutorial)
- [giteveryday - A useful minimum set of commands for Everyday Git](https://git-scm.com/docs/giteveryday)

**Excerpt from man description:**
> Git is a fast, scalable, distributed revision control system with an unusually rich command set that provides both high-level operations and full access to internals.

**Git tutorials in the man pages:**
- `man 7 gittutorial` - _A tutorial introduction to Git_
- `man 7 gittutorial-2` - _A tutorial introduction to Git: part two_
- `man 7 gitcore-tutorial`  - _A Git core tutorial for developers_
- `man 7 giteveryday` - _A useful minimum set of commands for Everyday Git_

`git help` - A structured entry point for the multiple git man pages.

**Local html files (open with a browser):**
1. Git User’s Manual: `file:///usr/share/doc/git/user-manual.html`
2. howto-index: `file:///usr/share/doc/git/howto-index.html`

**Source-Code Hosters:**  
- [GitHub](https://docs.github.com)
- [SourceForge](https://sourceforge.net)
- [GitLab](https://about.gitlab.com/)
- [wikipedia.org - Comparison of source-code-hosting facilities](https://en.wikipedia.org/wiki/Comparison_of_source-code-hosting_facilities)

# Git Config

For git commits a user.name and user.email need to be set. I do it globally, but it can be set individually for every repository.

**List current personal config:**  
```shell
git config -l
```
_Output might be empty, if no config has been set yet._

**Set/Change global git user name:**  
```shell
git config --global user.name "Rocky Balboa"
```

**Set/Change global git user email:**  
```shell
git config --global user.email "rocky.balboa@rambo.com"
```

# Simple local workflow

**Workflow:**
1. Create project directory
2. Initialize the git repository (repo)
3. Make changes or add files
4. Add the new files to staging area
5. Commit the changes

**Create an empty folder for your project and navigate into the new folder:**
```shell
mkdir -p ~/projects/abc && cd ~/projects/abc
```

**Initialize the empty git repo:**  
```shell
git init
```

**Now you should put or crate your project files in the folder.**

**Add all files to repo staging area:**  
```shell
git add .
```

**Commit changes to the local repo:**  
```shell
git commit
```

# Simple remote workflow (GitHub with ssh)

**Workflow:**  
1. Create project directory
2. Clone (download) the repo from the remote server
3. Make changes or add files
4. Add the new files to staging area
5. Commit all changes
6. Push (upload) the repo to the remote server

**Create an empty folder for your project and navigate into the new folder:**  
```shell
mkdir -p ~/projects/xyz && cd ~/projects/xyz
```

**Clone (download) the repo into current directory:**  
```shell
git clone git@github.com:Cin-Hub/IT-Notes.git
```

**Now you can add files and make changes.**

**Add all files to repo staging area:**
```shell
git add .
```

**Commit changes to the local repo:**  
```shell
git commit
```

**Transfer/Push the changes to the remote repo:**  
```shell
git push
```

# Useful commands

**Check repo status:**  
```shell
git status
```

**Show changes of a file:**  
```shell
git diff path/to/filename.txt
```

**List all commits of the repo:**  
```shell
git log
# or for short info
git log --oneline
```

**Reverse/Undo the effect of an earlier commit:**  
```shell
# List the commits
git log --oneline
# Enter the hash of the commit that needs to be undone
git revert kja3hfd
```
_A new commit gets created which documents the revert._

> [!note] &nbsp;Check the URL of a remote repo:
> 
> ```bash
> git remote -v
> ```
> 
> > [!quote] &nbsp;Outpot example:
> > 
> > ```
> > origin	git@git-server:/git-server/dotfiles.git (fetch)
> > origin	git@git-server:/git-server/dotfiles.git (push)
> > ```

> [!note] &nbsp;Change the URL of a remote repo:
> 
> ```bash
> git remote set-url origin git@git-server:./dotfiles.git
> ```

# Simple remote git server

The goal is to create a git-server, to manage, backup and distribute my scripts, projects, config-files and other data on a central location. Only basic tools will be used.

**Sources/Inspiration:**  
- [inmotionhosting.com - How to Create Your Own Git Server](https://www.inmotionhosting.com/support/website/git/git-server)
- [git-scm.com - 4.4 Git on the Server - Setting Up the Server](https://git-scm.com/book/en/v2/Git-on-the-Server-Setting-Up-the-Server)
- [linuxfoundation.org - Classic SysAdmin: How to Run Your Own Git Server](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-how-to-run-your-own-git-server)

**Hardware**: Raspberry Pi 3 Model B (aarch64)
**OS**: [DietPi](https://dietpi.com/) v8.25.1

**Features:**
- Restricted access
- auth-keys, no passwords
- Security features
- No unnecessary apps or tools

## Server - install and config git

```shell
sudo apt-get install git-core
```

**Check for git shell:**  
```shell
which git-shell
```
> /usr/bin/git-shell

**Add git-shell path to `/etc/shells`:**  
```shell
sudo -e /etc/shells
```

## Server - create and configure user git

```shell
sudo adduser --system --shell $(which git-shell) --group --disabled-login --disabled-password --home /mnt/dietpi_userdata/git-server git
```

**Change to git user with bash as shell:**  
```shell
sudo su -s $(which bash) git
```

**Set the default branch name to "main":**  
```shell
git config --global init.defaultBranch main
```
_This way you'll avoid error messages._

**Create ssh configuration:**  
```shell
mkdir ~/.ssh && \
touch ~/.ssh/authorized_keys && \
chmod -R u+rwX,go-rwx $HOME/.ssh
```

## Server - add public ssh keys to authorized_keys

Login to the server as standard user with sudo permissions. The keys will be added to the `authorized_keys` file of the git user.

**Add key from public key file:**  
```shell
sudo printf "no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty %s\n" "$(cat ~/.ssh/id_ed25519.pub)" >> /mnt/dietpi_userdata/git-server/.ssh/authorized_keys
```

**Add key directly from stdin:**  
```shell
sudo printf "no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty %s\n" "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINZmseTmW29M9HeCMlYuG33ylCRHf0JE/zKDLJYcH3Qw user1@example.com" >> /mnt/dietpi_userdata/git-server/.ssh/authorized_keys
```

**Copy&Paste codeblock:**  
```shell
sudo printf "no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty %s\n" "" >> /mnt/dietpi_userdata/git-server/.ssh/authorized_keys
```

## Server - create bare git repository

**Change to git user with bash as shell:**  
```shell
sudo su -s $(which bash) git
```

```shell
mkdir ~/scripts-home.git
```

```shell
cd ~/scripts-home.git
```

**Create an empty Repo:**  
```shell
git init --bare
```

## Client - add git-server to ~/.ssh/config

**Add on each client:**  
```shell
printf "\nHost git-server
    HostName 192.168.0.10
    Port 22
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes\n" >> ~/.ssh/config
```

_This makes it much easier to use._

## Client - create repo

Let's assume the project folder already exists and had data in it. Otherwise you'll need to create it.

**CD into the project folder:**  
```shell
cd ~/scripts-home
```

**Create the git repo:**  
```shell
git init
```

**Add all files to the index:**
```shell
git add .
```

**Commit the repo:**  
```shell
git commit -m "first commit after init"
```

**Add the remote server to the repo:**  
```shell
git remote add origin git@git-server:./scripts-home.git
```

**Push the files to remote repo:**  
```shell
git push --set-upstream origin master
```
__

> [!note]
> My default init branch name is "master". If yours is different, you'll need to adjust the push command. List the name with: `git branch -l`

**A little checking:**  
```shell
git fetch -v
```

Output: 
> From git-server:./scripts-home
>  = [up to date]      main       -> origin/main

**Congratulations, you're finished.
Now you can add, commit and push anytime you like.**

# Branches

A single Git repository can maintain multiple branches. 

# Link collection

https://initialcommit.com/blog/Git-Cheat-Sheet-Beginner
https://www.atlassian.com/git/glossary
