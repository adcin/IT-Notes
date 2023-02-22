# Running scripts

## Make script-file executable

```shell
chmod +x /path/to/my_script.sh
```

## Execute script

- Method 1: Enter whole path and filename   
    `/path/to/my_script.sh`
- Method 2: Enter `./` in front of filename    
    `./my_script.sh`
- Method 3: Specify the interpreter    
    `bash my_script.sh`

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

# Useful links

[ubuntuusers: Bash-Skripting-Guide für Anfänger](https://wiki.ubuntuusers.de/Shell/Bash-Skripting-Guide_f%C3%BCr_Anf%C3%A4nger)

