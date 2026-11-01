```
```
./a/b/c/c.mp3
./f/f.txt
./d/d.txt
./d/d.tsv
./d/e/e.mp3
```
```

I can use “l” to only find *links*:

```
```
$ find . -type l
./d/b.csv
```
```

I can also use “d” to only find *directories*:

```
```
$ find . -type d
.
./a
./a/b
./a/b/c
./f
./d
./d/e
```
```

While the challenge program will only try to find these types, `find` will accept several more `-type` values per the manual page:

```
```
-type t
    True if the file is of the specified type.  Possible
file types
    are as follows:

    b    block special
    c    character special
    d    directory
    f    regular file
    l    symbolic link
    p    FIFO
    s    socket
```
```

If you give a `-type` value not found in this list, `find` will stop with an error: