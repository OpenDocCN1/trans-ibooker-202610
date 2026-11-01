It will also print the number of times a line occurs before the line when run with the -c option. The count is right-justified in a field 4 characters wide and is followed by a single space and then the line:

```
$ uniq -c tests/inputs/one.txt
    1 a
```

Given input that contains two duplicate lines, only one line will be emitted:

```
$ cat tests/inputs/two.txt
a
a
$ uniq tests/inputs/two.txt
a
```

The line is shown to have a count of 2:

```
$ uniq -c tests/inputs/two.txt
    2 a
```

A longer input file shows that uniq only considers the lines in order and not globally as the line one shows several times in this input file:

```
$ cat tests/inputs/three.txt
a
a
b
b
a
c
c
c
a
d
d
d
d
```