# Ykman - the command line YubiKey manager
- [yubico.com - YubiKey Manager](https://www.yubico.com/support/download/yubikey-manager/)

# Basic commands

## Show ykman manual
General help:
```shell
ykman --help
# or for full help manual (I didn't see a difference to the normal help)
ykman --full-help
```

Help for a specific command (oath in this example):
```shell
ykman oath --help
```

## Identify device (YubiKey)

It is recommended to have a backup YubiKey device. So you can access your accounts even though you loose your Key. 

Most YubiKeys can be identified by the serial number:
```shell
ykman list --serials
```

The serial number is also listed in the general information:
```shell
ykman info
```

You can also identify the YubiKey online:

- [yubico.com - Identify via browser](https://www.yubico.com/products/identifying-your-yubikey/)
- [yubico.com - FAQ - Identifying Your YubiKey](https://support.yubico.com/hc/en-us/articles/360013642100-Identifying-Your-YubiKey)

# Troubleshooting

Get the diagnostic information:  
```shell
ykman --diagnose
```

## Issue: PC/SC daemon

- [Ubuntu ticket #1971984](https://bugs.launchpad.net/ubuntu/+source/pcsc-lite/+bug/1971984)

You man have an issue with the PC/SC Smart Card Daemon not starting automatically.

Example of Error message:

```shell
WARNING: PC/SC not available. Smart card (CCID) protocols will not function.  
ERROR: Unable to list devices for connection
```

**Fix:**

1. Enable the pcscd.socket manually:

```shell
sudo systemctl enable pcscd.socket
```

2. Reboot