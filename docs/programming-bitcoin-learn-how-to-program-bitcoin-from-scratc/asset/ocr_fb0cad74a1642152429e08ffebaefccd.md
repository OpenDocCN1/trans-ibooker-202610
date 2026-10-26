A Bloom filter is a computer science structure that can be used on any data in a set, so suppose that we have one item, “hello world”, that we want to create a Bloom filter for. We need a hash function, so we’ll use one we already have: hash256. The process of figuring out what bucket our item goes into looks like this:

```
>>> from helper import hash256
>>> bit_field_size = 10
>>> bit_field = [0] * bit_field_size
>>> h = hash256(b'hello world')
>>> bit = int.from_bytes(h, 'big') % bit_field_size
>>> bit_field[bit] = 1
>>> print(bit_field)
[0, 0, 0, 0, 0, 0, 0, 0, 0, 1]
```

+   1. Our bit_field is the list of “buckets,” and we want there to be 10.
+   2. We hash the item with hash256.
+   3. We interpret this as a big-endian integer and modulo by 10 to determine the bucket this item belongs to.
+   4. We indicate the bucket we want in the Bloom filter.

Conceptually, what we just did looks like Figure 12-1.

![](bbox=[0.13, 0.5, 0.85, 0.72])

Figure 12-1. 10-bit Bloom filter with one element

Our Bloom filter consists of:

+   1. The size of the bit field
+   2. The hash function used (and how we converted that to a number)
+   3. The bit field, which indicates the bucket we’re interested in