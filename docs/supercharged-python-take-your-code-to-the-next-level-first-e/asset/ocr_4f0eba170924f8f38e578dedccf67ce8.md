```
f.write(bytes([1, 2, 3, 0x10]))
```

The file can then be closed, and you can read back these same byte values.

Click here to view code image

```
with open('my.dat', 'rb') as f:
    bss = f.read()
    for i in bss:
        print(i, end=' ')
```

This example code prints

```
1 2 3 16
```

Most of the time, putting individual byte values into a file,
one at a time, is not likely to be a good way to support Python
applications or even examining existing file formats. Individual
byte values range from 0 to 255. But larger values require
combinations of bytes; there is no universal, clean
correspondence between these values and Python objects,
especially as factors such as “little endian” and data-field size
change everything. This raises questions of portability.
Fortunately, the struct, pickle, and shelve packages all
facilitate the transfer of data to and from binary files at a higher
level of abstraction. You’ll almost always want to use one of
those packages.