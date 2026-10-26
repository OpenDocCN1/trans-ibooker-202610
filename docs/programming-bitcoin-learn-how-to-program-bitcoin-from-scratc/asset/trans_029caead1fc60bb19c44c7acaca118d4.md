除前面提到的 0/O 和 l/I 以外，所有数字、大写字母和小写字母都可以使用。这样我们就剩下 10 + 26 + 26 − 4 = 58 个字符。这些字符中的每一个都代表 Base58 中的一个数字。我们可以用以下函数进行编码：

```
BASE58_ALPHABET = '123456789ABCDEFGHJKLMNPQRSTUVWXYZabcdefghijkmnopqrstuvwxyz'
...
def encode_base58(s):
    count = 0
    for c in s: ①
        if c == 0:
            count += 1
        else:
            break
    num = int.from_bytes(s, 'big')
    prefix = '1' * count
    result = ''
    while num > 0: ②
        num, mod = divmod(num, 58)
        result = BASE58_ALPHABET[mod] + result
    return prefix + result ③
```

① 这个循环的目的是确定开头有多少个字节是 0 字节。我们想在最后把它们加回去。

② 这个循环用于确定要使用哪个 Base58 数字。

③ 最后，我们把前面数出的所有 0 作为前缀添加到开头，否则它们不会以前缀 1 的形式出现。这种恼人的情况在支付到公钥哈希（p2pkh）中会出现；更多内容见第 6 章。

这个函数将接受 Python 3 中的任何字节，并将其转换为 Base58。

![](img/6c915536bcfd43e412f7adef942edf23_107_0.png)

## 为什么 Base58 正在被淘汰

Base58 已经使用了很长时间。虽然它确实比像 Base64 这样的编码在交流时稍方便一些，但其实并没有那么方便。大多数人更喜欢复制粘贴地址，而如果你曾经尝试口头传达一个 Base58 地址，你就会知道这可能是一场噩梦。

更好的方案是新的 Bech32 标准，它在 BIP0173 中定义。Bech32 使用一个 32 字符的字母表，只包含数字和小写字母，但 1、b、i 和 o 除外。到目前为止，它仅用于 Segwit（第 13 章）。

```python
def hash160(s):
    '''sha256 followed by ripemd160'''
    return hashlib.new('ripemd160', hashlib.sha256(s).digest()).digest()
```

注意，hashlib.sha256(s).digest() 执行 sha256，外层则执行 ripemd160。

我们还可以为 S256Point 添加 hash160 和 address 方法：

```python
class S256Point:
    ...
    def hash160(self, compressed=True):
        return hash160(self.sec(compressed))
    def address(self, compressed=True, testnet=False):
        '''Returns the address string'''
        h160 = self.hash160(compressed)
        if testnet:
            prefix = b'\x6f'
        else:
            prefix = b'\x00'
        return encode_base58_checksum(prefix + h160)
```

### 练习 5

找出与以下私钥值对应的公钥地址：

-   5002（在测试网上使用未压缩 SEC）
-   2020⁵（在测试网上使用压缩 SEC）
-   0x12345deadbeef（在主网上使用压缩 SEC）

## WIF 格式

在我们的场景中，私钥是一个 256 位的数字。通常，我们不需要经常序列化我们的私钥，因为它不会被广播（那会是个坏主意！）。尽管如此，在某些情况下，你可能希望将私钥从一个钱包转移到另一个钱包——例如，从纸钱包转移到软件钱包。

为此，你可以使用钱包导入格式（WIF）。WIF 是私钥的一种序列化形式，旨在便于人类阅读。WIF 使用与地址相同的 Base58 编码。

WIF 格式的创建方式如下：

1.  对于主网私钥，以前缀 0x80 开头；对于测试网，以前缀 0xef 开头。
2.  以 32 字节大端序对私钥进行编码。
3.  如果用于公钥地址的 SEC 格式是压缩的，则添加后缀 0x01。
4.  将从第 1 步得到的前缀、从第 2 步得到的序列化私钥以及从第 3 步得到的后缀组合起来。
5.  对第 4 步的结果进行 hash256 运算，并取前 4 个字节。
6.  将第 4 步和第 5 步的结果组合起来，用 Base58 编码。

现在我们可以为 PrivateKey 类创建 wif 方法：

```python
class PrivateKey:
    ...
    def wif(self, compressed=True, testnet=False):
        secret_bytes = self.secret.to_bytes(32, 'big')
        if testnet:
            prefix = b'\xef'
        else:
            prefix = b'\x80'
        if compressed:
            suffix = b'\x01'
        else:
            suffix = b''
        return encode_base58_checksum(prefix + secret_bytes + suffix)
```

### 练习 6

找出私钥值为以下内容的 WIF：

-   5003（压缩，测试网）
-   2021⁵（未压缩，测试网）
-   0x54321deadbeef（压缩，主网）

