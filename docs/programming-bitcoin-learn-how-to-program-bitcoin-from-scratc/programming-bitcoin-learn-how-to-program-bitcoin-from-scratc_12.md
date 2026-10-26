## 高级主题和下一步

如果你读到这里，恭喜你！你已经了解了很多关于比特币内部运作的知识，希望你能受到启发去学习更多。本书只是触及了表面。在本章中，我们将介绍一些你可能想要探索的其他主题，如何作为比特币开发者启动你的职业生涯，以及如何为社区做出贡献。

### 建议下一步学习主题

### 钱包

创建钱包是一项具有挑战性的任务，因为保护私钥非常困难。话虽如此，创建钱包有一堆标准可以帮助。

### 分层确定性钱包

出于隐私目的，重用地址是非常糟糕的（参见第 7 章）。这意味着我们需要创建大量地址。不幸的是，为每个生成的地址存储不同的密钥可能会成为安全和备份问题。你如何以安全的方式备份它们全部？你生成大量密钥然后备份吗？如果密钥用完了怎么办？你如何再次备份？使用什么系统可以确保备份是最新的？

为了解决这个问题，Armory，一个早期的比特币钱包，首先实现了确定性钱包。确定性钱包的想法是你可以用一个种子生成大量不同的地址。Armory 风格的确定性钱包很棒，除了人们想要对地址进行一些分组——因此，分层确定性（HD）钱包标准 BIP0032 诞生了。BIP0032 钱包有多层和密钥，每个都有唯一的派生路径。规范和测试向量在 BIP0032 标准中定义，因此在测试网上实现你自己的 HD 钱包是学习的好方法。

此外，BIP0044 定义了 BIP0032 层次结构中每一层可以意味着什么以及使用单个 HD 种子存储来自大量不同加密货币的硬币的最佳实践。实现 BIP0044 也可以更好地理解 HD 钱包基础设施。虽然许多钱包（Trezor、Coinomi 等）同时实现了 BIP0032 和 BIP0044，但一些钱包完全忽略了 BIP0044 并使用自己的 BIP0032 层次结构（Electrum 和 Edge 是两个例子）。

### 助记符种子

写下并转录一个 256 位种子很痛苦，并且容易出错。为了解决这个问题，BIP0039 描述了一种将种子编码成一堆英文单词的方法。有 2,048 个可能的单词，或 2¹¹，这意味着每个单词编码了种子的 11 位。该标准精确定义了助记符备份如何转换为 BIP0032 种子。BIP0039 连同 BIP0032 和 BIP0044 是大多数钱包实现备份和恢复的方式。编写一个在测试网上实现 BIP0039 的钱包是体验比特币开发的另一个好方法。

### 支付通道和闪电网络

支付通道是闪电网络的原子单元，学习它们的工作原理是下一步的好选择。有很多方法可以实现支付通道，但 BOLT 标准是闪电节点使用的规范。截至本文撰写时，规范仍在进行中，可在 https://github.com/lightningnetwork/lightning-rfc/ 获取。

### 贡献

比特币精神很大一部分是回馈社区。你可以通过开源项目做到这一点。有太多项目无法全部列出，但这里有一个样本：

- Bitcoin Core：参考客户端
- Libbitcoin：C++ 中的比特币替代实现
- btcd：基于 Golang 的比特币实现
- Bcoin：由 purse.io 维护的基于 JavaScript 的比特币实现





>>> print((52-30-38)%prime)
41

## 练习 3

编写相应的 __sub__ 方法，用于定义两个 FieldElement 对象的减法。

```
class FieldElement:
    ...
    def __sub__(self, other):
        if self.prime != other.prime:
            raise TypeError('Cannot subtract two numbers in different Fields')
        # self.num and other.num are the actual values
        # self.prime is what we need to mod against
        num = (self.num - other.num) % self.prime
        # we return an element of the same class
        return self.__class__(num, self.prime)
```

## 练习 4

在 F₉₇ 中求解以下方程（同样，假设 · 和幂运算为域版本）：

- 95 · 45 · 31
- 17 · 13 · 19 · 44
- 12⁷ · 77⁴⁹

