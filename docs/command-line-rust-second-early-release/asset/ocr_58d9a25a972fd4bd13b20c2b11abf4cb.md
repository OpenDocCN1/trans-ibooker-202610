```
a
b
c
d
```

Ensure that you get the same output (but now in column 2) if you reverse the argument order:

```
$ comm empty.txt file1.txt
    a
    b
    c
    d
```

Next, try with file1.txt and file2.txt. Note that the following is the output from BSD comm and is the expected behavior for the challenge program:

```
$ comm file1.txt file2.txt
        B
a
b
        c
d
```

GNU comm uses a different ordering for which lines to show first when they are not equal:

```
$ comm file1.txt file2.txt
a
b
        B
            c
d
```

This is one of those programs that was always rather mysterious to me until I wrote my own version. I suggest you start by trying to read a line from each file. The documentation for BufRead::lines notes that it will return a None when it reaches the end of the file.