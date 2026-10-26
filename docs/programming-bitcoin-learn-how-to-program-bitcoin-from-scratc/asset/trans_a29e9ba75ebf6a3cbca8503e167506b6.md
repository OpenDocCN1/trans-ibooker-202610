

2 第五个字节为 0 是我们判断该交易为 Segwit 的方式（这并不 foolproof，但我们将使用它）。我们根据是否为 Segwit 使用不同的解析器。

3 我们将流重置到检查前 5 个字节之前的位置。

4 我们已将旧的解析方法移至 parse_legacy。

以下是 Segwit 序列化的解析器：

```
class Tx:
    ...
        @classmethod
        def parse_segwit(cls, s, testnet=False):
            version = little_endian_to_int(s.read(4))
            marker = s.read(2)
            if marker != b'\x00\x01':
                raise RuntimeError('Not a segwit transaction {}'.format(marker))
            num_inputs = read_varint(s)
            inputs = []
            for _ in range(num_inputs):
                inputs.append(TxIn.parse(s))
            num_outputs = read_varint(s)
            outputs = []
            for _ in range(num_outputs):
                outputs.append(TxOut.parse(s))
            for tx_in in inputs:
                num_items = read_varint(s)
                items = []
                for _ in range(num_items):
                    item_len = read_varint(s)
                    if item_len == 0:
                        items.append(0)
                    else:
                        items.append(s.read(item_len))
                tx_in.witness = items
            locktime = little_endian_to_int(s.read(4))
            return cls(version, inputs, outputs, locktime,
                       testnet=testnet, segwit=True)
```

1 有两个新的字段；其中一个是 Segwit 标记。

2 第二个新字段是 witness，它包含每个输入的项目。

我们现在对序列化方法进行相应的更改：

```
class Tx:
    ...
        def serialize(self):
            if self.segwit:
                return self.serialize_segwit()
```

```
else:
    return self.serialize_legacy()

def serialize_legacy(self):
    result = int_to_little_endian(self.version, 4)
    result += encode_varint(len(self.tx_ins))
    for tx_in in self.tx_ins:
        result += tx_in.serialize()
    result += encode_varint(len(self.tx_outs))
    for tx_out in self.tx_outs:
        result += tx_out.serialize()
    result += int_to_little_endian(self.locktime, 4)
    return result

def serializewit(self):
    result = int_to_little_endian(self.version, 4)
    result += b'\x00\x01'
    result += encode_varint(len(self.tx_ins))
    for tx_in in self.tx_ins:
        result += tx_in.serialize()
    result += encode_varint(len(self.tx_outs))
    for tx_out in self.tx_outs:
        result += tx_out.serialize()
    for tx_in in self.tx_ins:
        result += int_to_little_endian(len(tx_in.witness), 1)
        for item in tx_in.witness:
            if type(item) == int:
                result += int_to_little_endian(item, 1)
            else:
                result += encode_varint(len(item)) + item
    result += int_to_little_endian(self.locktime, 4)
    return result
```

1 以前称为 serialize 的现在是 serialize_legacy。

2 Segwit 序列化添加了标记。

3 witness 在最后序列化。

我们还必须更改 hash 方法以使用旧序列化，即使对于 Segwit 交易也是如此，这样可以保持交易 ID 稳定：

```
class Tx:
    ...
    def hash(self):
        '''Binary hash of the legacy serialization'''
        return hash256(self.serialize_legacy())[::-1]
```

verify_input 方法对于 Segwit 交易需要不同的 z。Segwit 交易签名哈希计算在 BIP0143 中指定。此外，witness 字段会传递给脚本评估引擎：

```
class Tx:
    ...
    def verify_input(self, input_index):
        tx_in = self.tx_ins[input_index]
        script_pubkey = tx_in.script_pubkey(testnet=self.testnet)
        if script_pubkey.is_p2sh_script_pubkey():
            command = tx_in.script_sig.commands[-1]
            raw_redeem = int_to_little_endian(len(command), 1) + command
            redeem_script = Script.parse(BytesIO(raw_redeem))
            if redeem_script.is_p2wpkh_script_pubkey():
                z = self.sig_hash_bip143(input_index, redeem_script)
                witness = tx_in.witness
            else:
                z = self.sig_hash(input_index, redeem_script)
                witness = None
        else:
            if script_pubkey.is_p2wpkh_script_pubkey():
                z = self.sig_hash_bip143(input_index)
                witness = tx_in.witness
            else:
                z = self.sig_hash(input_index)
                witness = None
        combined_script = tx_in.script_sig + tx_in.script_pubkey(self.testnet)
        return combined_script.evaluate(z, witness)
```

