# Coinbase Transactions

Coinbase 交易与同名的美国公司无关。Coinbase 是每个区块的必需第一个交易，也是唯一允许将比特币带入存在的交易。coinbase 交易的输出由矿工实体指定保留，通常包括区块中其他交易的所有交易费用以及称为区块奖励的东西。

coinbase 交易使得矿工挖矿变得值得。Figure 9-1 显示了 coinbase 交易的样子。

```
01000000010000000000000000000000000000000000000000000000000000000000000000fffffffff5e03d71b07254d696e656420627920416e74506f6f6c20626a31312f4542312f4144362f43205914293101fabe6d6d678e2c8c34afc36896e7d9402824ed38e856676ee94bfdb0c6c4bcd8b2e5666a0400000000000000c7270000a5e00e00ffffffff01faf20b58000000001976a914338c8489423992471bffb1a54a8d9b1d69dc28a88ac00000000
- 01000000 - version
- 01 - # of inputs
- 000...00 - previous tx hash
- ffffffff - previous tx index
- 5e0...00 - ScriptSig
- ffffffff - sequence
- 01 - # of outputs
- faf20b58...00 - output amount
- 1976...ac - p2pkh ScriptPubKey
- 00000000 - locktime
```

Figure 9-1. Coinbase transaction

交易结构与其他比特币网络上的交易没有不同，只是有些例外：

- Coinbase 交易必须恰好有一个输入。
- 该输入必须具有 32 字节全为 00 的前一笔交易。
- 该输入必须具有 ffffffff 的前一笔交易索引。

这三个条件决定了交易是否为 coinbase 交易。

## Exercise 1

编写 Tx 类的 is_coinbase 方法。





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


不直接使用当前区块的时间戳，而是使用过去 11 个区块的中位时间（MTP）。

![](img/6c915536bcfd43e412f7adef942edf23_195_0.png)

## 比特币时间戳会溢出吗？

比特币区块头中的时间戳字段占 4 字节，即 32 位。这意味着一旦 Unix 时间戳超过 2³² − 1，就没有进一步扩展的空间了。2³² 秒大约是 136 年，也就是说，这个字段在 2106 年（1970 年后的 136 年）将没有更多空间。

许多人错误地认为我们只能用到 1970 年后的 68 年，即 2038 年，但那只是当字段为有符号整数时的情形（2³¹ 秒为 68 年），因此我们享受了额外那一位带来的好处，让我们可以用到 2106 年。

到了 2106 年，区块头将需要某种分叉，因为区块头中的时间戳将无法再持续增加。

## Bits

Bits 是一个字段，用于编码本区块所需的工作量证明。关于这一点将在下一节中详细讨论。

## Nonce

Nonce 代表“只使用一次的数字”（number used only once），即 n-once。矿工在寻找工作量证明时会更改这个数字。

## 工作量证明

工作量证明保障了比特币的安全，并且在更深层次上，使得比特币的去中心化挖矿成为可能。找到工作量证明使矿工有权将所附带的区块放入区块链中。由于工作量证明非常稀有，这并不是一项容易的任务。但正因为工作量证明是客观且易于验证的，任何人都可以选择成为矿工。

工作量证明被称为“挖矿”是有充分理由的。与实际挖矿一样，矿工们是在寻找某种东西。典型的金矿开采作业需要处理 45 吨泥土和岩石，才能累积 1 盎司黄金。这是因为黄金非常稀有。然而，一旦找到黄金，就很容易验证黄金是真的。有化学测试、试金石以及许多其他相对廉价的方法来判断找到的东西是否为黄金。

类似地，工作量证明是一个能够产生非常稀有结果的数字。为了找到工作量证明，比特币网络上的矿工必须在数值上翻遍相当于泥土和岩石的东西。与黄金一样，验证工作量证明远比实际找到它要廉价。

那么，什么是工作量证明？让我们看看之前见过的区块头的 hash256 来了解：

```
0200000208ec39428b17323fa0ddec8e887b4a7c53b8c0a0a220cfd000000000000000000
5b0750fce0a889502d40508d39576821155e9c9e3f5c3157f961db38fd8b25be1e77a759
e93c0118a4ffd71d
```

```
>>> from helper import hash256
>>> block_id = hash256(bytes.fromhex('0200000208ec39428b17323fa0ddec8e887b4a7c5\
3b8c0a0a220cfd00000000000000000005b0750fce0a889502d40508d39576821155e9c9e3f5c31\
57f961db38fd8b25be1e77a759e93c0118a4ffd71d'))[::-1]
>>> print('{0}'.format(block_id.hex()).zfill(64))
0000000000000000007e9e4c5864390bcdbe13b1370bdd9435d76a644d047523
```

