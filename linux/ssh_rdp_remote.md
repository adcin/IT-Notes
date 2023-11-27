# SSH

You might need to execute most commands as root (sudo).

## Installation

Server:  
```shell
sudo apt install openssh-server
```

Client:  
```shell
sudo apt install openssh-client
```

## SSH service

### Service status

```shell
sudo systemctl status sshd
```

```shell
sudo service sshd status
```

### Stop Service

```shell
sudo systemctl stop sshd
```

```shell
sudo service sshd stop
```

### Start Service

```shell
sudo systemctl start sshd
```

```shell
sudo service sshd start
```

### Restart Service

```shell
sudo systemctl reload-or-restart sshd
```

```shell
sudo service sshd reload
```

```shell
sudo service sshd --full-restart
```

### Enabling/disabling SSH

If a service is enabled, it will start at boot. The system won’t start a disabled service at boot.

```shell
sudo systemctl enable sshd
```

```shell
sudo systemctl disable sshd
```

## SSH process

```shell
ps aux | grep sshd
```

## SSH client usage

Simple connection port 22: 

```shell
ssh <HOST>
```

```shell
ssh <USERNAME>@<HOSTADDRESS>
```
- Example: `ssh root@192.168.0.69`

Connection to different port, using RSA key: 
```shell
ssh <USERNAME>@<HOSTADDRESS> -p <PORT> -i <PRIVAT-KEY-FILE>
```
- Example: `ssh root@192.168.0.69 -p 42654 -i ~/.ssh/private-key.rsa`

### Port forwarding

Port forwarding (SSH tunnel): 
```shell
ssh -L <LOCALPORT>:<HOSTADDRESS>:<HOSTPORT> <USERNAME>@<HOSTADDRESS>
```
- Example - IPv6 different ssh-port and rsa-key: `ssh -6 -L 3389:[2001:6666:5555:4444:3333:2222:1111:aaaa]:3389 marcin@2001:6666:5555:4444:3333:2222:1111:aaaa -p 31337 -i ~/.ssh/megasecret.rsa`  
	- `-6`: force using IPv6
	- `-L 3389:[2001:6666:5555:4444:3333:2222:1111:aaaa]:3389` - Forward local port 3389 to the servers port 3389
	- `marcin@2001:2001:6666:5555:4444:3333:2222:1111:aaaa` - user and IPv6 address of host server
	- `-p 31337` - ssh port of host server
	- `-i ~/.ssh/megasecret.rsa`: Location of rsa key file
	- On mac you might have to use `\[` and `\]` instead of `[` and `]`: `ssh -6 -L 3389:\[2001:6666:5555:4444:3333:2222:1111:aaaa\]:3389 marcin@2001:6666:5555:4444:3333:2222:1111:aaaa -p 31337 -i ~/.ssh/megasecret.rsa`

Executing a non interactive command on remote system:

```shell
ssh user@remote_address ls -la ~/
```

Execute an interactive command on remote system:

```shell
ssh -t user@remote_address "sudo apt update && sudo apt upgrade"
```

| Option | Description                                                                                                                                                                                                                                                 |
|:------:| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|   -t   | Force pseudo-terminal allocation. This can be used to execute arbitrary screen-based programs on a remote machine, which can be very useful, e.g. when implementing menu services.  Multiple -t options force tty allocation, even if ssh has no local tty. | 

## SSH key authentication

### Create authentication key pair:

`ssh-keygen` generates, manages and converts authentication keys for ssh. 

#### RSA

Default rsa 3072 bit key pair: 

```shell
ssh-keygen
```

Two files have been created: 
- _id_rsa_ - the private key is your SECRET client keyfile
- _id_rsa.pub_ - the keyfile with the public server key

Increase security with key length of 4.096 bit (4 x 1024):

```shell
ssh-keygen -b 4096 -C <Comment> -f <CUSTOM_FILENAME>
```

