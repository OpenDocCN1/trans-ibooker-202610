# SEE ALSO

csh(1), sh(1), false(1)

# STANDARDS

The true utility conforms to IEEE Std 1003.2-1992 ('`POSIX.2''').

BSD
June 27, 1991
BSD

As the documentation shows, this program does nothing successfully. If I run true, I see nothing, but I can inspect the bash variable $? to see the exit status of the most recent program:

```
$ true
$ echo $?
0
```

The false command is a corollary in that it “always exits with a nonzero exit code”:

```
$ false
$ echo $?
1
```

All the programs you will write in this book will be expected to return the value 0 when they terminate normally and a nonzero value when there is an error. I can write my own versions of true and false to show you how to do this. Start by creating a src/bin directory, then create src/bin/true.rs with the following contents:

```
$ cat src/bin/true.rs
fn main() {
    std::process::exit(0); ①
}
```

①