```
>>> prime = 97
>>> print(95*45*31 % prime)
23
>>> print(17*13*19*44 % prime)
68
>>> print(12**7*77**49 % prime)
63
```

## 练习 5

对于 k = 1, 3, 7, 13, 18，在 F₁₉ 中这个集合是什么？

{k · 0, k · 1, k · 2, k · 3, ... k · 18}

你注意到这些集合有什么特点吗？

```
>>> prime = 19
>>> for k in (1,3,7,13,18):
...     print([k*i % prime for i in range(prime)])
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18]
[0, 3, 6, 9, 12, 15, 18, 2, 5, 8, 11, 14, 17, 1, 4, 7, 10, 13, 16]
```

>>> y3 = s*(x1-x3)-y1
>>> print(x3,y3)
18.0 -77.0

## 练习 7

编写当 P₁ = P₂ 时的 __add__ 方法。

```
class Point:
    ...
    def __add__(self, other):
        ...
if self == other:
    s = (3 * self.x**2 + self.a) / (2 * self.y)
    x = s**2 - 2 * self.x
    y = s * (self.x - x) - self.y
    return self.__class__(x, y, self.a, self.b)
```

## 第 3 章：椭圆曲线密码学

## 练习 1

判断以下点是否在 F₂₂₃ 上的曲线 y² = x³ + 7 上：

(192,105), (17,56), (200,119), (1,193), (42,99)

```
>>> from ecc import FieldElement
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> def on_curve(x,y):
...     return y**2 == x**3 + a*x + b
>>> print(on_curve(FieldElement(192, prime), FieldElement(105, prime)))
True
>>> print(on_curve(FieldElement(17, prime), FieldElement(56, prime)))
True
>>> print(on_curve(FieldElement(200, prime), FieldElement(119, prime)))
False
>>> print(on_curve(FieldElement(1, prime), FieldElement(193, prime)))
True
>>> print(on_curve(FieldElement(42, prime), FieldElement(99, prime)))
False
```

## 练习 2

对于 F₂₂₃ 上的曲线 y² = x³ + 7，求：

- (170,142) + (60,139)
- (47,71) + (17,56)
- (143,98) + (76,66)

```
>>> from ecc import FieldElement, Point
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> p1 = Point(FieldElement(170, prime), FieldElement(142, prime), a, b)
>>> p2 = Point(FieldElement(60, prime), FieldElement(139, prime), a, b)
>>> print(p1+p2)
Point(220,181)_0_7 FieldElement(223)
>>> p1 = Point(FieldElement(47, prime), FieldElement(71, prime), a, b)
>>> p2 = Point(FieldElement(17, prime), FieldElement(56, prime), a, b)
>>> print(p1+p2)
Point(215,68)_0_7 FieldElement(223)
>>> p1 = Point(FieldElement(143, prime), FieldElement(98, prime), a, b)
>>> p2 = Point(FieldElement(76, prime), FieldElement(66, prime), a, b)
>>> print(p1+p2)
Point(47,71)_0_7 FieldElement(223)
```

## 练习 3

扩展 ECCTest，以测试上一练习中的加法运算。将此方法命名为 test_add。

```
def test_add(self):
    prime = 223
    a = FieldElement(0, prime)
    b = FieldElement(7, prime)
    additions = (
        (192, 105, 17, 56, 170, 142),
        (47, 71, 117, 141, 60, 139),
        (143, 98, 76, 66, 47, 71),
    )
    for x1_raw, y1_raw, x2_raw, y2_raw, x3_raw, y3_raw in additions:
        x1 = FieldElement(x1_raw, prime)
        y1 = FieldElement(y1_raw, prime)
        p1 = Point(x1, y1, a, b)
        x2 = FieldElement(x2_raw, prime)
        y2 = FieldElement(y2_raw, prime)
        p2 = Point(x2, y2, a, b)
        x3 = FieldElement(x3_raw, prime)
        y3 = FieldElement(y3_raw, prime)
        p3 = Point(x3, y3, a, b)
        self.assertEqual(p1 + p2, p3)
```

## 练习 4

对于 F₂₂₃ 上的曲线 y² = x³ + 7，求：