+   1. 这处理了 p2sh-p2wpkh 情况。
+   2. BIP0143 签名哈希生成代码在本章代码的 tx.py 中。
+   3. 这处理了 p2wpkh 情况。
+   4. witness 传递给评估引擎，以便 p2wpkh 可以构造正确的命令。

我们还在 script.py 中定义了 p2wpkh 脚本的样子：

```
def p2wpkh_script(h160):
    '''Takes a hash160 and returns the p2wpkh ScriptPubKey'''
    return Script([0x00, h160])
...
def is_p2wpkh_script_pubkey(self):
    return len(self.cmds) == 2 and self.cmds[0] == 0x00 \
        and type(self.cmds[1]) == bytes and len(self.cmds[1]) == 20
```

+   1. 这是 OP_0 <20-byte-hash>。
+   2. 这检查当前脚本是否为 p2wpkh ScriptPubKey。

最后，我们需要在 evaluate 方法中实现特殊规则。

```
5221026ccfb8061f235cc110697c0bfb3afb99d82c886672f6b9b5393b25a434c0cbf32103befa190
c0c22e2f53720b1be9476dcf11917da4665c44c9c71c3a2d28a933c352102be46dc245f58085743b1
cc37c82f0d63a960efa43b5336534275fc469b49f4ac53ae
```

+   - 52 - OP_2
+   - 21 - Length of <pubkeyx>
+   - 0...01 - <pubkeyx>
+   - 53 - OP_3
+   - ae - OP_CHECKMULTISIG

Figure 13-27. p2wsh WitnessScript

其余的 witness 字段被放在顶部以产生图 13-28 中的命令集。

![](img/6c915536bcfd43e412f7adef942edf23_263_0.png)

Figure 13-28. p2wsh step 3

正如你所见，这是一个 2-of-3 多签，与第 8 章中探讨的类似（图 13-29）。

与 p2sh-p2wpkh 一样，ScriptPubKey 与任何其他 p2sh 地址无法区分，ScriptSig 仅为 RedeemScript（图 13-33）。

![](img/6c915536bcfd43e412f7adef942edf23_266_0.png)

Figure 13-33. p2sh-p2wsh ScriptPubKey

我们以与 p2sh-p2wpkh 脚本评估开始完全相同的方式开始 p2sh-p2wsh 脚本评估（图 13-34）。

![](img/6c915536bcfd43e412f7adef942edf23_266_1.png)

Figure 13-34. p2sh-p2wsh start

RedeemScript 被推送到栈上（图 13-35）。

![](img/6c915536bcfd43e412f7adef942edf23_266_2.png)

Figure 13-35. p2sh-p2wsh step 1

OP_HASH160 将返回 RedeemScript 的哈希（图 13-36）。

![](img/6c915536bcfd43e412f7adef942edf23_266_3.png)

Figure 13-36. p2sh-p2wsh step 2

哈希被推送到栈上，然后我们到达 OP_EQUAL（图 13-37）。

![](img/6c915536bcfd43e412f7adef942edf23_267_0.png)

Figure 13-37. p2sh-p2wsh step 3

与 p2sh-p2wpkh 一样，如果哈希相等，pre-BIP0016 节点会将输入标记为有效，因为它们不知道 p2sh 验证规则。然而，post-BIP0016 节点会识别 p2sh 的特殊脚本序列，因此 RedeemScript 将被解释为新的脚本命令。RedeemScript 是 OP_0 32 字节哈希，这与 p2wsh 的 ScriptPubKey 相同（图 13-38）。

```
220020615ae01ed1bc1ffaad54da31d7805d0bb55b52dfd3941114330368c1bbf69b4c
- 22 - Length of <RedeemScript>
- 0020...4c - <RedeemScript>
```

Figure 13-38. p2sh-p2wsh RedeemScript

这使得脚本状态看起来像图 13-39。

![](img/6c915536bcfd43e412f7adef942edf23_267_1.png)

Figure 13-39. p2sh-p2wsh step 4

当然，这与 p2wsh 的起始状态完全相同（图 13-40）。

![](img/6c915536bcfd43e412f7adef942edf23_267_2.png)

Figure 13-40. p2sh-p2wsh step 5

32 字节哈希是一个元素，因此它被推送到栈上（图 13-41）。

