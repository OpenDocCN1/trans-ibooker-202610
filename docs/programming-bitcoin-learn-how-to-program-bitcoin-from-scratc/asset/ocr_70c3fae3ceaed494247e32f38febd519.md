```
>>> print('BIP141: {}'.format(b.version >> 1 & 1 == 1))
BIP141: True
```

1 The >> operator is the right bit-shift operator, which throws away the rightmost 29 bits, leaving just the top 3 bits. The 0b001 is a way of writing a number in binary in Python.

2 The & operator is the “bitwise and” operator. In our case, we right-shift by 4 bits first and then check that the rightmost bit is 1.

3 We shift 1 to the right because BIP0141 was assigned to bit 1.

# Exercise 6

Write the bip9 method for the Block class.

# Exercise 7

Write the bip91 method for the Block class.

# Exercise 8

Write the bip141 method for the Block class.

# Previous Block

All blocks have to point to a previous block. This is why the data structure is called a blockchain. Blocks link back all the way to the very first block, or the genesis block. The previous block field ends in a bunch of 00 bytes, which we will discuss more later in this chapter.

# Merkle Root

The Merkle root encodes all the ordered transactions in a 32-byte hash. We will discuss how this is important for simplified payment verification (SPV) clients and how they can use the Merkle root along with data from the server to get a proof of inclusion in Chapter 11.

# Timestamp

The timestamp is a Unix-style timestamp taking up 4 bytes. Unix timestamps are the number of seconds since January 1, 1970. This timestamp is used in two places: for validating timestamp-based locktimes on transactions included in the block and for calculating a new bits/target/difficulty every 2,016 blocks. The locktimes were at one point used directly for transactions within a block, but BIP0113 changed the behavior