- 2 · (192,105)
- 2·(143,98)
- 2·(47,71)
- 4·(47,71)
- 8·(47,71)
- 21·(47,71)

```
>>> from ecc import FieldElement, Point
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> x1 = FieldElement(num=192, prime=prime)
>>> y1 = FieldElement(num=105, prime=prime)
>>> p = Point(x1,y1,a,b)
>>> print(p+p)
Point(49,71)_0_7 FieldElement(223)
>>> x1 = FieldElement(num=143, prime=prime)
>>> y1 = FieldElement(num=98, prime=prime)
>>> p = Point(x1,y1,a,b)
>>> print(p+p)
Point(64,168)_0_7 FieldElement(223)
>>> x1 = FieldElement(num=47, prime=prime)
>>> y1 = FieldElement(num=71, prime=prime)
>>> p = Point(x1,y1,a,b)
>>> print(p+p)
Point(36,111)_0_7 FieldElement(223)
>>> print(p+p+p+p)
Point(194,51)_0_7 FieldElement(223)
>>> print(p+p+p+p+p+p+p+p)
Point(116,55)_0_7 FieldElement(223)
>>> print(p+p+p+p+p+p+p+p+p+p+p+p+p+p+p+p)
Point(infinity)
```

## 练习 5

对于 F₂₂₃ 上的曲线 y² = x³ + 7，求由 (15,86) 生成的群的阶。

```
>>> prime = 223
>>> a = FieldElement(0, prime)
>>> b = FieldElement(7, prime)
>>> x = FieldElement(15, prime)
>>> y = FieldElement(86, prime)
>>> p = Point(x, y, a, b)
>>> inf = Point(None, None, a, b)
>>> product = p
>>> count = 1
>>> while product != inf:
...     product += p
...     count += 1
>>> print(count)
7
```

```
>>> from ecc import PrivateKey
>>> priv = PrivateKey(5002)
>>> print(priv.point.address(compressed=False, testnet=True))
mmTPbQXFboEtNRkwhf6K51jvdtHLxGeMA
>>> priv = PrivateKey(2020**5)
>>> print(priv.point.address(compressed=True, testnet=True))
mopVkx p8UhxQRYbCYJsbeE1hifiF64jcoH
>>> priv = PrivateKey(0x1234deadbeef)
>>> print(priv.point.address(compressed=True, testnet=False))
1F1Pn2y6pDb68E5nYJJeba4TLg2U7B6KF1
```

## 练习 6

求以下私密值对应的私钥的 WIF：

- 5003 (压缩, 测试网)
- 2021⁵ (未压缩, 测试网)
- 0x54321deadbeef (压缩, 主网)

```
>>> from ecc import PrivateKey
>>> priv = PrivateKey(5003)
>>> print(priv.wif(compressed=True, testnet=True))
cMahea7zqjxrtgAbB7LSGbcQUr1uX1oJuat9jZodMN8rFTv2s fUK
>>> priv = PrivateKey(2021**5)
>>> print(priv.wif(compressed=False, testnet=True))
91avARGdfge8E4tZFYLoxeJ5sGbDNJQH4kvjpWAXgzcZjbCwxic
>>> priv = PrivateKey(0x54321deadbeef)
>>> print(priv.wif(compressed=True, testnet=False))
KwDiBf890gGbJEhKnhXJuH7LrciVrZi3qYjgiuQJv1h8Ytr2S53a
```

## 练习 7

编写一个函数 little_endian_to_int，它接受 Python 字节，将这些字节按小端序解释，并返回该数字。

```
def little_endian_to_int(b):
    """little_endian_to_int takes byte sequence as a little-endian number.
    Returns an integer"""
    return int.from_bytes(b, 'little')
```

## 练习 8

编写一个函数 int_to_little_endian，该函数是上一练习的逆操作。

```
def int_to_little_endian(n, length):
    """endian_to_little_endian takes an integer and returns the little-endian byte sequence of length"""
    return n.to_bytes(length, 'little')
```

```
inputs = []
for _ in range(num_inputs):
    inputs.append(TxIn.parse(s))
num_outputs = read_varint(s)
outputs = []
for _ in range(num_outputs):
    outputs.append(TxOut.parse(s))
locktime = little_endian_to_int(s.read(4))
return cls(version, inputs, outputs, locktime, testnet=testnet)
```

