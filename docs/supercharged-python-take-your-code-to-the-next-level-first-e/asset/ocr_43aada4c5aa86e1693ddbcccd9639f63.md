because you cannot assume that Python strings use single-byte format, it’s necessary to encode or decode them to get **bytes** strings.

Second, because strings vary in length, using binary operations raises the question of just how many characters you should read or write! This is a nontrivial problem. But there are at least two solutions. One solution is to specify how many characters to read or write as part of a function call.

Click here to view code image

```
from struct import pack, unpack, calcsize
def write_fixed_str(fname, n, s):
    with open(fname, 'wb') as f:
        bss = pack(str(n) + 's', s.encode('utf-8'))
        f.write(bss)
def read_fixed_str(fname, n):
    with open(fname, 'rb') as f:
        bss = f.read(n)
        return bss.decode('utf-8')
```

This pair of functions must agree ahead of time on precisely how long the string is to be read or written. So they must be perfectly in sync.

When the `write_fixed_str` calls the **pack** function, that function automatically truncates or pads the string (with additional null bytes) so that it comes out to length n.

Click here to view code image

```
write_fixed_str('king.d', 13, "I'm Henry the VIII I
```