|   Option    | Description                         |
|:-----------:| ----------------------------------- |
| -b \<bits\> | Number of bits in the key to create | 
| -C \<Text\> | Comment                             |
|     -f      | Filename of the key file            |

#### ecdsa

- [Wikipedia - Elliptic Curve Digital Signature Algorithm](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)

Generate ecdsa key pair:

```shell
ssh-keygen -t ecdsa -a 40 -b 521
```

| Option | Description                                                                                                                      |
|:------:| -------------------------------------------------------------------------------------------------------------------------------- |
|   -t   | Specifies the type of key to create                                                                                              |
|   -a   | Number of KDF rounds used.  Higher numbers = slower passphrase verification = increased resistance to brute-force (default = 16) |
|   -b   | Specifies ecdsa key length of 256, 384 or 521 bits                                                                               | 

#### Ed25519

Ed25519 is the most recommended public-key algorithm available today.  It is quite new, so check if it is supported.

Generate ed25519 key pair:
```shell
ssh-keygen -a 40 -t ed25519 -f ~/.ssh/id_ed25519 -C "user@example.com_on_device"
```

|   Option    | Description                                                                                                       |
|:-----------:| ----------------------------------------------------------------------------------------------------------------- |
|     -a      | Number of KDF rounds used.  Higher numbers = slower passphrase verification = increased resistance to brute-force (default = 16) |
|     -t      | Specifies the type of key to create                                                                               |
| -C \<Text\> | Comment                                                                                                           |
|     -f      | Filename of the key file                                                                                          | 

### Generate public key out of existing private key

OpenSSH format:

```shell
ssh-keygen -y -f privatekey.pem > key.pub
```

RFC4716 format:

```shell
ssh-keygen -e -f .ssh/id_rsa.pub > .ssh/id_rsa_rfc.pub
```

Source key can be either private or public.

| Option | Description                                                                                                                                     |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| -y     | Read a private OpenSSH format file and print an OpenSSH public key to stdout.                                                                   |
| -f     | Specifies the filename of the private key file.                                                                                                 |
| -e     | Read a private or public OpenSSH key file and print to stdout a public key in one of the formats specified by the -m option (default: RFC4716). | 

### Change passphrase of an existing private key

```shell
ssh-keygen -p -f ~/.ssh/my_private_superkey
```

### Copy public key to the server

The public ssh user key shall be inserted in the users home directory ~/.ssh/authorized_keys file.  Each line in the file contains one key.

#### Method 1 - `ssh-copy-id`
Works only when `ssh-copy-id` is installed on the server.   
```shell
ssh-copy-id -i <PUBLIC_KEYFILE> <USERNAME>@<HOST>
```
Example: `ssh-copy-id -i ~/.ssh/id_rsa.pub user@192.168.0.69`  

#### Method 2 - copy using ssh

```shell
cat <PUBLIC_KEYFILE> | ssh <USERNAME>@<HOST> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

Example: `cat ~/.ssh/id_rsa.pub | ssh username@remote_host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"`  

#### Method 3 - using scp

```shell
scp <PUBLIC_KEYFILE> <USERNAME>@<HOST>:~/.ssh/authorized_keys
```

Example: `scp $HOME/.ssh/id_rsa.pub user@192.168.0.69:~/.ssh/authorized_keys`

#### Method 4 - manually on the server

Show the key in the terminal and copy the public_key_string to clipboard:  
```shell
cat ~/.ssh/id_rsa.pub
```

Create ~/.ssh directory:
```shell
mkdir -p ~/.ssh
```

Insert key into ~/.ssh/authorized_keys:  
```shell
echo public_key_string >> ~/.ssh/authorized_keys
```
	The public_key_string is the output from cat ~/.ssh/id_rsa.pub.

### Test the connection

```shell
ssh <USERNAME>@<HOSTADDRESS> -i <PRIVAT-KEY-FILE>
```
- Example: `ssh root@192.168.0.69 -i ~/.ssh/private-key.rsa`

## SSH server configuration/hardening

