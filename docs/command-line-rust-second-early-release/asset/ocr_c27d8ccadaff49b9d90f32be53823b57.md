For example, I can count *tests/inputs/two.txt* and place the output into *out*:

```
$ uniq -c tests/inputs/two.txt out
$ cat out
    2 a
```

With no positional arguments, *uniq* will read from STDIN by default:

```
$ cat tests/inputs/two.txt | uniq -c
    2 a
```

If you want to read from STDIN *and* indicate the output filename, you must use a dash "-" for the input filename:

```
$ cat tests/inputs/two.txt | uniq -c - out
$ cat out
    2 a
```

The GNU version works basically the same while also providing many more options:

```
$ uniq --help
Usage: uniq [OPTION]... [INPUT [OUTPUT]]
Filter adjacent matching lines from INPUT (or standard input),
writing to OUTPUT (or standard output).

With no options, matching lines are merged to the first occurrence.

Mandatory arguments to long options are mandatory for short options too.
    -c, --count            prefix lines by the number of
occurrences
    -d, --repeated         only print duplicate lines, one for
each group
    -D, --all-repeated[=METHOD]  print all duplicate lines
                            groups can be delimited with an
empty line
```