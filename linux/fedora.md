# fedora installation tipps
Inspired by:
- [itsfoss.com - 17 Things to Do After Installing Fedora 40](https://itsfoss.com/things-to-do-after-installing-fedora/)

## Speed up dnf
Add or modify following lines in `/etc/dnf/dnf.conf`:
```
max_parallel_downloads=10
fastestmirror=true
```

## Enable rpm fusion
Add free and nonfree fusion repos:
```
sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

## Upgrade packaged
```
sudo dnf upgrade
```

## Install multimedia plugins

```
sudo dnf -y install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel && \
sudo dnf -y install lame* --exclude=lame-devel && \
sudo dnf -y group upgrade --with-optional --allowerasing Multimedia
```

Check also: [ostechnix.com](https://ostechnix.com/how-to-install-multimedia-codecs-in-fedora-linux/)

## Change Hostname (optional)
```
sudo hostnamectl set-hostname "beautiful-hostname"
```

## Basic ssh-server hardening (optional)

##### 1. Add key to `~/.ssh/authorized_keys`:
```
ssh-copy-id -i <PUBLIC_KEYFILE> <USERNAME>@<HOST>
```

##### 2. Create new ssh.conf file in `/etc/ssh/sshd_config.d/`:
```shell
cat << EOF | sudo tee /etc/ssh/sshd_config.d/mysshconfig.conf
PermitRootLogin no
PasswordAuthentication no
PermitEmptyPasswords no
StrictModes yes
EOF
```

##### 3. Restart the service
```shell
sudo systemctl reload sshd
```

##### 4. Enable sshd autostart at reboot:
```
sudo systemctl enable sshd
```
## Install some basic apps (optional):

```
sudo dnf -y install stow vim git curl htop mc inxi tmux ncdu
```

## Install docker (optional):
- https://docs.docker.com/engine/install/fedora/#install-using-the-repository

Add the docker repo:
```
sudo dnf -y config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
```

Install Docker packages:
```
sudo dnf -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Add current user to the docker group:
```
sudo usermod -aG docker $(logname)
```
_Activate the changes by relogging or executing `newgrp docker`._

Start docker:
```
sudo systemctl start docker
```

Test installations:
```
docker run hello-world
```

Optional cleanup after test:
```
docker rm $(docker ps -aq); docker image prune
```

Enable Docker autostart after boot:
```
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

# Upgrade to new release (39 to 40) - using the dnf system plugin

- [docs.fedoraproject.org - Upgrading Fedora Linux to a New Release](https://docs.fedoraproject.org/en-US/quick-docs/upgrading-fedora-new-release/)
- [docs.fedoraproject.org - Upgrading Fedora Linux Using DNF System Plugin](https://docs.fedoraproject.org/en-US/quick-docs/upgrading-fedora-offline/)

##### 1. First things first ... update software

```shell
sudo dnf upgrade --refresh -y
```

```shell
flatpak update -y
```

##### 2. Download and verify packages

```shell
sudo dnf system-upgrade download --releasever=40
```

You may need to verify some external repositories for example:
```
wget -q https://pkgs.tailscale.com/stable/fedora/repo.gpg
gpg --show-keys repo.gpg
```

> DNF will only download packages, install gpg keys, and check the transaction.

##### 2.1 Verify the Fedora 40 GPG key

My output:
```shell
Fedora 40 - x86_64                                                          1.6 MB/s | 1.6 kB     00:00    
Importing GPG key 0xA15B79CC:
 Userid     : "Fedora (40) <fedora-40-primary@fedoraproject.org>"
 Fingerprint: 115D F9AE F857 853E E844 5D0A 0727 707E A15B 79CC
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-fedora-40-x86_64
Is this ok [y/N]:
```

Excerpt from https://fedoraproject.org/security:
```
Fedora 40
id:rsa4096/A15B79CC 2023-01-24
Fingerprint:
115DF9AEF857853EE8445D0A0727707EA15B79CC
DNS OpenPGPKey:
4d0cd6e4349d5979387749daf5995f20d0de7f7b2fdfdc76d7eb21a1._openpgpkey.fedoraproject.org
```

_Seems legit :) ... `y`_

##### 2.2 My final output

```
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Complete!
Transaction saved to /var/lib/dnf/system-upgrade/system-upgrade-transaction.json.
Download complete! Use 'dnf system-upgrade reboot' to start the upgrade.
To remove cached metadata and transaction use 'dnf system-upgrade clean'
The downloaded packages were saved in cache until the next successful transaction.
You can remove cached packages by executing 'dnf clean packages'.
```

##### 3. Start release upgrade

```
sudo dnf system-upgrade reboot
```

