# gawk - pattern scanning and processing language

Description (excerpt from `man` page):  
> Gawk  is  the  GNU Project’s implementation of the AWK programming language.

- [GNU.org - GAWK: Effective AWK Programming: A User’s Guide for GNU Awk](https://www.gnu.org/software/gawk/manual/html_node/index.html)
- [Man7.org - gawk(1) — Linux manual page](https://man7.org/linux/man-pages/man1/gawk.1.html)

<br>

Create a testfile:  
```shell
printf "alpha beta gamma\ndelta epsilon zeta\neta theta iota\n" > FILE.TXT
```
```shell
marcin@server:~$ cat FILE.TXT
alpha beta gamma
delta epsilon zeta
eta theta iota
```

<br>

Print the content of FILE.TXT
```shell
awk '{print}' FILE.TXT
```
```shell
alpha beta gamma
delta epsilon zeta
eta theta iota
```

<br>

Print the 2nd field of every line:  
```shell
awk '{print $2}' FILE.TXT
```
```shell
beta
epsilon
theta
```

<br>

Print field 1 and 3:  
```shell
awk '{print $1,$3}' FILE.TXT
```
```shell
alpha gamma
delta zeta
eta iota
```

<br>

Print the last field:  
```shell
awk '{print $NF}' FILE.TXT
```
```shell
gamma
zeta
iota
```
_The variable `NF` refers to the total number of fields._

<br>

Change field 1 to a - (minus):  
```shell
awk '{ $1 = "-"; print $0 }' FILE.TXT
```
```shell
- beta gamma
- epsilon zeta
- theta iota
```