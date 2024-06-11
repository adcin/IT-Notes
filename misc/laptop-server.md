# Old laptop as a server

I have an old bulky Laptop, which is perfect for testing stuff. Recently I installed Proxmox on this laptop. Everything was fine, but there was one issue - the monitor did not turn off. Lets change this.

Source: [https://davenewman.tech/blog/install-proxmox-on-a-laptop](https://davenewman.tech/blog/install-proxmox-on-a-laptop/)

Use the root shell of the main node for the following commands.

## 1. Closing the lid should not effect the server

Edit `/etc/systemd/logind.conf`. Change/add the following settings:
```
HandleLidSwitch=ignore
HandleLidSwitchDocked=ignore
```

Restart the service:
```
systemctl restart systemd-logind.service
```

## 2. Switch off the screen after 5 min

Edit `/etc/default/grub`. Change/add the following setting:
```
GRUB_CMDLINE_LINUX="consoleblank=300"
```
_The monitor will turn off after 300 seconds._

Generate the grub2 config file:
```
update-grub
```

## 3. Reboot

```
reboot
# or
shutdown -r now
```

Now the display should turn off after 5 min.