## 练习 5

对于这笔交易，第二个输入的 ScriptSig、第一个输出的 ScriptPubKey 以及第二个输出的金额分别是什么？

010000000456919960ac691763688d3d3bcea9ad6ecaf875df5339e148a1fc61c6ed7a069e0100 0006a4703440220458bcdef85e6b1c6af5c2669d4830ff86e42dd205c0e089bc2a821657e951 c002201024a10366077f87d6bce1f7100ad8cfa8a064b39d4e8fe4e1a13a7b71aa8180f012102f0 da57e85eec2934a82a585ea337ce2f4998b50ae699dd79f5880e253dafafb7feffffffeb8f51f4 038dc17e6313cf831d4f02281c2a468bde0fafd37f1bf882729e7fd3000000006a473044022078 99531a5d2d59a6de200179928ca900254a36b8ddf8bb75f5f5d7b1cdc26152020008b422690b84 61c52c3cc303b30d23d57435187b2c7c361e9aae3649071c1a7160121035c93d9ac96881f19ba 1f686f15f009ded7c62efe85a872e6a19b43c15a2937feffffff567bf40595119d1bb8a3037c35 6efd56170b64cbc160fb028fa10704b45d775000000006a47304402204c7c818424c7f7911da 6cdc59655a70af1c5eaf17c69dadbfc74ffa0b662f02207599e08bc8023693a4de49527d42c3 4210f7a7d1ddfc8492b654a11e72620a0012120158b46fdbf65d0172b798ae8850aa0dae49 abfb84c81ae6e5b251a58ace5cfefffffd63a5e6c16e620f86f375925b21cabaf736c779f88fd 04dcad51d26690f7f345010000006a47304402200633ea0d3314bea0d95b3cd8dadb2ef79ea833 1ffe1e61f762c0f6daea0fabde022029f23b3e9c30f080446150b23852028751635dcee2be669c 2a1686a4b5edf304012103ffd6f4a67e94aba353a008e2e563ff772e6b4cff0ad6006e86ee20df e7520d55fefffff0251430f000000000001976a914ab0c0b2e98b1ab6dbf67d4750b0a56244948 a87988ac005a6202000000001976a914c82d7df364eb6c75be8c80df2b3eda8db57397088ac46 430600

```
>>> from io import BytesIO
>>> from tx import Tx
>>> hex_transaction = '01000000456919960ac691763688d3d3bcea9ad6ecaf875df5339e148a1fc61c6ed7a069e0100\n0006a4703440220458bcdef85e6b1c6af5c2669d4830ff86e42dd205c0e089bc2a821657e951\n c002201024a10366077f87d6bce1f7100ad8cfa8a064b39d4e8fe4e1a13a7b71aa8180f012102f0da57e85eec2934a82a585ea337ce2f4998b50ae699dd79f5880e253dafafb7feffffffeb8f51f4\n038dc17e6313cf831d4f02281c2a468bde0fafd37f1bf882729e7fd3000000006a473044022078\n99531a5d2d59a6de200179928ca900254a36b8ddf8bb75f5f5d7b1cdc26152020008b422690b84\n61c52c3cc303b30d23d57435187b2c7c361e9aae3649071c1a7160121035c93d9ac96881f19ba1f686f15f009ded7c62efe85a872e6a19b43c15a2937feffffff567bf40595119d1bb8a3037c356efd56170b64cbc160fb028fa10704b45d775000000006a47304402204c7c818424c7f7911da\n2204c7f781842c7f7911da6cdc59655a70af1c5eaf17c69dadbfc74ffa0b662f02207599e08bc8023693a4de49527d42c3\n4210f7a7d1ddfc8492b654a11e72620a0012120158b46fdbf65d0172b798ae8850aa0dae49abfb84c81ae6e5b251a58ace5cfefffffd63a5e6c16e620f86f375925b21cabaf736c779f88fd04dcad51d26690f7f345010000006a47304402200633ea0d3314bea0d95b3cd8dadb2ef79ea8331ffe1e61f762c0f6daea0fabde022029f23b3e9c30f080446150b23852028751635dcee2be669c2a1686a4b5edf304012103ffd6f4a67e94aba353a008e2e563ff7722eb4cff0ad6006e86ee20dfe7520d55fefffff0251430f00000000001976a914ab0c0b2e98b1a'
```





