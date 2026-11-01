What you will learn:

+   - How to read and write delimited text file using the `csv` module
+   - How to dereference a value using `*`
+   - More on using `Iterator::filter_map` to combine `filter` and `map` operations

# How cut Works

First let's review a portion for the BSD version of `cut`:

```
CUT(1)                                      BSD General Commands Manual
CUT(1)

NAME
    cut -- cut out selected portions of each line of a
file

SYNOPSIS
    cut -b list [-n] [file ...]
    cut -c list [file ...]
    cut -f list [-d delim] [-s] [file ...]

DESCRIPTION
    The cut utility cuts out selected portions of each
line (as specified by
    list) from each file and writes them to the standard
output.  If no file
    arguments are specified, or a file argument is a
single dash ('-'), cut
    reads from the standard input.  The items specified by
list can be in
    terms of column position or in terms of fields
delimited by a special
    character.  Column numbering starts from 1.

    The list option argument is a comma or whitespace
separated set of num-
    bers and/or number ranges.  Number ranges consist of a
```