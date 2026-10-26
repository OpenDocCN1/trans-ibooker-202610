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

① This takes care of p2sh-p2wsh.

② This takes care of p2wsh.

Next, we code a way to identify p2wsh in script.py:

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

① OP_0 <32-byte script> is what we expect.

Last, we handle the special rule for p2wsh:

```
class Script:
...
    def evaluate(self, z, witness):
...
        while len(commands) > 0:
...
```