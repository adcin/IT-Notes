# Pi-hole

- [Pi-hole.net](https://pi-hole.net)
- [Pi-hole.net - Docs - Basic install](https://docs.pi-hole.net/main/basic-install/)
- [Pi-hole.net - Docs - The  pihole  command](https://docs.pi-hole.net/core/pihole-command)

# Installation

- [Pi-hole.net - Docs - Basic install](https://docs.pi-hole.net/main/basic-install/)

My Proxmox LXC container stats for Ubuntu 22.04.4 LTS:
- 1 CPU core
- 512 MB RAM
- 5GB drive - The running system needs only ~2.5 GB. But the rest of the space is needed when updating the Pi-hole gravity db.

Download and start install script:
```
wget -O $HOME/Downloads/basic-install.sh https://install.pi-hole.net && \
sudo bash $HOME/Downloads/basic-install.sh
```

Web Interface (`http://.../admin`) ... e.g.: http://192.168.1.42/admin

# Some GUI menu reminders

###### Restore config from backup:
- Settings -> Teleporter -> Restore

# Useful terminal commands
- [Pi-hole.net - Docs - The  pihole  command](https://docs.pi-hole.net/core/pihole-command)

Default file locations: 
- `/usr/local/bin/pihole` - main command script
- `/var/log/pihole/pihole.log` - Logs
- `/opt/pihole/` - scripts executed by the pihole command

##### Show status and stats:
```
pihole status
```
Open live dashboard (chronometer):
```
pihole -c
```

##### Pi-hole version and update:
Show version info:
```
pihole -v
```
Update Pi-hole:
```
pihole -up
```

##### Set password for web interface:
```
pihole -a -p supersecurepassword123
```

##### Update gravity (block lists):
```
pihole -g
```
_This can take quite a while._

##### Repair/Reconfigure Pi-hole
Keep settings and try to repair the installation:
```
pihole repair
```
Rerun the installation process:
```
pihole reconfigure
```

##### Restart Pi-hole's DNS-service:
```
pihole restartdns
```

##### Create a backup of the config (teleporter):
```
pihole -a -t
```

##### Disable blocking for 5 min:
```
pihole disable 5m
```

