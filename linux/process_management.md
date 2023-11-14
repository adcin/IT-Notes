# View active processes  

Basic commands to view processes:  
- `ps` - List commands
- `top` - Dynamic real-time view of a running system
- `pgrep` - 

Additional CLI software:  
- `htop` - Powerful process managing tool [peteris.rocks - htop explained](https://peteris.rocks/blog/htop/#process-state)
- `glances` - Quite new system monitoring tool with advanced features

Install command:  
```shell
sudo apt update && sudo apt install htop glances -y
```

## `ps` - show a snapshot of active processes

| command          | description                                                                    |
| ---------------- | ------------------------------------------------------------------------------ |
| `ps`             | print active processes of the current terminal session                         |
| `ps ux`          | print all the processes of the current user                                    | 
| `ps aux`         | print every process on the system using BSD syntax                             |
| `ps -u <user> u` | Select by effective user whose file access permissions are used by the process |

## `top` - track the running processes

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

## `pgrep` - find process

```shell
PATTERN=
```

```shell
pgrep -a -f $PATTERN
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
