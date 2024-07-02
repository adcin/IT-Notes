# Docker Compose

All the Docker Compose commands should be executed in the directory of the corresponding docker-compose.yaml file. If you want to execute it from an other directory or with a script, use the `-f <PATH_TO_FILE>` option.  

- [Docker Compose commands - manual](https://docs.docker.com/engine/reference/commandline/compose/)
- [Docker run options](https://docs.docker.com/engine/reference/commandline/run/)

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

## down - Stop and remove everything created by `up`

```shell
docker compose down
```



## logs - show log output from container

```shell
docker compose logs -f
```

| Option | Description     |
| :----: | --------------- |
|   -f   | follow the logs |

## Update Containers to new version

Pull new images, recreate and restart container, delete unused images: 
```shell
docker compose pull && \
docker compose up --force-recreate --wait --detach && \
docker image prune -f
```  

Alternative to follow logs of a detached container (use `CTRL+C` to stop). Remove all unused images afterwards:
```bash
docker compose pull && docker compose up -d && docker compose logs -f
docker image prune -a
```

# Docker Compose secrets

_If your image/container can't handle "secrets", then check out the next chapter: [Docker compose environment variable file (`.env`)](docker-compose.md#Docker%20compose%20environment%20variable%20file%20(`.env`))_

> [!quote] Quote from official [docker docs](https://docs.docker.com/compose/use-secrets/)
> 
> A secret is any piece of data, such as a password, certificate, or API key, that shouldn’t be transmitted over a network or stored unencrypted in a Dockerfile or in your application’s source code.

I will create an extra files with very limited permissions, which store sensible data like username and password.

Original compose.yaml file for the example-service:
```yaml
services:
  example:
    image: example/server:latest
    ports:
      - 4242:4242
    environment:
      - MAIN_USER_NAME: "superduperadmin"
      - MAIN_USER_PASS: "my-password-123"
    volumes:
      - ./data:/srv/example/data
```

### 1. Create the secret files

```
touch ./secrets/user1-name ./secrets/user1-pass
chmod 700 ./secrets
chmod 600 ./secrets/user1*
```

Content of `./secrets/user1-name`:
```
superduperadmin
```
Content of  `./secrets/user1-pass`:
```
my-password-123
```

### 2. Edit the compose.yaml file

Modified compose.yaml file:
```yaml
services:
  example:
    image: example/server:latest
    ports:
      - 4242:4242
    environment:
      - MAIN_USER_NAME_FILE: /run/secrets/main_user
      - MAIN_USER_PASS_FILE: /run/secrets/main_pass
    volumes:
      - ./data:/srv/example/data
    secrets:
      - main_user
      - main_pass

secrets:
  main_user:
    file: ./secrets/user1-name
  main_pass:
    file: ./secrets/user1-pass
```

Changes:
- New top section called `secrets`
    - Each secret has a dedicated name (e.g. `main_user`)
    - The `file` attribute points to the relative secrets file location.
- New `secrets` attribute in the example-service
    - lists the secrets needed by the service
- Modified `environment` variables and values
    - The variable-names have now the `_FILE` suffix. That's a convention used by many images. Check the docs of your image if you get an error.
    - The value of the variables are now `/run/secrets/` followed by the name of the secret.

# Docker compose environment variable file (`.env`)

- [docker.com - Set environment variables within your container's environment](https://docs.docker.com/compose/environment-variables/set-environment-variables)

You can keep the environment variables in a separate file. You can even use multiple .env files, as described in the [official docs](https://docs.docker.com/compose/environment-variables/set-environment-variables/#use-the-env_file-attribute). But usually you need just one file per service.

Original compose.yaml example:
```
services:
  example:
    image: example/server:latest
    ports:
      - 4242:4242
    environment:
      DB_SERVER_ADDR: 10.11.12.13
      DB_SERVER_PORT: 3306
      DB_NAME: "example"
      DB_USER: "coolusername"
      DB_PASS: "sercret-password-123"
    volumes:
      - ./data:/srv/example/data
```

### 1. Create an `.env` file containing the environment variables

```
touch example.env
chmod 600 example.env
```

Content of  `example.env`:
```
DB_SERVER_ADDR: 10.11.12.13
DB_SERVER_PORT: 3306
DB_NAME: "example"
DB_USER: "coolusername"
DB_PASS: "sercret-password-123"
```

_Docker recommends using the compose "secrets" feature for usernames and passwords. But when the image doesn't support secrets, then this is better than putting them directly into the compose.yaml file._

### 2. Edit the compose.yaml file

Modified `compose.yaml` file:
```
services:
  example:
    image: example/server:latest
    ports:
      - 4242:4242
    env_file: "example.env"
    volumes:
      - ./data:/srv/example/data
```

Changes:
- the `environment` attribute and all of it's values (environment variables) are removed.
- New `env_file` attribute is added which points to the separated and more secure `example.env` file.

# Docker Compose yaml file reference

- [GitHub.com - Compose-Specs](https://github.com/compose-spec/compose-spec)
- [Services top-level elements](https://docs.docker.com/compose/compose-file/05-services/)
- [Networks top-level elements](https://docs.docker.com/compose/compose-file/06-networks/)
- [Volumes top-level element](https://docs.docker.com/compose/compose-file/07-volumes/)
- [How to use secrets in Docker Compose](https://docs.docker.com/compose/use-secrets/)

