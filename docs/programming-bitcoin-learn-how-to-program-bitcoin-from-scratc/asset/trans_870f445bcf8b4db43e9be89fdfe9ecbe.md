

# ScriptSig

coinbase 交易没有花费的先前输出，因此输入并未解锁任何内容。那么 ScriptSig 中有什么呢？

coinbase 交易的 ScriptSig 由挖出该交易的矿工设置。主要限制是 ScriptSig 至少为 2 字节且不超过 100 字节。除此限制和 BIP0034（在下一节中描述）之外，只要单独评估 ScriptSig（没有对应的 ScriptPubKey）是有效的，ScriptSig 可以是矿工想要的任何内容。以下是创世区块 coinbase 交易的 ScriptSig：

4d04ffff001d0104455468652054696d65732030332f4a616e2f32303039204368616e63656c6c6f72206f6e206272696e6b206f66207365636f6e64206261696c6c6f757420666f722062616e6b73

该 ScriptSig 由中本聪组成，包含我们可以阅读的消息：

```
>>> from io import BytesIO
>>> from script import Script
>>> stream = BytesIO(bytes.fromhex('4d04ffff001d0104455468652054696d6573203033\2f4a616e2f32303039204368616e63656c6c6f72206f6e206272696e6b206f66207365636f6e64206\206261696c6f757420666f722062616e6b73'))
>>> s = Script.parse(stream)
>>> print(s.cmds[2])
b'The Times 03/Jan/2009 Chancellor on brink of second bailout for banks'
```

这是 2009 年 1 月 3 日伦敦泰晤士报的头条新闻。这证明创世区块是在该日期或之后创建的，而不是在此之前。其他 coinbase 交易的 ScriptSig 包含类似的任意数据。

## BIP0034

BIP0034 规范了 coinbase 交易 ScriptSig 的第一个元素。这是由于网络问题，矿工对不同区块使用相同的 coinbase 交易。

coinbase 交易在字节上相同意味着交易 ID 也相同，因为交易的 hash256 是确定的。为了防止交易 ID 重复，Gavin Andresen 撰写了 BIP0034，这是一条软分叉规则，将正在挖矿的区块的高度添加到 coinbase ScriptSig 的第一个元素中。

高度被解释为小端整数，必须等于区块的高度（即自创世区块以来的区块数量）。因此，coinbase 交易不能在不同区块之间在字节上相同，因为区块高度会不同。以下是我们如何从图 9-1 中的 coinbase 交易解析高度：

区块头是区块的元数据。与交易不同，区块头中的每个字段长度都是固定的，如图 9-2 所列；区块头占用恰好 80 字节。截至本文撰写时，大约有 550,000 个区块，或 ~45 MB 的区块头。另一方面，整个区块链大约为 200 GB，因此头大约占 .023% 的大小。头远小于此是一个重要的特性，当我们查看第 11 章中的简化支付验证时，我们会看到这一点。

与交易 ID 类似，区块 ID 是将头以小端解释的 hash256 的十六进制表示。区块 ID 很重要：

```
>>> from helper import hash256
>>> block_hash = hash256(bytes.fromhex('020000208ec39428b17323fa0dd ec8e887b4a7\nc53b8c0a0a220cfd000000000000000005b0750fce0a889502d40508d39576821155e9c9e3f5c\n3157f961db38fd8b25be1e77a759e93c0118a4ffd71d'))[::-1]
>>> block_id = block_hash.hex()
>>> print(block_id)
000000000000000007e9e4c586439b0cdbe13b1370bdd9435d76a644d047523
```

此 ID 被放入构建在此区块之上的区块的 prev_block 中。现在，注意 ID 以大量零开头。我们将在第 170 页的“Proof-of-Work”中回到这个问题，在那里我们将更仔细地查看区块头中的字段。

我们可以基于我们已知的知识开始编写 Block 类：

```
class Block:
    def __init__(self, version, prev_block, merkle_root, timestamp, bits, nonce):
        self.version = version
        self.prev_block = prev_block
        self.merkle_root = merkle_root
        self.timestamp = timestamp
        self.bits = bits
        self.nonce = nonce
```

# Exercise 3

编写 Block 的 parse 方法。

# Exercise 4

编写 Block 的 serialize 方法。

# Exercise 5

编写 Block 的 hash 方法。

## Version

