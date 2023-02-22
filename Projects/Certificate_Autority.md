# Introduction

Based on: https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-a-certificate-authority-on-ubuntu-22-04 

The Certificate Authority is a server which manages the certificates of my OpenVPN server. It's on a separate system due to extra security.

System info (`neofetch distro kernel shel`):

| distro | Debian GNU/Linux 11 (bullseye) x86_64 |
| ------ | ------------------------------------- |
| kernel | 5.10.0-21-amd64                       | 
| shell  | bash 5.1.4                            |

## Installation

```shell
sudo apt update
sudo apt install easy-rsa
```

## Certificate Authority  infrastructure

```shell
mkdir ~/easy-rsa
```

```shell
ln -s /usr/share/easy-rsa/* ~/easy-rsa/
```

```shell
chmod 700 ~/easy-rsa
```

```shell
cd ~/easy-rsa
```

```shell
./easyrsa init-pki
```

## Setup the Certificate Authority

```shell
cp ~/easy-rsa/vars.example ~/easy-rsa/vars
```

```shell
nano ~/easy-rsa/vars
```

Insert the following text at the end and modify the values. Don't leave any blank:  

```shell
set_var EASYRSA_REQ_COUNTRY    "US" # modify
set_var EASYRSA_REQ_PROVINCE   "California" # modify
set_var EASYRSA_REQ_CITY       "San Francisco" # modify
set_var EASYRSA_REQ_ORG        "Copyleft Certificate Co" # modify
set_var EASYRSA_REQ_EMAIL      "me@example.net" # modify
set_var EASYRSA_REQ_OU         "My Organizational Unit" # modify
set_var EASYRSA_ALGO           "ec"
set_var EASYRSA_DIGEST         "sha512"
```

Option 1 - Create Keys with Passphrase. You'll need to enter the pass every time you interact with the CA:  

```shell
./easyrsa build-ca
```

Option 2 - Create Keys without Passphrase (less secure):  

```shell
./easyrsa build-ca nopass
```