### Adjust the access permissions to the authentication keys

```shell
chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys

# Alternative:

chmod -R u+rwX,go-rwx $HOME/.ssh
```

### Set up configuration

1. Check sshd configuration:

```shell
sudo nano /etc/ssh/sshd_config
```

Search for: `Include /etc/ssh/sshd_config.d/*.conf`

2. Create new configuration file in ".d" folder:
```shell
sudo nano /etc/ssh/sshd_config.d/mysshconfig.conf
```

| Entry                       | Example      | Description                                                                                                       | 
|:--------------------------- |:------------ |:----------------------------------------------------------------------------------------------------------------- |
| `Port <NUMBER>`             | `Port 12345` | change standard ssh port                                                                                          |
| `PermitRootLogin no`        |              | no ssh login as root                                                                                              |
| `PasswordAuthentication no` |              | force to use RSA key authentication (you might need to test the key authentication first!)                        |
| `PermitEmptyPasswords no`   |              | empty passwords not allowed                                                                                       |
| `StrictModes yes`           |              | Check file modes and ownership of the user's files and home directory before accepting login to enhance security. |

All setting in one command:  
```shell
sudo cat << EOF | sudo tee /etc/ssh/sshd_config.d/mysshconfig.conf
PermitRootLogin no
PasswordAuthentication no
PermitEmptyPasswords no
StrictModes yes
EOF
```

3. Restart the service

```shell
sudo systemctl reload sshd
```

## Tweaking ~/.ssh/config 

In the `~/.ssh/config` you can store ssh information and settings for your servers.

User's configuration file: `~/.ssh/config`
System-wide configuration file: `/etc/ssh/ssh_config`

