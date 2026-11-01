I can use -v|--invert-match option to find the lines that don't match the pattern:

```
$ grep -v Nobody nobody.txt
Then there's a pair of us!
Don't tell! they'd advertise–you know!

How dreary–to be–Somebody!
How public–like a Frog–
To tell one's name–the livelong June–
To an admiring Bog!
```

The -c|--count option will cause the output to be a summary of the number of times a match occurs:

```
$ grep -c Nobody nobody.txt
2
```

I can combine -v and -c to count the lines not matching:

```
$ grep -vc Nobody nobody.txt
7
```

When searching multiple input files, the output includes filename:

```
$ grep The *.txt
bustle.txt:The bustle in a house
bustle.txt:The morning after death
bustle.txt:The sweeping up the heart,
fox.txt:The quick brown fox jumps over the lazy dog.
nobody.txt:Then there's a pair of us!
```

The filename is also included for the counts:

```
$ grep -c The *.txt
bustle.txt:3
empty.txt:0
fox.txt:1
nobody.txt:1
```