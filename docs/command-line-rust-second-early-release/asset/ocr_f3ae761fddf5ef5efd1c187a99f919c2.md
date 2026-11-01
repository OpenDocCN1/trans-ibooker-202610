in the first or second column, respectively. When they are the same, I need to print the value in column 3. When the first value is less than the second, I should print the first value in column 1; otherwise, I should print the second value in column 2. To understand this last point, consider the following two input files which I’ll place side by side so you can imagine how the code will read the lines:

```
$ cat tests/inputs/file1.txt      $ cat
tests/inputs/file2.txt
 a                                 B
 b                                 C
 c
 d
```

To help you see the output from BSD comm, I will pipe the output into Perl to replace the tab characters with the string ---> to make it clearer which columns are being printed:

```
$ comm tests/inputs/file1.txt tests/inputs/file2.txt | perl
-pe "s/\t/--->/g"
--->B
a
b
--->--->c
d
```

Now imagine your code reads the first line from each input and has a from file1.txt and B from file2.txt. They are not equal, so the question is which to print. The goal is to mimic BSD comm, so I know that the B should come first and be printed in the second column. When I compare a and B, I find that a is greater than B when they are ordered by their code point or numerical value. To help you see this, I’ve included a program in util/ascii that will show you a range of the ASCII table starting at the first printable character. Note that a has a value of 97 while B is 66:

```
33: !   52: 4   71: G   90: Z   109: m
34: "   53: 5   72: H   91: [   110: n
```