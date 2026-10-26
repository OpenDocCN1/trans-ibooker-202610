

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

