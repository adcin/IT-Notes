# `tree` - list contents of directories in a tree‐like format

List directories and files starting at the current working directory - except hidden ones:
```
tree
# same as
tree ./
```

List all, including hidden:
```
tree -a ./
```

List the paths to all directories in my home dir - except the hidden ones or other mounted file-systems:
```
tree -dfix --noreport ~/
```

List the locations of all files named `foo.txt`:
```
sudo tree -i --noreport --prune -P foo.txt /
```


# Common options

|    Option    | Description                                                                                        |
| :----------: | -------------------------------------------------------------------------------------------------- |
|     `‐d`     | List directories only.                                                                             |
|     `‐f`     | Prints the full path prefix for each file.                                                         |
|     `‐i`     | Makes tree not print the indentation lines, useful when used in conjunction  with  the  ‐f option. |
|     `‐x`     | Stay on the current file‐system only.                                                              |
| `‐P pattern` | List  only  those  files that match the wild‐card pattern (check man page for pattern syntax).     |
| `‐I pattern` | Do not list those files that match the wild‐card pattern (check man page for pattern syntax).      |
| `‐‐noreport` | Omits  printing  of the file and directory report at the end of the tree listing.                  |
|  `‐‐prune`   | Omit listing empty directories. Useful when used in conjunction with ‐P or ‐I.                     |