我们特意将这个数字打印为 64 位十六进制数字，以显示它在 256 位尺度下有多小。

众所周知，sha256 会生成均匀分布的值。因此，我们可以将两轮 sha256（即 hash256）视为一个随机数。任何一个随机 256 位数字小到这种程度的概率是极小的。一个 256 位数字第一位为 0 的概率是 0.5，前两位为 00 的概率是 0.25，前三位为 000 的概率是 0.125，以此类推。注意，刚才显示的十六进制数字中的每个 0 代表四个 0 位。在本例中，前 73 位为 0，其概率为 0.5^73，大约为 10^22 分之一。这确实是一个非常小的概率。平均而言，需要生成 10^22（即 100 万亿亿）个随机 256 位数字，才能找到一个这么小的数字。换句话说，平均需要计算 10^22 次哈希才能找到这么小的数字。回到之前的类比，寻找工作量证明的过程需要我们处理大约 10^22 个数值意义上的泥土和岩石，才能找到我们的数值金块。

### 矿工如何生成新的哈希

矿工从哪里获得新的数值“泥土”来处理，以检查它是否满足工作量证明？这就是 nonce 字段的用武之地。矿工可以随意更改 nonce 字段来改变区块头的哈希。

不幸的是，nonce 字段的 4 字节或 32 位（即矿工可以尝试的 2^32 个 nonce）不足以满足所需的工作量证明。现代 ASIC 设备每秒可以计算远超 2^32 个不同的哈希。例如，AntMiner S9 每秒计算 12 terahash（Th/s）。这大约是每秒 2^43 次哈希，这意味着整个 nonce 空间只需 0.0003 秒就能消耗殆尽。

当 nonce 字段耗尽时，矿工可以更改 coinbase 交易，这进而会改变默克尔根，从而每次为矿工提供全新的 nonce 空间。另一种选择是滚动版本字段或使用公开的 ASICBOOST。每当区块中的任何交易发生变化时，默克尔根如何随之改变的机制将在第 11 章中讨论。

## 目标值

工作量证明要求比特币中每个区块头的哈希必须低于某个目标值。目标值是一个 256 位数字，直接从 bits 字段（在我们的例子中是 e93c0118）计算得出。与平均 256 位数字相比，这个目标值非常小。

bits 字段实际上由两个不同的数字组成。第一个是指数，即最后一个字节。第二个是系数，即其余三个字节，按小端序排列。根据这两个数字计算目标值的公式为：

target = coefficient × 256^{exponent−3}

下面是在 Python 中根据 bits 字段计算目标值的方法：

```
>>> from helper import little_endian_to_int
>>> bits = bytes.fromhex('e93c0118')
>>> exponent = bits[-1]
>>> coefficient = little_endian_to_int(bits[:-1])
>>> target = coefficient * 256**(exponent - 3)
>>> print('{:x}'.format(target).zfill(64))
0000000000000000013ce90000000000000000000000000000000000000000
```

① 我们特意将这个数字打印为 64 位十六进制数字，以显示这个数字在 256 位尺度下有多小。

有效的工作量证明是区块头的一个哈希，将其解释为小端整数后，其值低于目标值。工作量证明哈希极其稀有，挖矿的过程就是寻找这种哈希的过程。要为上述目标值找到一个工作量证明，整个网络必须计算 3.8 × 10²¹ 次哈希，而当这个区块被发现时，大约每 10 分钟就能完成一次。为了让你对这个数字有更直观的认识，世界上最好的 GPU 挖矿显卡平均需要运行 5 万年才能找到一个低于此目标值的工作量证明。

我们可以按如下方式检查该区块头的哈希是否满足工作量证明：

```
>>> from helper import little_endian_to_int
>>> proof = little_endian_to_int(hash256(bytes.fromhex('0200000208ec39428b17323fa0ddec8e887b4a7c53b8c0a0a220cfd0000000000000000005b0750fce0a889502d40508d39576821155e9c9e3f5c3157f961db38fd8b25be1e77a759e93c0118a4ffd71d')))
>>> print(proof < target)
True
```

① target 在上面已计算得出。

我们可以将数字按 64 位十六进制字符对齐，从而看出工作量证明更小：

```
Target: 0000000000000000013ce90000000000000000000000000000000000000000
ID:     000000000000000007e9e4c586439b0cdbe13b1370bdd9435d76a644d047523
```

