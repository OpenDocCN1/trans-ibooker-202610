

```
3121f2427005551aeb0ec6a402ac7d7f0e4235954d801187f5da9f5')))
>>> first_block = Block.parse(BytesIO(bytes.fromhex('00000201ecd89664fd205a37\566e694269ed76e4258030628ab0100000000000000000bfcade29d080d9aae8fd461254b0418\05ae442749f2a40100440fc0e3d5868e5501934595d4d0118a1721b2e')))
>>> time_differential = last_block.timestamp - first_block.timestamp
>>> if time_differential > TWO_WEEKS * 4:
...     time_differential = TWO_WEEKS * 4
>>> if time_differential < TWO_WEEKS // 4:
...     time_differential = TWO_WEEKS // 4
>>> new_target = last_block.target() * time_differential // TWO_WEEKS
>>> print('{ :x}'.format(new_target).zfill(64))
000000000000000007615000000000000000000000000000000000000000
```

1 注意 TWO_WEEKS = 60*60*24*14 是 2 周内秒数的数量：60 秒 × 60 分钟 × 24 小时 × 14 天。

2 这确保了如果找到最后 2,015 个区块花费超过 8 周，我们不会过度降低难度。

3 这确保了如果找到最后 2,015 个区块花费少于 3.5 天，我们不会过度提高难度。

注意我们只需要区块头来计算下一个区块的目标值。一旦我们有了目标，我们就可以将目标转换为位。逆向操作如下所示：

```
def target_to_bits(target):
    '''Turns a target integer back into bits'''
    raw_bytes = target.to_bytes(32, 'big')
    raw_bytes = raw_bytes.lstrip(b'\x00')  1
    if raw_bytes[0] > 0x7f:  2
        exponent = len(raw_bytes) + 1
        coefficient = b'\x00' + raw_bytes[:2]
    else:
        exponent = len(raw_bytes)  3
        coefficient = raw_bytes[:3]  4
    new_bits = coefficient[::-1] + bytes([exponent])  5
    return new_bits
```

1 去掉所有前导零。

2 位格式是一种简洁地表达非常大数字的方式，并且可用于负数和正数。如果系数中的第一位是 1，则位字段被解释为负数。由于对我们来说目标始终为正，如果第一位是 1，我们将整个内容向右移动 1 个字节。

3 指数是该数字在基数 256 中的长度。

4 系数是基数 256 数字的前三位数字。

```
f9beb4d97665727369f6e0000000000065000005f1a69d2721101000100000000000000bc8f5e540
0000000010000000000000000000000000000000000000ffffc61b6409208d0100000000000000000000
0000000000000fffffcb0071c0208d12803cbc97953f80f2f5361746f7368693a302e392e332fc0
5050001

- f9beb4d9 - network magic (always 0xf9beb4d9 for mainnet)
- 7665727369f6e00000000 - command, 12 bytes, human-readable
- 65000000 - payload length, 4 bytes, little-endian
- 5f1a69d2 - payload checksum, first 4 bytes of hash256 of the payload
- 7211...01 - payload
```

Figure 10-1. A network message—the envelope that contains the actual payload

接下来的 12 个字节是命令字段，或是对负载实际携带内容的描述。有许多不同的命令；详尽的列表可以在文档中看到。命令字段旨在可读性强，此特定消息是 ASCII 中的字节字符串“version”，后面填充 0 字节。

接下来的 4 个字节是以小端序表示的负载长度。正如我们在第 5 章和第 9 章中所见，负载长度是必要的，因为负载是可变的。2³² 约为 40 亿，因此负载可以大到 4 GB，尽管参考客户端拒绝任何超过 32 MB 的负载。在图 10-1 中的消息中，我们的负载为 101 字节。

接下来的 4 个字节是校验和字段。校验和算法有点奇怪的选择，因为它是负载的 hash256 的前 4 个字节。这是一个奇怪的选择，因为网络协议校验和通常设计为具有纠错能力，而 hash256 没有。话虽如此，hash256 在比特币协议的其他部分很常见，这可能是它在这里被使用的原因。

处理网络消息的代码要求我们创建一个新的类：

```
NETWORK_MAGIC = b'\xf9\xbe\xb4\xd9'
TESTNET_NETWORK_MAGIC = b'\x0b\x11\x09\x07'
```

```
class NetworkEnvelope:

    def __init__(self, command, payload, testnet=False):
        self.command = command
        self.payload = payload
        if testnet:
            self.magic = TESTNET_NETWORK_MAGIC
        else:
            self.magic = NETWORK_MAGIC

    def __repr__(self):
        return '{: }'.format(
            self.command.decode('ascii'),
```

