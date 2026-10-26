

![](img/6c915536bcfd43e412f7adef942edf23_250_0.png)

图 13-6. p2wpkh 步骤 2

此时，旧节点将停止，因为没有更多 Script 命令需要处理。由于栈顶元素非零，这将被计为一个有效的脚本。这与 p2sh（第 8 章）非常相似，因为旧节点无法进行进一步验证。然而，新节点有一个特殊的 Segwit 规则，就像 p2sh 的特殊规则一样（参见第 8 章）。请回忆，与 p2sh 情况一样，精确的脚本序列 <RedeemScript> OP_HASH160 <hash> OP_EQUAL 会触发一个特殊规则。

对于 p2wpkh 来说，脚本序列是 OP_0 <20 字节哈希>。当遇到该脚本序列时，来自见证字段的公钥和签名以及 20 字节哈希会以与 p2pkh 完全相同的顺序添加到命令集中，即 <signature> <pubkey> OP_DUP OP_HASH160 <20-byte hash> OP_EQUALVERIFY OP_CHECKSIG。图 13-7 显示了接下来遇到的状态。

![](img/6c915536bcfd43e412f7adef942edf23_250_1.png)

图 13-7. p2wpkh 步骤 3

p2wpkh 的其余处理与第 6 章中 p2pkh 的处理相同。结束状态是栈上只有一个 1，当且仅当 20 字节哈希是公钥的 hash160 且签名有效时（图 13-8）。

![](img/6c915536bcfd43e412f7adef942edf23_250_2.png)

图 13-8. p2wpkh 步骤 4

对于旧节点，处理在 <20-byte hash> 0 处停止，因为旧节点不知道特殊的 Segwit 规则。只有升级后的节点才会进行其余的验证，就像 p2sh 一样。注意，发送给旧节点的数据更少。此外，如果节点不愿意，节点可以选择不下载（因此不验证）x 个区块旧的交易。在某种意义上，签名已被一群人见证，节点可以选择信任其有效性而不是直接验证。

还要注意，这是一个针对 Segwit 版本 0 的特殊规则。Segwit 版本 1 可以有完全不同的处理路径。<20-byte hash> 1 可能是触发不同规则的特殊脚本序列。Segwit 的升级可以引入 Schnorr 签名、Graftroot，甚至完全不同的脚本系统，如 Simplicity。Segwit 为我们提供了一个清晰的升级路径。能够验证 Segwit 版本 X 的软件将验证此类交易，但不了解 Segwit 版本 X 的软件仅处理到特殊规则为止。

## p2sh-p2wpkh

p2wpkh 很棒，但不幸的是，这是一种新型脚本，旧钱包无法向 p2wpkh ScriptPubKeys 发送比特币。p2wpkh 使用一种新的地址格式 Bech32，由 BIP0173 定义，其 ScriptPubKeys 旧钱包不知道如何创建。

Segwit 作者通过将 p2wpkh “包装”在 p2sh 内部，使 Segwit 向后兼容，这是一种巧妙的方法。这称为“嵌套”Segwit，因为 Segwit 脚本嵌套在 p2sh RedeemScript 中。

p2sh-p2wpkh 地址是一个普通的 p2sh 地址，但 RedeemScript 是 OP_0 <20-byte hash>，即 p2wpkh 的 ScriptPubKey。与 p2wpkh 一样，发送给旧节点（图 13-9）和新节点（图 13-10）的交易不同。

```
010000001712e5b4e97ab549d50ca60a4f5968b2225215e9fab82dae4720078711406972f000000
017160014848202fc47fb475289652fbd1912cc853ecb0096feffffff023236000000000001976a9
14121ae7a2d55d2f0102ccc117cbcb70041b0e037f88ac10270000000000001976a914ec0be509516
51261765cfa71d7bd41c7b9245bb388ac075a0700

- 01000000 - version
- 01 - # of inputs
- 712e...2f - previous tx hash
- 00000000 - previous tx index
- 1716...96 - ScriptSig
- feffffff - sequence
- 02 - # of outputs
- 3236...00 - output amounts
- 1976...ac - ScriptPubKey
- 075a0700 - locktime
```

图 13-9. Pay-to-script-hash-pay-to-witness-pubkey-hash（p2sh-p2wpkh）发送给 pre-BIP0141 软件

```
01000000000101712e5b4e97ab549d50ca60a4f5968b2225215e9fab82dae4720078711406972f000
0000017160014848202fc47fb475289652fbd1912cc853ecb0096feffffff0232360000000000019
76a914121ae7a2d55d2f0102ccc117cbcb70041b0e037f88ac10270000000000001976a914ec0be50
951651261765cfa71d7bd41c7b9245bb388ac024830450221009263c7de80c297d5b21aba846cf6f0
a970e1d339568167d1e4c1355c7711bc1602202c9312b8d32fd9c7acc54c46cab50eb7255ce3c0122
14c41fe1ad91bcb16a13012102ebdf6fc448431a2bd6380f912a0fa6ca291ca3340e79b6f0c1fdaf
f73cf54061075a0700

- 01000000 - version
- 00 - Segwit marker
- 01 - Segwit flag
- 01 - # of inputs
- 712e...2f - previous tx hash
- 00000000 - previous tx index
- 1716...96 - ScriptSig
- feffffff - sequence
- 02 - # of outputs
- 3236...00 - output amounts
- 1976...ac - ScriptPubKey
- 0248...61 - witness
- 075a0700 - locktime
```