### 练习 9

在 helper.py 中编写 bits_to_target 函数。

### 难度

人类很难理解目标值。目标值是哈希必须低于的数字，但作为人类，要看出 180 位数字和 190 位数字之间的差异并不容易。前者要小一千倍，但仅从目标值来看，这么大的数字并不容易建立直观概念。

为了使不同的目标值更容易比较，“难度”的概念应运而生。关键在于，难度与目标值成反比，从而使比较更容易。具体公式为：

difficulty = 0xffff × 256^(0x1d-3) / target

代码如下：

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

比特币在创世区块时的难度为 1。这让我们对当前主网的难度有了直观认识。难度可以理解为现在挖矿比刚开始挖矿困难了多少倍。上述代码中的挖矿难度大约比比特币刚诞生时难了 8880 亿倍。

难度通常显示在区块浏览器和比特币价格图表服务中，因为它是一种更直观地理解创建新区块所需工作量的方式。

### 练习 10

为 Block 编写 difficulty 方法。




```
3121f2427005551aeb0ec6a402ac7d7f0e4235954d801187f5da9f5')))
>>> first_block = Block.parse(BytesIO(bytes.fromhex('00000201ecd89664fd205a37\566e694269ed76e4258030628ab0100000000000000000bfcade29d080d9aae8fd461254b0418\05ae442749f2a40100440fc0e3d5868e5501934595d4d0118a1721b2e')))
>>> time_differential = last_block.timestamp - first_block.timestamp
>>> if time_differential > TWO_WEEKS * 4:
...     time_differential = TWO_WEEKS * 4
>>> if time_differential < TWO_WEEKS // 4:
...     time_differential = TWO_WEEKS // 4
>>> new_target = last_block.target() * time_differential // TWO_WEEKS
>>> print('{ :x}'.format(new_target).zfill(64))
000000000000000007615000000000000000000000000000000000000000
```

1 注意 TWO_WEEKS = 60*60*24*14 是 2 周内秒数的数量：60 秒 × 60 分钟 × 24 小时 × 14 天。

2 这确保了如果找到最后 2,015 个区块花费超过 8 周，我们不会过度降低难度。

3 这确保了如果找到最后 2,015 个区块花费少于 3.5 天，我们不会过度提高难度。

注意我们只需要区块头来计算下一个区块的目标值。一旦我们有了目标，我们就可以将目标转换为位。逆向操作如下所示：

```
def target_to_bits(target):
    '''Turns a target integer back into bits'''
    raw_bytes = target.to_bytes(32, 'big')
    raw_bytes = raw_bytes.lstrip(b'\x00')  1
    if raw_bytes[0] > 0x7f:  2
        exponent = len(raw_bytes) + 1
        coefficient = b'\x00' + raw_bytes[:2]
    else:
        exponent = len(raw_bytes)  3
        coefficient = raw_bytes[:3]  4
    new_bits = coefficient[::-1] + bytes([exponent])  5
    return new_bits
```

1 去掉所有前导零。

2 位格式是一种简洁地表达非常大数字的方式，并且可用于负数和正数。如果系数中的第一位是 1，则位字段被解释为负数。由于对我们来说目标始终为正，如果第一位是 1，我们将整个内容向右移动 1 个字节。

3 指数是该数字在基数 256 中的长度。

4 系数是基数 256 数字的前三位数字。

```
f9beb4d97665727369f6e0000000000065000005f1a69d2721101000100000000000000bc8f5e540
0000000010000000000000000000000000000000000000ffffc61b6409208d0100000000000000000000
0000000000000fffffcb0071c0208d12803cbc97953f80f2f5361746f7368693a302e392e332fc0
5050001

- f9beb4d9 - network magic (always 0xf9beb4d9 for mainnet)
- 7665727369f6e00000000 - command, 12 bytes, human-readable
- 65000000 - payload length, 4 bytes, little-endian
- 5f1a69d2 - payload checksum, first 4 bytes of hash256 of the payload
- 7211...01 - payload
```

Figure 10-1. A network message—the envelope that contains the actual payload

接下来的 12 个字节是命令字段，或是对负载实际携带内容的描述。有许多不同的命令；详尽的列表可以在文档中看到。命令字段旨在可读性强，此特定消息是 ASCII 中的字节字符串“version”，后面填充 0 字节。

接下来的 4 个字节是以小端序表示的负载长度。正如我们在第 5 章和第 9 章中所见，负载长度是必要的，因为负载是可变的。2³² 约为 40 亿，因此负载可以大到 4 GB，尽管参考客户端拒绝任何超过 32 MB 的负载。在图 10-1 中的消息中，我们的负载为 101 字节。