```python
b6dbf67d4750b0a56244948a87988ac005a6202000000001976a9143c82d7df364eb6c75be8c80df2b3eda8db57397088ac464306001
>>> stream = BytesIO(bytes.fromhex(hex_transaction))
>>> tx_obj = Tx.parse(stream)
>>> print(tx_obj.tx_ins[1].script_sig)
304402207899531a52d59a6de200179928ca900254a36b8dff8bb75f5f5d71b1cdc26125022008b422690b8461cb52c3c30330b23d574351872b7c361e9aae3649071c1a71601 035d5c93d9ac96881f9ba1f686f15f009ded7c62efe85a872e6a19b43c15a2937
>>> print(tx_obj.tx_outs[0].script_pubkey)
OP_DUP OP_HASH160 ab0c0b2e98b1ab6dbf67d4750b0a56244948a879 OP_EQUALVERIFY OP_CHECKSIG
>>> print(tx_obj.tx_outs[1].amount)
40000000
```

### Exercise 6

为 Tx 类编写 fee 方法。

```python
class Tx:
    ...
    def fee(self, testnet=False):
        input_sum, output_sum = 0, 0
        for tx_in in self.tx_ins:
            input_sum += tx_in.value(testnet=testnet)
        for tx_out in self.tx_outs:
            output_sum += tx_out.amount
        return input_sum - output_sum
```

## Chapter 6: Script

### Exercise 1

编写 op_hash160 函数。

```python
def op_hash160(stack):
    if len(stack) < 1:
        return False
    element = stack.pop()
    h160 = hash160(element)
    stack.append(h160)
    return True
```

### Exercise 2

在 op.py 中编写 op_checksig 函数。

```python
def op_checksig(stack, z):
    if len(stack) < 2:
        return False
    sec_pubkey = stack.pop()
    der_signature = stack.pop()[:-1]
```

### Exercise 4

创建一个 testnet 交易，将单个 UTXO 的 60% 发送到 mwJn1YPMq7y5F8J3LkC5Hxg9PHyZSK4cFv。剩余金额减去手续费后应退回你自己的找零地址。这应为一输入、两输出的交易。

你可以在 https://live.blockcypher.com/btc/pushtx 上广播交易。

```python
>>> from ecc import PrivateKey
>>> from helper import decode_base58, SIGHASH_ALL
>>> from script import p2pkh_script, Script
>>> from tx import TxIn, TxOut, Tx
>>> prev_tx = bytes.fromhex('75a1c4bc671f55f626dda1074c7725991e6f68b8fcefcfca7b64405ca3b45f1c')
>>> prev_index = 1
>>> target_address = 'miKegze5FQNcGw6PKyqUbYUeBa4x2hFeM'
>>> target_amount = 0.01
>>> change_address = 'mzX5YhAH9KNhtcN481uWkJeHJYtVeKVh2'
>>> change_amount = 0.009
>>> secret = 8675309
>>> priv = PrivateKey(secret=secret)
>>> tx_ins = []
>>> tx_ins.append(TxIn(prev_tx, prev_index))
>>> tx_outs = []
>>> h160 = decode_base58(target_address)
>>> script_pubkey = p2pkh_script(h160)
>>> target_satoshis = int(target_amount*100000000)
>>> tx_outs.append(TxOut(target_satoshis, script_pubkey))
>>> h160 = decode_base58(change_address)
>>> script_pubkey = p2pkh_script(h160)
>>> change_satoshis = int(change_amount*100000000)
>>> tx_outs.append(TxOut(change_satoshis, script_pubkey))
>>> tx_obj = Tx(1, tx_ins, tx_outs, 0, testnet=True)
>>> print(tx_obj.sign_input(0, priv))
True
>>> print(tx_obj.serialize().hex())
0100000001c5fb4a35c40647bcaefeffcb868f1e9925774c07a1dd26f6551f67bcc4a17501000000b483045022100a08ebb9422b3599a2d2fcdaa11f8f807a66ccf33e7f4a9ff0a3c51f1b1ec5dd02205ed21dfede5925362b8d9833e908646c54be7ac6664e31650159e8f69b6ca539012103935581e52c354cd2f484fe8ed83af7a3097005b2f9c60bff71d35bd795f54b67fffffff024020f0000000001976a9141ec51b3654c1f1d0f4929d11a1f702937eaf50c888ac9fbb0d0000000001976a914d52ad7ca9b3d096a38e752c2018e6fbc40cdf26f88ac00000000
```

