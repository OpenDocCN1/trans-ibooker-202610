

![](img/6c915536bcfd43e412f7adef942edf23_268_0.png)

*图 13-41. p2sh-p2wsh 步骤 6*

此时，预 Segwit 节点会将此输入标记为有效，因为它们不知道 Segwit 验证规则。然而， post-Segwit 节点会识别出 p2wsh 的特殊脚本序列。见证字段（图 13-42）包含 WitnessScript（图 13-43）。WitnessScript 的 sha256 会与 32 字节哈希进行校验，如果相等，则 WitnessScript 会被解释为脚本命令并放入命令集（图 13-44）。

```
04004730440220010d2854b86b90b7c33661ca25f9d9f15c24b88c5c4992630f77ff004
b998fb802204106fc3ec8481fa98e07b7e78809ac91b6ccaf60bf4d3f729c5a75899bb6
64a501473044022046d66321c6766abcb1366a793f9bfd0e11e0b080354f18188588961
ea76c5ad00922762381a0661d66f5c39825202524c45f29d500c6476176cd910b16911
76858701695221026ccfb8061f235cc110697c0bfb3afb99d82c886672f6b9b5393b25a434c0cbf32103befa190
c0c22e2f53720b1be9476dcf11917da4665c44c9c71c3a2d28a
933c352102be46dc245f58085743b1cc37c82f0d63a960efa43b5336534275fc469b49f
4ac53ae
```

*图 13-42. p2sh-p2wsh 见证*

- 04 - 见证元素数量
- 00 - OP_0
- 47 - <signaturex> 的长度
- 3044...01 - <signaturex>
- 69 - WitnessScript 的长度
- 5221...ae - <WitnessScript>

```
5221026ccfb8061f235cc110697c0bfb3afb99d82c886672f6b9b5393b25a434c0cbf32103befa190
c0c22e2f53720b1be9476dcf11917da4665c44c9c71c3a2d28a933c352102be46dcf245f58085743b1
cc37c82f0d63a960efa43b5336534275fc469b49f4ac53ae
```

*图 13-43. p2sh-p2wsh WitnessScript*

- 52 - OP_2
- 21 - <pubkeyx> 的长度
- 0...01 - <pubkeyx>
- 53 - OP_3
- ae - OP_CHECKMULTISIG

```
command = tx_in.witness[-1]
raw_witness = encode_varint(len(command)) + command
witness_script = Script.parse(BytesIO(raw_witness))
z = self.sig_hash_bip143(input_index,
                        witness_script=witness_script)
witness = tx_in.witness
else:
    z = self.sig_hash(input_index, redeem_script)
    witness = None
else:
    if script_pubkey.is_p2wpkh_script_pubkey():
        z = self.sig_hash_bip143(input_index)
        witness = tx_in.witness
    elif script_pubkey.is_p2wsh_script_pubkey():
        command = tx_in.witness[-1]
        raw_witness = encode_varint(len(command)) + command
        witness_script = Script.parse(BytesIO(raw_witness))
        z = self.sig_hash_bip143(input_index,
                                witness_script=witness_script)
        witness = tx_in.witness
    else:
        z = self.sig_hash(input_index)
        witness = None
combined_script = tx_in.script_sig + tx_in.script_pubkey(self.testnet)
return combined_script.evaluate(z, witness)
```

- ① 这处理了 p2sh-p2wsh。
- ② 这处理了 p2wsh。

接下来，我们在 script.py 中编写一种识别 p2wsh 的方法：

```
def p2wsh_script(h256):
    '''Takes a hash160 and returns the p2wsh ScriptPubKey'''
    return Script([0x00, h256])
...
class Script:
...
    def is_p2wsh_script_pubkey(self):
        return len(self.cmds) == 2 and self.cmds[0] == 0x00 \
            and type(self.cmds[1]) == bytes and len(self.cmds[1]) == 32
```

- ① OP_0 <32 字节脚本> 是我们期望的。

最后，我们处理 p2wsh 的特殊规则：

```
class Script:
...
    def evaluate(self, z, witness):
...
        while len(commands) > 0:
...
else:
    stack.append(command)
    ...
    if len(stack) == 2 and stack[0] == b'' and len(stack[1]) == 32:
        s256 = stack.pop() ①
        stack.pop() ②
        cmds.extend(witness[:-1]) ③
        witness_script = witness[-1] ④
        if s256 != sha256(witness_script): ⑤
            print('bad sha256 {} vs {}'.format
                  (s256.hex(), sha256(witness_script).hex()))
            return False
        stream = BytesIO(encode_varint(len(witness_script))
                         + witness_script)
        witness_script_cmds = Script.parse(stream).cmds ⑥
        cmds.extend(witness_script_cmds)
```

- ① 栈顶元素是 WitnessScript 的 sha256 哈希。
- ② 第二个元素是见证版本，0。
- ③ 除 WitnessScript 之外的所有内容都会被添加到命令集中。
- ④ WitnessScript 是见证字段的最后一项。
- ⑤ WitnessScript 的哈希必须等于栈中的 sha256。
- ⑥ 我们解析 WitnessScript 并将其添加到命令集中。

### 其他改进

Segwit 还通过签名哈希的不同计算方式修复了二次哈希问题。签名哈希 z 的许多计算可以被重用，而无需为每个输入都要求一个新的 hash256 哈希。签名哈希的计算详细说明在 BIP0143 中，可以在 code-ch13/tx.py 中看到。

另一个改进是，按策略，不再允许使用未压缩的 SEC 公钥；Segwit 仅使用压缩的 SEC 公钥，从而节省空间。

### 结论

本章介绍了 Segwit 的细节，作为现在可能实现内容的品尝。第 14 章讨论了你在比特币开发者之旅中可以采取的下一步。

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

