
```python
...     parent = merkle_parent(hashes[i], hashes[i+1])
...     parent_level.append(parent)
>>> for item in parent_level:
...     print(item.hex())
8b30c5ba100f6f2e5ad1e2a742e5020491240f8eb514fe97c713c31718ad7ecd
7f4e6f9e224e20fda0ae4c44114237f97cd35aca38d83081c9bfd41feb907800
3ecf6115380c77e8aae56660f5634982ee897351ba906a6837d15ebc3a225df0
```

① We add the last hash on the list, hashes[-1], to the end of hashes to make the length of hashes even.

② This is how we skip by two in Python. i will be 0 the first time through the loop, 2 the second, 4 the third, and so on.

This code results in a new list of hashes that correspond to the Merkle parent level.

## Exercise 2

Write the merkle_parent_level function.

## Merkle Root

To get the Merkle root we calculate successive Merkle parent levels until we get a single hash. If, for example, we have items A through G (7 items), we calculate the Merkle parent level first as follows:

[H(A||B), H(C||D), H(E||F), H(G||G)]

Then we calculate the Merkle parent level again:

[H(H(A||B)||H(C||D)), H(H(E||F)||H(G||G))]

We are left with just two items, so we calculate the Merkle parent level one more time:

H(H(H(A||B)||H(C||D))||H(H(E||F)||H(G||G)))

Since we are left with exactly one hash, we are done. Each level will halve the number of hashes, so doing this process over and over will eventually result in a final single item called the Merkle root:

```python
>>> from helper import merkle_parent_level
>>> hex_hashes = [
...     'c117ea8ec828342f4dfb0ad6bd140e03a50720ece40169ee38bdc15d9eb64cf5',
...     'c131474164b412e3406696da1ee20ab0fc9bf41c8f05fa8ceea7a08d672d7cc5',
...     'f391da6ecfeed1814efae39e7fcb3838ae0b02c02ae7d0a5848a66947c0727b0',
...     '3d238a92a94532b946c90e19c49351c763696cff3db400485b813aecb8a13181',
...     '10092f2633be5f3ce349bf9ddbde36caa3dd10dfa0ec8106bce23acbff637dae',
...     '7d37b3d54fa6a64869084bfd2e831309118b9e833610e6228adacdbd1b4ba161',
...     '8118a77e542892fe15ae3fc771a4abfd2f5d5d5997544c3487ac36b5c85170fc',
...     'dff6879848c2c9b62fe65270bd8df527093acfaa5a43cdb369fe2466a3877',
...     'b825c0745f46ac58f7d3759e6c535a1fec780377f24d4c2cad55c0cb59',
...     '95513952a04bd8992721e9b7e2937f1c04ba31e0469fbe615a78197f68f52b7c',
...     '2e6d722e5e4dbdf2447decc9f7dabb8e299bae921c99ad5b0184cd9eb8e5908',
...     'b13a750047bc0bdceb2473e5fe488c2596d7a712b4e716fdd29b046ef99bbf0',
... ]
>>> hashes = [bytes.fromhex(x) for x in hex_hashes]
>>> current_hashes = hashes
>>> while len(current_hashes) > 1:
...     current_hashes = merkle_parent_level(current_hashes)
>>> print(current_hashes[0].hex())
acbcab8bcc1af95d8d563b77d24c3d19b18f1486383d75a5085c4e86c86beed6
```

1. We loop until there's one hash left.
2. We've exited the loop so there should only be one item.

## Exercise 3

Write the merkle_root function.

## Merkle Root in Blocks

Calculating the Merkle root in blocks should be pretty straightforward, but due to endianness issues, it turns out to be tricky. Specifically, we use little-endian ordering for the leaves of the Merkle tree. After we calculate the Merkle root, we use little-endian ordering again.

In practice, this means reversing the leaves before we start and reversing the root at the end:

```python
>>> from helper import merkle_root
>>> tx_hex_hashes = [
...     '42f6f52f17620653dcc909e58bb352e0bd4bd1381e2955d19c00959a22122b2e',
...     '94c3af34b9667bf787e1c6a0a009201589755d01d02fe2877cc69b929d2418d4',
...     '959428d7c48113cb9149d0566bde3d46e98cf02805c3522b8fa7f835241a953',
...     'a9f27b99d5d108dede757510d4a1ffa2c74af70b4ca71726fa57d68454e609a2',
...     '62af110031e29de1efcad103b3ad4bec7bdcfc6bc9c9f4afd586981795516577',
...     '766900590ece194667e9da2984018057512887110bf54fe0aa800157aec796ba',
...     'e8270fb475763bc8d855cfe45ed98060988c1bdcad2ffc8364f783c98999a208',
... ]
>>> tx_hashes = [bytes.fromhex(x) for x in tx_hex_hashes]
>>> hashes = [h[::-1] for h in tx_hashes]
>>> print(merkle_root(hashes)[::-1].hex())
654d6181e18e4ac4368383fdc5eead11bf138f9b7ac1e15334e4411b3c4797d9
```

