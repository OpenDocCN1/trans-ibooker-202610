A Bloom filter can be optimized by changing the number of hash functions and bit field size to get a desirable false-positive rate.

# BIP0037 Bloom Filters

BIP0037 specifies Bloom filters in network communication. The information contained in a Bloom filter is:

+   1. The size of the bit field, or how many buckets there are. The size is specified in bytes (8 bits per byte) and rounded up if necessary.
+   2. The number of hash functions.
+   3. A “tweak” to be able to change the Bloom filter slightly if it hits too many items.
+   4. The bit field that results from running the Bloom filter over the items of interest.

While we could define lots of hash functions (sha512, keccak384, ripemd160, blake256, etc.), in practice, we use a single hash function with a different seed. This allows the implementation to be simpler.

The hash function we use is called murmur3. Unlike sha256, murmur3 is not cryptographically secure, but it is much faster. The task of filtering and getting a deterministic, evenly distributed modulo does not require cryptographic security but benefits from speed, so murmur3 is the appropriate tool for the job. The seed formula is defined this way:

```
i*0xfba4c795 + tweak
```

fba4c795 is a constant for Bitcoin Bloom filters. i is 0 for the first hash function, 1 for the second, 2 for the third, and so on. The tweak is a bit of entropy that can be added if the results of of one tweak are not satisfactory. The hash functions and the size of the bit field are used to calculate the bit field, which then gets transmitted:

```
>>> from helper import murmur3
>>> from bloomfilter import BIP37_CONSTANT
>>> field_size = 2
>>> num_functions = 2
>>> tweak = 42
>>> bit_field_size = field_size * 8
>>> bit_field = [0] * bit_field_size
>>> for phrase in (b'hello world', b'goodbye'):
...     for i in range(num_functions):
...         seed = i * BIP37_CONSTANT + tweak
...         h = murmur3(phrase, seed=seed)
...         bit = h % bit_field_size
...         bit_field[bit] = 1
>>> print(bit_field)
[0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0]
```