# Running scripts

## Make script-file executable

```shell
chmod +x /path/to/my_script.sh
```

## Execute script

### Execute immediately 

- Method 1: Enter whole path and filename   
    `/path/to/my_script.sh`
- Method 2: Enter `./` in front of filename    
    `./my_script.sh`
- Method 3: Specify the interpreter    
    `bash my_script.sh`

### Execute at particular time

`at` - [Debian manpage](https://manpages.debian.org/at.1.en.html)

```shell
which at
```

Installation:
```shell
sudo apt update && sudo apt install at
```

Run script at 23:00 o'clock
```shell
at 23:00 -f /home/myusername/script.sh
```

Alternatively by using the `at` command prompt:
```shell
at 23:00
```
The command prompt `at>` can be exited with `ctrl`+`D`.
```shell
at> bash /home/myusername/script.sh
at>
```
Or use `echo`and pipe `at`:
```shell
echo "bash /home/myusername/script.sh" | at 23:00
```

| Time examples           | 
| ----------------------- |
| at 23:00 tomorrow       |
| at 23:00 sunday         |
| at 11pm + 2 days        |
| at HH:MM DD.MM.\[CC\]YY |

List current users queued `at` jobs:
```shell
atq
```
Delete an `at` job (example job no. 9):
```shell
atrm 9
```

-----------------

# Shebang

Tells the shell what program to interpret the script with, when executed.

| **Shell Name**        | **Code**            |
| --------------------- | ------------------- |
| Standard system shell | `#!/bin/sh`         |
| Bash shell            | `#!/bin/bash`       |
| C shell               | `#!/bin/csh`        |
| Perl                  | `#!/bin/bin/perl`   |
| PHP CLI               | `#!/bin/bin/php`    |
| Python                | `#!/bin/bin/python` |
| Ruby                  | `#!/bin/bin/ruby`   |

-----------------

# Read user input

 User input is saved into `$VARIABLE`. If user hits Enter without input, a warning is printed, then the input message comes again:

```shell
VARIABLE=''
while [ -z "$VARIABLE" ]; do
  read -p 'Input something: ' VARIABLE
  if [ -z "$VARIABLE" ]; then
    echo -e "You didn't input anything!\n"
  fi
done
```

If user doesn't input anything, `$VARIABLE` is set to `Y`.
```shell
read -p 'Do you want to start (Y/n): ' VARIABLE
VARIABLE=${VARIABLE:=Y}
```

| Command | Option | Description                                                                    |
|:-------:|:------:|:------------------------------------------------------------------------------ |
|  read   |   -p   | prompt output the string without a trailing newline before attempting to read. |
|  echo   |   -e   | enables the interpretation of backslash escapes                                |

Tipp: Use `read` to pause script until input:

```shell
read -p "Press Enter to continue" </dev/tty
```  

# Arguments

Arguments are passed to the script by writing them separated by `Space`. The arguments (also known as positional parameters) can be accessed within the bash script by using the variables $1, $2, $3 ... $n.

-------------------

# Special Bash shell variables

|   Variable    | Description                                          |
|:-------------:|:---------------------------------------------------- |
|     `$0`      | Absolute path and name of the bash script.                         |
| `$1, $2...$n` | The bash script arguments.                           |
|     `$$`      | The process id of the current shell.                 |
|     `$#`      | The total number of arguments passed to the script.  |
|     `$@`      | The value of all the arguments passed to the script. |
|     `$?`      | The exit status of the last executed command.        | 
|     `$!`      | The process id of the last executed command.         |

*Store the path to the current script file in a variable ([source](https://stackoverflow.com/a/4774063)):*

```bash
SCRIPTPATH="$( cd -- "$(dirname "$0")" >/dev/null 2>&1 ; pwd -P )"
```

-------------------

# Download files from URL

This script (download.sh) will loop through our URLs file (urls.txt) and execute the wget command for each line.

download.sh:

```shell
#!/bin/bash

while read url; do
    wget $url
done < urls.txt
```

urls.txt:

```txt
http://example.com/file1.tar
http://example.com/file2.tar
http://example.com/file3.tar
```

-----------------

# Wait for background process to end

| trigger | explanation                                                                                                               |
| ------- | ------------------------------------------------------------------------------------------------------------------------- |
| `wait`  | Without any parameters, the **`wait`** command waits for all background processes to finish before continuing the script. |
| `&`     | The ampersand sign (**`&`**) after a command indicates a background job                                                   |

-----------------

# Pause script

## `sleep`

[sleep Debian Manpage](https://manpages.debian.org/bullseye/coreutils/sleep.1.en.html)

`sleep` delays the execution of the script for a defined amount of time (seconds by default).

```shell
sleep <NUMBER>[suffix]
sleep 5m
sleep 0.5s
sleep 90s
sleep 1m 30s
```

| suffix | duration |
| ------ | -------- |
| s      | seconds  |
| m      | minutes  |
| h      | hours    |
| d      | days     |
 
-----------------

# Menu

- [Text based menu](#Text%20based%20Menu%20Template)
- [Dialog - Graphical CLI menu](https://invisible-island.net/dialog/)
- [Zenity - GUI menu](https://help.gnome.org/users/zenity/stable/index.html)

## Text based Menu Template

Menu embedded  in a function:
```shell
#!/bin/bash

mymenu() {
echo -ne "\nFancy menu title
  1) Option 1
  2) Option 2
  3) Option 3
  0) Exit menu
Choose an option: "
    read MENU_DECISION
    case $MENU_DECISION in
        1)
            echo -e "\n=>\tYou've chosen \"Option 1\"."
            mymenu;;
        2)
            echo -e "\n=>\tYou've chosen \"Option 2\"."
            mymenu;;
        3)
            echo -e "\n=>\tYou've chosen \"Option 3\"."
            mymenu;;
        0)
            echo -e "\nOK, that's it.\n"
            exit 0;;
        *)
            echo -e "\n=>\t!!! \"$MENU_DECISION\" IS NOT AN OPTION !!!"
            mymenu;;
    esac
}

mymenu
```

-----------------

# Useful links

- [ubuntuusers: Bash-Skripting-Guide für Anfänger](https://wiki.ubuntuusers.de/Shell/Bash-Skripting-Guide_f%C3%BCr_Anf%C3%A4nger)
- [devhints.io - Bash scripting cheatsheet](https://devhints.io/bash)

