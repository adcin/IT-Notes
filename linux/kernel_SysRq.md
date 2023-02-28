# SysRq

The kernel will respond to triggered SysRq commands regardless of whatever else it is doing, unless it is completely locked up.

- [kernel.org - Linux Magic System Request Key Hacks](https://www.kernel.org/doc/html/latest/admin-guide/sysrq.html)
- [linuxconfig.org - How to enable all SysRq functions on Linux](https://linuxconfig.org/how-to-enable-all-sysrq-functions-on-linux)

## SysRq values

Check the current SysRq value:
```shell
cat /proc/sys/kernel/sysrq
```
Output:
```text
176
```

This value corresponds to the sum of activated functions:

| Value | Function                                          | 
| ----- | ---------------------------------------------------- |
| 0     | disable sysrq completely                             |
| 1     | enable all functions of sysrq                        |
| 2     | enable control of console logging level              |
| 4     | enable control of keyboard (SAK, unraw)              |
| 8     | enable debugging dumps of processes etc.             |
| 16    | enable sync command                                  |
| 32    | enable remount read-only                             |
| 64    | enable signaling of processes (term, kill, oom-kill) |
| 128   | allow reboot/poweroff                                |
| 256   | allow nicing of all RT tasks                         |

Calculating the functions of 176:

- 176 = 128 + 32 + 16

| Value | Description              | 
| ----- | ------------------------ |
| 16    | enable sync command      |
| 32    | enable remount read-only |
| 128   | allow reboot/poweroff    |

Set SysRq value:
```shell
echo "number" >/proc/sys/kernel/sysrq
```

## SysRq commands

Trigger a SysRq function with a command in the root shell:
```shell
echo <COMMAND> > /proc/sysrq-trigger
```

Example:
```shell
echo t > /proc/sysrq-trigger
```


| Command | Function                                                                                                                                                                                                           |
|:-------:| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|    b    | Will immediately reboot the system without syncing or unmounting your disks.                                                                                                                                       |
|    c    | Will perform a system crash and a crashdump will be taken if configured.                                                                                                                                           |
|    d    | Shows all locks that are held.                                                                                                                                                                                     |
|    e    | Send a SIGTERM to all processes, except for init.                                                                                                                                                                  |
|    f    | Will call the oom killer to kill a memory hog process, but do not panic if nothing can be killed.                                                                                                                  |
|    g    | Used by kgdb (kernel debugger)                                                                                                                                                                                     |
|    h    | Will display help (actually any other key than those listed here will display help. but h is easy to remember :-)                                                                                                  |
|    i    | Send a SIGKILL to all processes, except for init.                                                                                                                                                                  |
|    j    | Forcibly “Just thaw it” - filesystems frozen by the FIFREEZE ioctl.                                                                                                                                                |
|    k    | Secure Access Key (SAK) Kills all programs on the current virtual console. NOTE: See important comments below in SAK section.                                                                                      |
|    l    | Shows a stack backtrace for all active CPUs.                                                                                                                                                                       |
|    m    | Will dump current memory info to your console.                                                                                                                                                                     |
|    n    | Used to make RT tasks nice-able                                                                                                                                                                                    |
|    o    | Will shut your system off (if configured and supported).                                                                                                                                                           |
|    p    | Will dump the current registers and flags to your console.                                                                                                                                                         |
|    q    | Will dump per CPU lists of all armed hrtimers (but NOT regular timer_list timers) and detailed information about all clockevent devices.                                                                           |
|    r    | Turns off keyboard raw mode and sets it to XLATE.                                                                                                                                                                  |
|    s    | Will attempt to sync all mounted filesystems.                                                                                                                                                                      |
|    t    | Will dump a list of current tasks and their information to your console.                                                                                                                                           |
|    u    | Will attempt to remount all mounted filesystems read-only.                                                                                                                                                         |
|    v    | Forcefully restores framebuffer console                                                                                                                                                                            |
|    v    | Causes ETM buffer dump [ARM-specific]                                                                                                                                                                              |
|    w    | Dumps tasks that are in uninterruptable (blocked) state.                                                                                                                                                           |
|    x    | Used by xmon interface on ppc/powerpc platforms. Show global PMU Registers on sparc64. Dump all TLB entries on MIPS.                                                                                               |
|    y    | Show global CPU Registers [SPARC-64 specific]                                                                                                                                                                      |
|    z    | Dump the ftrace buffer                                                                                                                                                                                             |
|   0-9   | Sets the console log level, controlling which kernel messages will be printed to your console. (0, for example would make it so that only emergency messages like PANICs or OOPSes would make it to your console.) |

