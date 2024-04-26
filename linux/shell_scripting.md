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
    `/bin/bash my_script.sh`

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

| **Shell Name**                      | **Code**            |
| ----------------------------------- | ------------------- |
| Standard Bourne shell or compatible | `#!/bin/sh`         |
| Bash shell                          | `#!/bin/bash`       |
| C shell                             | `#!/bin/csh`        |
| Perl                                | `#!/bin/bin/perl`   |
| PHP CLI                             | `#!/bin/bin/php`    |
| Python                              | `#!/bin/bin/python` |
| Ruby                                | `#!/bin/bin/ruby`   |



-----------------

# POSIX

There are a lot of different types and versions of operating systems and shells in the Unix-Like universe. And because even basic commands often have unique options or a slightly different syntax in these -nunixes, IT-People often have trouble to write or reuse scripts.

**POSIX** (Portable Operating System Interface) is an attempt at unifying all the various UNIX-like systems. It is maintained by The Austin Group, a joint working group which consists of representatives from The Open Group, IEEE and ISO/IEC.

**SUS** (Single UNIX Specification) is a standard based on POSIX. An operating system requires the compliance with the SUS standard, to be qualified for using the UNIX trademark. The UNIX trademark belongs to The Open Group, which is also chairs the Austin Group and manages its day-to-day running.

TL;DR: __**"Use commands and syntax based on the UNIX standard you n00b."**__

# Redirection

> [!quote] Quote from Wikipedia article "Standard streams"
>
> In computer programming, standard streams are preconnected input and output communication channels between a computer program and its environment when it begins execution. The three input/output connections are called standard input (stdin), standard output (stdout) and standard error (stderr).

**In bash the standard streams are represented by file descriptors:**

| Stream | File Descriptor |
| ------ | :-------------: |
| stdin  |        0        |
| stdout |        1        |
| stderr |        2        |

| Example                                                                                     | Effect                                         |
| ------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| `somecommand &> /dev/null`<br>`somecommand >& /dev/null`<br>`somecommand > /dev/null 2>&1`  | stdout and stderr to null                      |
| `somecommand > output.txt`<br>`somecommand 1> output.txt`                                   | stdout to file output.txt                      |
| `somecommand > /dev/null 2> errorlog.txt`                                                   | stdout to null and stderr to file errorlog.txt |
| `somecommand >> output.txt`                                                                 | append stdout to output.txt                    |
| `somecommand &>> output.txt`<br>`somecommand >>& output.txt`<br>`somecommand >> output.txt` | append stdout and stderr to output.txt         | 


- _The standard stream for `>` is 1 - so `>` and `1>` are equal._
- _The standard stream for `<` is 0 - so `<` and `0<` are equal._
- _It doesn't matter if there's a space after the redirection operator - so `somecommand > output.txt` and `somecommand >output.txt` are the same._

## Testing POSIX compliance without tools

Let's assume the standard shell `/bin/sh` is linked to a POSIX-compliant shell like dash:
```shell
$ ls -l /bin/sh  
lrwxrwxrwx 1 root root 4 Mai  5 14:09 /bin/sh -> dash
```

Use the standard shell Shebang in the script:
```shell
#!/bin/sh
```

Test your script by using sh:
```shell
/bin/sh ~/myscripts/supercompliant.sh
```



## ShellCheck shell script analysis tool