- [openbsd.org - OpenSSH client config manual](https://man.openbsd.org/ssh_config)
- [ssh.com - OpenSSH client config manual](https://www.ssh.com/academy/ssh/config)
- [howtogeek.com - How to Manage an SSH Config File in Windows and Linux](https://www.howtogeek.com/devops/how-to-manage-an-ssh-config-file-in-windows-linux/)

Create ~/.ssh/config file:  
```shell
mkdir -p -m 700 ~/.ssh && touch ~/.ssh/config && chmod 600 ~/.ssh/config
```  

ssh_config structure:  

```shell
Host Hostname1
    ssh_option value
    ssh_option value

HostHostname2
    ssh_option value

Host *
    ssh_option value
```  

The \* pattern sets the option to all other hosts, except a host has the same option already defined.

Example ~/.ssh/config file: 
```shell
Host example
    Hostname example.com
    User xmpl
    IdentityFile ~/.ssh/example.key
    IdentitiesOnly yes

Host craftmine
    Hostname 192.168.0.66

Host proxy
    Hostname 192.168.0.69

Host vpn
    Hostname 111.222.111.222
    User fancyname
    Port 6969

Host 192.168.0.*
    User root

Host * !192.168.0.*
    IdentityFile ~/.ssh/mystandard.key
    IdentitiesOnly yes

Host *
    ServerAliveInterval 120
    ServerAliveCountMax 10
```  

Connecting to server with example above:  

```shell
ssh craftmine
```  

</br>

## ssh agent

Ssh-agent is a program to hold private keys used for public key authentication.

- [man ssh-add](https://man7.org/linux/man-pages/man1/ssh-add.1.html)

Check if agent is running and activate:  
```shell
eval "$(ssh-agent -s)"
```

Add key identifier to the agent:  
```shell
ssh-add ~/.ssh/nameofkey
```

List fingerprints of all identities currently represented by the agent:  
```shell
ssh-add -l
```

Remove all keys from the agent:  
```shell
ssh-agent -D
```

</br>

## ProxyJump (Bastion Host)

- [ProxyJump on Red Hat - Enable Sysadmin](https://www.redhat.com/sysadmin/ssh-proxy-bastion-proxyjump)

-J option (ProxyJump) is used to connect to your target through an ssh connection to one or more jump-hosts (also called bastion hosts). Very useful if you can't connect directly to your target and more convenient then opening the ssh-sessions one by one.

Single Jump Host:

```shell
ssh -J user@<jump-host:port> <user@target-host:port>
```

Multiple Jump Hosts:

```shell
ssh -J <jump-host1>,<jump-host2> <target-host>
```

I recommend using ~/.ssh/config especially for key authentication. You also can define the jump host in the ssh config:

```shell
Host jump-host-nickname
  HostName jump-hostname

Host target-host-nickname
  HostName target-hostname
  ProxyJump jump-host-nickname
```

ProxyJump is available since ssh version 7.3. For earlier versions you can use `ProxyCommand` instead.

## Useful ssh commands

### Execute program/script on a remote desktop session

1. Establish ssh connection to remote desktop
2. Identify the display ID  
   
```shell
ps aux | grep Xorg
```

Output example:

marcin       987     985 30 18:54 ?        00:16:00 /usr/lib/xorg/Xorg :***10*** -auth .Xauthority -config xrdp/xorg.conf -noreset -nolisten tcp -logfile .xorgxrdp.%s.log  
marcin      2242    2207  0 19:47 pts/0    00:00:00 grep --color=auto Xorg

> First Line: "Xorg:***10***" → The display ID is: ***10***  
> The second line is the `ps` request itself - it can be ignored.  

3. Define the display for the shell session

```shell
export DISPLAY=:10
```

4. Execute program/script on the remote display

If a command like `filezilla` will now be executed, the GUI will start on the remote display. But the shell output will still be on the local shell. It might be blocked and when closed, the executed process (filezilla) will also be closed.  
That's why the shell output (stdout) needs to be redirected and the process put to the background.

Example:  
```shell
filezilla >/dev/null 2>&1 &
```

Command explained:  

| **Command**       | **Description**                                                                                                                  |   
| ------------- | ---------------------------------------------------------------------------------------------------------------------------- |  
| filezilla     | Start FileZilla FTP client                                                                                                   |  
| \>\/dev\/null | No standard shell output (redirect stdout to the null device                                                                 |  
| 2\>\&1        | Redirect the error shell output to stdout, which then discards it as well since standard output has already been redirected. |  
| \&            | The job is put to the background.                                                                                            |  

5. Alternative everything in one command

Already established ssh connection:  
```shell
DISPLAY=:10 filezilla >/dev/null 2>&1 &
```

Create ssh connection and run command:
```shell
ssh <REMOTE_SERVER> "DISPLAY=:10 filezilla >/dev/null 2>&1 &"
```

### known_hosts

Host address or IP for copy\&paste:
```shell
HOST_ADDR=
```

Add server to known_hosts:

```shell
ssh-keyscan -H $HOST_ADDR >> ~/.ssh/known_hosts
```

Remove server from known_hosts method 1:

```shell
ssh-keygen -f ~/.ssh/known_hosts -R $HOST_ADDR
```

Remove server from known_hosts method 2 (deleting the line - line 6 in this example):

```shell
sed -i '6d' ~/.ssh/known_hosts
```

-------------------------------- 

# RDP Remote Desktop `xrdp`

## Install:  
```shell
sudo apt install xrdp
```

## Optional - check service status after install:  

```shell
sudo systemctl status xrdp
```

## Optimise performance

### xorg.conf - disable compositing

Backup original: 
```shell
sudo cp /etc/X11/xrdp/xorg.conf /etc/X11/xrdp/xorg.conf.orin
```

Edit xorg.conf: 

```shell
sudo nano /etc/X11/xrdp/xorg.conf
```

```shell
Section "Extensions"
    Option "Composite" "Disable"
EndSection
```

### xrdp.ini

Backup original: 
```shell
sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.orig
```

Edit xrdp.ini:  

```shell
sudo nano /etc/xrdp/xrdp.ini
```

```shell
crypt_level=none
max_bpp=16
```
