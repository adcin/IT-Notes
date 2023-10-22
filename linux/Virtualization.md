# Determine the virtualization platform

</br>

Sometimes it is necessary to know the virtualization platform of a vServer.

</br>

## systemd-detect-virt

Excerpt from man description:
> systemd-detect-virt detects execution in a virtualized environment. It identifies the virtualization technology and can distinguish full machine virtualization from container virtualization.

</br>

Print an identifier for the detected virtualization:  
```shell
systemd-detect-virt
```

</br>

Output all currently known and detectable container and VM environments:  
```shell
systemd-detect-virt --list
```

</br>

## hostnamectl

Excerpt from man description:
> This tool distinguishes three different hostnames: the high-level "pretty" hostname which might include all kinds of special characters (e.g. "Lennart's Laptop"), the static hostname which is used to initialize the kernel hostname at boot (e.g. "lennarts-laptop"), and the transient hostname which is a fallback value received from network configuration.

</br>

```shell
hostnamectl | grep -i virtualization
```


</br>

## virt-what

Excerpt from man description:
> "virt-what" is a shell script which can be used to detect if the program is running in a virtual machine.

</br>

```shell
sudo virt-what
```