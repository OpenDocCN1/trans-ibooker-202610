## 交易创建和验证

在比特币中编码最棘手的事情之一是验证交易。另一个是创建交易。在本章中，我们将介绍执行这两者的确切步骤。章节末尾，我们将创建一个测试网交易并广播它。

### 验证交易

每个节点在接收交易时都会确保每笔交易都遵守网络规则。此过程称为 *交易验证*。节点检查的主要内容如下：

1. 交易的输入是之前未花费的。
2. 输入的总和大于或等于输出的总和。
3. ScriptSig 成功解锁之前的 ScriptPubKey。

#1 防止双花。任何已被花费的输入（即已包含在区块链中）都不能再次花费。

#2 确保没有新比特币被创建（除了一种特殊类型的交易称为 coinbase 交易；将在第 9 章详细介绍）。

#3 确保组合脚本有效。在绝大多数交易中，这意味着检查 ScriptSig 中的一个或多个签名是否有效。

让我们看看每个条件是如何检查的。

```python
>>> from ecc import S256Point, Signature
>>> sec = bytes.fromhex('0349fc4e631e3624a545de3f89f5d8684c7b8138bd94bdd531d2e213bf016b278a')
>>> der = bytes.fromhex('3045022100ed81ff192e75a3fd2304004dcadb746fa5e24c5031ccfcf21320b0277457c98f02207a986d955c6e0cb35d446a89d3f56100f4d7f67801c31967743a9c8e10615bed')
>>> z = 0x27e0c5994dec7824e56dec6b2fcb342eb7cdb0d0957c2fce9882f715e85d81a6
>>> point = S256Point.parse(sec)
>>> signature = Signature.parse(der)
>>> print(point.verify(z, signature))
True
```

SEC 公钥和 DER 签名在执行像 OP_CHECKSIG 这样的命令时位于栈中，使得获取公钥和签名变得相当直接（参见第 6 章）。困难的部分是获取签名哈希。一种天真的方法是按图 7-1 所示对交易序列化进行哈希。不幸的是，我们不能这样做，因为签名是 ScriptSig 的一部分，而签名不能签署自身。

![](img/6c915536bcfd43e412f7adef942edf23_157_0.png)

图 7-1. 签名位于黄色高亮部分，即 ScriptSig

相反，我们在签署交易前修改交易。也就是说，我们为每个输入计算不同的签名哈希。程序如下。

### 步骤 1：清空所有 ScriptSigs

第一步是在检查签名时清空所有 ScriptSigs（图 7-2）。创建签名时使用相同的程序，只是 ScriptSigs 通常已经为空。

![](img/6c915536bcfd43e412f7adef942edf23_157_1.png)

图 7-2. 清空每个输入的 ScriptSig（黄色高亮字段，现为 00）

请注意，此示例只有一个输入，因此只清空该输入的 ScriptSig，但可能有多个输入。在这种情况下，每个输入都将被清空。





## 第 2 步：用之前的 ScriptPubKey 替换正在签名的输入的 ScriptSig

每个输入都指向一个之前的交易输出，该输出具有一个 ScriptPubKey。回想一下第 6 章中的图表，如图 7-3 所示。

![](img/6c915536bcfd43e412f7adef942edf23_158_0.png)

*图 7-3. 合并 ScriptPubKey 和 ScriptSig*

我们取出输入所指向的 ScriptPubKey 并将其放在空的 ScriptSig 位置（图 7-4）。这可能需要在区块链上进行查找，但在实践中签名者已经知道 ScriptPubKey，因为该输入是签名者拥有私钥的输入。

```
01000000 1813f79011acb80925dfe69b3def355fe914bd1d96a3f5f71bf8303c6a989c7d100000000
1976a914a802fc56c704ce87c42d7c92eb75e7896bdc41ae88acfeffffff02a135ef010000000019
76a914bc3b654dca7e56b04dca18f2566cdaf02e8d9ada88ac99c398000000000001976a914c4bc762
dd5423e332166702cb75f40df79fea1288ac19430600
```

*图 7-4. 用之前的 ScriptPubKey 替换其中一个输入的 ScriptSig（黄色高亮字段）*

## 第 3 步：附加哈希类型

最后，我们在末尾添加一个 4 字节的哈希类型。这是为了指定签名正在授权的内容。签名可以授权此输入与所有其他输入和输出一起使用（SIGHASH_ALL）、与特定输出一起使用（SIGHASH_SINGLE），或与任何输出一起使用（SIGHASH_NONE）。后两种在理论上有一些用例，但在实践中，几乎所有交易都使用 SIGHASH_ALL 进行签名。还有一种很少使用的哈希类型称为 SIGHASH_ANYONECANPAY，可以与前面三种中的任何一种结合使用，我们这里不讨论它。对于 SIGHASH_ALL，最终交易必须具有签名时所用的确切输出，否则输入签名无效。

