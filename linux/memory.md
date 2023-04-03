# `free` - Display amount of free and used memory in the system

```shell
free -h
```

| Option | Description    |
|:------:| -------------- |
|   -h   | Human readable |

Output example:

|       | total | used  | free  | shared | buff/cache | available |
| ----- | ----- | ----- | ----- | ------ | ---------- | --------- |
| Mem:  | 15Gi  | 3,0Gi | 8,0Gi | 983Mi  | 4,5Gi      | 11Gi      |
| Swap: | 1,9Gi | 0B    | 1,9Gi |        |            |           |

_Linux uses "unused" memory to increase performance. So the important value is in the **available**  column, as if you want to check your system having enough memory._

# swap

