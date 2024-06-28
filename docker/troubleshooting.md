# Troubleshoot/Fix docker issues
## Container won't stop

##### 1. List containers with IDs (don't truncate the output)
```
docker ps -a --no-trunc
```

_Identify the ID's of the containers you need to get stopped/removed._

##### 2. Stop the docker daemon/service
```
sudo service docker stop
```

##### 3. Remove the remaining container files
```
sudo rm -rf /var/lib/docker/containers/<FULL_CONTAINER_ID>
```

##### 4. Start docker daemon
```
sudo service docker start
```

##### 5. Finished
The problematic containers should be gone now. List all containers to check it:
```
docker ps -a
```

## Warning-Message: `The “something” variable is not set. Defaulting to a blank string`

In this case you probably have used a dollar sign `$` somewhere in your docker run script or docker compose file. Maybe as an environment variable - a password or hash? I've found a few solutions online, but tried only one and it worked:

Put a 2nd dollar sign next to the existing one: `$` -> `$$`

For example you'll need to change `ca$h` into `ca$$h` and that's it.