图 13-10. 发送给 post-BIP0141 软件的 p2sh-p2wpkh

与 p2wpkh 的不同之处在于 ScriptSig 不再为空。ScriptSig 包含一个 RedeemScript，其等于 p2wpkh 中的 ScriptPubkey。由于这是一个 p2sh 脚本，ScriptPubKey 与任何其他 p2sh 脚本中的相同。组合后的脚本看起来像图 13-11。

![](img/6c915536bcfd43e412f7adef942edf23_253_0.png)

图 13-11. p2sh-p2wpkh ScriptPubKey 与普通 p2sh ScriptPubKey 相同

我们像图 13-12 一样开始脚本评估。

![](img/6c915536bcfd43e412f7adef942edf23_253_1.png)

图 13-12. p2sh-p2wpkh 开始

注意，要处理的命令正是触发 p2sh 特殊规则的命令。RedeemScript 进入栈（图 13-13）。

![](img/6c915536bcfd43e412f7adef942edf23_253_2.png)

图 13-13. p2sh-p2wpkh 步骤 1

OP_HASH160 将返回 RedeemScript 的哈希（图 13-14）。

![](img/6c915536bcfd43e412f7adef942edf23_253_3.png)

图 13-14. p2sh-p2wpkh 步骤 2

哈希进入栈，然后我们到达 OP_EQUAL（图 13-15）。

![](img/6c915536bcfd43e412f7adef942edf23_255_0.png)

图 13-18. p2sh-p2wpkh 步骤 6

其余的处理与 p2pkh 相同（第 6 章）。假设签名和公钥有效，我们剩下图 13-19。

![](img/6c915536bcfd43e412f7adef942edf23_255_1.png)

图 13-19. p2sh-p2wpkh 结束

正如你所见，p2sh-p2wpkh 交易完全向后兼容到 BIP0016 之前。pre-BIP0016 节点会在 RedeemScripts 相等时认为脚本有效，而 post-BIP0016、pre-Segwit 节点会在 20 字节哈希处认为脚本有效。两者都不会进行完整验证并接受交易。post-Segwit 节点会进行完整验证，包括检查签名和公钥。

![](img/6c915536bcfd43e412f7adef942edf23_255_2.png)

## 任何人都可以花费 Segwit 输出吗？

Segwit 的反对者称 Segwit 输出为“任何人可花费”。如果比特币社区拒绝了 Segwit，这将是真的。换句话说，如果比特币社区中经济上重要的一部分拒绝进行 Segwit 验证并通过接受非 Segwit 有效的交易而积极分裂网络，输出将是任何人可花费的。然而，由于各种经济激励，Segwit 已在网络上激活，没有发生网络分裂，现在大量比特币被锁定在 Segwit 输出中，并且 Segwit 交易已按照软分叉规则由绝大多数经济节点验证。我们现在可以自信地说，反对者是错的。

## 编码 p2wpkh 和 p2sh-p2wpkh

我们要做的第一个更改是 Tx 类，我们需要标记交易是否为 Segwit：

```
class Tx:
    command = b'tx'

    def __init__(self, version, tx_ins, tx_outs,
        locktime, testnet=False, segwit=False):
        self.version = version
        self.tx_ins = tx_ins
        self.tx_outs = tx_outs
        self.locktime = locktime
        self.testnet = testnet
        self.segwit = segwit
        self._hash_prevouts = None
        self._hash_sequence = None
        self._hash_outputs = None
```

接下来，我们根据收到的序列化更改 parse 方法：

```
class Tx:
    ...
    @classmethod
    def parse(cls, s, testnet=False):
        s.read(4) ①
        if s.read(1) == b'\x00': ②
            parse_method = cls.parse_segwit
        else:
            parse_method = cls.parse_legacy
        s.seek(-5, 1) ③
        return parse_method(s, testnet=testnet)

    @classmethod
    def parse_legacy(cls, s, testnet=False):
        version = little_endian_to_int(s.read(4)) ④
        num_inputs = read_varint(s)
        inputs = []
        for _ in range(num_inputs):
            inputs.append(TxIn.parse(s))
        num_outputs = read_varint(s)
        outputs = []
        for _ in range(num_outputs):
            outputs.append(TxOut.parse(s))
        locktime = little_endian_to_int(s.read(4))
        return cls(version, inputs, outputs, locktime,
            testnet=testnet, segwit=False)
```

① 为确定我们是否有 Segwit 交易，我们查看第五个字节。前四个是版本，第五个是 Segwit 标记。

