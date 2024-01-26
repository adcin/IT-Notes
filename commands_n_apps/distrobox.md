# Distrobox
- [github.com - distrobox by 89luca89](https://github.com/89luca89/distrobox)

## Quote from [github - distrobox:](https://github.com/89luca89/distrobox#distrobox)
> Use any Linux distribution inside your terminal. Enable both backward and forward compatibility with software and freedom to use whatever distribution you’re more comfortable with. Distrobox uses podman, docker or lilipod to create containers using the Linux distribution of your choice. The created container will be tightly integrated with the host, allowing sharing of the HOME directory of the user, external storage, external USB devices and graphical apps (X11/Wayland), and audio.

# Installation

> [!info]- Jan. 2024, System:
> OS: Kubuntu 23.10 x86_64
> Kernel: 6.5.0-15-generic
> Shell: bash 5.2.15
> DE: Plasma 5.27.8
> WM: KWin
> Terminal: tmux
> CPU: Intel i7-4702MQ (8) @ 3.200GHz
> GPU: NVIDIA GeForce GTX 760M
> GPU: Intel 4th Gen Core Processor
> Memory: 5084MiB / 15863MiB

Install podman:
```shell
sudo apt install podman
```

Install distrobox:  
```shell
sudo apt install distrobox
```

Checkout the help page:  
```shell
distrobox --help
```

> [!quote] Output:
> ```shell
> distrobox version: 1.4.2.1
> 
> Choose one of the available commands:
>         create
>         enter
>         list
>         rm
>         stop
>         upgrade
>         ephemeral
>         generate-entry
>         version
> ```

# Create a Distro Container
Show compatible images:  
```shell
distrobox create --compatibility
# or for more details: 
# man distrobox-compatibility
```

> [!quote]- Output:
> ```shell
> container-registry.oracle.com/os/oraclelinux:8
> container-registry.oracle.com/os/oraclelinux:8-slim
> container-registry.oracle.com/os/oraclelinux:9
> container-registry.oracle.com/os/oraclelinux:9-slim
> docker.io/debian/eol:jessie
> docker.io/debian/eol:wheezy
> docker.io/gentoo/stage3:latest
> docker.io/kalilinux/kali-rolling:latest
> docker.io/library/alpine:3.15
> docker.io/library/alpine:3.16
> docker.io/library/alpine:latest
> docker.io/library/archlinux:latest
> docker.io/library/clearlinux:base
> docker.io/library/clearlinux:latest
> docker.io/library/debian:10
> docker.io/library/debian:9
> docker.io/library/debian:stable
> docker.io/library/debian:stable-backports
> docker.io/library/debian:testing
> docker.io/library/debian:testing-backports
> docker.io/library/debian:unstable
> docker.io/library/mageia
> docker.io/library/neurodebian:nd100
> docker.io/library/sl:7
> docker.io/library/ubuntu:14.04
> docker.io/library/ubuntu:16.04
> docker.io/library/ubuntu:18.04
> docker.io/library/ubuntu:20.04
> docker.io/library/ubuntu:22.04
> docker.io/vbatts/slackware:14.2
> ghcr.io/void-linux/void-linux:latest-full-x86_64
> ghcr.io/void-linux/void-linux:latest-full-x86_64-musl
> Images
> public.ecr.aws/amazonlinux/amazonlinux:1
> public.ecr.aws/amazonlinux/amazonlinux:2
> public.ecr.aws/amazonlinux/amazonlinux:2022.0.20220531.0
> quay.io/almalinux/8-base:8
> quay.io/almalinux/8-init:8
> quay.io/almalinux/almalinux:8
> quay.io/almalinux/almalinux:9
> quay.io/almalinux/almalinux:9-minimal
> quay.io/centos/centos:7
> quay.io/centos/centos:stream8
> quay.io/centos/centos:stream9
> quay.io/fedora/fedora:35
> quay.io/fedora/fedora:36
> quay.io/fedora/fedora:38
> quay.io/rockylinux/rockylinux:8
> quay.io/rockylinux/rockylinux:8-minimal
> quay.io/rockylinux/rockylinux:9
> quay.io/rockylinux/rockylinux:latest
> registry.access.redhat.com/ubi7/ubi
> registry.access.redhat.com/ubi7/ubi-init
> registry.access.redhat.com/ubi8/ubi
> registry.access.redhat.com/ubi8/ubi-init
> registry.access.redhat.com/ubi8/ubi-minimal
> registry.access.redhat.com/ubi9/ubi
> registry.access.redhat.com/ubi9/ubi-init
> registry.access.redhat.com/ubi9/ubi-minimal
> registry.fedoraproject.org/fedora:37
> registry.fedoraproject.org/fedora-toolbox:37
> registry.opensuse.org/opensuse/leap:latest
> registry.opensuse.org/opensuse/toolbox:latest
> registry.opensuse.org/opensuse/tumbleweed:latest
> ```

Create an Ubuntu 18.04 container: 
```shell
distrobox create --image ubuntu:18.04 --name lin-ess-ubuntu-18.04 --home ~/distrobox/lin-ess-ubuntu-18.04
```

> [!quote]- Output:
> ```shell
> Image ubuntu:18.04 not found.
> Do you want to pull the image now? [Y/n]: Y
> Resolved "ubuntu" as an alias (/etc/containers/registries.conf.d/shortnames.conf)
> Trying to pull docker.io/library/ubuntu:18.04...
> Getting image source signatures
> Copying blob 7c457f213c76 done
> Copying config f9a80a55f4 done
> Writing manifest to image destination
> Storing signatures
> f9a80a55f492e823bf5d51f1bd5f87ea3eed1cb31788686aa99a2fb61a27af6a
> Creating 'lin-ess-ubuntu-18.04' using image ubuntu:18.04         [ OK ]
> Distrobox 'lin-ess-ubuntu-18.04' successfully created.
> To enter, run:
> 
> distrobox enter lin-ess-ubuntu-18.04
> 
> lin-ess-ubuntu-18.04
> ```

> [!info]- Info: distrobox create --help
> ```shell
> distrobox version: 1.4.2.1
> 
> Usage:
> 
>         distrobox create --image alpine:latest --name test --init-hooks "touch /var/tmp/test1 && touch /var/tmp/test2"
>         distrobox create --image fedora:35 --name test --additional-flags "--env MY_VAR-value"
>         distrobox create --image fedora:35 --name test --volume /opt/my-dir:/usr/local/my-dir:rw --additional-flags "--pids-limit -1"
>         distrobox create -i docker.io/almalinux/8-init --init --name test --pre-init-hooks "dnf config-manager --enable powertools && dnf -y install epel-release"
>         distrobox create --clone fedora-35 --name fedora-35-copy
>         distrobox create --image alpine my-alpine-container
>         distrobox create --image registry.fedoraproject.org/fedora-toolbox:35 --name fedora-toolbox-35
>         distrobox create --pull --image centos:stream9 --home ~/distrobox/centos9
> 
>         DBX_NON_INTERACTIVE=1 DBX_CONTAINER_NAME=test-alpine DBX_CONTAINER_IMAGE=alpine distrobox-create
> 
> Options:
> 
>         --image/-i:             image to use for the container  default: registry.fedoraproject.org/fedora-toolbox:36
>         --name/-n:              name for the distrobox          default: my-distrobox
>         --pull/-p:              pull the image even if it exists locally (implies --yes)
>         --yes/-Y:               non-interactive, pull images without asking
>         --root/-r:              launch podman/docker with root privileges. Note that if you need root this is the preferred
>                                 way over "sudo distrobox" (note: if using a program other than 'sudo' for root privileges is necessary,
>                                 specify it through the DBX_SUDO_PROGRAM env variable, or 'distrobox_sudo_program' config variable)
>         --clone/-c:             name of the distrobox container to use as base for a new container
>                                 this will be useful to either rename an existing distrobox or have multiple copies
>                                 of the same environment.
>         --home/-H:              select a custom HOME directory for the container. Useful to avoid host's home littering with temp files.
>         --volume:               additional volumes to add to the container
>         --additional-flags/-a:  additional flags to pass to the container manager command
>         --init-hooks:           additional commands to execute during container initialization
>         --pre-init-hooks:       additional commands to execute prior to container initialization
>         --init/-I:              use init system (like systemd) inside the container.
>                                 this will make host's processes not visible from within the container.
>         --compatibility/-C:     show list of compatible images
>         --help/-h:              show this message
>         --no-entry:             do not generate a container entry in the application list
>         --dry-run/-d:           only print the container manager command generated
>         --verbose/-v:           show more verbosity
>         --version/-V:           show version
> 
> Compatibility:
> 
>         for a list of compatible images and container managers, please consult the man page:
>                 man distrobox-compatibility
>         or run
>                 distrobox create --compatibility
>         or consult the documentation page on: https://github.com/89luca89/distrobox/blob/main/docs/compatibility.md
> ```

# Run a container

```shell
distrobox enter lin-ess-ubuntu-18.04
```

> [!quote]- Output:
> ```shell
> Container lin-ess-ubuntu-18.04 is not running.
> Starting container lin-ess-ubuntu-18.04
> run this command to follow along:
> 
>  podman logs -f lin-ess-ubuntu-18.04
> 
>  Starting container...                           [ OK ]
>  Installing basic packages...                    [ OK ]
>  Setting up read-only mounts...                  [ OK ]
>  Setting up read-write mounts...                 [ OK ]
>  Setting up host's sockets integration...        [ OK ]
>  Setting up read-only mounts...                  [ OK ]
>  Setting up read-write mounts...                 [ OK ]
>  Setting up host's sockets integration...        [ OK ]
>  Integrating host's themes, icons, fonts...      [ OK ]
>  Setting up package manager exceptions...        [ OK ]
>  Setting up dpkg exceptions...                   [ OK ]
>  Setting up apt hooks...                         [ OK ]
>  Setting up sudo...                              [ OK ]
>  Setting up groups...                            [ OK ]
>  Setting up users...                             [ OK ]
>  Executing init hooks...                         [ OK ]
> 
> Container Setup Complete!
> marcin@lin-ess-ubuntu-18:/home/marcin$
> ```

> [!info]- Info: distrobox enter --help
> ```shell
> distrobox version: 1.4.2.1
> 
> Usage:
> 
>         distrobox-enter --name fedora-35 -- bash -l
>         distrobox-enter my-alpine-container -- sh -l
>         distrobox-enter --additional-flags "--preserve-fds" --name test -- bash -l
>         distrobox-enter --additional-flags "--env MY_VAR=value" --name test -- bash -l
>         MY_VAR=value distrobox-enter --additional-flags "--preserve-fds" --name test -- bash -l
> 
> Options:
> 
>         --name/-n:              name for the distrobox                                          default: my-distrobox
>         --/-e:                  end arguments execute the rest as command to execute at login   default: bash -l
>         --no-tty/-T:            do not instantiate a tty
>         --no-workdir/-nw:               always start the container from container's home directory
>         --additional-flags/-a:  additional flags to pass to the container manager command
>         --help/-h:              show this message
>         --root/-r:              launch podman/docker with root privileges. Note that if you need root this is the preferred
>                                 way over "sudo distrobox" (note: if using a program other than 'sudo' for root privileges is necessary,
>                                 specify it through the DBX_SUDO_PROGRAM env variable, or 'distrobox_sudo_program' config variable)
>         --dry-run/-d:           only print the container manager command generated
>         --verbose/-v:           show more verbosity
>         --version/-V:           show version
> ```

# Show containers
```shell
distrobox list
```

> [!quote]- Output:
> ```shell
> ID           | NAME                 | STATUS                         | IMAGE
> 21de9804371f | lin-ess-ubuntu-18.04 | Up 4 minutes ago               | docker.io/library/ubuntu:18.04
> ```

> [!info]- Info: distrobox list --help
> ```shell
> distrobox version: 1.4.2.1
> 
> Usage:
> 
>         distrobox-list
> 
> Options:
> 
>         --help/-h:              show this message
>         --no-color:             disable color formatting
>         --root/-r:              launch podman/docker with root privileges. Note that if you need root this is the preferred
>                                 way over "sudo distrobox" (note: if using a program other than 'sudo' for root privileges is necessary,
>                                 specify it through the DBX_SUDO_PROGRAM env variable, or 'distrobox_sudo_program' config variable)
>         --verbose/-v:           show more verbosity
>         --version/-V:           show version
> ```

# Stop a container



```shell
distrobox stop lin-ess-ubuntu-18.04
```

> [!quote]- Output:
> ```shell
> Do you really want to stop lin-ess-ubuntu-18.04? [Y/n]: Y
> lin-ess-ubuntu-18.04
> ```

> [!info]- Info: distrobox stop --help
> ```shell
> distrobox version: 1.4.2.1
> 
> Usage:
> 
>         distrobox-stop --name container-name
>         distrobox-stop container-name
> 
> Options:
> 
>         --name/-n:              name for the distrobox
>         --yes/-Y:               non-interactive, stop without asking
>         --help/-h:              show this message
>         --root/-r:              launch podman/docker with root privileges. Note that if you need root this is the preferred
>                                 way over "sudo distrobox" (note: if using a program other than 'sudo' for root privileges is necessary,
>                                 specify it through the DBX_SUDO_PROGRAM env variable, or 'distrobox_sudo_program' config variable)
>         --verbose/-v:           show more verbosity
>         --version/-V:           show version
> ```