SIGHASH_ALL 对应的整数为 1，并且必须以小端格式编码为 4 字节，这使得修改后的交易看起来像图 7-5。

![](img/6c915536bcfd43e412f7adef942edf23_159_0.png)

图 7-5. 附加哈希类型（SIGHASH_ALL），即棕色的 01000000

该修改后交易的 hash256 被解释为大端整数以生成 z。 将修改后的交易转换为 z 的代码如下：

```
>>> from helper import hash256
>>> modified_tx = bytes.fromhex('010000001813f79011acb80925dfe69b3def355fe914\nbd1d96a3f5f71bf8303c6a989c7d1000000001976a914a802fc56c704ce87c42d7c92eb75e7896\nbdc41ae88acfeffffff02a135ef01000000001976a914bc3b654dca7e56b04dca18f2566cdaf02\ne8d9ada88ac99c398000000000001976a914c4bc762dd5423e332166702cb75f40df79fea1288a\nc1943060001000000')
>>> h256 = hash256(modified_tx)
>>> z = int.from_bytes(h256, 'big')
>>> print(hex(z))
0x27e0c5994dec7824e56dec6b2fcb342eb7cdb0d0957c2fce9882f715e85d81a6
```

现在我们有了 z，我们可以取 ScriptSig 中的 SEC 格式的公钥和 DER 格式的签名来验证签名：

```
>>> from ecc import S256Point, Signature
>>> sec = bytes.fromhex('0349fc4e631e3624a545de3f89f5d8684c7b8138bd94bdd531d2e213bf016b278a')
>>> der = bytes.fromhex('3045022100ed81ff192e75a3fd2304004dcadb746fa5e24c5031c\ncfcf21320b0277457c98f02207a986d955c6e0cb35d446a89d3f56100f4d7f67801c31967743a9\nc8e10615bed')
>>> z = 0x27e0c5994dec7824e56dec6b2fcb342eb7cdb0d0957c2fce9882f715e85d81a6
>>> point = S256Point.parse(sec)
>>> signature = Signature.parse(der)
>>> point.verify(z, signature)
True
```

我们可以将此交易验证过程编码为 Tx 的方法。幸运的是，Script 引擎已经可以处理签名验证（参见第 6 章），因此我们这里的任务是将所有内容组合起来。我们需要 z，即签名哈希，传递给 evaluate 方法，并且需要组合 ScriptSig 和 ScriptPubKey。

- ❷ 我们确保每个输入都有正确的 ScriptSig。

请注意，完整的节点会验证更多内容，例如检查双花和本章未讨论的其他共识规则（最大 sigops、ScriptSig 大小等），但这对于我们的库来说已经足够了。

## 创建交易

验证交易的代码将有助于创建交易。我们可以创建符合验证过程的交易。我们创建的交易将要求输入的总和大于或等于输出的总和。同样，我们创建的交易将要求一个 ScriptSig，当与 ScriptPubKey 结合时是有效的。

要创建交易，我们至少需要一个我们收到的输出。也就是说，我们需要 UTXO 集中 ScriptPubKey 可以解锁的输出。大多数情况下，我们需要一个或多个与 ScriptPubKey 中哈希的公钥对应的私钥。

本章的其余部分将关注创建输入由 p2pkh ScriptPubKeys 锁定的交易。

## 构造交易

构造交易需要回答一些基本问题：

- 1. 我们希望比特币去哪里？
- 2. 我们可以花费哪些 UTXO？
- 3. 我们希望这笔交易多快进入区块链？

我们将使用 testnet 作为此示例，尽管这可以很容易地应用于 mainnet。

第一个问题是关于我们想向谁支付多少。我们可以向一个或多个地址付款。在此示例中，我们将向 mnrVtF8DWjMu839VW3rBfgYaAfKk8983Xf 支付 0.1 个测试网比特币（tBTC）。

第二个问题是关于我们钱包里有什么。我们有什么可花费的？在此示例中，我们有一个由交易 ID 和输出索引表示的输出：

```
0d6fe5213c0b3291f208cba8bfb59b7476dffacc4e5cb66f6eb20a080843a299:13
```

当我们在测试网区块浏览器上查看此输出（图 7-6）时，我们可以看到我们的输出价值为 0.44 tBTC。

