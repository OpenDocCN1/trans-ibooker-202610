I must use parentheses to group the -type arguments when the - name condition follows an or expression:

```
$ find . \( -type f -o -type l \) -name "*.csv"
./g.csv
./a/b/b.csv
./d/b.csv
```

I can also list multiple search paths as positional arguments:

```
$ find a/b d -name "*.mp3"
a/b/c/c.mp3
d/e/e.mp3
```

If the given search path is an invalid directory, find will print an error:

```
$ find blargh
find: blargh: No such file or directory
```

I find it odd that find accepts files as a path argument, simply printing the filename:

```
$ find a/a.txt
a/a.txt
```

The challenge program, however, will only accept readable directory names as valid arguments. While find can do much more, this is as much as you will implement in this chapter.

# Getting Started

The program you write will be called findr (pronounced find-er), and I recommend you run cargo new findr to start. Update Cargo.toml with the following: