# Users
## User information

Info about a user and his groups with corresponding IDs. Just `id` shows Info about current user.

```shell
id <USER>
```

Show all users:

```shell
cat /etc/passwd
```

## Add user `adduser`

```shell
adduser <USER>
```

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
