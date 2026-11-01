+   - How to write and run a unit test
+   - How to use a guard with a `match` arm
+   - How to convert between types using `From`, `Into`, and `as`
+   - How to use `take` on an iterator or a filehandle
+   - How to preserve line endings while reading a filehandle
+   - How to read bytes from a filehandle

# How head Works

You should keep in mind that there are many implementations of the original AT&T Unix operating system, such as BSD (Berkeley Standard Distribution), SunOS/Solaris, HP-UX, and Linux. Most of these operating systems have some version of a `head` program that will default to showing the first ten lines of one or more files. Most will probably have options `-n` to control the number of lines shown and `-c` to instead show some number of bytes. The BSD version has only these two options, which I can see via `man head`:

```
HEAD(1)                                 BSD General Commands Manual
HEAD(1)

NAME
        head -- display first lines of a file

SYNOPSIS
        head [-n count | -c bytes] [file ...]

DESCRIPTION
        This filter displays the first count lines or bytes of
each of the speci-
        fied files, or of the standard input if no files are
specified.  If count
        is omitted it defaults to 10.

        If more than a single file is specified, each file is
```