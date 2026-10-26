

```
raw = bytes.fromhex(response.text.strip())
except ValueError:
    raise ValueError('unexpected response: {}'.format(response.text))
if raw[4] == 0:
    raw = raw[:4] + raw[6:]
    tx = Tx.parse(BytesIO(raw), testnet=testnet)
    tx.locktime = little_endian_to_int(raw[-4:])
else:
    tx = Tx.parse(BytesIO(raw), testnet=testnet)
if tx.id() != tx_id:
    raise ValueError('not the same id: {} vs {}'.format(tx.id(), tx_id))

cls.cache[tx_id] = tx
cls.cache[tx_id].testnet = testnet
return cls.cache[tx_id]
```

① 我们检查 ID 是否与预期一致。

你可能会好奇，为什么我们不只获取交易的特定输出，而是获取整笔交易。这是因为我们不想信任第三方！通过获取整笔交易，我们可以验证交易 ID（其内容的 hash256），从而确信我们确实得到了所请求的交易。除非收到整笔交易，否则这是不可能的。

![](img/6c915536bcfd43e412f7adef942edf23_126_0.png)

## 为什么我们尽量不信任第三方

正如 Nick Szabo 在其开创性文章《受信任的第三方是安全漏洞》中雄辩地指出的那样，信任第三方提供正确的数据并不是一种良好的安全实践。第三方现在可能行为良好，但你永远不知道它何时会被入侵、有员工叛变，或者开始实施违背你利益的政策。比特币安全性的部分原因在于不信任，而是验证我们获得的数据。

现在我们可以为 `TxIn` 创建适当的方法来获取前序交易，以及获取前序交易输出的金额和 ScriptPubKey（后者将在第 6 章中使用）的方法：

```
class TxIn:
    ...
    def fetch_tx(self, testnet=False):
        return TxFetcher.fetch(self.prev_tx.hex(), testnet=testnet)

    def value(self, testnet=False):
        """通过查找交易哈希获取输出值。
        返回以聪为单位的金额。
        """
        tx = self.fetch_tx(testnet=testnet)
        return tx.tx_outs[self.prev_index].amount
```

锁定和解锁币的能力是我们转移比特币的机制。*锁定*是将一些比特币授予某个实体。*解锁*是花费你收到的一些比特币。

在本章中，我们研究这种锁定/解锁机制，它通常被称为*智能合约*。椭圆曲线密码学（第 3 章）被 Script 用来验证交易是否被正确授权（第 5 章）。Script 本质上允许人们证明他们有权花费某些 UTXO。不过，我们有点超前了，让我们从 Script 如何工作开始，然后再深入探讨。

## Script 的机制

如果你对什么是智能合约感到困惑，不用担心。“智能合约”是“可编程”的花哨说法，“智能合约语言”只是一种编程语言。在比特币中，Script 就是智能合约语言，也就是用来表达比特币可花费条件的编程语言。

比特币在 Script 中拥有数字化的合约等价物。Script 是一种基于栈的语言，类似于 Forth。它故意有所限制，避免某些特性。具体来说，Script 避免任何循环机制，因此不是图灵完备的。

## Script 如何工作

Script 是一种编程语言，与大多数编程语言一样，它一次处理一条命令。这些命令操作一个元素栈。命令有两种可能的类型：元素和操作。

元素是数据。从技术上讲，处理一个元素会将该元素压入栈中。元素是长度为 1 到 520 的字节串。一个典型的元素可能是 DER 签名或 SEC 公钥（图 6-1）。

![](img/6c915536bcfd43e412f7adef942edf23_130_0.png)

图 6-1. 元素

操作会对数据做一些处理（图 6-2）。它们从处理栈中消耗零个或多个元素，并将零个或多个元素压回栈中。

![](img/6c915536bcfd43e412f7adef942edf23_130_1.png)

图 6-2. 操作

一个典型的操作是 OP_DUP（图 6-3），它会复制栈顶元素（消耗 0 个），并将新元素压入栈中（压入 1 个）。

![](img/6c915536bcfd43e412f7adef942edf23_130_2.png)

图 6-3. OP_DUP 复制栈顶元素

所有命令求值完毕后，栈顶元素必须非零，脚本才能解析为有效。栈中没有元素或栈顶元素为 0 都会解析为无效。解析为无效意味着包含解锁脚本的交易不会在网络中被接受。

## 操作示例

除了 OP_DUP 之外，还有许多其他操作。OP_HASH160（图 6-4）对栈顶元素执行 sha256，然后执行 ripemd160（即 hash160）（消耗 1 个），并将新元素压入栈中（压入 1 个）。请注意图中 y = hash160(x)。

