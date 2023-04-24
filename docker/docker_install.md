# Docker CE (Community Edition) server installation  

- [Official manual](https://docs.docker.com/engine/install/)

</br>

## Server installation on Debian 11  

Based on [Debian install guide](https://docs.docker.com/engine/install/debian/).

</br>

### 1. Set up the repository  

```shell
sudo apt-get update
```

```shell
sudo apt-get install ca-certificates curl gnupg lsb-release -y
```

```shell
sudo mkdir -m 0755 -p /etc/apt/keyrings
```

```shell
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```shell
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

</br>

### 2. Install Docker Engine  

```shell
sudo apt-get update
```

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

</br>

### 3. Test installation  

```shell
sudo docker run hello-world
```

Clean up after test:
```shell
sudo docker rm -v cool_wright && sudo docker rmi hello-world
```