## Exercise 4

Write the serialize method for VersionMessage.

## Network Handshake

网络握手是节点建立通信的方式：

+   - A 想连接到 B 并发送 version 消息。
+   - B 收到 version 消息，回应 verack 消息，并发送自己的 version 消息。
+   - A 收到 version 和 verack 消息并发送回 verack 消息。
+   - B 收到 verack 消息并继续通信。

一旦握手完成，A 和 B 就可以随意通信。注意这里没有身份验证，节点需要自行验证收到的所有数据。如果节点发送了错误的交易或区块，它可以预期会被禁止或断开连接。

## Connecting to the Network

由于网络的异步特性，网络通信很棘手。为了实验，我们可以同步地建立与网络上节点的连接：

```
>>> import socket
>>> from network import NetworkEnvelope, VersionMessage
>>> host = 'testnet.programmingbitcoin.com' ①
>>> port = 18333
>>> socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
>>> socket.connect((host, port))
>>> stream = socket.makefile('rb', None) ②
>>> version = VersionMessage() ③
>>> envelope = NetworkEnvelope(version.command, version.serialize())
>>> socket.sendall(envelope.serialize()) ④
>>> while True:
...     new_message = NetworkEnvelope.parse(stream) ⑤
...     print(new_message)
```

① 这是我为 testnet 设置的一个服务器。testnet 的默认端口是 18333。

② 我们创建一个流以能够从套接字读取。以这种方式创建的流可以传递给所有解析方法。

③ 握手的第一个步骤是发送 version 消息。

④ 我们现在以正确的信封发送消息。

⑤ 这行代码将读取通过我们连接的套接字传入的任何消息。

以这种方式连接，我们不能在接收之前发送，也不能一次智能地响应多个消息。更健壮的实现将使用异步库（如 Python 3 中的 asyncio）来在不被阻塞的情况下发送和接收。

我们还需要一个 verack 消息类，我们将在此处创建：

```
class VerAckMessage:
    command = b'verack'

    def __init__(self):
        pass

    @classmethod
    def parse(cls, s):
        return cls()

    def serialize(self):
        return b''
```

VerAckMessage 是一个最小的网络消息。

让我们现在通过创建一个将为我们处理通信的类来自动化这个过程：

```
class SimpleNode:

    def __init__(self, host, port=None, testnet=False, logging=False):
        if port is None:
            if testnet:
                port = 18333
            else:
                port = 8333
        self.testnet = testnet
        self.logging = logging
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.socket.connect((host, port))
        self.stream = self.socket.makefile('rb', None)

    def send(self, message): ①
        '''Send a message to the connected node'''
        envelope = NetworkEnvelope(
            message.command, message.serialize(), testnet=self.testnet)
        if self.logging:
            print('sending: {}'.format(envelope))
        self.socket.sendall(envelope.serialize())

    def read(self): ②
        '''Read a message from the socket'''
```

```
envelope = NetworkEnvelope.parse(self.stream, testnet=self.testnet)
if self.logging:
    print('receiving: {}'.format(envelope))
return envelope
```

```
def wait_for(self, *message_classes):  ③
'''Wait for one of the messages in the list'''
command = None
command_to_class = {m.command: m for m in message_classes}
while command not in command_to_class.keys():
    envelope = self.read()
    command = envelope.command
    if command == VersionMessage.command:
        self.send(VerAckMessage())
    elif command == PingMessage.command:
        self.send(PongMessage(envelope.payload))
return command_to_class[command].parse(envelope.stream())
```

① send 方法通过套接字发送消息。消息对象中预期存在 command 属性和 serialize 方法。

② read 方法从套接字读取新消息。

③ wait_for 方法让我们等待多个命令中的任意一个（具体来说是消息类）。与该类的同步特性一起，这种方法使编程更简单。商业级节点绝对不会使用类似的东西。

现在我们有了节点，我们可以与其他节点进行握手：

```
>>> from network import SimpleNode, VersionMessage
>>> node = SimpleNode('testnet.programmingbitcoin.com', testnet=True)
>>> version = VersionMessage()  ①
>>> node.send(version)  ②
>>> verack_received = False
>>> version_received = False
>>> while not verack_received and not version_received:  ③
...     message = node.wait_for(VersionMessage, VerAckMessage)  ④
...     if message.command == VerAckMessage.command:
...         verack_received = True
...     else:
...         version_received = True
...         node.send(VerAckMessage())
```

① 大多数节点不关心 version 中的字段如 IP 地址。我们可以使用默认值连接，一切都会很顺利。

② 我们通过发送 version 消息开始握手。

