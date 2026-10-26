```
    left_hash = tree.get_left_node()
    right_hash = tree.get_right_node()
    if left_hash is None:  3
        tree.left()
    elif right_hash is None:  4
        tree.right()
    else:  5
        tree.set_current_node(merkle_parent(left_hash, right_hash))
        tree.up()
>>> print(tree)
```

597c4baf...
6382df3f..., 87cf8fa3...
3ba6c080..., 8e894862..., 7ab01bb6..., 3df760ac...
272945ec..., 9a38d037..., 4a64abd9..., ec7c95e1..., 3b67006c..., 850683df..., \
d40d268b..., 8636b7a3...
9745f717..., 5573c8ed..., 82a02ecb..., 507cca e5..., a7a4aec2..., bb626766..., \
ea6d7ac1..., 45774386..., 76880292..., b1ae7f15..., 9b74f89f..., b3a92b5b..., \
b5c0b915..., c9d52c5c..., c555bc5f..., f9dbfafc...

+   1. We traverse until we calculate the Merkle root. Each time through the loop, we are at a particular node.

+   2. If we are at a leaf node, we already have that hash, so we don’t need to do anything but go back up.

+   3. If we don’t have the left hash, then we calculate the value first before calculating the current node’s hash.

+   4. If we don’t have the right hash, we calculate the value before calculating the current node’s hash. Note that we already have the left one due to the depth-first traversal.

+   5. We have both the left and the right hash, so we calculate the Merkle parent value and set that to the current node. Once set, we can go back up.

This code will only work when the number of leaves is a power of two, as edge cases where there are an odd number of nodes on a level are not handled.

We handle the case where the parent is the parent of the rightmost node on a level with an odd number of nodes:

```
>>> from merkleblock import MerkleTree
>>> from helper import merkle_parent
>>> hex_hashes = [
    "9745f7173ef14ee4155722d1cbf13304339fd00d900b759c6f9d58579b5765fb",
    "5573c8ede34936c29cdfdfe743f7f5fdfbd4f54ba0705259e62f39917065cb9b",
    "82a02ecbb6623b4274dfcab82b336dc017a27136e08521091e443e62582e8f05",
    "507cca e5ed9b340363a06d65f65af148be9c b1c8766ccc922f83e4ae681658308",
    "a7a4aec28e7162e1e9ef33dfa0f0b c0526e6cf4b11a576f6c5de58593898330",
    "bb6267664bd833fd9fc82582853ab144fece26b7a8a5bf328f8a059445b59add",
```