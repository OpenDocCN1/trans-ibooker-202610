```
7a08d672d7cc5')
>>> parent = hash256(hash0 + hash1)
>>> print(parent.hex())
8b30c5ba100f6f2e5ad1e2a742e5020491240f8eb514fe97c713c31718ad7ecd
```

The reason why we hash the concatenation to get the parent is because we can provide a proof of inclusion. Specifically, we can show that L is represented in the parent, P, by revealing R. That is, if we want proof L is represented in P, the producer of P can show us R and let us know that L is the left child of P. We can then combine L and R to produce P and have proof that L was used to produce P. If L is not represented in P, being able to provide R would be the equivalent to providing a hash pre-image, which we know is very difficult. This is what we mean by a proof of inclusion.

# Exercise 1

Write the merkle_parent function.

# Merkle Parent Level

Given an ordered list of more than two hashes, we can calculate the parents of each pair, or what we call the Merkle parent level. If we have an even number of hashes, this is straightforward, as we can simply pair them up in order. If we have an odd number of hashes, then we need to do something, as we have a lone hash at the end. We can solve this by duplicating the last item.

That is, for a list like [A, B, C] we can add C again to get [A, B, C, C]. Now we can calculate the Merkle parent of A and B and calculate the Merkle parent of C and C to get:

[H(A||B), H(C||C)]

Since the Merkle parent always consists of two hashes, the Merkle parent level always has exactly half the number of hashes, rounded up. Here is how we calculate a Merkle parent level:

```
>>> from helper import merkle_parent
>>> hex_hashes = [
...     'c117ea8ec828342f4dfb0ad6bd140e03a50720ece40169ee38bdc15d9eb64cf5',
...     'c131474164b412e3406696da1ee20ab0fc9bf41c8f05fa8ceea7a08d672d7cc5',
...     'f391da6ecfeed1814efa e39e7f c b3838ae0b02c02ae7d0a5848a66947c0727b0',
...     '3d238a92a94532b946c90e19c49351c763696cff3db400485b813aecb8a13181',
...     '10092f2633be5f3ce349bf9ddbde36caa3dd10dfa0ec8106bce23acbff637dae',
... ]
>>> hashes = [bytes.fromhex(x) for x in hex_hashes]
>>> if len(hashes) % 2 == 1:
...     hashes.append(hashes[-1]) ①
>>> parent_level = []
>>> for i in range(0, len(hashes), 2): ②
```