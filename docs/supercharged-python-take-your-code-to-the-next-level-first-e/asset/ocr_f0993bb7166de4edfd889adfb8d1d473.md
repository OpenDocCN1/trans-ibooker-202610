```
from struct import pack, unpack, calcsize

def write_floats(fname, x, y, z):
    with open(fname, 'wb') as f:
        bss = pack('fff', x, y, z)
        f.write(bss)

def read_floats(fname):
    with open(fname, 'rb') as f:
        bss = f.read(calcsize('fff'))
        return unpack('fff', bss)
```

Note that `'fff'` can be replaced by `'3f'` in this example.
The next example shows how you’d use these functions to read
and write three floating-point numbers at a time.

Click here to view code image

```
write_floats('silly.dat', 1, 2, 3.14)
x, y, z = read_floats('silly.dat')
print(x, y, z, sep=' ')
```

The three values are printed—the last with a noticeable
rounding error.

Click here to view code image

```
1.0   2.0   3.140000104904175
```

# ## 8.11.3 Writing and Reading a Fixed-Length String

Strings, which you might think should be simplest of all to
handle, present special problems for binary storage. First,