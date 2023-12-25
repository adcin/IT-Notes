# expect - programmed dialogue with interactive programs

Expect is a tool for automation of text/terminal based applications. In simple terms it listens for output and if the 'expected' output comes the predefined action is executed.

<br>

**Description (excerpt from `man` page): **

> Expect is a program that "talks" to other interactive programs according  to  a script.  Following the script, Expect knows what can be expected from a program and what the correct response should be.
> 
>  An interpreted language provides branching and  high‐level  control  structures  to direct the dialogue.  In addition, the user can take control and interact directly when desired, afterward returning control to the script.

<br>

**Some links:**  
- [die.net - man page](https://linux.die.net/man/1/expect)
- [phoenixnap.com - Linux expect Command With Examples](https://phoenixnap.com/kb/linux-expect)
- [geeksforgeeks.org - expect command in Linux with Examples](https://www.geeksforgeeks.org/expect-command-in-linux-with-examples/)
- [baeldung.com - Tag: expect](https://www.baeldung.com/linux/tag/expect)
- [wikipedia.org - Expect](https://en.wikipedia.org/wiki/Expect)

# Basic usage

**Create test-file `expect-test1.sh` and make it executable:**  
```shell
cat << EOF | tee ./expect-test1.sh
#!/bin/bash
echo "What is your name?"
read NAME
echo "What is your hobby?"
read HOBBY
echo "Hi \$NAME, so you like \$HOBBY?"
EOF
chmod +x ./expect-test1.sh
```

<br>

**Execute `expect-test1.sh` normally:**  
```shell
./expect-test1.sh
```

<br>

**Create expect script `expect-test2.exp`:**  
```shell
cat << FOE | tee ./expect-test2.exp
#!/usr/bin/expect
set timeout -1
spawn ./expect-test1.sh
expect "What is your name?"
send -- "Bugs Bunny\r"
expect "What is your hobby?"
send -- "Teasing Doc\r"
expect eof
FOE
chmod +x ./expect-test2.exp
```

<br>

**Output when executing `./expect-test2.exp`:**  
```text
$ ./expect-test2.exp
spawn ./expect-test1.sh
What is your name?
Bugs Bunny
What is your hobby?
Teasing Doc
Hi Bugs Bunny, so you like Teasing Doc?
```

<br>

**Some common expect commands:**  

| Command  | Description                           |
|:--------:| ------------------------------------- |
|  spawn   | Creates a new process.                | 
|   send   | Sends a reply to the program.         |
|  expect  | Waits for output.                     |
| interact | Enables interacting with the program. |