# View active processes  

Basic commands to view processes:  
- `ps` - report a snapshot of the current processes
- `top` - display a dynamic real-time view of a running system
- `pgrep` - look up processes based on name and other attributes

Additional CLI software:  
- `htop` - Powerful process managing tool [peteris.rocks - htop explained](https://peteris.rocks/blog/htop/#process-state)
- `glances` - Quite new system monitoring tool with advanced features

Install command:  
```shell
sudo apt update && sudo apt install htop glances -y
```

# `ps` - show a snapshot of active processes

| command          | description                                                                    |
| ---------------- | ------------------------------------------------------------------------------ |
| `ps`             | print active processes of the current terminal session                         |
| `ps ux`          | print all the processes of the current user                                    |
| `ps aux`         | print every process on the system using BSD syntax                             |
| `ps -u <user> u` | Select by effective user whose file access permissions are used by the process |
| `ps -el`         | Display all processes (`-e` same as `-A`) in long format (`-l`).               |

> [!attention]
> The command `ps`  displays a process priority range (PRI) from -40 to 99 (instead of 0-139). The true priority is obtained by adding 40 (e.g. 80 + 40 = 120).

# `top` - track the running processes

Output terminology:

| column  | description                                  |
| ------- | -------------------------------------------- |
| PID     | Unique Process ID given to each process      |
| User    | Username of the process owner                |
| PR      | Priority given to a process while scheduling |
| NI      | ‘nice’ value of a process                    |
| VIRT    | Amount of virtual memory used by a process   |
| RES     | Amount of physical memory used by a process  |
| SHR     | Amount of memory shared with other processes |
| S       | state of the process:                        |
|         | ‘D’ = uninterruptible sleep                  |
|         | ‘R’ = running                                |
|         | ‘S’ = sleeping                               |
|         | ‘T’ = traced or stopped                      |
|         | ‘Z’ = zombie                                 |
| %CPU    | Percentage of CPU used by the process        |
| %MEM    | Percentage of RAM used by the process        |
| TIME+   | Total CPU time consumed by the process       |
| Command | Command used to activate the process         | 

> [!attention]
> The command `top`  displays a process priority range (PRI) from -100 to 39 (instead of 0-139). The true priority is obtained by adding 100 (e.g. 20 + 100 = 120). So real-time processes are identified by negative numbers or the acronym `rt`.

# `pgrep` - find process

```shell
pgrep -a -f <PATTERN>
```

| Option | Description                                                                                                         |
|:------:| ------------------------------------------------------------------------------------------------------------------- |
|   -a   | List the full command line as well as the process ID.                                                               |
|   -f   | The _pattern_ is normally only matched against the process name. When **-f** is set, the full command line is used. | 

# Back-/ Foreground

If a running job is blocking the terminal use:
1. `ctrl`+ `Z` -> to stop/pause the job
2. `bg` -> resume job in background
3. `fg` -> resume job in foreground

Show all jobs from running session:  
```shell
jobs
or
jobs -l
```

Resume specific job in foreground:
```
fg %<job-number>
```

Resume specific paused job in background:
```
bg %<job-number>
```

**⚠ When you exit the session, the job also will be stopped!**

# Stop process

```shell
kill [options] <pid> [...]
```

|  Option  | Description                                                                                                                                                         |
|:--------:| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -SIGTERM | Termination signal - standard option, used when no option is specified. Soft kill, the process may choose to ignore it, child processes aren't forced to be killed. |
| -SIGSTOP | Stops the process until you resume it (with -SIGCONT).                                                                                                              | 
| -SIGCONT | Continue a stopped process.                                                                                                                                         |
| -SIGKILL | Kill the process and its child processes immediately. This signal cannot be ignored or blocked.                                                                     |

List all signals: `kill -L`
Read more about signals: `man 7 signal`

Example:
```shell
sudo kill -SIGSTOP 369
```

# Process scheduling and priorities

Every process has a scheduling-policy and -priority, which affect the scheduling.

- Types of scheduling policies:
    - Normal policies
        - Most processes have normal scheduling policies.
        - They usually have the same priority value (120).
        - The "nice value" can define execution priority.
        - Only processes with "normal" policy will be affected by tuning the process scheduling.
    - Real-time policies
        - Higher priority than normal policies
        - Scheduled by their priority values directly. A lower priority process will gain CPU control only if higher priority processes are idle or waiting for hardware response.
- Scheduling priority
    - The lower the value, the higher the priority.
    - 0-99 - Static priority range for real-time processes.
    - 100-139 - Static priority range for normal processes (standard is 120)
    - The priority value can be found in the `sched` file of the process (`/proc/<PID>/sched`). Command example:
      `grep ^prio /proc/1/sched`
    - The **Niceness** value is used to manipulate the priority of normal processes.
        - The default nice value is 0 (priority 120).
        - Nice numbers range from -20 (less nice, high priority) to 19 (more nice, low priority).
            - nice value -20 = priority 100
            - nice value 19 = priority 139
        - Only root can set a negative nice value (-20 to -1)

# Commands: `nice` and `renice`
##### nice - run a program with modified scheduling priority

SYNOPSIS
```shell
nice [OPTION] [COMMAND [ARG]...]
```

Option | Description
---- | ----
`-n`, `--adjustment=N` | add integer N to the niceness (default 10)

Backup (`tar`) the home directory of user1 with a lower priority (niceness 15 = priority 135):
```
nice -n 15 tar czf home_user1_backup.tar.gz /home/user1
```

##### renice - alter priority of running processes

SYNOPSIS
```shell
renice [-n] priority [-g|-p|-u] identifier...
```

Raise the priority of process 12345 to 110 (niceness -10):
```shell
sudo renice -10 -p 12345
```

Lower prio of processes executed by group `theothers` to 139 (niceness 19):
```shell
sudo renice 19 -g theothers
```

Option | Description
--- | ---
`-p <PID>` | Execute on process with PID (the default).
`-g <Groupname or GID>` | Execute on processes from group.
`-u <Username or UID>` | Execute on processes from user.

> [!important]
> The first argument must be the priority value.

# Command: `schedtool`
- https://github.com/freequaos/schedtool
##### schedtool - query and set CPU scheduling parameters

> [!info] DESCRIPTION (excerpt from man page):
> 
> `schedtool` can set all CPU scheduling parameters Linux is capable of or display information for given processes.
> 
> Long-running, non-interactive tasks may benefit from SCHED_BATCH as timeslices are longer, less system-time is wasted by computing the next runnable process and the caches stay stable.
> 
> Audio/video or other near-realtime applications may run with less skipping if set to SCHED_RR. Use the static priority-switch -p to designate inter-process-hierarchies.

