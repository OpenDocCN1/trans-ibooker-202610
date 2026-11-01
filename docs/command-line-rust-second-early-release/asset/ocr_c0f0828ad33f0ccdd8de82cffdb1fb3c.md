egrep can handle extended regular expressions (EREs). See re_format(7) for more information on regular expressions. fgrep is quicker than both grep and egrep, but can only handle fixed patterns (i.e. it does not interpret regular expressions). Patterns may consist of one or more lines, allowing any of the pattern lines to match a portion of the input.

# The GNU version is very similar:

```
GREP(1)                                 General Commands Manual
GREP(1)

NAME
        grep, egrep, fgrep - print lines matching a pattern

SYNOPSIS
        grep [OPTIONS] PATTERN [FILE...]
        grep [OPTIONS] [-e PATTERN | -f FILE] [FILE...]

DESCRIPTION
        grep searches the named input FILES (or standard
input if no files are
        named, or if a single hyphen-minus (-) is given as
file name) for lines
        containing a match to the given PATTERN.  By
default, grep prints the
        matching lines.
```

To demonstrate how grep works, I’ll use the 09_grepr/tests/inputs directory:

```
$ cd 09_grepr/tests/inputs
$ wc -l *
        9 bustle.txt
        0 empty.txt
        1 fox.txt
        9 nobody.txt
       19 total
```