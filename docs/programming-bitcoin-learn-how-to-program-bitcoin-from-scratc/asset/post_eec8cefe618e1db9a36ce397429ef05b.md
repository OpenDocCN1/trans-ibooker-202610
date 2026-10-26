
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

Write the fee method for the Tx class.

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

Write the op_hash160 function.

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

Write the op_checksig function in op.py.

```python
def op_checksig(stack, z):
    if len(stack) < 2:
        return False
    sec_pubkey = stack.pop()
    der_signature = stack.pop()[:-1]
```

### Exercise 4

Create a testnet transaction that sends 60% of a single UTXO to mwJn1YPMq7y5F8J3LkC5Hxg9PHyZSK4cFv. The remaining amount minus fees should go back to your own change address. This should be a one-input, two-output transaction.

You can broadcast the transaction at https://live.blockcypher.com/btc/pushtx.

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

Advanced: Get some more testnet coins from a testnet faucet and create a two-input, one-output transaction. One input should be from the faucet, the other should be from the previous exercise; the output can be your own address.

You can broadcast the transaction at https://live.blockcypher.com/btc/pushtx/.

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

Determine what this network message is:

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

Write the serialize method for NetworkEnvelope.

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

Write the serialize method for VersionMessage.

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

Write the handshake method for `SimpleNode`.

```python
class SimpleNode:
    ...
    def handshake(self):
        version = VersionMessage()
        self.send(version)
        self.wait_for(VerAckMessage)
```

### Exercise 6

Write the `serialize` method for `GetHeadersMessage`.

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

Write the `merkle_parent` function.

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

Write the parse method for MerkleBlock.

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

Write the is_valid method for MerkleBlock.

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
