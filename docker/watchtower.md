# Watchtower

> [!quote] Quote from "[containrrr.dev - Watchtower](https://containrrr.dev/watchtower)"
> 
> A container-based solution for automating Docker container base image updates.

Watchtower container check scheduled for 5 am:
```bash
version: "3"

services:
  watchtower:
    container_name: watchtower
    restart: always
    image: containrrr/watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      WATCHTOWER_SCHEDULE: 0 0 5 * * *
      TZ: Europe/Berlin
```