# Docker commands

</br>

## Version

```shell
docker version
```

</br>

## List all containers  
```shell
docker ps -a
```  

</br>

## Start one or more stopped containers  
```shell
docker start <CONTAINER_NAME_or_ID>
```

</br>

## Stop one or more running containers  
```shell
docker stop <CONTAINER_NAME_or_ID>
```  

</br>

Stop all running containers
```shell
docker stop $(docker ps -a -q)
```  

</br>

## List all volumes
```shell
docker volume ls
```

</br>

## Remove all unused local volumes

```shell
docker volume prune
```

</br>

## Remove specific unused volume  
```shell
docker volume rm <VOLUME_NAME_or_ID>
```  

</br>

## List images  
```shell
docker images -a
```  

</br>

## Remove images  
```shell
docker rmi <IMAGE_NAME_or_ID> <IMAGE_NAME_or_ID>.....
```  

</br>

## Remove all unused or dangling images  
```shell
docker image prune -a -f
```  

</br>

## rm - Remove container  
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

</br>

## Search the Docker Hub for images  
```shell
docker search <TERM>
```

</br>

## Pull an image or a repository  
```shell
docker pull <IMAGENAME[:TAG|@DIGEST]>
```  

</br>

## Run a command in a new container  
```shell
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```  

</br>

## List port mappings for the container  
```shell
docker container port <CONTAINER_NAME_or_ID>
```  

</br>

## Open bash shell in Container  
```shell
docker exec -it <CONTAINER_NAME_or_ID> /bin/bash
```

</br>

## Execute command inside container  
```shell
docker exec -it <CONTAINER_NAME_or_ID> <COMMAND>
```

Example: `docker exec -it wireguard wg`

-------

</br>

# Docker Compose

All the Docker Compose commands should be executed in the directory of the corresponding docker-compose.yaml file. If you want to execute it from an other directory or with a script, use the `-f <PATH_TO_FILE>` option.  

[Docker Compose commands - manual](https://docs.docker.com/engine/reference/commandline/compose/)

</br>

## up - Create and start containers

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

</br>

## down - Stop and remove everything created by `up`

```shell
docker compose down
```

</br>

## Update Containers to new version

Pull new images, recreate and restart container, delete unused images: 
```shell
docker compose pull && \
docker compose up --force-recreate --wait --detach && \
docker image prune -f
```  

</br>
