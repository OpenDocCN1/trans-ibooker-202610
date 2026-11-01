output. This will cancel out any prior usage
of the -m option.

-l      The number of lines in each input file is
written to the standard
            output.

-m      The number of characters in each input file is
written to the
            standard output.  If the current locale does
not support multi-
            byte characters, this is equivalent to the -c
option.  This will
            cancel out any prior usage of the -c option.

-w      The number of words in each input file is
written to the standard
            output.

When an option is specified, wc only reports the
information requested by
    that option.  The order of output always takes the
form of line, word,
    byte, and file name.  The default action is equivalent
to specifying the
    -c, -l and -w options.

If no files are specified, the standard input is used
and no file name is
    displayed.  The prompt will accept input until
receiving EOF, or [^D] in
    most environments.

A picture is worth a kilobyte of words, so I'll show you some
examples using the test files in the 05_wcr directory. First, consider
an empty file that should report 0 lines, words, and bytes, each of
which should be right-justified in a field 8 characters wide:

```
$ cd 05_wcr
$ wc tests/inputs/empty.txt
      0       0       0 tests/inputs/empty.txt
```