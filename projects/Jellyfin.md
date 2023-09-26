
-------------

# ⚠ Work in progress ⚠

-------------


</br>


# Jellyfin

- [Jellyfin.org - Official Website](https://jellyfin.org/)
- [reddit.com - List of Jellyfin plugins](https://www.reddit.com/r/jellyfin/comments/ozncze/list_of_all_known_jellyfin_plugin_repositories/)

</br>

Quote from official website: 
> Jellyfin enables you to collect, manage, and stream your media. Run the Jellyfin server on your system and gain access to the leading free-software entertainment system, bells and whistles included.

</br>

------------

# Installation as a Docker Container

- [Jellyfin.org - Docker Container](https://jellyfin.org/docs/general/installation/container#docker)

Create a system user:  
```shell
sudo useradd --system -s /usr/sbin/nologin jellyfin
```

Add your main user to the jellyfin group: 

```shell
sudo usermod -aG jellyfin marcin
```

Create a directory for the docker-compose.yml file and the container volumes.

```shell
sudo mkdir -m 775 /mnt/jellyfin /mnt/jellyfin/volumes/config /mnt/jellyfin/volumes/cache /mnt/jellyfin/volumes/media
```


Change owner to jellyfin:  
```shell
sudo chown -R jellyfin:jellyfin /mnt/jellyfin
```

docker-compose.yml:  
```shell
version: '3.5'
services:
  jellyfin:
    image: jellyfin/jellyfin
    container_name: jellyfin
    user: 999:996 # jellyfin:jellyfin didn't work but UID:GID was fine 'ID jellyfin'
    network_mode: 'host'
    volumes:
      - ./volumes/config:/config
      - ./volumes/cache:/cache
      - ./volumes/media:/media
    restart: 'unless-stopped'
    # Optional - alternative address used for autodiscovery
    environment:
#      - JELLYFIN_PublishedServerUrl=http://example.com
    extra_hosts:
      - "host.docker.internal:host-gateway"
```