![](img/6c915536bcfd43e412f7adef942edf23_162_0.png)

图 7-6. 我们正在花费的 UTXO

由于这比 0.1 tBTC 多，我们希望将剩余部分发送回自己。尽管重复使用地址通常是不好的隐私和安全做法，但我们仍将比特币发送回 mzx5YhAH9kNHtcN481u6WkjeHjYtVeKVh2 以使交易构造更简单。

![](img/6c915536bcfd43e412f7adef942edf23_163_0.png)

## 为什么重复使用地址是个坏主意

在第 6 章中，我们讨论了 p2pk 相对于 p2pkh 的劣势，部分原因是它仅受 ECDSA 保护。另一方面，p2pkh 还受 sha256 和 ripemd160 保护。然而，由于区块链是公开的，一旦我们从对应于我们地址的 ScriptPubKey 花费，我们就会在 ScriptSig 中公开我们的公钥。一旦我们公开了该公钥，sha256 和 ripemd160 就不再保护我们，因为攻击者知道公钥而无需猜测它。

截至本文撰写时，我们仍然受离散对数问题的保护，这在不久的将来不太可能被攻破。从安全角度来看，了解我们受什么保护是很重要的。

不重复使用地址的另一个原因是隐私。拥有一个用于所有交易的地址意味着人们可以将我们的交易联系起来。例如，如果我们购买了一些私密的东西（比如我们不想让别人知道的某种疾病的药物）并用同一个 ScriptPubKey 的另一个输出向某个慈善机构捐款，那么该慈善机构和药物供应商就可以识别出我们与对方做过生意。

隐私泄露往往会随着时间的推移变成安全漏洞。

第三个问题实际上是关于费用。如果我们希望交易更快进入区块链，我们就必须支付更多费用；如果我们不介意等待，我们可以支付更少。在我们的例子中，我们将使用 0.01 tBTC 作为费用。

③ 第一个字节是网络前缀，最后 4 个是校验和。中间的 20 个是实际的 20 字节哈希（又称 hash160）。

我们还需要一种方法将 20 字节哈希转换为 ScriptPubKey。我们称此函数为 p2pkh_script，因为我们将 hash160 转换为 p2pkh：

```
def p2pkh_script(h160):
    '''Takes a hash160 and returns the p2pkh ScriptPubKey'''
    return Script([0x76, 0xa9, h160, 0x88, 0xac])
```

请注意，0x76 是 OP_DUP，0xa9 是 OP_HASH160，h160 是一个 20 字节元素，0x88 是 OP_EQUALVERIFY，0xac 是 OP_CHECKSIG。这是第 6 章中的 p2pkh ScriptPubKey 命令集。

有了这些工具，我们就可以进行交易创建：

```
>>> from helper import decode_base58, SIGHASH_ALL
>>> from script import p2pkh_script, Script
>>> from tx import TxIn, TxOut, Tx
>>> prev_tx = bytes.fromhex('0d6fe5213c0b3291f208cba8bfb59b7476dffacc4e5cb66f6\neb20a080843a299')
>>> prev_index = 13
>>> tx_in = TxIn(prev_tx, prev_index)
>>> tx_outs = []
>>> change_amount = int(0.33*100000000) ①
>>> change_h160 = decode_base58('mzx5YhAH9kNHtcN481u6WkJeHJYtVeKVh2')
>>> change_script = p2pkh_script(change_h160)
>>> change_output = TxOut(amount=change_amount, script_pubkey=change_script)
>>> target_amount = int(0.1*100000000) ①
>>> target_h160 = decode_base58('mnrVtF8DNjMu839VW3r8fgYaAfKk8983Xf')
>>> target_script = p2pkh_script(target_h160)
>>> target_output = TxOut(amount=target_amount, script_pubkey=target_script)
>>> tx_obj = Tx(1, [tx_in], [change_output, target_output], 0, True) ②
>>> print(tx_obj)
tx: cd30a8da777d28ef0e61efe68a9f7c559c1d3e5bcd7b265c850ccb4068598d11
version: 1
tx_ins:
0d6fe5213c0b3291f208cba8bfb59b7476dffacc4e5cb66f6eb20a080843a299:13
tx_outs:
33000000:OP_DUP OP_HASH160 d52ad7ca9b3d096a38e752c2018e6fbc40cdf26f OP_EQUALVERIFY OP_CHECKSIG
10000000:OP_DUP OP_HASH160 507b27411ccf7f16f10297de6cef3f291623eddf OP_EQUALVERIFY OP_CHECKSIG
locktime: 0
```

