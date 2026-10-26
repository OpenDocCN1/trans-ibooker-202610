# Elements Longer Than 75 Bytes

![](bbox=[0.17, 0.09, 0.25, 0.16])

You might be wondering what would happen if you had an element with a length greater than 0x4b (75 in decimal). There are three specific opcodes for handling such elements: OP_PUSHDATA1, OP_PUSHDATA2, and OP_PUSHDATA4. OP_PUSHDATA1 means that the next byte contains how many bytes we need to read for the element. OP_PUSHDATA2 means that the next 2 bytes contain how many bytes we need to read for the element. OP_PUSHDATA4 means that the next 4 bytes contain how many bytes we need to read for the element.

Practically speaking, this means if we have an element that’s between 76 and 255 bytes inclusive, we use OP_PUSHDATA1 <1-byte length of the element> <element>. For anything between 256 bytes and 520 bytes inclusive, we use OP_PUSHDATA2 <2-byte length of the element in little-endian> <element>. Anything larger than 520 bytes is not allowed on the network, so OP_PUSHDATA4 is unnecessary, though OP_PUSHDATA4 <4-byte length of the element in little-endian, but value less than or equal to 520> <element> is still legal.

It is possible to encode a number below 76 using OP_PUSHDATA1 or a number below 256 using OP_PUSHDATA2 or even any number below 521 using OP_PUSHDATA4. However, these are considered nonstandard transactions, meaning most Bitcoin nodes (particularly those running Bitcoin Core software) will not relay them.

There are many more opcodes, which are coded in op.py, and the full list can be found at https://en.bitcoin.it/wiki/Script.

# Coding a Script Parser and Serializer

Now that we know how Script works, we can write a script parser:

```
class Script:

    def __init__(self, cmds=None):
        if cmds is None:
            self.cmds = []
        else:
            self.cmds = cmds ①

    ...
    @classmethod
    def parse(cls, s):
        length = read_varint(s) ②
        cmds = []
        count = 0
        while count < length: ③
```