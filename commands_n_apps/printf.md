# `printf`
Printf (print formatted) is a tool to format and print data.

- [gnu.org - printf documentation](https://www.gnu.org/software/coreutils/printf)
- [gnu.org - formatted output](https://www.gnu.org/software/libc/manual/html_node/Formatted-Output.html)
- [man7.org - printf(1)](https://man7.org/linux/man-pages/man1/printf.1.html)
- [man7.org - printf(3)](https://man7.org/linux/man-pages/man3/fprintf.3.html)
- [wkipedia.org - printf](https://en.wikipedia.org/wiki/Printf)

> [!note]
> 
> Printf is a very powerful tool with a lot of features. I describe just a few of them, which I use or have used. These notes are far from being a complete description of `printf`.

# Syntax

Mostly used syntax:  
```
printf FORMAT [ARGUMENT]...
```

If needed to print into a variable (var) instead of the stdout:
```
printf -v var FORMAT [ARGUMENT]...
```

# Format sequences

|   Sequence   | Description                                                      |
| :----------: | ---------------------------------------------------------------- |
|     `\"`     | double quote                                                     |
|     `\\`     | backslash                                                        |
|     `\a`     | alert (bell)                                                     |
|     `\b`     | backspace                                                        |
|     `\c`     | produce no further output                                        |
|     `\e`     | escape                                                           |
|     `\f`     | form feed                                                        |
|     `\n`     | new line                                                         |
|     `\r`     | carriage return                                                  |
|     `\t`     | horizontal tab                                                   |
|     `\v`     | vertical tab                                                     |
|    `\NNN`    | byte with octal value NNN (1 to 3 digits)                        |
|    `\xHH`    | byte with hexadecimal value HH (1 to 2 digits)                   |
|   `\uHHHH`   | Unicode (ISO/IEC 10646) character with hex value HHHH (4 digits) |
| `\UHHHHHHHH` | Unicode character with hex value HHHHHHHH (8 digits)             |

# Arguments

In printf you put "conversion specifiers" into the format/text and the arguments/variables behind the format/text. That's the biggest syntax difference to `echo`:

```
printf "text <conv-specs_1> more text <conv-specs_2>" <Argument_1> <Argument_2>
```

##### Common conversion specifiers:

|  Specifier   | Conversion                                                              |
| :----------: | ----------------------------------------------------------------------- |
|     `%s`     | Argument converted to String                                            |
| `%d` or `%i` | Integer converted to decimal integer (signed)                           |
|     `%u`     | Integer converted to decimal integer (unsigned)                         |
| `%x` or `%X` | Integer converted to hexadecimal integer (`x` lower and `X` upper case) |
|     `%f`     | Argument converted to floating point number                             |
|     `%%`     | A literal percentage symbol "%"                                         |

Example with string argument:  
```shell
printf "Hello %s\!\n" "World"
# same result as
VAR="World"
printf "Hello %s\!\n" $VAR
```
Output: 
```
Hello World!
```

_I'm using "`\!`", because the exclamation mark "`!`" might be interpreted as an escape character in some shells._

# Conversion specifiers
- [gnu.org - formatted output](https://www.gnu.org/software/libc/manual/html_node/Formatted-Output.html)

The conversion feature of `printf` is much more powerful, than described before. I will now describe a few methods, which I use regularly. For more info, check the link above to the GNU online manual. 

The full syntax is ([quote gnu.org](https://www.gnu.org/software/libc/manual/html_node/Output-Conversion-Syntax.html)): 
```
% [ param-no $] flags width [ . precision ] type conversion
or
% [ param-no $] flags width . * [ param-no $] type conversion
```

Syntax we focus on:
```
%[flags][width][.precision]conversion
```

|     Part     | Description                                                                                                                                                                                                                                            |     |
| :----------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --- |
|     `%`      | Indicates the start of a conversion specifier                                                                                                                                                                                                          |     |
|   `flags`    | Modifies the normal behavior of the specifier (e.g `-` lets a string align left when a `width` is defined, default alignment is right)                                                                                                                 |     |
|   `width`    | Defines the minimal field width. If the normal conversion would be shorter than the defined width, then the remaining field is padded with spaces "` `" by default. Normally a wider input would not be truncated, unless you specify the `precision`. |     |
| `.precision` | Specifies usually the number of digits. But can also be used to truncate strings.                                                                                                                                                                      |     |
| `conversion` | The [conversion specifiers](printf.md#Common%20conversion%20specifiers)                                                                                                                                                                                |     |

Example script:
```shell
#!/bin/bash

tbl_content() # Attributes - 1:Date 2:Location 3:°C 4:kWh
{
    printf "| %-10s | %-15s | %5.2f | %8.6f |\n" $1 $2 $3 $4
}

printf "| %-10s | %-15s | %6s | %8s |\n" "Date" "Location" "°C" "kWh"
printf "|:%-10s |:%-15s | %5s:| %8s:|\n" | tr " " "-"

tbl_content "2024-04-13" "srv-asl_03.12" 67,987 2,98765654
```
_This script generates a report in markdown format, about the temperature of server racks and the power consumption of the associated air conditioning systems. For a real report there should be some kind of a loop instead of the last line, but you get the point._

Output:
```markdown
| Date       | Location        |    °C |      kWh |
|:-----------|:----------------|------:|---------:|
| 2024-04-13 | srv-asl_03.12   | 67,99 | 2,987657 |
```

Experiments with float number conversion:
```shell
FLOAT=0,987654321

echo ""
echo "Individual tests (number: $FLOAT):" 
printf "\n\t%s | %s\n" "Conversion" "Result"
printf "\t%-10s |\n"
printf "\t%-10s | %s\n" "%s" $FLOAT
printf "\t%-10s | %.s\n" "%.s" $FLOAT
printf "\t%-10s | %f\n" "%f" $FLOAT
printf "\t%-10s | %.f\n" "%.f" $FLOAT

echo ""
echo "Float precision loops (number: $FLOAT):" 
printf "\n\t%s | %s\n" "Conversion" "Result"
printf "\t%-10s |\n"
for ((i = 0 ; i < 13 ; i++)); do
  printf "\t%-10s | %.*f\n" "$(printf "%s%s%s" "%." $i "f")" $i $FLOAT
done

echo ""
echo "String precision loops (number: $FLOAT):" 
printf "\n\t%s | %s\n" "Conversion" "Result"
printf "\t%-10s |\n"
for ((i = 0 ; i < 13 ; i++)); do
  printf "\t%-10s | %.*s\n" "$(printf "%s%s%s" "%." $i "s")" $i $FLOAT
done

echo ""
echo "Float width loops with precision of 5 (number: $FLOAT):" 
printf "\n\t%s | %s\n" "Conversion" "Result"
printf "\t%-10s |\n"
for ((i = 0 ; i < 13 ; i++)); do
  printf "\t%-10s | %*.5f\n" "$(printf "%s%s%s" "%" $i ".5f")" $i $FLOAT
done

echo ""
echo "String width loops with precision of 5 (number: $FLOAT):" 
printf "\n\t%s | %s\n" "Conversion" "Result"
printf "\t%-10s |\n"
for ((i = 0 ; i < 13 ; i++)); do
  printf "\t%-10s | %*.5s\n" "$(printf "%s%s%s" "%" $i ".5s")" $i $FLOAT
done
```
_You can copy & paste the whole thing in your terminal and let it run. Worked for me. ^-^_

# Examples

Draw a horizontal line over the whole width of the terminal:
```shell
printf %$(tput cols)s | tr " " "-"
```

Create a table (markdown-style):
```shell
printf "| %-15s | %-15s |\n" "Header-1" "Header-2"
printf "| %15s | %15s |\n" | tr " " "-"
printf "| %-15s | %-15s |\n" "Value1-1" "Value1-2"
printf "| %-15s | %-15s |\n" "Value2-1" "Value2-2"
```

Output:
```markdown
| Header-1        | Header-2        |
|-----------------|-----------------|
| Value1-1        | Value1-2        |
| Value2-1        | Value2-2        |
```

##### Command autopsy:

`printf "| %-15s | %-15s |\n" "Header-1" "Header-2"` (and also `printf "| %-15s | %-15s |\n" "Value1-1" "Value1-2"` etc.)
- `printf` - Any questions? ^^
- `"| ` - Print the vertical bar and a space `| `
- `%-15s` - This one step by step:
    - `%s` - Print the argument "Header-x" as a sting.
    - `%15s` - Print it in a 15 character wide section and pad the unused space of this section with blanks ` `.
    - `%-15s` - Align it to the left (default is right alignment).
- `|\n` - Print a vertical bar `|` and start a new line `\n`
`printf "| %15s | %15s |\n" | tr " " "-"` (only the tricky parts)
- `| %15s |` - Lets see:
    - `%15s` creates a 15 character wide section for a argument. But because there is no argument, the space is filled with 15 blanks "` `".
    - The vertical bars `|` and spaces "` `" have no special meaning, they're printed literally.
- `| tr` - The `printf` command is piped `|` to the `tr` command
- `tr " " "-"` - Changes (translates) the blanks `" "` from the `printf` output into minus signs `"-"` and prints everything to the standard output.
And voila.

# printf vs echo

##### printf:
- more powerful in formatting
- usually POSIX compliant
##### echo:
- easier to use
- better readability in code