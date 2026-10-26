# Coding p2wpkh and p2sh-p2wpkh

The first change we're going to make is to the Tx class, where we need to mark whether the transaction is Segwit or not:

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

Next, we change the parse method depending on the serialization we receive:

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

① To determine whether we have a Segwit transaction or not, we look at the fifth byte. The first four are the version, the fifth is the Segwit marker.