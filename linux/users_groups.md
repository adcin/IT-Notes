# Users

- [systemd.io - User/Group Name Syntax](https://systemd.io/USER_NAMES)

## User information

Info about a user and his groups with corresponding IDs. Just `id` shows Info about current user.

```shell
id <USERAME>
```

```shell
finger <USERNAME>
```

Show all users:

```shell
cat /etc/passwd
```

## Add user

**`adduser` vs `useradd`**
- `useradd` is the main Linux command to create a user or update the user information.
- `adduser` is a more user friendly perl script which uses the `useradd` command. It is more comfortable than `useradd`, but you also have less control of the process. 

</br>

Add a normal user:  
```shell
adduser <USER>
```

</br>

Add a new sudo user (marcin) on a osmd installation:  

```shell
sudo useradd --create-home --user-group --shell $(which bash) --groups osmc,adm,disk,lp,dialout,cdrom,audio,video,sudo marcin && passwd marcin
```

Command autopsy: 

| Section                     | Description                                                              |
|:--------------------------- | ------------------------------------------------------------------------ |
| sudo                        | well, sudo ;)                                                            |
| useradd                     | create new user                                                          |
| --create-home               | create home directory                                                    |
| --user-group                | create a new default group for the user                                  |
| --shell $(which bash)       | set the default shell to bash                                            |
| --groups osmc,adm,....,sudo | add the new user to these groups                                         |
| marcin                      | username of the new user                                                 |
| && passwd marcin            | after executing useradd successfully, set a new password for marcin |

</br>

Add a system user (database01) without a home-directory and without the ability to login into a shell:  
```shell
sudo useradd --system database01
```
_This is useful if you want a separate user for a service like samba or a docker container_

## Remove user `userdel` / `deluser`

Optional preparation: Log out the user and kill all user’s running processes:  
```shell
sudo killall -u <USER>
```

Remove User:  
```shell
userdel -rf <USER>
```

| Option | Description                                                                                                                        |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------- |
| -r     | Remove home directory and mail spool                                                                                               |
| -f     | Forcefully remove the user account, even if the user is still logged in or if there are running processes that belong to the user. | 

Debian script `deluser`:

```shell
sudo deluser --remove-home <USER>
```

## Disable user

Method 1 - Lock a user's password. This puts a '!' in front of the encrypted password, effectively disabling the password:

```shell
sudo usermod -L <USERNAME>
```

Method 2 - Change users default login shell to `/sbin/nologin` or `/usr/sbin/nologin`:

```shell
sudo usermod <USERNAME> -s /sbin/nologin
```

_When trying to login the user will see the message "This account is currently not available."_

Method 3 - Change users default login shell to `/bin/false` if you don't want the user to receive a message linke in method 2:

```shell
sudo usermod <USERNAME> -s /bin/false
```

## Re-enable user

Unlock users password:

```shell
sudo usermod -U <USERNAME>
```

Change default login shell:

```shell
sudo usermod <USERNAME> -s /bin/bash
```

## Change password

```shell
passwd <USER>
```

## Change to an other user

```shell
su <USER>
```

Change to root \(Insert the root password, not the current user\!\)
```shell
su -
```

Alternative change to root \(Insert current user password\):
```shell
sudo -i
```

## Send message

Send a message to all users currently logged in:
```shell
sudo wall -n THIS IS MY MESSAGE!
```

| Option | Description                                   |
| :----: | --------------------------------------------- |
|   -n   | Send only the message and suppress the banner |

# Methods to get the current users name in a script

Check the difference when you execute this script with or without sudo:
```
#!/bin/bash
printf "\nMethods to get the current users name in a script:\n\n"
printf "\t| %-13s | %-13s |\n" "Method" "Output"
printf "\t| %-13s | %-13s |\n" | tr " " "-"
printf "\t| %-13s | %-13s |\n" '$(logname)' $(logname)
printf "\t| %-13s | %-13s |\n" '$SUDO_USER' $SUDO_USER
printf "\t| %-13s | %-13s |\n" '$USER' $USER
printf "\t| %-13s | %-13s |\n" '$(whoami)' $(whoami)
printf "\n"
```

----------

# Groups 

- [systemd.io - User/Group Name Syntax](https://systemd.io/USER_NAMES)

## Group information

List all groups:

```shell
cat /etc/group
```

```shell
getent group
```

`groups` - print the groups a user is in:

```shell
groups <USER>
```

List all group members

```shell
getent group <GROUP>
```
```
sudo groupmems -g <GROUPNAME> -l
```

## Create group

```shell
sudo groupadd <GROUP>
```

## Delete group

```shell
sudo groupdel <GROUP>
```

## Add user to group

```shell
usermod -aG <GROUP> <USER>
```

## Remove user from group

```shell
gpasswd -d <USER> <GROUP>
```

## Reinitialise current session to activate group changes

```shell
newgrp - <GROUP>
```

## Change user's initial login group

The group must exist. Any file from the user's home directory owned by the previous primary group of the user will be owned by this new group. The group ownership of files outside of the user's home directory must be fixed manually.  

```shell
usermod -g GROUP USER
```
