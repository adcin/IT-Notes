# `tee` - read from standard input and write to standard output and files

Useful, if you want to see the output of a command/script and also save it in a file. But it can do much more tricky things, I won't cover here for now - check out the links ;).

- [GNU.org - tee](https://www.gnu.org/software/coreutils/manual/html_node/tee-invocation.html#tee-invocation)

Basic usage:
```shell
echo "This is just some random text." | tee ~/my-random.txt
```

Append to a file with `-a` option:
```shell
echo "Even more rendom text!" | tee -a ~/my-random.txt
```

> [!quote]- &nbsp;`$ tee --help`
> 
> ```
> Usage: tee [OPTION]... [FILE]...
> Copy standard input to each FILE, and also to standard output.
> 
>   -a, --append              append to the given FILEs, do not overwrite
>   -i, --ignore-interrupts   ignore interrupt signals
>   -p                        operate in a more appropriate MODE with pipes.
>       --output-error[=MODE]   set behavior on write error.  See MODE below
>       --help        display this help and exit
>       --version     output version information and exit
> 
> MODE determines behavior with write errors on the outputs:
>   warn           diagnose errors writing to any output
>   warn-nopipe    diagnose errors writing to any output not a pipe
>   exit           exit on error writing to any output
>   exit-nopipe    exit on error writing to any output not a pipe
> The default MODE for the -p option is 'warn-nopipe'.
> With "nopipe" MODEs, exit immediately if all outputs become broken pipes.
> The default operation when --output-error is not specified, is to
> exit immediately on error writing to a pipe, and diagnose errors
> writing to non pipe outputs.
> 
> GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
> Report any translation bugs to <https://translationproject.org/team/>
> Full documentation <https://www.gnu.org/software/coreutils/tee>
> or available locally via: info '(coreutils) tee invocation'
> ```

