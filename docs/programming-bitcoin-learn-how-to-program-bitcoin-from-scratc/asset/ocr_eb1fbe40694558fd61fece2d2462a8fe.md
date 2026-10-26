transactions (say, 100 BTC), the full nodes may have an economic incentive to deceive you. Transactions that large should generally be validated using a full node.

# Merkle Block

When a full node sends a proof of inclusion, there are two pieces of information that need to be included. First, the light client needs the Merkle tree structure, and second, the light client needs to know which hash is at which position in the Merkle tree. Once both pieces of information are given, the light client can reconstruct the partial Merkle tree to reconstruct the Merkle root and validate the proof of inclusion. A full node communicates these two pieces of information to a light client using a Merkle block.

To understand what’s in a Merkle block, we need to understand a bit about how a Merkle tree, or more generally, binary trees, can be traversed. In a binary tree, nodes can be traversed breadth-first or depth-first. Breadth-first traversal would go level by level like in Figure 11-3.

![](bbox=[0.13, 0.44, 0.82, 0.72])

Figure 11-3. Breadth-first ordering

The breadth-first ordering starts at the root and goes from root to leaves, level by level, left to right.

Depth-first ordering is a bit different and looks like Figure 11-4.