### Exercise 5

高级：从 testnet 水龙头获取更多 testnet 币并创建一个双输入、单输出的交易。一个输入应来自水龙头，另一个应来自上一练习；输出可以是你的地址。

你可以在 https://live.blockcypher.com/btc/pushtx/ 上广播交易。

```python
        command = command.strip(b'\x00')
        payload_length = little_endian_to_int(s.read(4))
        checksum = s.read(4)
        payload = s.read(payload_length)
        calculated_checksum = hash256(payload)[:4]
        if calculated_checksum != checksum:
            raise IOError('checksum does not match')
        return cls(command, payload, testnet=testnet)
```

### Exercise 2

确定这条网络消息是什么：

f9beb4d976657261636b000000000000000000005df6e0e2

```python
class NetworkEnvelope:
    ...
>>> from network import NetworkEnvelope
>>> from io import BytesIO
>>> message_hex = 'f9beb4d976657261636b000000000000000000005df6e0e2'
>>> stream = BytesIO(bytes.fromhex(message_hex))
>>> envelope = NetworkEnvelope.parse(stream)
>>> print(envelope.command)
b'verack'
>>> print(envelope.payload)
b''
```

### Exercise 3

为 NetworkEnvelope 编写 serialize 方法。

```python
class NetworkEnvelope:
    ...
    def serialize(self):
        result = self.magic
        result += self.command + b'\x00' * (12 - len(self.command))
        result += int_to_little_endian(len(self.payload), 4)
        result += hash256(self.payload)[:4]
        result += self.payload
        return result
```

### Exercise 4

为 VersionMessage 编写 serialize 方法。

```python
class VersionMessage:
    ...
    def serialize(self):
        result = int_to_little_endian(self.version, 4)
        result += int_to_little_endian(self.services, 8)
        result += int_to_little_endian(self.timestamp, 8)
        result += int_to_little_endian(self.receiver_services, 8)
        result += b'\x00' * 10 + b'\xff\xff' + self.receiver_ip
        result += int_to_little_endian(self.receiver_port, 2)
        result += int_to_little_endian(self.sender_services, 8)
        result += b'\x00' * 10 + b'\xff\xff' + self.sender_ip
        result += int_to_little_endian(self.sender_port, 2)
        result += self.nonce
        result += encode_varint(len(self.user_agent))
        result += self.user_agent
        result += int_to_little_endian(self.latest_block, 4)
        if self.relay:
            result += b'\x01'
        else:
            result += b'\x00'
        return result
```

### Exercise 5

为 `SimpleNode` 编写 handshake 方法。

```python
class SimpleNode:
    ...
    def handshake(self):
        version = VersionMessage()
        self.send(version)
        self.wait_for(VerAckMessage)
```

### Exercise 6

为 `GetHeadersMessage` 编写 `serialize` 方法。

```python
class GetHeadersMessage:
    ...
    def serialize(self):
        result = int_to_little_endian(self.version, 4)
        result += encode_varint(self.num_hashes)
        result += self.start_block[::-1]
        result += self.end_block[::-1]
        return result
```

## Chapter 11: Simplified Payment Verification

### Exercise 1

编写 `merkle_parent` 函数。

```python
def merkle_parent(hash1, hash2):
    '''Takes the binary hashes and calculates the hash256'''
    return hash256(hash1 + hash2)
```

```python
>>> for level in merkle_tree:
...     print(level)
[None]
[None, None]
[None, None, None, None]
[None, None, None, None, None, None, None]
[None, None, None, None, None, None, None, None, None, None, None, None, None, None, None]
[None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None]
```

