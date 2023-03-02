# Introduction

Tailscale is an easy alternative to VPN.

## Links

- [Tailscale Wesite](https://tailscale.com/)
- [Download Site](https://tailscale.com/download/)
- [Documentation](https://tailscale.com/kb/)

# Installation

## 1. Sign up to Tailscale

[Choose identity provider](https://login.tailscale.com/start)

## 2. Installation

### Desktop

Executed on:  

| distro | Kubuntu 22.04.2 LTS x86_64 |
| ------ | -------------------------- |
| kernel | 5.15.0-60-generic          |
| shell  | bash 5.1.16                |
| de     | Plasma 5.24.7              |
| wm     | KWin                       | 

Execute installation script based on [official Guide](https://tailscale.com/download/linux):

```shell
curl -fsSL https://tailscale.com/install.sh | sh
```

Follow the instructions and use the link from the shell to authenticate via browser. I believe you can use any browser, even your smartphone.

### Headless

It's basically the same as the desktop installation.

Executed on:     
  
| distro | Debian GNU/Linux 11 (bullseye) aarch64 |
| ------ | -------------------------------------- |
| kernel | 5.15.84-v8+                            | 
| shell  | bash 5.1.4                             |

1. Connect to the server via ssh: [My ssh notes](ssh_rdp_remote.md)

2. Execute installation script based on [official Guide](https://tailscale.com/download/linux):

```shell
curl -fsSL https://tailscale.com/install.sh | sh
```

Follow the instructions and use the link from the shell to authenticate via browser of your host.

# Commands

Show state of tailscaled and its connections:
```shell
tailscale status
```

Show help:
```shell
tailscale --help
```

| Subcommand | Function                                               |
| ---------- | ------------------------------------------------------ |
| up         | Connect to Tailscale, logging in if needed             |
| down       | Disconnect from Tailscale                              |
| set        | Change specified preferences                           |
| login      | Log in to a Tailscale account                          |
| logout     | Disconnect from Tailscale and expire current node key  |
| switch     | Switches to a different Tailscale account              |
| netcheck   | Print an analysis of local network conditions          |
| ip         | Show Tailscale IP addresses                            |
| status     | Show state of tailscaled and its connections           |
| ping       | Ping a host at the Tailscale layer, see how it routed  |
| nc         | Connect to a port on a host, connected to stdin/stdout |
| ssh        | SSH to a Tailscale machine                             |
| version    | Print Tailscale version                                |
| web        | Run a web server for controlling Tailscale             |
| file       | Send or receive files                                  |
| bugreport  | Print a shareable identifier to help diagnose issues   |
| cert       | Get TLS certs                                          |
| lock       | Manage tailnet lock                                    |
| licenses   | Get open source license information                    | 