## 再谈大端序与小端序

了解如何在 Python 中处理大端序和小端序将非常有用，因为在接下来的几章中，我们会大量地在数字与大小端序之间进行解析和序列化。特别是，Satoshi 在比特币中大量使用了小端序，但遗憾的是，并没有一条易学的规则来说明哪里使用小端序、哪里使用大端序。回想一下，SEC 格式使用大端序编码，地址和 WIF 也是如此。从第 5 章开始，我们将更多地使用小端序编码。因此，我们来做接下来的两个练习。本节的最后一个练习是为你自己创建一个测试网地址。

## 交易

交易是比特币的核心。简单来说，交易就是价值从一个实体转移到另一个实体。我们将在第 6 章看到，这里的“实体”实际上是智能合约——不过我们有些超前了。让我们先看看比特币中的交易是什么、它们长什么样，以及如何解析它们。

### 交易组成部分

从高层来看，一笔交易实际上只有四个组成部分。它们是：

1.  版本
2.  输入
3.  输出
4.  锁定时间

对这些字段进行一个总体概述可能会有所帮助。版本表示交易使用了哪些附加功能；输入定义了正在花费哪些比特币；输出定义了比特币的去向；锁定时间定义了该交易从何时开始有效。我们将深入讲解每个组成部分。

图 5-1 显示了一个典型交易的十六进制转储，展示了哪部分对应哪个字段。

```
010000001813f79011acb80925dfe69b3def355fe914bd1d96a3f5f71bf8303c6a989c7d1000000
06b483045022100ed81ff192e75a3fd2304004dcadb746fa5e24c5031ccfcf21320b0277457c98f02
207a986d955c6e0cb35d44a89d3f56100f4d7f67801c31967743a9c8e10615bed01210349fc4e631
e362a545de3f89f5d8684c7b8138bd94bdd53d12e213bf016b278afefffffff02a135ef0100000000
1976a914bc3b654dca7e56b04dca18f2566cdaf02e8d9ada88ac99c39800000000001976a9141c4bc
762dd5423e332166702cb75f40df79fea1288ac19430600
```

*图 5-1：交易组成部分：版本、输入、输出和锁定时间*

不同高亮部分分别代表版本、输入、输出和锁定时间。

基于这一点，我们可以开始构建交易类，我们将其命名为 Tx：

```python
class Tx:

    def __init__(self, version, tx_ins, tx_outs, locktime, testnet=False):
        self.version = version
        self.tx_ins = tx_ins
        self.tx_outs = tx_outs
        self.locktime = locktime
        self.testnet = testnet

    def __repr__(self):
        tx_ins = ''
        for tx_in in self.tx_ins:
            tx_ins += tx_in.__repr__() + '\n'
        tx_outs = ''
        for tx_out in self.tx_outs:
            tx_outs += tx_out.__repr__() + '\n'
        return 'tx: {}\nversion: {}\ntx_ins:{}\ntx_outs:\n{}locktime: {}'.format(
            self.id(),
            self.version,
            tx_ins,
            tx_outs,
            self.locktime,
        )

    def id(self):
        '''Human-readable hexadecimal of the transaction hash'''
        return self.hash().hex()

    def hash(self):
        '''Binary hash of the legacy serialization'''
        return hash256(self.serialize())[::-1]
```

1.  输入和输出是非常通用的术语，因此我们需要指定它们具体是哪类输入。我们稍后会定义具体的对象类型。
2.  我们需要知道这笔交易在哪个网络上，才能完全验证它。
3.  id 是区块浏览器用来查找交易的方式。它是交易的 hash256 的十六进制格式。
4.  hash 是序列化的小端序 hash256。注意我们还没有 serialize 方法；所以在实现之前，这段代码无法运行。

本章其余部分将关注如何解析交易。在这一点上，我们可以编写这样的代码：

```python
class Tx:
    ...
    @classmethod ①
    def parse(cls, serialization):
        version = serialization[0:4] ②
        ...
```

① 这个方法必须是类方法，因为序列化将返回一个新的 Tx 对象实例。

② 这里我们假设变量 serialization 是一个字节数组。

这当然可行，但交易可能非常大。理想情况下，我们希望改为从流中解析。这样我们就不需要在开始解析之前获得完整的序列化交易，从而可以及早失败并提高效率。因此，解析交易的代码将更像是这样：

```python
class Tx:
    ...
    @classmethod
    def parse(cls, stream):
        serialized_version = stream.read(4) ①
        ...
```

① read 方法允许我们即时解析，因为不必等待 I/O。

从工程角度来看，这是有利的，因为流可以是网络上的套接字连接或文件句柄。我们可以立即开始解析流，而不必等待整个内容先被传输或读取完毕。我们的方法将能够处理任何类型的流，并返回我们所需的 Tx 对象。
