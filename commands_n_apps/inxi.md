# `inxi`

Inxi is a tool/script, which prints technical information often needed for troubleshooting.

> [!quote] &nbsp;Excerpt from man page:
> 
> ```
> DESCRIPTION
> inxi is a command line system information script built for console and IRC. It is also used a  debugging  tool  for  forum  technical support to quickly ascertain users' system configurations and hardware. inxi shows system hardware, CPU, drivers, Xorg, Desktop,  Kernel,  compiler  version(s), Processes, RAM usage, and a wide variety of other useful information.
> ```

# Usage

Inxi has a lot of features, options and levels of verbosity. I will only cover a few basics, so check out the manual for details.

### Security
Some system information may be a security risk, if posted on the internet. Therefore inxi has some security filters for critical data:

| Option | Description |
|--------| ----------- |
| -z, --filter | Adds security filters for IP addresses, serial numbers, MAC, location (-w), and user home directory name. Removes Host:. On by default for IRC clients.  |
| --za, --filter-all | Shortcut to trigger -z, --zl, --zu, --zv. All the filters, that is.  |
| --zl, --filter-label | Filter partition label names from -j, -o, -p, -P, and -Sa (root=LABEL=...). Generally only useful in very specialized cases.  |
| --zu, --filter-uuid | Filter partition UUIDs from -j, -o, -p, -P, -Sa (root=UUID=...), -Mxxx board UUID. Useful in specialized cases.  |
| --zv, --filter-v, --filter-vulnerabilities | Filter Vulnerabilities report from -Ca. Generally only useful in very specialized cases. |

So it's recommended to use the `--za` or at least `-z` option, when sharing the inxi output. Well, unless the filtered Info is needed.

Activate security filter:
```shell
inxi --za
# or at least
inxi -z
```

### Verbosity

You can increase the info details by adding 1-3 `x` characters to the options.

Example - increasing verbosity of systems-audio-information:
```
inxi -A
inxi -Ax
inxi -Axx
inxi -Axxx
```

### System overview

##### Get a brief system information:
```shell
inxi --za
```
Output example:
```
CPU: dual core Intel Core i5-4250U (-MT MCP-) speed/min/max: 1141/800/2600 MHz  
Kernel: 6.8.11-300.fc40.x86_64 x86_64 Up: 3h 48m Mem: 2.7/3.76 GiB (71.8%)  
Storage: 113 GiB (25.2% used) Procs: 354 Shell: Sudo inxi: 3.3.34
```

##### Get basic system information:

```shell
inxi --za -b
```
Output example:
```
System:
  Kernel: 6.8.11-300.fc40.x86_64 arch: x86_64 bits: 64
  Desktop: KDE Plasma v: 6.0.5 Distro: Fedora Linux 40 (KDE Plasma)
Machine:
  Type: Laptop System: Apple product: MacBookAir6,2 v: 1.0
    serial: <superuser required>
  Mobo: Apple model: Mac-7DF21CB3ED6977E5 v: MacBookAir6,2
    serial: <superuser required> UEFI: Apple v: MBA61.88Z.0099.B23.1610201523
    date: 10/20/2016
Battery:
  ID-1: BAT0 charge: 30.5 Wh (96.8%) condition: 31.5/54.3 Wh (57.9%)
CPU:
  Info: dual core Intel Core i5-4250U [MT MCP] speed (MHz): avg: 938
    min/max: 800/2600
Graphics:
  Device-1: Intel Haswell-ULT Integrated Graphics driver: i915 v: kernel
  Display: wayland server: X.org v: 1.20.14 with: Xwayland v: 24.1.0
    compositor: kwin_wayland driver: X: loaded: modesetting unloaded: fbdev,vesa
    dri: crocus gpu: i915 resolution: 1440x900
  API: OpenGL v: 4.6 compat-v: 4.5 vendor: intel mesa v: 24.0.8
    renderer: Mesa Intel HD Graphics 5000 (HSW GT3)
Network:
  Device-1: Broadcom BCM4360 802.11ac Dual Band Wireless Network Adapter
    driver: wl
Drives:
  Local Storage: total: 113 GiB used: 28.47 GiB (25.2%)
Info:
  Memory: total: 4 GiB available: 3.76 GiB used: 2.56 GiB (68.1%)
  Processes: 322 Uptime: 4h 19m Shell: Bash inxi: 3.3.34
```

