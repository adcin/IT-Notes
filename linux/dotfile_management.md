----
# Dotfiles Management

</br>

In Linux application settings are saved in files - located in the users home directory. My goal is to manage my app-settings / dotfiles in one place and backup them automatically. Therefor I'm gonna use **GNU Stow** and **GitHub**.

- [GNU Stow](https://www.gnu.org/software/stow)
- [GitHub](https://github.com/)

</br>

---
# Create a GitHub repo

</br>

Create a new repository in you Git-Hub account. Mine is public, to make things easier: [Cin-Hub on Git-Hub](https://github.com/Cin-Hub/.dotfiles)

_Tip: The name of my repo begins with a `.` dot, so it will be a hidden directory when I clone it to my Home directory._

</br>

Clone the repo into your home folder:

```shell
git clone git@github.com:Cin-Hub/.dotfiles.git
```

</br>

----

# Set up stow structure

</br>

## Install GNU Stow

```shell
sudo apt install stow
```

</br>

## Create folder and file structure inside the .dotfiles folder

Example based on htop config.

</br>

### 1. Create an htop folder inside `~/.dotfiles`

```shell
mkdir ~/.dotfiles/htop
```

</br>

### 2. Recreate the standard path of the htop config file inside the new htop folder.

   Standard location of the htop config file: `~/.config/htop/htoprc`

Recreate the path:  
```shell
mkdir -p ~/.dotfiles/htop/.config/htop
```
Create the blank config file:
```shell
touch ~/.dotfiles/htop/.config/htop/htoprc
```

</br>

## Use stow to create symlinks

```shell
stow --adopt -vt ~ htop
```

Stdout:
```shell
MV: .config/htop/htoprc -> .dotfiles/htop/.config/htop/htoprc
LINK: .config/htop/htoprc => ../../.dotfiles/htop/.config/htop/htoprc
```

_Stow will copy the original config file to the .dotfiles folder and create a symlink instead._

</br>

-----

# Additional notes

</br>

Option `-n` to simulate stow  
```shell
stow --adopt -nvt ~ *
```

</br>

Adopt everything  
```shell
stow --adopt -vt ~ *
```

</br>

Adopt multiple defined folders  
```shell
stow --adopt -vt ~ bash htop
```

</br>

----

# Conclusion

</br>

## TL;DR

1. Create a dotfiles repo on GitHub
2. Clone repo to $HOME directory
3. Recreate config file structure inside dotfiles repo
4. Use stow to create symlinks

</br>

## Executed on  

| Distro  | Kubuntu 23.04 x86_64 |
| ------- | -------------------- |
| Kernel  | 6.2.0-20-generic     |
| Shell   | bash 5.2.15          |
| Desktop | Plasma 5.27.4        |

</br>

## Inspiration / Links / Source

- [GNU Stow](https://www.gnu.org/software/stow/)
- [YouTube - DevInsideYou](https://www.youtube.com/watch?v=CFzEuBGPPPg&t=24s)