1. We reverse each hash before we begin using a Python list comprehension.
2. We reverse the root at the end.

We want to calculate Merkle roots for a Block, so we add a tx_hashes parameter:

```python
class Block:
    def __init__(self, version, prev_block, merkle_root,
                timestamp, bits, nonce, tx_hashes=None):
        self.version = version
        self.prev_block = prev_block
        self.merkle_root = merkle_root
        self.timestamp = timestamp
        self.bits = bits
        self.nonce = nonce
        self.tx_hashes = tx_hashes
```

1. We now allow the transaction hashes to be set as part of the initialization of the block. The transaction hashes have to be ordered.

As a full node, if we are given all of the transactions, we can calculate the Merkle root and check that the Merkle root is what we expect.

## Exercise 4

Write the validate_merkle_root method for Block.

## Using a Merkle Tree

Now that we know how a Merkle tree is constructed, we can create and verify proofs of inclusion. Light nodes can get proofs that transactions of interest were included in a block without having to know all the transactions of a block (Figure 11-2).

Say that a light client has two transactions that are of interest, which would be the hashes represented by the green boxes, H_K and H_N in Figure 11-2. A full node can construct a proof of inclusion by sending us all of the hashes marked by blue boxes: H_ABCDEFGH, H_IJ, H_L, H_M, and H_OP. The light client would then perform these calculations:

- H_KL = merkle_parent(H_K, H_L)
- H_MN = merkle_parent(H_M, H_N)
- H_IJKL = merkle_parent(H_IJ, H_KL)
- H_MNOP = merkle_parent(H_MN, H_OP)
- H_IJKLMNOP = merkle_parent(H_IJKL, H_MNOP)
- H_ABCDEFGHIJKLMNOP = merkle_parent(H_ABCDEFGH, H_IJKLMNOP)

![](img/6c915536bcfd43e412f7adef942edf23_221_0.png)

Figure 11-2. Merkle proof

You can see that in Figure 11-2, the dotted boxes correspond to the hashes that the light client calculates. In particular, the Merkle root is H_ABCDEFGHIJKLMNOP, which can then be checked against the block header whose proof-of-work has been validated.

## How Secure Is an SPV Proof?

The full node can send a limited amount of information about the block and the light client can recalculate the Merkle root, which can then be verified against the Merkle root in the block header. This does not guarantee that the transaction is in the longest blockchain, but it does assure the light client that the full node would have had to spend a lot of hashing power or energy creating a valid proof-of-work. As long as the reward for creating such a proof-of-work is greater than the amounts in the transactions, the light client can at least know that the full node has no clear economic incentive to lie.

Since block headers can be requested from multiple nodes, light clients have a way to verify if one node is trying to show them block headers that are not the longest. It only takes a single honest node to invalidate a hundred dishonest ones, since proof-of-work is objective. Therefore, isolation of a light client (that is, control of who the light client is connected to) is required to deceive in this way. The security of SPV requires that there be lots of honest nodes on the network.

In other words, light client security is based on a robust network of nodes and the economic cost of producing proof-of-work. For small transactions relative to the block subsidy (currently 12.5 BTC), there’s probably little to worry about. For large transactions (say, 100 BTC), the full nodes may have an economic incentive to deceive you. Transactions that large should generally be validated using a full node.

## Merkle Block

When a full node sends a proof of inclusion, there are two pieces of information that need to be included. First, the light client needs the Merkle tree structure, and second, the light client needs to know which hash is at which position in the Merkle tree. Once both pieces of information are given, the light client can reconstruct the partial Merkle tree to reconstruct the Merkle root and validate the proof of inclusion. A full node communicates these two pieces of information to a light client using a Merkle block.

To understand what’s in a Merkle block, we need to understand a bit about how a Merkle tree, or more generally, binary trees, can be traversed. In a binary tree, nodes can be traversed breadth-first or depth-first. Breadth-first traversal would go level by level like in Figure 11-3.

![](img/6c915536bcfd43e412f7adef942edf23_222_0.png)

Figure 11-3. Breadth-first ordering

The breadth-first ordering starts at the root and goes from root to leaves, level by level, left to right.

Depth-first ordering is a bit different and looks like Figure 11-4.
