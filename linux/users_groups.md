# Users
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

```shell
adduser <USER>
```

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
|:------:| --------------------------------------------- |
|   -n   | Send only the message and suppress the banner | 

----------
# Groups 

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

Print all users in a group

```shell
getent group <GROUP>
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
