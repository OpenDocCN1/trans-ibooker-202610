+   - How to use `Iterator::collect` to turn an iterator into a vector

# How wc Works

Here is an excerpt from the BSD wc manual page that describes how the tool works:

```
WC(1)                                          BSD General Commands Manual
WC(1)

NAME
    wc -- word, line, character, and byte count

SYNOPSIS
    wc [-clmw] [file ...]

DESCRIPTION
    The wc utility displays the number of lines, words,
and bytes contained
    in each input file, or standard input (if no file is
specified) to the
    standard output.  A line is defined as a string of
characters delimited
    by a <newline> character.  Characters beyond the final
<newline> charac-
    ter will not be included in the line count.

    A word is defined as a string of characters delimited
by white space
    characters.  White space characters are the set of
characters for which
    the iswspace(3) function returns true.  If more than
one input file is
    specified, a line of cumulative counts for all the
files is displayed on
    a separate line after the output for the last file.

    The following options are available:

        -c      The number of bytes in each input file is
written to the standard
```