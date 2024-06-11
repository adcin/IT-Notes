# `su` - run a command with substitute user and group ID

`su` lets you switch to another user and/or execute commands as another user. If you don't have or use root privileges, then you will probably need to have the other users password.

## Login vs. non-login shells

- login: Will load the users environment (configurations and variables).
- non-login: May not load the users environment. This depends also on other factors. So a non-login shell may also load the users environment partially or even completely.

## Invoke interactive login shells:

Invoke a new interactive login shell as another user with the very creative name someuser:
```shell
su --login someuser
# or
su -l someuser
# or
su - someuser
```

Invoke a new interactive login shell as root:
```shell
su - root
# or
su -
```

## Invoke interactive non-login shells:

Invoke a new interactive non-login shell as another user with the very creative name someuser:
```shell
su someuser
```

Invoke a new interactive non-login shell as root:
```shell
su root
# or
su
```