

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

597c4baf...
6382df3f..., 87cf8fa3...
3ba6c080..., 8e894862..., 7ab01bb6..., 3df760ac...
272945ec..., 9a38d037..., 4a64abd9..., ec7c95e1..., 3b67006c..., 850683df..., \
d40d268b..., 8636b7a3...
9745f717..., 5573c8ed..., 82a02ecb..., 507cca e5..., a7a4aec2..., bb626766..., \
ea6d7ac1..., 45774386..., 76880292..., b1ae7f15..., 9b74f89f..., b3a92b5b..., \
b5c0b915..., c9d52c5c..., c555bc5f..., f9dbfafc...

- 1. 我们遍历直到计算出 Merkle 根。每次循环，我们都处于某个特定节点。
- 2. 如果我们处于叶子节点，我们已经有该哈希，因此无需做任何事，只需向上返回。
- 3. 如果我们没有左哈希，则在计算当前节点的哈希之前先计算其值。
- 4. 如果我们没有右哈希，则在计算当前节点的哈希之前先计算其值。请注意，由于深度优先遍历，我们已经有了左值。
- 5. 我们同时拥有左哈希和右哈希，因此计算 Merkle 父值并将其设为当前节点。设置后，我们可以向上返回。

此代码仅在叶子数量为 2 的幂时才有效，因为未处理某一层节点数为奇数的边界情况。

我们处理父节点为某一层最右节点父节点且该层节点数为奇数的情况：

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
...
"ea6d7ac1ee77fbacee58fc717b990c4fccc f1b19af43103c090f601677fd8836",
...
"457743861de496c429912558a106b810b0507975a49773228aa788df40730d41",
...
"7688029288efc9e9a0011c960a6ed9e5466581abf3e3a6c26ee317461add619a",
...
"b1ae7f15836cb2286cdd42e3c7bf9bb7d0a0284d6d0867a429f654b2e7f383c9",
...
"9b74f89fa3f93e7iff2c241f32945d877281a6a50a6bf94adac002980aafe5ab",
...
"b3a92b5b255019bdaff754875633c2de9fec2ab03e6b8ce669d07cb5b18804638",
...
"b5c0b915312b9bdaedd2b86aa2d0f8effc73a2d37668fd90119726e12e5e263",
...
"c9d52c5cb1e557b92c84c5e7c4bfbce859408bedffc8a5560fde6e31e0b8800",
...
"c555bc5fc3bc096df0a0c953f2f07640bfb76bfe4fc1ace214b8b228a1297a4c2",
...
"f9dbfafc3af3400954975da24eb325e326960a2b5b7fffe2e3eef3e7ed2fb610e",
...
"38faf8c811988dff0a7e6080b1771c97bcc0801c64d9068cff b85e6e7aacaf51",
... ]
>>> tree = MerkleTree(len(hex_hashes))
>>> tree.nodes[5] = [bytes.fromhex(h) for h in hex_hashes]
>>> while tree.root() is None:
...     if tree.is_leaf():
...         tree.up()
...     else:
...         left_hash = tree.get_left_node()
...         if left_hash is None: ①
...             tree.left()
...         elif tree.right_exists(): ②
...             right_hash = tree.get_right_node()
...             if right_hash is None: ③
...                 tree.right()
...             else: ④
...                 tree.set_current_node(merkle_parent(left_hash, right_hash))
...                 tree.up()
...         else: ⑤
...             tree.set_current_node(merkle_parent(left_hash, left_hash))
...             tree.up()
>>> print(tree)
0a313864...
597c4baf..., 6f8a8190...
6382df3f..., 87cf8fa3..., 5647f416...
3ba6c080..., 8e894862..., 7ab01bb6..., 3df760ac..., 28e93b98...
272945ec..., 9a38d037..., 4a64abd9..., ec7c95e1..., 3b67006c..., 850683df..., \
d40d268b..., 8636b7a3..., ce26d40b...
9745f17..., 5573c8ed..., 82a02ecb..., 507cca e5..., a7a4aec2..., bb626766..., \
ea6d7ac1..., 45774386..., 76880292..., b1ae7f15..., 9b74f89f..., b3a92b5b..., \
b5c0b915..., c9d52c5c..., c555bc5f..., f9dbfafc..., 38faf8c8...