##### Get full system information:
```shell
inxi --za -F
```
Output example from an old MacBook Air with [fedora](https://fedoraproject.org):
```
System:
  Kernel: 6.8.11-300.fc40.x86_64 arch: x86_64 bits: 64
  Desktop: KDE Plasma v: 6.0.5 Distro: Fedora Linux 40 (KDE Plasma)
Machine:
  Type: Laptop System: Apple product: MacBookAir6,2 v: 1.0
    serial: <superuser required>
  Mobo: Apple model: Mac-7DF21CB3ED6977E5 v: MacBookAir6,2
    serial: <superuser required> UEFI: Apple v: MBA61.88Z.0099.B23.1610201523
    date: 10/20/2016
Battery:
  ID-1: BAT0 charge: 30.5 Wh (96.8%) condition: 31.5/54.3 Wh (57.9%)
CPU:
  Info: dual core model: Intel Core i5-4250U bits: 64 type: MT MCP cache:
    L2: 512 KiB
  Speed (MHz): avg: 1131 min/max: 800/2600 cores: 1: 1300 2: 1300 3: 1127
    4: 800
Graphics:
  Device-1: Intel Haswell-ULT Integrated Graphics driver: i915 v: kernel
  Display: wayland server: X.org v: 1.20.14 with: Xwayland v: 24.1.0
    compositor: kwin_wayland driver: X: loaded: modesetting unloaded: fbdev,vesa
    dri: crocus gpu: i915 resolution: 1440x900
  API: EGL v: 1.5 drivers: crocus,swrast
    platforms: wayland,x11,surfaceless,device
  API: OpenGL v: 4.6 compat-v: 4.5 vendor: intel mesa v: 24.0.8
    renderer: Mesa Intel HD Graphics 5000 (HSW GT3)
  API: Vulkan v: 1.3.283 drivers: N/A surfaces: xcb,xlib,wayland
Audio:
  Device-1: Intel Haswell-ULT HD Audio driver: snd_hda_intel
  Device-2: Intel 8 Series HD Audio driver: snd_hda_intel
  Device-3: Broadcom 720p FaceTime HD Camera driver: N/A
  API: ALSA v: k6.8.11-300.fc40.x86_64 status: kernel-api
  Server-1: PipeWire v: 1.0.7 status: active
Network:
  Device-1: Broadcom BCM4360 802.11ac Dual Band Wireless Network Adapter
    driver: wl
  IF: wlp3s0 state: up mac: <filter>
  IF-ID-1: tailscale0 state: unknown speed: -1 duplex: full mac: N/A
Bluetooth:
  Device-1: Apple Bluetooth USB Host Controller driver: btusb type: USB
  Report: btmgmt ID: hci0 state: up address: <filter> bt-v: 4.0
Drives:
  Local Storage: total: 113 GiB used: 28.47 GiB (25.2%)
  ID-1: /dev/sda vendor: Apple model: SSD SM0128F size: 113 GiB
Partition:
  ID-1: / size: 111.4 GiB used: 28.16 GiB (25.3%) fs: btrfs dev: /dev/dm-0
  ID-2: /boot size: 973.4 MiB used: 288.7 MiB (29.7%) fs: ext4
    dev: /dev/sda2
  ID-3: /boot/efi size: 600 MiB used: 36.9 MiB (6.2%) fs: hfsplus
    dev: /dev/sda1
  ID-4: /home size: 111.4 GiB used: 28.16 GiB (25.3%) fs: btrfs
    dev: /dev/dm-0
Swap:
  ID-1: swap-1 type: zram size: 3.76 GiB used: 2.8 GiB (74.6%) dev: /dev/zram0
Sensors:
  System Temperatures: cpu: 59.0 C mobo: N/A
  Fan Speeds (rpm): N/A
Info:
  Memory: total: 4 GiB available: 3.76 GiB used: 2.59 GiB (68.8%)
  Processes: 331 Uptime: 4h 42m Shell: Bash inxi: 3.3.34
```

You can go more into details with options like `sudo inxi -Frmxx`, so check out the man page.

### Common hardware flags

I use the `--za` even if not needed, because I'm lazy ^^.

Audio:
```shell
inxi --za -A
```

CPU:
```shell
inxi --za -C
```

HDD/SSD  drive info:
```shell
inxi --za -D
```

Bluetooth:
```shell
inxi --za -E
```

Graphic device(s):
```shell
inxi --za -G
```

USB:
```shell
inxi --za -J
```

Memory  (RAM):
```shell
sudo inxi --za -J
```

Machine data: Device, Motherboard, BIOS, and if present, System Builder (Like Lenovo)
```shell
inxi --za -M
```

Network  device(s)
```shell
inxi --za -N
```

Basic Partition information:
```shell
inxi --za -P
```

Operating System:
```shell
inxi --za -S
```

RAID:
```shell
inxi --za -R
```