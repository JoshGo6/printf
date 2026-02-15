# `printf`

This article contains the following topics:

- [Introduction](#Introduction)
- [Worked examples](#Worked%20examples)
- [Format strings](#Format%20strings)
- [Reference examples](#Reference%20examples)

## Introduction

Unlike `echo`, `printf` does *not* automatically add a newline, is consistent across shells, and gives you precise formatting control through the following syntax:

```bash
printf <format-string> <value>...
```

The format string is a template that contains regular text, format specifiers for variables, and escape sequences. It does not contain the variable values themselves. These are placed after the format string in the order that they're referenced. 

## Worked examples

Let's take a look at an example with three `%d` placeholders and three values. When you have multiple format specifiers and multiple values, the first value listed corresponds to the first format specifier, the second format specifier corresponds to the second format specifier, and so forth, as shown here:

```shellsession
$ first_var=1 && second_var=2 && third_var=3
$ printf "The first var is %d, the second var is %d, and the third var is %d.\n" "$first_var" "$second_var" "$third_var"
The first var is 1, the second var is 2, and the third var is 3.
```

You can use `%-13s` to produce a placeholder that's left justified with a fixed width of 13 characters. That way, if you have a number of items of varying width that need to be in a column, all of these items take up the same amount of space before beginning the second field, producing tabular data:

```shellsession
$ var1=coconuts && var2=blueberries && num1=6 && num2=7
$ printf "%-13s%d\n%-13s%d\n" "$var1" "$num1" "$var2" "$num2"
coconuts     6  
blueberries  7
```

If the items in the second field vary in width, as well, you can use the same process to create left-justified output for that field, too:

```shellsession
$ var1=coconuts: && var2=blueberries: && num1=6 && num2=367
$ printf "%-13s%-5d%s\n%-13s%-5d%s\n" "$var1" "$num1" "items" "$var2" "$num2" "containers"
coconuts:    6    items
blueberries: 367  containers
```

## Format strings

The following table contains a sample of format strings you can use with `printf`. 

| Specifier | Meaning                                            |
| --------- | -------------------------------------------------- |
| `%s`      | String                                             |
| `%d`      | Integer (base 10)                                  |
| `\n`      | New line (escape sequence)                         |
| `%-3d`    | Left-align number in 3-character width             |
| `%5d`     | Right-align number in 5-character width            |
| `%05d`    | 5-characters wide  padded (on the left) with zeros |
| `%q`      | Non-rendered representation of escape sequences.   |

> [!NOTE]
> This table is not an exhaustive list, and some of the examples here are meant to illustrate possibilities, so that you can create format strings that work for you. For example, by varying `%-3d` to `%-6d`, you would change the width from three characters to six characters. 

## Reference examples

The following code samples show various ways to use `printf` that you can customize for your own needs:

```shellsession
# Print a string
$ declare name="Mary"
$ printf "Hello, %s.\n" "$name"
Hello, Mary.

# Print an integer
$ declare -i count=5
$ printf "Pick up %d dozen eggs.\n" "$count"
Pick up 5 dozen eggs.

# Print multiple variables
$ declare first="Mary" && declare last="Smith"
$ printf "Greetings, %s %s.\n" "$first" "$last"
Greetings, Mary Smith.

# Print without adding a newline
$ declare variable="Loading..."
$ printf "%s" "$variable"
Loading...
[No newline added in output]

# Extract information from the output of a command and print the info
$ read perms_file link_nums file_owner rest_of_output < <(ls -l "printf.md")
$ printf "The file has permissions %s, contains %d hard links, and is owned by %s.\n" "$perms_file" "$link_nums" "$file_owner"
The file has permissions -rw-rw-r--, contains 2 hard links, and is owned by josh.

# Print in fixed-width (tabular) format
# name is a left-aligned string of width 20
# size is a right-aligned integer of width 10
$ declare name="Document.md" && declare -i size=2048
$ printf "%-20s %10d bytes\n" "$name" "$size"
Document.md                2048 bytes

# Print zero-padded integers
$ num1=7 && num2=23 && num3=347 
$ printf "%03i\n%03i\n%03i\n" "$num1" "$num2" "$num3"
007
023
347

# Show the characters in IFS, but don't render non-printing characters.
$ printf "%q\n" "$IFS"
$' \t\n'
# The dollar sign is part of the output, and indicates Ansi-C quoting syntax.
```


