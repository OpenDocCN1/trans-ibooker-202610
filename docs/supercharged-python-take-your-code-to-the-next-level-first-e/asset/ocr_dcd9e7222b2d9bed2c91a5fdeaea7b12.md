```
with open(fname, 'wb') as f:
    bname = name.encode('utf-8')
    baddr = addr.encode('utf-8')
    bss = pack('9s10sf', bname, baddr, rating)
    f.write(bss)

def read_rec(fname):
    with open(fname, 'rb') as f:
        bss = f.read(calcsize('9s10sf'))
        bname, baddr, rating = unpack(
            '9s10sf', bss)
        name = bname.decode('utf-8').rstrip('\x00')
        addr = baddr.decode('utf-8').rstrip('\x00')
        return name, addr, rating
```

# Here’s a sample usage:

> Click here to view code image

```
write_rec('goofy.dat', 'Cleo', 'Main St.', 5.0)
print(read_rec('goofy.dat'))
```

# These statements produce the following tuple, as expected:

```
('Cleo', 'A Str.', 5.0)
```

> ### Note
> The **pack** function has the virtue of putting in internal padding as needed, thereby making sure that data types align correctly. For example, four-byte floating-point values need to start on an address that’s a multiple of 4. In the preceding example, the **pack** function adds extra null bytes so that the floating-point value starts on a properly aligned address.
> 
> However, the limitation here is that even though using the **pack** function aligns everything within a single record, it does not necessarily set up correct writing and reading of the *next* record. If the last item written or read is a string of nonaligned size, then it may be necessary to pad each record with bytes. For example, consider the