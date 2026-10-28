However, it is to be noted that although SHA-3 offers the same digest sizes of its predecessor SHA-2, it also has two new variants to its algorithm: *SHAKE128* and *SHAKE256*. While these two operate in 128 bits and 256 bits internally, their message digest length can be arbitrarily chosen. Either one of the SHAKE algorithms could be assigned to produce a hash value of 20, 21, or, say, 60 bits in width.

# Padding

The term *padding* in cryptography refers to the adding of gibberish into encrypted data. This often helps to further protect the plaintext messages in encrypted datasets. There are several techniques for padding in the current day. AES, for one, works with many of these varieties. Let’s take a quick glance at some of the most popular methods.

*Bit padding* is used by the previously mentioned hashing algorithms, MD5 and SHA; it involves adding an extra bit and possibly an arbitrary number of zeros (e.g., *1000 00*) to the end of a data block. The *ANSI X9.23 standard* uses a byte-sized approach, outputting random bytes after a packet of data (as a reminder, one byte equals eight bits). *Zero padding* is the elegant, but efficient approach of adding a random number of zeros after data. Again, all padding techniques add to the security of encrypted and/or hashed data.

# Would You Like Some Salt with Your Data?

*Salting* is not a concept only used in food-related spaces. In cryptography, it refers to the augmentation of a key/password with additional randomly generated symbols without the key holder being aware of them (see Table 3-9). This makes for far more robust security than a simple key. Salt strings can be of any length. For our purposes, let’s use a small 8-bit (i.e., 4-byte) variety of hexadecimal values.