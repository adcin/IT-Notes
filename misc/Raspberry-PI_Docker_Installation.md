# Docker installation on a Raspberry Pi 3 B

</br>

Step by step process of Docker installation on a freshly installed Raspberry Pi OS Lite (64-bit). I used the installation script provided by docker.

</br>

------------------

# Installation

1. Update and upgrade packages:  
```shell
sudo apt update && sudo apt upgrade -y
```

</br>

2. Create a download folder (optional):  
```shell
mkdir ~/Downloads && cd ~/Downloads
```

</br>

3. Download the installation script and execute it:  
```shell
curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh
```

</br>

4. Add your non root user (marcin) to the docker group (optional):  
```shell
sudo usermod -aG docker marcin
```

</br>

5. Reinitialize the user's environment or restart the terminal session:  
```shell
newgroup - docker
```

</br>

6. List Docker version to check if it is installed:  
```shell
docker version
```

</br>

----------------------

# Verification

</br>

Run the "hello-world" container: 
```shell
docker run hello-world
```

</br>

List all containers:  
```shell
docker ps -a
```

</br>

Delete the hello-world container by using its ID:  
```shell
docker rm <CONTAINER ID>
```

</br>

Remove the hello-world image:  
```shell
docker rmi hello-world
```

</br>

----------

# Meta information

</br>

**Executed on**: Raspberry Pi 3 Model B Rev 1.2

| distro | Debian GNU/Linux 11 (bullseye) aarch64 |
| ------ | -------------------------------------- |
| kernel | 6.1.21-v8+                             |
| shell  | zsh 5.8                                | 
