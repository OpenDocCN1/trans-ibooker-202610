## Bloom 过滤器

在第 11 章中，我们学习了如何验证 Merkle 区块。完整节点可以通过 merkleblock 命令为感兴趣的交易提供包含证明。但完整节点如何知道哪些交易是感兴趣的？

轻客户端可以告诉完整节点其地址（或 ScriptPubKeys）。完整节点可以检查与这些地址相关的交易，但这会损害轻客户端的隐私。轻客户端不希望，例如，向完整节点透露它拥有 1,000 BTC。隐私泄露就是安全泄露，在比特币中，通常最好在可能的情况下不泄露任何隐私。

一种解决方案是让轻客户端告诉完整节点足够的信息来创建一个所有感兴趣交易的上界。为了创建此上界，我们使用所谓的 Bloom 过滤器。

## 什么是 Bloom 过滤器？

Bloom 过滤器是对所有可能交易的过滤器。完整节点将交易通过 Bloom 过滤器运行，并发送使过滤器通过的交易的 merkleblock 命令。

假设总共有 50 笔交易。轻客户端对其中一笔交易感兴趣。轻客户端希望“隐藏”该交易在五笔交易组中。这需要一个函数将 50 笔交易分组到 10 个不同的桶中，然后完整节点可以发送一个交易桶，换句话说。这种分组必须是确定性的——也就是说，每次都相同。如何实现？

解决方案是使用哈希函数获取确定性数字并使用取模来将交易组织到桶中。

Bloom 过滤器是一种计算机科学结构，可用于集合中的任何数据，因此假设我们有一个项目“hello world”，我们想为其创建一个 Bloom 过滤器。我们需要一个哈希函数，所以我们使用我们已有的一个：hash256。确定我们的项目进入哪个桶的过程如下所示：

```
>>> from helper import hash256
>>> bit_field_size = 10
>>> bit_field = [0] * bit_field_size
>>> h = hash256(b'hello world')
>>> bit = int.from_bytes(h, 'big') % bit_field_size
>>> bit_field[bit] = 1
>>> print(bit_field)
[0, 0, 0, 0, 0, 0, 0, 0, 0, 1]
```

- 1. 我们的 bit_field 是“桶”的列表，我们希望有 10 个。
- 2. 我们用 hash256 对项目进行哈希。
- 3. 我们将其解释为大端整数并对 10 取模以确定该项目属于哪个桶。
- 4. 我们在 Bloom 过滤器中指示我们想要的桶。

概念上，我们刚刚做的事情看起来像图 12-1。

![](img/6c915536bcfd43e412f7adef942edf23_237_0.png)

图 12-1. 包含一个元素的 10 位 Bloom 过滤器

我们的 Bloom 过滤器由以下几部分组成：

- 1. 位字段的大小
- 2. 使用的哈希函数（以及我们如何将其转换为数字）
- 3. 位字段，指示我们感兴趣的桶

这对于单个项目非常有效，因此对于单个地址/ScriptPubKey/交易 ID 感兴趣的情况也有效。当我们对不止一个项目感兴趣时怎么办？

我们可以将第二个项目通过相同的过滤器运行并将该位也设置为 1。完整节点然后可以发送多个交易桶而不是单个桶。让我们使用以下代码创建包含两个项目“hello world”和“goodbye”的 Bloom 过滤器：

```
>>> from helper import hash256
>>> bit_field_size = 10
>>> bit_field = [0] * bit_field_size
>>> for item in (b'hello world', b'goodbye'):
...     h = hash256(item)
...     bit = int.from_bytes(h, 'big') % bit_field_size
...     bit_field[bit] = 1
>>> print(bit_field)
[0, 0, 1, 0, 0, 0, 0, 0, 0, 1]
```

1 我们在这里为两个项目创建过滤器，但这可以扩展到更多项目。

图 12-2 概念上显示了这一点。

![](img/6c915536bcfd43e412f7adef942edf23_238_0.png)

图 12-2. 包含两个元素的 10 位 Bloom 过滤器

如果所有可能项目的空间为 50，平均有 10 个项目会通过此过滤器，而不是只有 1 个感兴趣项目时的 5 个，因为返回了 2 个桶，而不是 1 个。