接下来的 4 个字节是校验和字段。校验和算法有点奇怪的选择，因为它是负载的 hash256 的前 4 个字节。这是一个奇怪的选择，因为网络协议校验和通常设计为具有纠错能力，而 hash256 没有。话虽如此，hash256 在比特币协议的其他部分很常见，这可能是它在这里被使用的原因。

处理网络消息的代码要求我们创建一个新的类：

```
NETWORK_MAGIC = b'\xf9\xbe\xb4\xd9'
TESTNET_NETWORK_MAGIC = b'\x0b\x11\x09\x07'
```

```
class NetworkEnvelope:

    def __init__(self, command, payload, testnet=False):
        self.command = command
        self.payload = payload
        if testnet:
            self.magic = TESTNET_NETWORK_MAGIC
        else:
            self.magic = NETWORK_MAGIC

    def __repr__(self):
        return '{: }'.format(
            self.command.decode('ascii'),
```

## Exercise 4

Write the serialize method for VersionMessage.

## Network Handshake

网络握手是节点建立通信的方式：

+   - A 想连接到 B 并发送 version 消息。
+   - B 收到 version 消息，回应 verack 消息，并发送自己的 version 消息。
+   - A 收到 version 和 verack 消息并发送回 verack 消息。
+   - B 收到 verack 消息并继续通信。

一旦握手完成，A 和 B 就可以随意通信。注意这里没有身份验证，节点需要自行验证收到的所有数据。如果节点发送了错误的交易或区块，它可以预期会被禁止或断开连接。

## Connecting to the Network

由于网络的异步特性，网络通信很棘手。为了实验，我们可以同步地建立与网络上节点的连接：

```
>>> import socket
>>> from network import NetworkEnvelope, VersionMessage
>>> host = 'testnet.programmingbitcoin.com' ①
>>> port = 18333
>>> socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
>>> socket.connect((host, port))
>>> stream = socket.makefile('rb', None) ②
>>> version = VersionMessage() ③
>>> envelope = NetworkEnvelope(version.command, version.serialize())
>>> socket.sendall(envelope.serialize()) ④
>>> while True:
...     new_message = NetworkEnvelope.parse(stream) ⑤
...     print(new_message)
```

① 这是我为 testnet 设置的一个服务器。testnet 的默认端口是 18333。

② 我们创建一个流以能够从套接字读取。以这种方式创建的流可以传递给所有解析方法。

③ 握手的第一个步骤是发送 version 消息。

④ 我们现在以正确的信封发送消息。

⑤ 这行代码将读取通过我们连接的套接字传入的任何消息。

以这种方式连接，我们不能在接收之前发送，也不能一次智能地响应多个消息。更健壮的实现将使用异步库（如 Python 3 中的 asyncio）来在不被阻塞的情况下发送和接收。

我们还需要一个 verack 消息类，我们将在此处创建：

```
class VerAckMessage:
    command = b'verack'

    def __init__(self):
        pass

    @classmethod
    def parse(cls, s):
        return cls()

    def serialize(self):
        return b''
```

VerAckMessage 是一个最小的网络消息。

让我们现在通过创建一个将为我们处理通信的类来自动化这个过程：

```
class SimpleNode:

    def __init__(self, host, port=None, testnet=False, logging=False):
        if port is None:
            if testnet:
                port = 18333
            else:
                port = 8333
        self.testnet = testnet
        self.logging = logging
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.socket.connect((host, port))
        self.stream = self.socket.makefile('rb', None)

    def send(self, message): ①
        '''Send a message to the connected node'''
        envelope = NetworkEnvelope(
            message.command, message.serialize(), testnet=self.testnet)
        if self.logging:
            print('sending: {}'.format(envelope))
        self.socket.sendall(envelope.serialize())

    def read(self): ②
        '''Read a message from the socket'''
```

```
envelope = NetworkEnvelope.parse(self.stream, testnet=self.testnet)
if self.logging:
    print('receiving: {}'.format(envelope))
return envelope
```

```
def wait_for(self, *message_classes):  ③
'''Wait for one of the messages in the list'''
command = None
command_to_class = {m.command: m for m in message_classes}
while command not in command_to_class.keys():
    envelope = self.read()
    command = envelope.command
    if command == VersionMessage.command:
        self.send(VerAckMessage())
    elif command == PingMessage.command:
        self.send(PongMessage(envelope.payload))
return command_to_class[command].parse(envelope.stream())
```

