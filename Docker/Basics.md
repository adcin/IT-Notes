# Official Links

- [Official homepage](https://www.docker.com/)
- [Docker hub](https://hub.docker.com/)
- [Docker Engine installation guide](https://docs.docker.com/engine/install/)
- [Docker CLI commands](https://docs.docker.com/engine/reference/commandline/docker)
- [Docker Compose CLI commands](https://docs.docker.com/compose/reference/)


## Docker commands

### Version

```shell
docker version
```


### List all containers  
```shell
docker ps -a
```  

### Start one or more stopped containers  
```shell
docker start <CONTAINER_NAME_or_ID>
```

### Stop one or more running containers  
```shell
docker stop <CONTAINER_NAME_or_ID>
```  

Stop all running containers
```shell
docker stop $(docker ps -a -q)
```  

### List all volumes
```shell
docker volume ls
```

### Remove all unused local volumes

```shell
docker volume prune
```

### Remove specific unused volume  
```shell
docker volume rm <VOLUME_NAME_or_ID>
```  

### List images  
```shell
docker images -a
```  

### Remove images  
```shell
docker rmi <IMAGE_NAME_or_ID> <IMAGE_NAME_or_ID>.....
```  

### Remove all unused or dangling images  
```shell
docker image prune -a -f
```  

### rm - Remove container  
[docker rm - online manual](https://docs.docker.com/engine/reference/commandline/rm/)  
```shell
docker rm <CONTAINER_NAME_or_ID>
```  
```shell
docker rm -v <CONTAINER_NAME_or_ID>
```  

| Option | Description                                            |
|:------:| ------------------------------------------------------ |
|   -v   | Remove anonymous volumes associated with the container | 


### Search the Docker Hub for images  
```shell
docker search <TERM>
```

### Pull an image or a repository  
```shell
docker pull <IMAGENAME[:TAG|@DIGEST]>
```  

### Run a command in a new container  
```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```  

### List port mappings for the container  
```shell
docker container port <CONTAINER_NAME_or_ID>
```  

### Open bash shell in Container  
```shell
docker exec -it <CONTAINER_NAME_or_ID> /bin/bash
```

### Execute command inside container  
```shell
docker exec -it <CONTAINER_NAME_or_ID> <COMMAND>
```

Example: `docker exec -it wireguard wg`



## Docker Compose

All the Docker Compose commands should be executed in the directory of the corresponding docker-compose.yaml file. If you want to execute it from an other directory or with a script, use the `-f <PATH_TO_FILE>` option.  

[Docker Compose commands - manual](https://docs.docker.com/engine/reference/commandline/compose/)

### up - Create and start containers

```shell
docker compose up -d
```

```shell
docker compose up --force-recreate -d
```

|      Option      | Description                                                                |
|:----------------:| -------------------------------------------------------------------------- |
|        -d        | Detached mode: Run containers in the background                            |
| --force-recreate | Recreate containers even if their configuration and image haven’t changed. |

### down - Stop and remove everything created by `up`

```shell
docker compose down
```

### Reload Container (Update Image Version)
```shell
docker compose pull && \
docker compose up -d
```  