① 如果我们没有左节点的值，我们遍历到左节点，因为所有内部节点都保证有左子节点。
② 我们首先检查此节点是否有右子节点。这为真，除非此节点恰好是该层最右节点且子层节点数为奇数。
③ 如果我们没有右节点的值，我们遍历到该节点。

- 4 如果我们同时拥有左节点和右节点的值，我们使用 merkle_parent 计算当前节点的值。
- 5 我们拥有左节点的值，但右子节点不存在。这是该层最右节点，因此我们将左值组合两次。

我们现在可以遍历叶子数量不是 2 的幂的树。

# The merkleblock Command

全节点在通信 Merkle 块时发送验证感兴趣的交易是否在 Merkle 树中的所有必要信息。merkleblock 网络命令用于通信此信息；它看起来像图 11-6。

```
0000020df3b053dc46f162a9b00c7f0d5124e2676d47bbe7c5d0793a500000000000000ef445fef2ed495c275892206ca533e741190791013ab83e3b47bdd0692d14d4dc7c835b67d8001ac157e670bf0d00000aba412a0d1480e370173072c9562becffe87a661c1e4a6dbc305d38ec5dc088a7cf92e6458aca7b32edae818f9c2c98c37e06bf72ae0c9e80649a38655ee1e27d3d9421d940b16732f24b94023e9d572a7f9ab8033434a4feb532d2adf8c2c2158785d1bd04eb9fd2e86c54bc13e139862897127400efd5d72c280222c4caee7261831e1550db8fa828853be9fe506fc5fda7f3b919d8fe74b6282f92763cef8e625f977af7c8619c32a369b832bc2d051ecd9c73c51e76370ceaad4bF25097c256597fa898d404ed53425de608ac6bfe426f6e2bb457f1c554866eb69dcb8db6f6f880e9a59b3cd053e6c7060eea caacf4dac6697dac20e4bd3f38a2ea2543d1ab7953e3430790a9f81e1c67f5b58c825acf46bd02848384eebe9af917274cdfbb1a28a5d58a23a17977def0de10d644258d9c54f886d47d293a411cb6226103b55635
- 0000020 - version, 4 bytes, LE
- df3b...00 - previous block, 32 bytes, LE
- ef44...d4 - Merkle root, 32 bytes, LE
- dc7c835b - timestamp, 4 bytes, LE
- 67d8001a - bits, 4 bytes
- c157e670 - nonce, 4 bytes
- bf0d0000 - number of total transactions, 4 bytes, LE
- 0a - number of hashes, varint
- ba41...61 - hashes, 32 bytes * number of hashes
- 03b55635 - flag bits
```

Figure 11-6. Parsed merkleblock

前六个字段与第 9 章的区块头完全相同。最后四个字段是包含证明。

交易数量字段指示此特定 Merkle 树将拥有的叶子数量。这允许轻客户端构建空的 Merkle 树。hashes 字段保存图 11-5 中的蓝色和绿色方块。由于 hashes 字段中的哈希数量不是固定的，因此前面加上了数量。最后，flags 字段提供有关哈希在 Merkle 树中位置的信息。标志使用 bytes_to_bits_field 解析，将其转换为位列表（1 和 0）：

```
def bytes_to_bit_field(some_bytes):
    flag_bits = []
    for byte in some_bytes:
        for _ in range(8):
            flag_bits.append(byte & 1)
            byte >>= 1
    return flag_bits
```

字节的顺序有点奇怪，但它是为了便于转换为重建 Merkle 根所需的标志位。

# Exercise 6

编写 MerkleBlock 的 parse 方法。

# Using Flag Bits and Hashes

标志位使用深度优先顺序告知哈希的位置。

标志位的规则为：

- 1. 如果节点的值在 hashes 字段中给出（图 11-7 中的蓝色方块），则标志位为 0。
- 2. 如果节点是内部节点且值由轻客户端计算（图 11-7 中的虚线轮廓），则标志位为 1。
- 3. 如果节点是叶子节点且是感兴趣的交易（图 11-7 中的绿色方块），则标志为 1 且节点的值也在 hashes 字段中给出。这些是证明其包含在 Merkle 树中的项目。

