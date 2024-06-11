# Proxmox

- [Proxmox.com](https://www.proxmox.com)
- [Proxmox.com - Proxmox VE Documentation](https://pve.proxmox.com/pve-docs)
- [Proxmox.com - wiki](https://pve.proxmox.com/wiki/Package_Repositories)
- [Proxmox.com - wiki Linux Container (LXC)](https://pve.proxmox.com/wiki/Linux_Container)
- [Proxmox.com - wiki - Resize disks](https://pve.proxmox.com/wiki/Resize_disks)
- [Techlabs.blog - Expand Linux Virtual Machine Disk Partition using GParted](https://techlabs.blog/categories/debian-linux/expand-linux-virtual-machine-disk-partition-using-gparted)
- [YouTube.com - Sempervideo "Proxmox" (ger)](https://youtube.com/playlist?list=PL3-bM7Aq1pUpSTIQiffrCiR29Z-SCS9wd)
- [YouTube.com - Sempervideo "Die 1,- EUR Serie" (ger)](https://youtube.com/playlist?list=PL3-bM7Aq1pUpOC1_5aSMuT1CDfavzbqUX)

# Proxmox Virtual Environment (Proxmox VE; PVE) installation

> [!info]- My System
> 
> ![HP t630|150](../zz_rss/Pasted%20image%2020240210083631.png)
> [hp.com - HP t630 Thin Client Specifications](https://support.hp.com/us-en/document/c05240287)
> [hp.com - HP t630 Thin Client Support](https://support.hp.com/us-en/product/details/hp-t630-thin-client/10522151)

Current Proxmox VE version: 8.1.3

The installation of the hypervisor is very straight forward - very similar to the installation of a Linux distro. Follow the instructions from the [guide on the official website](https://proxmox.com/en/proxmox-virtual-environment/get-started).

After the first system reboot you should not need the monitor, keyboard or mouse any more. The next steps are executed via the browser.

## PVE config for non subscription use

> [!note] Login via browser (port 8006)
> **PVE UI**: http://???.???.???.???:8006
> **Username**: root
> **Password**: YourInstallationPassword

##### 1. Choose the node
![300](../zz_rss/Pasted%20image%2020240210102410.png)

##### 2. Open the shell

##### 3. Edit /etc/apt/sources.list ([pve-wiki-page](https://pve.proxmox.com/wiki/Package_Repositories#sysadmin_no_subscription_repo)): 
```shell
vi /etc/apt/sources.list
```
> [!quote] /etc/apt/sources.list
> 
> Add: deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
> 
> ```
> deb http://ftp.debian.org/debian bookworm main contrib
> deb http://ftp.debian.org/debian bookworm-updates main contrib
> 
> # Proxmox VE pve-no-subscription repository provided by proxmox.com,
> # NOT recommended for production use
> deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
> 
> # security updates
> deb http://security.debian.org/debian-security bookworm-security main contrib
> ```

##### 4. Edit /etc/apt/sources.list.d/pve-enterprise.list ([PVE wiki page](https://pve.proxmox.com/wiki/Package_Repositories#sysadmin_enterprise_repo))
```shell
vi /etc/apt/sources.list.d/pve-enterprise.list
```

> [!quote] /etc/apt/sources.list.d/pve-enterprise.list
> 
> Comment out the pve-enterprise repo:
> 
> ```
> # deb https://enterprise.proxmox.com/debian/pve bookworm pve-enterprise
> ```

_Alternatively, you could simply delete the file `/etc/apt/sources.list.d/pve-enterprise.list`._

##### 5. Edit /etc/apt/sources.list.d/ceph.list ([PVE wiki page](https://pve.proxmox.com/wiki/Package_Repositories#_ceph_quincy_no_subscription_repository))
```shell
vi /etc/apt/sources.list.d/ceph.list
```

> [!quote] /etc/apt/sources.list.d/pve-enterprise.list
> 
> Comment out the enterprise and add the no-subscription repository: 
> ```
> # deb https://enterprise.proxmox.com/debian/ceph-quincy bookworm enterprise
> deb http://download.proxmox.com/debian/ceph-quincy bookworm no-subscription
> ```

##### 6. Now should be able to update and upgrade the system

```shell
apt update
apt -y upgrade
apt -y autoremove
```

> [!attention]
> If you still get an error message after update, check the folder `/etc/apt/sources.list.d` for other active enterprise repos and [rtfm](https://pve.proxmox.com/wiki/Package_Repositories)

# virtual local area network (vLAN)

### Create vLAN bridge

> [!note] Name of my node: hp-prox

The purpose of this vLAN is to establish internal communication between the VMs and also from the VMs to the internet/LAN.

> [!example] UI Window: Main
> 
> | Position   | Menu                   |
> | ---------- | ---------------------- |
> | Left       | hp-prox                |
> | Center-Left | System -> Network      |
> | Center-Top  | Create -> Linux Bridge | 

> [!important]
> 
> The network must be different to your local network(s). So if you have a 192.168.0.1/24 or 192.168.1.1/24 network, then you can't use these networks for the vLAN. I will use 192.168.100.1/24.

> [!example] UI Window: Create: Linux Bridge
> 
> | Setting    | Value            |
> | ---------- | ---------------- |
> | Name:      | vmbr1            |
> | IPv4/CIDR: | 192.168.100.1/24 |
> 
> - **Create**

> [!example] UI Window: Main
> 
> | Position   | Menu                       |
> | ---------- | -------------------------- |
> | Center-Top | Apply Configuration -> Yes | 

### Firewall/iptables configuration

Iptables needs to be setup, so the VMs can connect to the internet.

> [!example] UI Window: Main
> 
> | Position    | Menu      |
> | ----------- | --------- |
> | Left        | hp-prox   |
> | Center-Left | >\_ Shell | 

Install iptables-persistent:  
```shell
apt install iptables-persistent -y
```

Set iptables rules:  
```shell
iptables -A POSTROUTING -t nat -s 192.168.100.0/24 -j MASQUERADE && \
iptables -A POSTROUTING -t nat -s 192.168.100.0/24 -o vmbr0 -j MASQUERADE && \
iptables -D POSTROUTING -t nat -s 192.168.100.0/24 -o vmbr0 -j MASQUERADE
```

> [!important]
> 
> Set the correct IP-Address.

Save the rules to make them persistent:  
```shell
iptables-save > /etc/iptables/rules.v4
```

# Create container from template

- [YouTube.com - Sempervideo "Die 1,- EUR Serie: Virtuelles Netzwerk" (ger)](https://www.youtube.com/watch?v=VCAkkSNG4mM)

# Wireguard Container

- [YouTube.com - Sempervideo "Die 1,- EUR Serie: Wireguard Zugang" (ger)](https://www.youtube.com/watch?v=ytrzBcVAwT8)

##### 1. Create LTX Container. My container has the ID 111 and ubuntu 22.04.

- Do not start the container yet.
- Change the options to "Start at boot: yes"

##### 2. Edit the config of container `111` on the node.
```shell
nano /etc/pve/lxc/111.conf
```
> [!quote] Add et the end of file:
> ```
> lxc.cgroup.devices.allow: c 10:200 rwm
> lxc.mount.entry: /dev/net dev/net none bind,create=dir
> ```

##### 3. Start the container
Update, upgrade + install curl:  
```bash
apt update && apt upgrade -y && apt autoremove -y && apt install curl -y
```

Even tough the hosts time is correct, a new container may have a wrong time.

Configure timezone (Container Terminal):  
```bash
dpkg-reconfigure tzdata
```

##### 4. Install wireguard

```shell
wget https://git.io/wireguard -O wireguard-install.sh && bash wireguard-install.sh
```

# Good To Know

## Timezone issue with new containers

Even tough the hosts time is correct, a new container may have a wrong time.

Configure timezone (Container Terminal):  
```shell
dpkg-reconfigure tzdata
```

## LXC config files

Location of the Linux Container's (LXCs) config files: `/etc/pve/lxc` (_Softlink to `/etc/pve/nodes/<NODE_NAME>/lxc`_)

## Download iso for vm-creation directly on the pve host

Typical directory for the ISOs: `/var/lib/vz/template/iso`

##### 1. Login as root to host shell
##### 2. Change Directory to `/var/lib/vz/template/iso`
```
cd /var/lib/vz/template/iso
```
##### 3. Download the distro image (e.g. debian-12.5.0-amd64-netinst.iso)
```
wget https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-12.5.0-amd64-netinst.iso
```
##### 4. finished
When the download is finished, you can use the iso to create VMs.
Optionally check the checksum:
```
root@prox:/var/lib/vz/template/iso# cksum -a sha256 -c <<< "013f5b44670d81280b5b1bc02455842b250df2f0c6763398feb69af1a805a14f  debian-12.5.0-amd64-netinst.iso"
```
Output:
```
debian-12.5.0-amd64-netinst.iso: OK
```

## Expanding Linux VM disk partition (offline method with GParted)
This guide will describe the overall process and not each and every click one by one. I also recommend to check out at least the "Resize disk" section of the Proxmox Wiki, before starting. And you should:

> [!warning] BACKUP
> Don't forget to back up everything important before resizing the disk. One time it failed for me and the partition table was destroyed. Luckily I could still retrieve the data, but I ended up with reinstalling the VM after several tries to repair the broken partition. It's been a PITA!

Inspired by:
- [Proxmox.com - wiki - Resize disks](https://pve.proxmox.com/wiki/Resize_disks)
- [Techlabs.blog - Expand Linux Virtual Machine Disk Partition using GParted](https://techlabs.blog/categories/debian-linux/expand-linux-virtual-machine-disk-partition-using-gparted)

Current date: 2023-04-16
PVE Version: 8.1.10

##### 1. Download the GParted Live image from: [gparted.org](https://gparted.org)
Download the ISO directly to the host as described above: [Download iso for vm-creation directly on the pve host](Proxmox.md#Download%20iso%20for%20vm-creation%20directly%20on%20the%20pve%20host)

##### 2. Prepare the VM

1. I recommend opening the Concole in a separate Window:
    1. Select the VM
    2. Click `>_ Console` (top right corner) → noVNC
    3. Now a new Window with the Console of the VM should have opened. I recommend not to close this window, until the resizing is finished and the VM is up and running.
2. Shutdown the VM

##### 3. Expand the Hard Disk by using the Proxmox VE GUI

1. Select the VM → Hardware → Hard Disk
2. Disk Action (at the top) → Resize
3. Type in the amount of GiB you want to increase the size → Resize disk

Now the size of the virtual hard disk has increased, but the size of the partitions is still the same. So the extra space is not yet available to the OS.

##### 4. Boot the VM from the GParted ISO

To boot from the GParted ISO we need to open the boot-menu, before the VM boots the OS. It's done by pressing "`ESC`" after powering up the VM.

1. Attach the GParted ISO to the VM
    1. Select the VM → Hardware → CD/DVD Drive
    2. Click Edit (menu bar above): 
        - Storage: local
        - ISO Image: pulldown menu → Select the GParted ISO
        - OK
2. Focus on the noVNC Console window you opened before.
3. Click "Start Now" to power up the VM.
4. Click "`ESC`" as soon as the "Press ESC for Boot Menu" message appears at the bottom.
5. Select the CD/DVD Iso as boot-device (No. 2)
6. Follow the instruction on screen to boot GParted.

##### 5. Edit the Partitions

0. I recommend taking a screenshot of the Swap-Partition's "Information" window. I assume its size is 1GB.
1. Free space: Delete the "linux-swap" partition → Delete the empty "extended" partition → Apply all operations
    - Now the space behind the primary partition should be unallocated (free), so we can expand it.
2. Expanding: Open the "Resize/Move" window → expand the size, so at the end you have:
    - Free space preceding (MiB): 0
    - Free space following (MiB): 1024 (size of the deleted linux-swap partition)
    Click the "Resize/Move" button to confirm → Apply all operations
3. Create Swap: Select the 1024 free space
    1. Create a new extended partition → Apply all changes
    2. Create a new Swap partition:
        - New size (MiB): 1023
        - File system: linux-swap
4. Apply all operations.

You are done with GParted.

5. Close the GParted application → doubleclick the Exit button → and "shutdown"
6. Detach the GParted ISO from the VM (select "Do not use any media" in the CD/DVD window)

##### 6. Update the Swap partition's UUID

Booting the VM will probably take much longer now and you will get "Timed out" error messages. That's because the system doesn't know the new UUID of the Swap partitions and fails to mount it.

Check if swap is attached:
```
:~$ sudo swapon --show
:~$
```
_No output means no swap._

Get the UUID of the swap partition:
```
:~$ sudo blkid | grep swap
```

Edit `/etc/fstab` and replace the old swap UUID with the new one:
```
sudo vim /etc/fstab
```

Reboot and run `swapon` again:
```
:~$ sudo swapon --show
NAME      TYPE       SIZE USED PRIO
/dev/sda5 partition 1023M   0B   -2
```

##### 7. Check and fix the file system

You may also have other annoying boot-messages like "/dev/sda1: recovering journal ...". Running a file system check with `fsck` may solve these issues.

1. Boot the VM from the GParted image. But at the question "Which mode do you prefer?" select: 
    - `(2) Enter command line prompt`
    - Now you should be logged in as: `root@debian:/#` 
2. Execute `fsck` on the resized partition:

```
fsck -fy /dev/sda1
```

`fsck` options:
- `-f`: Force checking even if the file system seems clean.
- `-y`: Assume an answer of ‘yes’ to all questions.

3. When finished shutdown the VM like this:

```
poweroff; exit
```
_When you simply exit the terminal, then the GParted Live Desktop would open. Otherwise, when you try to shutdown the VM with `poweroff` or `shutdown -h now` it will wait, until your current bash session process ends._

4. Detach the ISO from the CD/DVD device and restart the actual VM OS.

# Command `pct` - Tool to manage Linux Containers (LXC) on Proxmox VE

##### SYNOPSIS
`pct <COMMAND> [ARGS] [OPTIONS]`

LXC container index (per node):
```
pct list
```

Launch a shell for the specified container:
```
pct enter <vmid>
```

Mount a containers filesystem on the host:
```
pct mount <vmid>
```

Start a container:
```
pct start <vmid>
```

Stop a container:
```
pct stop <vmid>
```