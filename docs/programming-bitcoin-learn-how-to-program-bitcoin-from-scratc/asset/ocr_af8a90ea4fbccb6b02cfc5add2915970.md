① target is calculated above.

We can see that the proof-of-work is lower by lining up the numbers in 64 hex characters:

```
TG: 000000000000000013ce90000000000000000000000000000000000000000
ID: 000000000000000007e9e4c586439b0cdbe13b1370bdd9435d76a644d047523
```

# Exercise 9

Write the bits_to_target function in helper.py.

# Difficulty

Targets are hard for human beings to comprehend. The target is the number that the hash must be below, but as humans, it's not easy to see the difference between a 180-bit number and a 190-bit number. The first is a thousand times smaller, but from looking at targets, such large numbers are not easy to contextualize.

To make different targets easier to compare, the concept of difficulty was born. The trick is that difficulty is inversely proportional to the target, to make comparisons easier. The specific formula is:

difficulty = 0xffff × 256^(0x1d-3) / target

The code looks like this:

```
>>> from helper import little_endian_to_int
>>> bits = bytes.fromhex('e93c0118')
>>> exponent = bits[-1]
>>> coefficient = little_endian_to_int(bits[:-1])
>>> target = coefficient*256**(exponent-3)
>>> difficulty = 0xffff * 256**(0x1d-3) / target
>>> print(difficulty)
888171856257.3206
```

The difficulty of Bitcoin at the genesis block was 1. This gives us context for how difficult mainnet currently is. The difficulty can be thought of as how much more difficult mining is now than it was at the start. The mining difficulty in the preceding code is roughly 888 billion times harder than when Bitcoin started.

Difficulty is often shown in block explorers and Bitcoin price charting services, as it's a much more intuitive way to understand the effort required to create a new block.

# Exercise 10

Write the difficulty method for Block.