# Sed command description

source man sed:
> Sed is a stream editor.  A stream editor is used to perform basic text transformations on an input stream (a file or input from a pipeline).

---------------------

</br>

# Basics: change string in a file

</br>

Change "foo" to "bar" from example.txt:  

```shell
sed 's/foo/bar/' example.txt
```

- _The changes will be printed to stdout, but the file itself stays the same._
- _Delimiter: The character after the `s` is the delimiter - in this case it's a slash `/`. But other characters can be used as well - for example: `.` `;` `|`_
- _foo can be a string or a regular expression._
- Only the first `foo` in each line will be changed. The syntax for all foo is `'s/foo/bar/g'` (g for global)

</br>

Change every "foo" to "bar" in example.txt:  

```shell
sed -i 's/foo/bar/g' example.txt
# or create a backup example.txt.bak, then change original
sed -i.bak 's/foo/bar/g' example.txt
```

|    Option    | Description                          |
|:------------:| ------------------------------------ |
|      -i      | Edit files in place.                 |
| -i\[SUFFIX\] | Create a backup, then edit the file. | 

--------------------------

</br>

# Examples

Read the public ssh key from a file, put a string with options in front and append it to the `~/.ssh/authorized_keys` file: 
```shell
cat ~/.ssh/id_ed25519.pub | sed 's/^/no-port-forwarding,no-X11-forwarding,no-agent-forwarding,no-pty /' >> ~/.ssh/authorized_keys
```