在普通软件中，版本指的是特定的一组功能。对于区块来说也是类似的，因为版本字段反映了生成该区块的软件的能力。过去这被用作指示矿工准备部署的单个功能的方式。版本 2 意味着软件已准备好 BIP0034，该 BIP 引入了本章前面提到的 coinbase 交易区块高度功能。版本 3 意味着软件已准备好 BIP0066，该 BIP 强制执行严格的 DER 编码。版本 4 意味着软件已准备好 BIP0065，该 BIP 指定了 OP_CHECKLOCKTIMEVERIFY。

不幸的是，版本号的递增意味着网络上一次只能发出一个功能的信号。为了解决这个问题，开发者提出了 BIP0009，它允许同时发出多达 29 个不同功能的信号。

BIP0009 的工作方式是将 4 字节（32 位）头的第一个 3 位固定为 001，以指示矿工正在使用 BIP0009。第一个 3 位必须是 001，因为这迫使旧客户端将版本字段解释为大于或等于 4 的数字，而 4 是 BIP0009 之前使用的最后一个版本号。

这意味着在十六进制中，第一个字符将始终为 2 或 3。其余 29 位可以分配给不同的软分叉功能，供矿工发出就绪信号。例如，可以将位 0（最右边的位）翻转为 1 以发出对一个软分叉的就绪信号，可以将位 1（从右数第二个位）翻转为 1 以发出对另一个的就绪信号，可以将位 2（从右数第三个位）翻转为 1 以发出对另一个的就绪信号，依此类推。

BIP0009 要求在给定的 2,016 区块周期（难度调整的周期；本章后面会详细介绍）中，95% 的区块发出就绪信号，然后软分叉功能才能在网络上激活。截至本文撰写时，使用 BIP0009 的软分叉已是 BIP0068/BIP0112/BIP0113（OP_CHECKSEQUENCEVERIFY 及相关更改）和 BIP0141（Segwit）。这些 BIP 分别使用了位 0 和位 1 进行信号发送。BIP0091 使用了类似 BIP0009 的方法，但阈值为 80% 且区块周期更小，因此它并非严格使用 BIP0009。位 4 用于发出 BIP0091 的信号。

检查这些功能相对简单：

```
>>> from io import BytesIO
>>> from block import Block
>>> b = Block.parse(BytesIO(bytes.fromhex('02000208ec39428b17323fa0ddec8e887b4a7c53b8c0a0a220cfd000000000000000005b0750fce0a889502d40508d39576821155e9c9e3f5c3157f961db38fd8b25be1e77a759e93c0118a4ffd71d')))
>>> print('BIP9: {}'.format(b.version >> 29 == 0b001))
BIP9: True
>>> print('BIP91: {}'.format(b.version >> 4 & 1 == 1))
BIP91: False
```

```
>>> print('BIP141: {}'.format(b.version >> 1 & 1 == 1))
BIP141: True
```

1 >> 运算符是右移位运算符，它丢弃最右边的 29 位，只留下顶部 3 位。0b001 是在 Python 中以二进制编写数字的一种方式。

2 & 运算符是“按位与”运算符。在我们的例子中，我们先右移 4 位，然后检查最右边的位是否为 1。

3 我们将 1 右移，因为 BIP0141 被分配给位 1。

# Exercise 6

为 Block 类编写 bip9 方法。

# Exercise 7

为 Block 类编写 bip91 方法。

# Exercise 8

为 Block 类编写 bip141 方法。

## Previous Block

所有区块都必须指向前一个区块。这就是为什么数据结构被称为区块链。区块一直链接回第一个区块，即创世区块。previous block 字段以一堆 00 字节结尾，我们将在本章后面讨论更多。

## Merkle Root

Merkle 根以 32 字节的哈希编码有序交易。我们将在第 11 章讨论这对于简化支付验证（SPV）客户端的重要性，以及它们如何使用 Merkle 根以及来自服务器的数据来获得包含证明。

## Timestamp

时间戳是一个占用 4 字节的 Unix 风格时间戳。Unix 时间戳是自 1970 年 1 月 1 日以来的秒数。此时间戳用于两个地方：用于验证区块中包含的交易的时间戳锁定时间，以及用于每 2,016 个区块计算新的 bits/target/difficulty。锁定时间在一段时间内直接用于区块内的交易，但 BIP0113 改变了行为