## 练习 1

使用 hash160 哈希函数在 10 位字段上计算“hello world”和“goodbye”的 Bloom 过滤器。





# 更进一步

假设所有项目的空间为 100 万个，我们仍然希望桶的大小为 5。我们需要一个长度为 1,000,000 / 5 = 200,000 位的 Bloom 过滤器。每个桶平均有 5 个项目，我们会得到我们感兴趣的项目数量的 5 倍，其中 20% 是感兴趣的项目。200,000 位是 25,000 字节，传输量很大。我们能做得更好吗？

使用多个哈希函数的 Bloom 过滤器可以显著缩短位域。如果我们在 32 位的位域上使用 5 个哈希函数，则该 32 位字段中大约有 32!/(27!5!) ~ 200,000 种可能的 5 位组合。在 100 万个可能的项目中，平均有 5 个项目应具有该 5 位组合。与传输 25K 字节相比，我们只需传输 32 位，即 4 字节！

以下是其外观。 为简单起见，我们坚持使用相同的 10 位字段，但仍然有两个感兴趣的项目：

```
>>> from helper import hash256, hash160
>>> bit_field_size = 10
>>> bit_field = [0] * bit_field_size
>>> for item in (b'hello world', b'goodbye'):
...     for hash_function in (hash256, hash160):
...         h = hash_function(item)
...         bit = int.from_bytes(h, 'big') % bit_field_size
...         bit_field[bit] = 1
>>> print(bit_field)
[1, 1, 1, 0, 0, 0, 0, 0, 1]
```

① 我们迭代了两个不同的哈希函数（hash256 和 hash160），但同样可以轻松使用更多。

概念上，图 12-3 显示了前述代码的作用。

![](img/6c915536bcfd43e412f7adef942edf23_239_0.png)

图 12-3. 具有两个元素和两个哈希函数的 10 位 Bloom 过滤器

通过更改哈希函数数量和位域大小来优化 Bloom 过滤器，以获得理想的误报率。

## BIP0037 Bloom 过滤器

BIP0037 指定了网络通信中的 Bloom 过滤器。Bloom 过滤器包含的信息是：

+   1. 位域的大小，或有多少个桶。大小以字节为单位指定（每字节 8 位），必要时向上取整。
+   2. 哈希函数的数量。
+   3. 一个“tweak”，如果命中太多项目，可以稍微改变 Bloom 过滤器。
+   4. 通过对感兴趣的项目运行 Bloom 过滤器而产生的位域。

虽然我们可以定义很多哈希函数（sha512、keccak384、ripemd160、blake256 等），但在实践中，我们使用一个具有不同种子的单一哈希函数。这使得实现更简单。

我们使用的哈希函数称为 murmur3。与 sha256 不同，murmur3 在密码学上并不安全，但速度快得多。过滤和获得确定性、均匀分布的模数不需要密码学安全性，但受益于速度，因此 murmur3 是这项工作的合适工具。种子公式定义如下：

```
i*0xfba4c795 + tweak
```

fba4c795 是比特币 Bloom 过滤器的常数。i 对于第一个哈希函数为 0，对于第二个为 1，对于第三个为 2，依此类推。tweak 是可以添加的一点熵，如果一个 tweak 的结果不令人满意，可以更改。哈希函数和位域的大小用于计算位域，然后将其传输：

```
>>> from helper import murmur3
>>> from bloomfilter import BIP37_CONSTANT
>>> field_size = 2
>>> num_functions = 2
>>> tweak = 42
>>> bit_field_size = field_size * 8
>>> bit_field = [0] * bit_field_size
>>> for phrase in (b'hello world', b'goodbye'):
...     for i in range(num_functions):
...         seed = i * BIP37_CONSTANT + tweak
...         h = murmur3(phrase, seed=seed)
...         bit = h % bit_field_size
...         bit_field[bit] = 1
>>> print(bit_field)
[0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0]
```

将 Bloom 过滤器本身传达给全节点。设置 Bloom 过滤器的命令称为 filterload。有效负载如下图 12-4 所示。

