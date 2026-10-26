# Version

Version in normal software refers to a particular set of features. For a block, this is similar, in the sense that the version field reflects the capabilities of the software that produced the block. In the past this was used as a way to indicate a single feature that was ready to be deployed by the block’s miner. Version 2 meant that the software was ready for BIP0034, which introduced the coinbase transaction block height feature mentioned earlier in this chapter. Version 3 meant that the software was ready for BIP0066, which enforced strict DER encoding. Version 4 meant that the software was ready for BIP0065, which specified OP_CHECKLOCKTIMEVERIFY.

Unfortunately, the incremental increase in version number meant that only one feature was signaled on the network at a time. To alleviate this, the developers came up with BIP0009, which allows up to 29 different features to be signaled at the same time.

The way BIP0009 works is by fixing the first 3 bits of the 4-byte (32-bit) header to be 001 to indicate that the miner is using BIP0009. The first 3 bits have to be 001, as that forces older clients to interpret the version field as a number greater than or equal to 4, which was the last version number that was used pre-BIP0009.

This means that in hexadecimal, the first character will always be 2 or 3. The other 29 bits can be assigned to different soft-fork features for which miners can signal readiness. For example, bit 0 (the rightmost bit) can be flipped to 1 to signal readiness for one soft fork, bit 1 (the second bit from the right) can be flipped to 1 to signal readiness for another, bit 2 (the third bit from the right) can be flipped to 1 to signal readiness for another, and so on.

BIP0009 requires that 95% of blocks signal readiness in a given 2,016-block epoch (the period for a difficulty adjustment; more on that later in this chapter) before the soft fork feature gets activated on the network. Soft forks that used BIP0009 as of this writing have been BIP0068/BIP0112/BIP0113 (OP_CHECKSEQUENCEVERIFY and related changes) and BIP0141 (Segwit). These BIPs used bits 0 and 1 for signaling, respectively. BIP0091 used something like BIP0009 but with an 80% threshold and a smaller block period, so it wasn’t strictly using BIP0009. Bit 4 was used to signal BIP0091.

Checking for these features is relatively straightforward:

```
>>> from io import BytesIO
>>> from block import Block
>>> b = Block.parse(BytesIO(bytes.fromhex('02000208ec39428b17323fa0ddec8e887b4a7c53b8c0a0a220cfd000000000000000005b0750fce0a889502d40508d39576821155e9c9e3f5c3157f961db38fd8b25be1e77a759e93c0118a4ffd71d')))
>>> print('BIP9: {}'.format(b.version >> 29 == 0b001))
BIP9: True
>>> print('BIP91: {}'.format(b.version >> 4 & 1 == 1))
BIP91: False
```