① send 方法通过套接字发送消息。消息对象中预期存在 command 属性和 serialize 方法。

② read 方法从套接字读取新消息。

③ wait_for 方法让我们等待多个命令中的任意一个（具体来说是消息类）。与该类的同步特性一起，这种方法使编程更简单。商业级节点绝对不会使用类似的东西。

现在我们有了节点，我们可以与其他节点进行握手：

```
>>> from network import SimpleNode, VersionMessage
>>> node = SimpleNode('testnet.programmingbitcoin.com', testnet=True)
>>> version = VersionMessage()  ①
>>> node.send(version)  ②
>>> verack_received = False
>>> version_received = False
>>> while not verack_received and not version_received:  ③
...     message = node.wait_for(VersionMessage, VerAckMessage)  ④
...     if message.command == VerAckMessage.command:
...         verack_received = True
...     else:
...         version_received = True
...         node.send(VerAckMessage())
```

① 大多数节点不关心 version 中的字段如 IP 地址。我们可以使用默认值连接，一切都会很顺利。

② 我们通过发送 version 消息开始握手。





```
start_block=None, end_block=None):
    self.version = version
    self.num_hashes = num_hashes
    if start_block is None:
        raise RuntimeError('a start block is required')
    self.start_block = start_block
    if end_block is None:
        self.end_block = b'\x00' * 32
    else:
        self.end_block = end_block
```

1 就本章而言，我们假设区块头组的数量为 1。更健壮的实现会处理多个区块组，但我们可以使用单个组来下载区块头。

2 需要起始区块，否则我们无法创建正确的消息。

3 结束区块我们假设为 null，如果未定义，则服务器会发送多少我们就接收多少。

## 练习 6

为 GetHeadersMessage 编写 serialize 方法。

## 头消息响应

现在我们可以创建一个节点，执行握手，然后索取一些头消息：

```
>>> from io import BytesIO
>>> from block import Block, GENESIS_BLOCK
>>> from network import SimpleNode, GetHeadersMessage
>>> node = SimpleNode('mainnet.programmingbitcoin.com', testnet=False)
>>> node.handshake()
>>> genesis = Block.parse(BytesIO(GENESIS_BLOCK))
>>> getheaders = GetHeadersMessage(start_block=genesis.hash())
>>> node.send(getheaders)
```

现在我们需要一种从其他节点接收头消息的方法。对方节点会发送回 headers 命令。这是一份区块头列表（图 10-4），我们在第 9 章已经学习了如何解析它。HeadersMessage 类在解析时可以利用这一点。

```
0200000020df3b053dc46f162a9b00c7f0d5124e2676d47bbe7c5d0793a5000000000000000ef445fef2ed495c27589220ca533e7411907971013ab83e3b47bd0d692d14d4dc7c835b67d8001ac157e67000000002030eb2540c41025690160a0104c577061596e32e426b712c7ca00000000000000768b89f07044e6130ead292a3f5f1951adbd2202df447d98789339937fd006bd44880835b67d8001ade09204600
```

- `02` - 区块头数量
- `00...67` - 区块头
- `00` - 交易数量（始终为 0）

图 10-4. 解析后的头消息

头消息以 varint 形式的头消息数量开头，该数量是 1 到 2,000 之间的整数。我们知道每个区块头是 80 字节。然后是交易数量。头消息中的交易数量始终为 0。一开始这可能有点令人困惑，因为我们只请求了头消息，并没有请求交易。节点之所以不嫌麻烦地发送交易数量，是因为头消息的设计要与区块消息的格式兼容；区块消息由区块头、交易数量以及交易本身组成。通过将交易数量指定为 0，我们可以使用与解析完整区块相同的解析引擎：

```
class HeadersMessage:
    command = b'headers'

    def __init__(self, blocks):
        self.blocks = blocks

    @classmethod
    def parse(cls, stream):
        num_headers = read_varint(stream)
        blocks = []
        for _ in range(num_headers):
            blocks.append(Block.parse(stream))  ①
            num_txs = read_varint(stream)  ②
            if num_txs != 0:  ③
                raise RuntimeError('number of txs not 0')
        return cls(blocks)
```

① 每个区块都用 Block 类的 parse 方法解析，使用的是我们已有的同一数据流。
② 交易数量始终为 0，这是区块解析遗留的部分。
③ 如果得到的不是 0，那就说明有问题。

有了我们建立好的网络连接，我们可以下载头消息，检查它们的工作量证明，并验证区块头难度调整，如下所示：
