# Yubikey - Security Key

YubiKey is a hardware authentication device manufactured by [yubiko](https://www.yubico.com).  I'm using the [YubiKey 5c NFC](https://www.yubico.com/product/yubikey-5c-nfc) for these notes, but other Security Keys should work similar.

**Attention!** You always should have a backup key, in case you loose or destroy the main one. Setup the backup key the same way you setup the main key and store it in a safe place!

# Setup local authentication

## Associating the YubiKey with you System

Install required package:

```bash
sudo apt install libpam-u2f
```

Config directory:  

```bash
mkdir -m 700 ~/.config/Yubico
```

Register the YubiKey in a file and harden the permissions:

```bash
pamu2fcfg > ~/.config/Yubico/u2f_keys && chmod 600 ~/.config/Yubico/*
```

Now the YubiKey is registered in the system, but other than that no functions are enabled.

Register a 2nd or a backup key for same User:

```bash
pamu2fcfg -n >> ~/.config/Yubico/u2f_keys
```  

------

## sudo with YubiKey + first test ##

**Attention!** Do not exit the main terminal session until your tests are successful. Otherwise you might to have trouble undoing the changes.

```bash
sudo nano /etc/pam.d/sudo
```  

Search: `@include common-auth`  

Insert underneath:
```bash
auth    required    pam_u2f.so    cue
```  



**Testing**

1. Open new terminal window!
2. Remove the YubiKey.
3. Execute a sudo command in the new terminal. It should not work even with the correct password:  
```bash
sudo echo test
```  
 4. Insert the YubiKey
5. Execute the command again, but now activate the YubiKey after entering the password.
6. Everything ok? Congrats ;) ... If not, you can undo the changes and start troubleshooting.

## Secure other critical commands

According to the [Yubico FAQ](https://support.yubico.com) you might want to add the YubiKey authentication to the following commands as well (in Ubuntu 22.04):

| Command                                                                         | File Location        |
| ------------------------------------------------------------------------------- | -------------------- |
| [runuser](https://manpages.ubuntu.com/manpages/jammy/en/man1/runuser.1.html)    | /etc/pam.d/runuser   |
| [runuser -l](https://manpages.ubuntu.com/manpages/jammy/en/man1/runuser.1.html) | /etc/pam.d/runuser-l |
| [su](https://manpages.ubuntu.com/manpages/jammy/en/man1/su.1.html)              | /etc/pam.d/su        |
| [sudo -i](https://manpages.ubuntu.com/manpages/jammy/en/man8/sudo.8.html)       | /etc/pam.d/sudo-i    |
| [su -l](https://manpages.ubuntu.com/manpages/jammy/en/man1/su.1.html)           | /etc/pam.d/su-l      |

Same procedure as with [sudo](#sudo%20with%20YubiKey%20+%20first%20test).

------

## Login with Yubikey

What's my default display manager:

```bash
cat /etc/X11/default-display-manager
```

Output on Kubuntu 22.04.1:
```bash
/usr/bin/sddm
```  

**sddm** = Simple Desktop Display Manager ([sddm on github](https://github.com/sddm/sddm))

### Activate YubiKey for the login in sddm

```bash
sudo nano /etc/pam.d/sddm
```  

Search: `@include common-auth`  

Insert underneath:
```text
auth    required    pam_u2f.so
```  

### Activate YubiKey for TTY login

```bash
sudo nano /etc/pam.d/login
```  

Search: `@include common-auth`  

Insert underneath:
```text
auth    required    pam_u2f.so
```  