- [ShellCheck website](https://www.shellcheck.net)

Installation:  
```shell
sudo apt install shellcheck -y
```

Syntax example:  
```shell
shellcheck --shell=sh ~/myscripts/supercompliant.sh
```

**TIPP:** ShellCheck can be integrated in many popular editors: https://github.com/koalaman/shellcheck#in-your-editor



## Helpful Links:
- [Wikipedia](https://wikipedia.org/wiki/POSIX)
- [OpenGroup POSIX FAQ](http://www.opengroup.org/austin/papers/posix_faq.html)
- [Baeldung - A Guide to POSIX](https://www.baeldung.com/linux/posix)
- [Baeldung - How to Test for POSIX Compliance](https://www.baeldung.com/linux/test-posix-compliance-shell-scripts)



-------------------
# Special shell variables



|   Variable    | Description                                          |
|:-------------:|:---------------------------------------------------- |
|     `$0`      | Path to the script.                         |
| `$1, $2...$n` | The bash script arguments.                           |
|     `$$`      | The process id of the current shell.                 |
|     `$#`      | The total number of arguments passed to the script.  |
|     `$@`      | The value of all the arguments passed to the script. |
|     `$?`      | The exit status of the last executed command.        | 
|     `$!`      | The process id of the last executed command.         |



Simple test script:  

```shell
#!/bin/bash

echo
echo "# arguments called with ---->  ${@}     "
echo "# \$1 ----------------------->  $1       "
echo "# \$2 ----------------------->  $2       "
echo "# path to me --------------->  ${0}     "
echo "# parent path -------------->  ${0%/*}  "
echo "# my name ------------------>  ${0##*/} "
echo
exit
```

stdout of: `./test.sh "one" "two"`
```text

# arguments called with ---->  one two
# $1 ----------------------->  one
# $2 ----------------------->  two
# path to me --------------->  ./test.sh
# parent path -------------->  .
# my name ------------------>  test.sh

```



-------------------
# Temporary files - `mktemp`

[Man page on man7.org](https://man7.org/linux/man-pages/man3/mktemp.3.html):
>mktemp - create a temporary file or directory

Syntax: `mktemp [OPTION]... [TEMPLATE]`



Best practice for most scripts is to use variables for the path to the file/directory.



Create a temporary file: 
```shell
TEMP_FILE=$(mktemp)
```



Create a temporary directory: 
```shell
TEMP_DIR=$(mktemp -d)
```



Use example:  
```shell
TEMP_FILE=$(mktemp)
ls -la $HOME > $TEMP_FILE
cat $TEMP_FILE
rm -f $TEMP_FILE
```



| Option | Description |
|:------:| ----------- |
| -d     |  create a directory, not a file           |




-------------------
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
read -r -p 'Do you want to start (Y/n): ' VARIABLE
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

## Flags

Example Code for flags a,s,d:

```shell
#!/bin/sh

while getopts a:s:d: flag
do
    case "${flag}" in
        a)
            printf "The a argument was: %s\n" ${OPTARG}
        ;;
        s) 
            printf "The s argument was: %s\n" ${OPTARG}
        ;;
        d)
            printf "The d argument was: %s\n" ${OPTARG}
        ;;
        \?)
            printf "Unknown argument!\n"
        ;;
    esac
done

printf "Your arguments have been processed - bye.\n"
```

Starting the script with flags:

```shell
./example.sh -a awbubfds -s 21317571345 -d "This is my D input"
```

Output:

```shell
The a argument was: awbubfds
The s argument was: 21317571345
The d argument was: This is my D input
Your arguments have been processed - bye.
```


-------------------

# Location of the current script file

The special Variable `$0` equals the location/path of the scrypt-file. But be careful with symlinks. If you execute the symlink, then `$0` equals the location of the symlink, not the actual script-file.

Save path in a variable:
```bash
SCRIPTPATH=`realpath "$0"` # realpath prints the actual path, even when $0 is a symlink
SCRIPT_DIR=`dirname $(realpath "$0")` # dirname strips the last component from a path
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

# Some code snippets

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



## Create directory/file if it doesn't exist

Check for a directory:  
```shell
[[ ! -d $HOME/output ]] && mkdir $HOME/output
```

Check for a file:  
```shell
[[ ! -f ./result.txt ]] && touch ./result.txt
```


## Timestamp variable

```bash
TIMESTAMP=$(date +"%Y.%m.%d_%H.%M")
```

## Check for root privileges

```bash
#!/bin/bash

if [ "$EUID" -ne 0 ]
then
    exec sudo -s "$0" "$@"
fi

# script
# script
# ...

sudo -k
```

- `if [ "$EUID" -ne 0 ]` ▷▷▷ Check if Effective User ID is 0 (root)
- `exec sudo -s "$0" "$@"` ▷▷▷ restart current script with root privileges
- `sudo -k` ▷▷▷ reset sudo timestamp, so password needs to be entered again

## Check if dependency (needed app) is installed

Check if xclip is installed: 
```
[[ -z "$(command -v xclip)" ]] && printf "\n\tPlease install nessecary software: xclip\n\n" && exit 1
```


-----------------
# Useful links

- [ubuntuusers: German Bash Skripting Guide](https://wiki.ubuntuusers.de/Shell/Bash-Skripting-Guide_f%C3%BCr_Anf%C3%A4nger)
- [devhints.io - Bash scripting cheatsheet](https://devhints.io/bash)
- [ShellCheck](https://www.shellcheck.net)