```
0a4000600a08000000109400500000006300000000
- 0a4000600a0800000010940 - 位域，可变字段
- 05000000 - 哈希计数，4 字节，小端
- 63000000 - Tweak，4 字节，小端
- 00 - 匹配项目标志
```

图 12-4. 解析的 filterload

Bloom 过滤器的元素被编码为字节。位域、哈希函数计数和 tweak 在此消息中编码。最后一个字段，匹配项目标志，是要求全节点将任何匹配的交易添加到 Bloom 过滤器的一种方式。

## 练习 4

为 BloomFilter 类编写 filterload 方法。

## 获取 Merkle 块

轻客户端还需要一个命令：关于来自全节点的感兴趣交易的 Merkle 块信息。getdata 命令是通信块和交易的内容。轻客户端将从全节点想要的特定数据类型称为过滤块。过滤块是请求通过 Bloom 过滤器的交易的形式为 Merkle 块。换句话说，轻客户端可以请求其感兴趣的交易与 Bloom 过滤器匹配的 Merkle 块。

图 12-5 描述了 getdata 的有效负载。

```
020300000030eb2540c41025690160a1014c577061596e32e426b712c7ca00000000000000030000001049847939585b0652fba793661c361223446b6fc41089b8be00000000000000
- 02 - 数据项数
- 03000000 - 数据项类型（tx、block、filtered block、compact block），小端
- 30...00 - 哈希标识符
```

图 12-5. 解析的 getdata

作为 varint 的项目数指定了我们想要的项目数。每个项目都有一个类型。类型值为 1 是交易（第 5 章），2 是普通块（第 9 章），3 是 Merkle 块（第 11 章），4 是紧凑块（本书未涵盖）。

我们可以在 network.py 中创建此消息：

```
class GetDataMessage:
    command = b'getdata'

    def __init__(self):
        self.data = []

    def add_data(self, data_type, identifier):
        self.data.append((data_type, identifier))
```

1 我们存储想要的项目。

2 我们使用 add_data 方法向消息添加项目。

## 练习 5

为 GetDataMessage 类编写 serialize 方法。

## 获取感兴趣的交易

加载了 Bloom 过滤器的轻客户端将获得证明感兴趣的交易包含在特定块中的所需的所有信息：

```
>>> from bloomfilter import BloomFilter
>>> from helper import decode_base58
>>> from merk leblock import MerkleBlock
>>> from network import FILTERED_BLOCK_DATA_TYPE, GetHeadersMessage, GetDataMessage, HeadersMessage, SimpleNode
>>> from tx import Tx
>>> last_block_hex = '00000000000538d5c2246336644f9a4956551afb44ba47278759ec55ea912e19'
>>> address = 'mwJn1YPMq7y5F8J3LkC5Hxg9PHyZ5K4cFv'
>>> h160 = decode_base58(address)
>>> node = SimpleNode('testnet.programmingbitcoin.com', testnet=True, logging=False)
>>> bf = BloomFilter(size=30, function_count=5, tweak=90210)
>>> bf.add(h160)
>>> node.handshake()
>>> node.send(bf.filterload())
>>> start_block = bytes.fromhex(last_block_hex)
>>> getheaders = GetHeadersMessage(start_block=start_block)
>>> node.send(getheaders)
>>> headers = node.wait_for(HeadersMessage)
>>> getdata = GetDataMessage()
>>> for b in headers.blocks:
...     if not b.check_pow():
...         raise RuntimeError('proof of work is invalid')
...     getdata.add_data(FILTERED_BLOCK_DATA_TYPE, b.hash())
>>> node.send(getdata)
```

## 练习 6

获取当前 testnet 块 ID，向自己发送一些 testnet 币，*不使用区块浏览器*找到对应 testnet 币的 UTXO，使用该 UTXO 作为输入创建交易，并在 testnet 网络上广播 tx 消息。

## 结论

在本章中，我们创建了作为轻客户端点对点连接所需的一切，并请求和接收构造交易所需的 UTXO，同时通过使用 Bloom 过滤器保留了一定的隐私。

我们现在转向 Segwit，这是一种在 2017 年激活的新型交易。