① 金额必须以聪为单位；鉴于每 BTC 有 100,000,000 聪，我们必须乘以并转换为整数。

② 我们必须指定要使用 testnet=True 参数查找哪个网络。





## 签署交易

我们已经创建了实际的交易，但该交易中的每个 ScriptSig 目前都是空的。填充它是我们下一步要做的事情。

签署交易可能有点棘手，但我们知道如何从本章前面获取签名哈希 z。如果我们拥有其公钥 hash160 为 ScriptPubKey 中 20 字节哈希的私钥，我们就可以对 z 进行签名并生成 DER 签名：

```
>>> from ecc import PrivateKey
>>> from helper import SIGHASH_ALL
>>> z = transaction.sig_hash(0)
>>> private_key = PrivateKey(secret=8675309)
>>> der = private_key.sign(z).der()
>>> sig = der + SIGHASH_ALL.to_bytes(1, 'big')
>>> sec = private_key.point.sec()
>>> script_sig = Script([sig, sec])
>>> transaction.tx_ins[0].script_sig = script_sig
>>> print(transaction.serialize().hex())
0100000001813f79011acb80925df6e69b3def355fe914bd1d96a3f5f71bf8303c6a989c7d10000000006a47304402207db2402a331a3b845b038885e3dd889c08126a8570f26a844e3e4049c482a11022010178cdca4129eacbeab7c44648bf5ac1f9cac217cd609d216ec2ebc8d242c0a012103935581e52c354cd2f484fe8ed83af7a3097005b2f9c60bff71d35bd795f54b67fefffff02a135ef01000000001976a914c4bc3b654dca7e56b04dca18f2566cdaf02e8d9ada88ac99c3980000000001976a914c4bc762dd5423e332166702cb75f40df79fea1288ac19430600
```

① 我们只需要签署第一个输入——只有一个。多个输入需要我们用正确的私钥签署每个输入。

② 签名实际上是 DER 签名和哈希类型的组合，在我们的例子中是 SIGHASH_ALL。

③ p2pkh 的 ScriptSig 正好有两个元素，正如我们在第 6 章中所看到的那样：签名和 SEC 格式的公钥。

④ 我们只有一个需要签署的输入，但如果有更多，则需要对每个输入执行此 ScriptSig 的创建过程。

### 练习 3

为 Tx 类编写 sign_input 方法。

## 在 testnet 上创建自己的交易

要创建自己的交易，先为自己获取一些币。为此你需要一个地址。如果你完成了第 4 章的最后一个练习，你应该已经拥有自己的 testnet 地址和私钥。如果你不记得了，这里是如何操作：

```
>>> from ecc import PrivateKey
>>> from helper import hash256, little_endian_to_int
>>> secret = little_endian_to_int(hash256(b'Jimmy Song secret'))
>>> private_key = PrivateKey(secret)
>>> print(private_key.point.address(testnet=True))
mn81594PzKZa9K3Jyy1ushpuEzrnTnxhVg
```

① 请使用除 Jimmy Song secret 之外的短语。

一旦你有了地址，你就可以从提供免费 testnet 币用于测试的 testnet 水龙头之一获取一些币。你可以 Google “testnet bitcoin faucet” 来找到一个，或者使用 wiki 上的列表中的一个。我的网站 https://faucet.programmingbitcoin.com 也已更新，指向一个可用的 testnet 水龙头。将你的新 testnet 地址输入到这些水龙头中的任何一个即可获取一些 testnet 币。

收到一些币后，使用此库花费它们。对于一个初出茅庐的比特币开发者来说，这是一个巨大的成就，所以请花一些时间完成这些练习。

### 练习 4

创建一个 testnet 交易，将单个 UTXO 的 60% 发送到 mwJn1YPMq7y5FBJ3LkC5Hxg9PHyZ5K4cFv。剩余金额减去手续费应退回你自己的找零地址。这应该是一个一输入、两输出的交易。

你可以在 https://live.blockcypher.com/btc/pushtx 广播交易。

### 练习 5

高级：从 testnet 水龙头获取更多 testnet 币并创建一个两输入、一输出的交易。一个输入应来自水龙头，另一个应来自之前的练习；输出可以是你自己的地址。

你可以在 https://live.blockcypher.com/btc/pushtx/ 广播交易。

## 结论

我们已成功验证区块链上的现有交易，并且你也在 testnet 上创建了自己的交易！这是一个重大的成就，你应该为此感到自豪。

我们目前的代码可以处理 p2pkh 和 p2pk。在下一章，我们将转向更高级的智能合约，p2sh。