### Exercise 6

为 MerkleBlock 编写 parse 方法。

```python
class MerkleBlock:
    ...
    @classmethod
    def parse(cls, s):
        version = little_endian_to_int(s.read(4))
        prev_block = s.read(32)[::-1]
        merkle_root = s.read(32)[::-1]
        timestamp = little_endian_to_int(s.read(4))
        bits = s.read(4)
        nonce = s.read(4)
        total = little_endian_to_int(s.read(4))
        num_hashes = read_varint(s)
        hashes = []
        for _ in range(num_hashes):
            hashes.append(s.read(32)[::-1])
        flags_length = read_varint(s)
        flags = s.read(flags_length)
        return cls(version, prev_block, merkle_root, timestamp, bits,
                   nonce, total, hashes, flags)
```

### Exercise 7

为 MerkleBlock 编写 is_valid 方法。

```python
class MerkleBlock:
    ...
    def is_valid(self):
        flag_bits = bytes_to_bit_field(self.flags)
        hashes = [h[::-1] for h in self.hashes]
        merkle_tree = MerkleTree(self.total)
        merkle_tree.populate_tree(flag_bits, hashes)
        return merkle_tree.root()[::-1] == self.merkle_root
```


```python
...     getdata.add_data(FILTERED_BLOCK_DATA_TYPE, b.hash())
...     # set the last block to the current hash
...     last_block = b.hash()
>>> # send the getdata message
>>> node.send(getdata)
>>> # initialize prev_tx, prev_index, and prev_amount to None
>>> prev_tx, prev_index, prev_amount = None, None, None
>>> # loop while prev_tx is None
>>> while prev_tx is None:
...     # wait for the merkleblock or tx commands
...     message = node.wait_for(MerkleBlock, Tx)
...     # if we have the merkleblock command
...     if message.command == b'merkleblock':
...         # check that the MerkleBlock is valid
...         if not message.is_valid():
...             raise RuntimeError('invalid merkle proof')
...     # else we have the tx command
... else:
...     # set the tx's testnet to be True
...     message.testnet = True
...     # loop through the tx outs
...     for i, tx_out in enumerate(message.tx_outs):
...         # if our output has the same address as our address we found it
...         if tx_out.script_pubkey.address(testnet=True) == addr:
...             # we found our utxo; set prev_tx, prev_index, and tx
...             prev_tx = message.hash()
...             prev_index = i
...             prev_amount = tx_out.amount
...             print('found: {:}:{}'.format(prev_tx.hex(), prev_index))
found: b2cddd41d18d00910f88c31aa58c6816a190b8fc30fe7c665e1cd2ec60efd3f:7
>>> # create the TxIn
>>> tx_in = TxIn(prev_tx, prev_index)
>>> # calculate the output amount (previous amount minus the fee)
>>> output_amount = prev_amount - fee
>>> # create a new TxOut to the target script with the output amount
>>> tx_out = TxOut(output_amount, target_script)
>>> # create a new transaction with the one input and one output
>>> tx_obj = Tx(1, [tx_in], [tx_out], 0, testnet=True)
>>> # sign the only input of the transaction
>>> print(tx_obj.sign_input(0, private_key))
True
>>> # serialize and hex to see what it looks like
>>> print(tx_obj.serialize().hex())
0100000001fd...e500000000001976a914ad346f8eb57dee9a37981716a498120ae80e44f788ac00000000
>>> # send this signed transaction on the network
>>> node.send(tx_obj)
>>> # wait a sec so this message goes through with time.sleep(1)
>>> time.sleep(1)
>>> # now ask for this transaction from the other node
>>> # create a GetDataMessage
>>> getdata = GetDataMessage()
>>> # ask for our transaction by adding it to the message
>>> getdata.add_data(TX_DATA_TYPE, tx_obj.hash())
>>> # send the message
>>> node.send(getdata)
>>> # now wait for a Tx response
>>> received_tx = node.wait_for(Tx)
>>> # if the received tx has the same id as our tx, we are done!
>>> if received_tx.id() == tx_obj.id():
...     print('success!')
success!
```
