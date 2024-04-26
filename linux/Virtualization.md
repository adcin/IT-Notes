# Determine the virtualization platform

Sometimes it is necessary to know the virtualization platform of a vServer.

## systemd-detect-virt

Excerpt from man description:
> systemd-detect-virt detects execution in a virtualized environment. It identifies the virtualization technology and can distinguish full machine virtualization from container virtualization.

Print an identifier for the detected virtualization:  
```shell
systemd-detect-virt
```

Output all currently known and detectable container and VM environments:  
```shell
systemd-detect-virt --list
```

## hostnamectl

Excerpt from man description:
> This tool distinguishes three different hostnames: the high-level "pretty" hostname which might include all kinds of special characters (e.g. "Lennart's Laptop"), the static hostname which is used to initialize the kernel hostname at boot (e.g. "lennarts-laptop"), and the transient hostname which is a fallback value received from network configuration.

```shell
hostnamectl | grep -i virtualization
```

## virt-what

Excerpt from man description:
> "virt-what" is a shell script which can be used to detect if the program is running in a virtual machine.

```shell
sudo virt-what
```

# Virtual I/O Device (VIRTIO)

- [kernel.org - Virtio on Linux](https://docs.kernel.org/driver-api/virtio/virtio.html)
- [oasis-open.org - Virtual I/O Device (VIRTIO) Version 1.2](https://docs.oasis-open.org/virtio/virtio/v1.2/virtio-v1.2.html)
- [linux-kvm.org](https://www.linux-kvm.org/page/Virtio)
- [fedorapeople.org - virtio-win repo](https://fedorapeople.org/groups/virt/virtio-win/)
- [proxmox.com - wiki - Windows VirtIO Drivers](https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers)

> [!quote] Quote from [kernel.org](https://docs.kernel.org/driver-api/virtio/virtio.html#virtio-on-linux)
> Virtio is an open standard that defines a protocol for communication between drivers and devices of different types, ... . Originally developed as a standard for paravirtualized devices implemented by a hypervisor, it can be used to interface any compliant device (real or emulated) with a driver.

VirtIO are drivers for virtual systems. On modern Linux systems they are usually already implemented into the kernel. On Windows VMs the drivers need to be installed on the guest system. Check out the [Proxmox wiki](https://pve.proxmox.com/wiki/Windows_VirtIO_Drivers) for a tutorial.