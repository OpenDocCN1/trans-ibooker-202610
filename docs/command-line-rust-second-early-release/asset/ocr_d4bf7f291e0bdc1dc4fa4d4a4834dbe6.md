-c      Precede each output line with the count of the number of times
the line occurred in the input, followed by a single space.

-d      Only output lines that are repeated in the input.

-f num  Ignore the first num fields in each input line when doing compar-
isons.  A field is a string of non-blank characters separated
from adjacent fields by blanks.  Field numbers are one based,
i.e., the first field is field one.

-s chars
        Ignore the first chars characters in each input line when doing
comparisons.  If specified in conjunction with the -f option, the
first chars characters after the first num fields will be
ignored.  Character numbers are one based, i.e., the first char-
acter is character one.

-u      Only output lines that are not repeated in the input.

-i      Case insensitive comparison of lines.

In the *06_uniq* directory of the Git repository, you will find various input files I'll use for testing. To start, note that `uniq` will print nothing when given an empty file:

```
$ uniq tests/inputs/empty.txt
```

Given a file with just one line, the one line will be printed:

```
$ uniq tests/inputs/one.txt
a
```