![](img/6c915536bcfd43e412f7adef942edf23_131_0.png)

图 6-4. OP_HASH160 对栈顶元素执行 sha256 后接 ripemd160

另一个非常重要的操作是 OP_CHECKSIG（图 6-5）。OP_CHECKSIG 从栈中消耗两个元素，第一个是公钥，第二个是签名，并检查该签名对于给定公钥是否有效。如果是，OP_CHECKSIG 向栈中压入 1；否则，压入 0。

![](img/6c915536bcfd43e412f7adef942edf23_131_1.png)

图 6-5. OP_CHECKSIG 检查公钥的签名是否有效

## 编写操作码

现在我们可以编写 OP_DUP，给定一个栈。OP_DUP 只是复制栈顶元素：

```
def op_dup(stack):
    if len(stack) < 1:  # ①
        return False
    stack.append(stack[-1])  # ②
    return True
...
OP_CODE_FUNCTIONS = {
...
    118: op_dup,  # ③
...
}
```

① 我们必须至少有一个元素才能复制；否则无法执行此操作码。

② 这就是我们复制栈顶元素的方式。

③ 118 = 0x76，是 OP_DUP 的代码。

请注意，我们使用此操作码返回一个布尔值，作为操作是否成功的指示。失败的操作会自动使脚本求值失败。

这是另一个 OP_HASH256 的示例。此操作码会消耗栈顶元素，对其执行 hash256 操作，并将结果压入栈中：

```
def op_hash256(stack):
    if len(stack) < 1:
        return False
    element = stack.pop()
    stack.append(hash256(element))
    return True

OP_CODE_FUNCTIONS = {
    ...
    170: op_hash256,
    ...
}
```

### 练习 1

编写 op_hash160 函数。

## 解析脚本字段

ScriptPubKey 和 ScriptSig 都以相同的方式解析。如果字节在 0x01 到 0x4b 之间（我们称其值为 n），则读取接下来的 n 个字节作为一个元素。否则，该字节代表一个操作，我们需要查找它。以下是一些操作及其字节码：

| 字节码 | 操作 |
|-----------|-----------|
| `0x00` | `OP_0` |
| `0x51` | `OP_1` |
| `0x60` | `OP_16` |
| `0x76` | `OP_DUP` |
| `0x93` | `OP_ADD` |
| `0xa9` | `OP_HASH160` |
| `0xac` | `OP_CHECKSIG` |

## 超过 75 字节的元素

![](img/6c915536bcfd43e412f7adef942edf23_133_0.png)

你可能会想知道，如果元素长度大于 0x4b（十进制 75）会发生什么。有三个专门的操作码用于处理此类元素：OP_PUSHDATA1、OP_PUSHDATA2 和 OP_PUSHDATA4。OP_PUSHDATA1 表示下一个字节包含我们需要为元素读取的字节数。OP_PUSHDATA2 表示接下来的 2 个字节包含我们需要为元素读取的字节数。OP_PUSHDATA4 表示接下来的 4 个字节包含我们需要为元素读取的字节数。

实际上，这意味着如果我们有一个长度在 76 到 255 字节（含）之间的元素，我们使用 `OP_PUSHDATA1 <元素长度的1字节> <元素>`。对于任何长度在 256 字节到 520 字节（含）之间的内容，我们使用 `OP_PUSHDATA2 <元素长度的2字节（小端序）> <元素>`。任何大于 520 字节的内容在网络中都不被允许，因此 OP_PUSHDATA4 是不必要的，尽管 `OP_PUSHDATA4 <元素长度的4字节（小端序，但值小于或等于 520）> <元素>` 仍然是合法的。

可以使用 OP_PUSHDATA1 编码小于 76 的数字，或使用 OP_PUSHDATA2 编码小于 256 的数字，甚至可以使用 OP_PUSHDATA4 编码小于 521 的任何数字。然而，这些被视为非标准交易，这意味着大多数比特币节点（尤其是运行 Bitcoin Core 软件的节点）不会转发它们。

还有更多的操作码，它们编码在 op.py 中，完整列表可以在 https://en.bitcoin.it/wiki/Script 找到。

## 编写脚本解析器和序列化器

现在我们知道 Script 是如何工作的，可以编写一个脚本解析器了：

```
class Script:

    def __init__(self, cmds=None):
        if cmds is None:
            self.cmds = []
        else:
            self.cmds = cmds  # ①

    ...
    @classmethod
    def parse(cls, s):
        length = read_varint(s)  # ②
        cmds = []
        count = 0
        while count < length:  # ③
```

