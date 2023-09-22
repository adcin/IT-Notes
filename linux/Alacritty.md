# Alacritty terminal emulator

Alacritty is a fast, cross-platform, OpenGL terminal emulator.

- [GitHub.com - alacritty](https://github.com/alacritty/alacritty)

</br>

--------------------

# Installation

Alacritty can be installed easily with a package manager or on many other ways:

- [See the official documentation](https://github.com/alacritty/alacritty#installation)

</br>

## Build from source

I decided to build it from source as described in the documentation on GitHub: [Alacritty build docs on GitHub](https://github.com/alacritty/alacritty/blob/master/INSTALL.md#building)

</br>

1. Clone the repo

```shell
git clone https://github.com/alacritty/alacritty.git
```

2. Install rust

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

3. Restart Terminal
4. Check the Installation

```shell
rustup override set stable
```

```shell
rustup update stable
```

5. Install dependencies

```shell
sudo apt install cmake pkg-config libfreetype6-dev libfontconfig1-dev libxcb-xfixes0-dev libxkbcommon-dev python3
```

6. Build Alacritty

Remember - you need to be inside the git repo directory:
```shell
cd ~/git/alacritty
```

7. Alacritty should now be installed and executable:
```shell
~/git/alacritty/target/release/alacritty
```

-------------

</br>

# Post build set ups

</br>

## terminfo 

Check if alacritty terminfo is set up:

```shell
infocmp alacritty
```

> _[man infocmp](https://linux.die.net/man/1/infocmp): infocmp - compare or print out terminfo descriptions_

Stdout error message:

```shell
infocmp: couldn't open terminfo file /usr/share/terminfo/a/alacritty.
```

Install the terminfo globally (you need to be inside the repo directory `cd ~/git/alacritty`):

```shell
sudo tic -xe alacritty,alacritty-direct extra/alacritty.info
```

_[man tic](https://linux.die.net/man/1/tic): tic - the terminfo entry-description compiler_

Check alacritty terminfo again:

```shell
infocmp alacritty
```

Stdout without errors:
```shell
#	Reconstructed via infocmp from file: /etc/terminfo/a/alacritty
alacritty|alacritty terminal emulator,
...
...
...
```

</br>

## Desktop Entry

⚠ Execute the following commands from inside the repo directory (`cd ~/git/alacritty`).

```shell
sudo cp target/release/alacritty /usr/local/bin; \
sudo cp extra/logo/a-term.svg /usr/share/pixmaps/Alacritty.svg; \
sudo desktop-file-install extra/linux/Alacritty.desktop; \
sudo update-desktop-database
```

Test: Search Alacritty in the desktop application menu.

</br>

## Man pages

Required dependencies:
```shell
sudo apt install gzip scdoc
```

⚠ Execute the following commands from inside the repo directory (`cd ~/git/alacritty`).

```shell
sudo mkdir -p /usr/local/share/man/man1
scdoc < extra/man/alacritty.1.scd | gzip -c | sudo tee /usr/local/share/man/man1/alacritty.1.gz > /dev/null
scdoc < extra/man/alacritty-msg.1.scd | gzip -c | sudo tee /usr/local/share/man/man1/alacritty-msg.1.gz > /dev/null
scdoc < extra/man/alacritty.5.scd | gzip -c | sudo tee /usr/local/share/man/man5/alacritty.5.gz > /dev/null
scdoc < extra/man/alacritty-bindings.5.scd | gzip -c | sudo tee /usr/local/share/man/man5/alacritty-bindings.5.gz > /dev/null
```

My errors:
```shell
tee: /usr/local/share/man/man5/alacritty.5.gz: No such file or directory  
tee: /usr/local/share/man/man5/alacritty-bindings.5.gz: No such file or directory
```

My solution:

```shell
sudo mkdir -p /usr/local/share/man/man5
```

Repeat man5 commands:

```shell
scdoc < extra/man/alacritty.5.scd | gzip -c | sudo tee /usr/local/share/man/man5/alacritty.5.gz > /dev/null
scdoc < extra/man/alacritty-bindings.5.scd | gzip -c | sudo tee /usr/local/share/man/man5/alacritty-bindings.5.gz > /dev/null
```

Test:

```shell
man alacritty
```

```shell
man 5 alacritty
```

## Shell completions

See the [official docs on GitHub](https://github.com/alacritty/alacritty/blob/master/INSTALL.md#shell-completions)