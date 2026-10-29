

# 整合起来

到目前为止，我们已经学习了 socket API 的不同方法，但为了清楚地了解每个函数如何以及在何处使用，我将在此处对所有内容进行总结。我们将同时运行两个程序。一个将是监听传入连接的服务器，另一个将是尝试建立连接的客户端。让我们看一下下面的图表，了解 socket API 中各部分是如何配合的：

![](img/d31345bbfd15f63be2920f0de5fa30dc_135_0.png)

图 6.2 – Python 中客户端和服务器的 socket 使用情况

该图表显示了两个并发运行的独立程序，即客户端和服务器。你可能在想这个客户端和服务器与我们的黑客目的有什么关系？嗯，我们将采用类似的方法来开发我们的恶意软件。我们将编写两个程序。一个程序将在黑客机器上运行，我们称之为服务器/黑客程序，另一个将在客户端运行；我们将称之为受害者程序。受害者程序将尝试与黑客程序连接。这样，由于连接是由受害者机器发起的，反病毒软件或 IDS 将不会阻止它。在本节中，我们学习了 Python 中 socket 编程的工作原理。我们没有详细说明如何创建这些程序。在下一节中，我们将利用此 socket API 创建恶意软件的受害者部分和黑客部分。

# 创建恶意软件

现在我们已经看到了恶意软件程序的大致轮廓，让我们开始编写黑客和受害者程序。

# 黑客服务器

在本节中，我们将为黑客服务器编写一个程序，该程序将不断监听来自受害者机器发往黑客的传入连接。让我们进入 Kali 机器并创建一个名为 hacker server 的新项目。此外，像之前章节一样创建一个新的虚拟环境。本节中我们不需要任何外部库，但使用虚拟环境来跟踪程序中的依赖项始终是一个好主意。此外，创建一个名为 server.py 的新文件。

我们的 Kali 机器的 IP 地址是 192.168.74.128，受害者 Windows 机器的是 192.168.74.129。接下来，我们需要选择要监听传入连接的端口。你可以选择大于 1024 且小于 65355 的任意端口。然而，我们将使用端口号 8008。这将是服务器绑定的端口，如果客户端想要与服务器连接，则需要使用此端口。让我们导入 socket 模块并创建一个 socket 对象。看一下以下代码：

```
import socket

if __name__ == "__main__":
    hacker_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

这里，第一行我们只是从 Python 标准库导入 socket 模块。接下来，我们正在创建一个 socket 对象。两个参数是 socket.AF_INET 和 socket.SOCK_STREAM。让我们看看它们的含义。还记得我们谈论过 IPv4 和 IPv6 地址吗？这正是 socket.AF_INET 的含义。我们使用的是 IPv4，用 socket.AF_INET 表示。如果你想使用 IPv6（你可能不会），你可以选择 socket.AF_INET6。接下来，我们需要定义想要使用的网络层协议。这里，我们可以选择 TCP 或 UDP。在我们的示例中，我们想要一个可靠的连接，因此我们将选择 TCP。socket.SOCK_STREAM 表示我们正在创建一个 TCP socket。如果你想创建一个 UDP socket（同样，大多数情况下你可能不会），你可以使用 socket.SOCK_DGRAM。

接下来，我们将此服务器绑定到 Kali 的 IP 地址和端口 8008：

```
IP = "192.168.74.128"
Port = 8008
```

```
socket_address = (IP, Port)
hacker_socket.bind(socket_address)
```

请注意，你必须以元组形式提供 IP 地址和端口给 socket.bind() 方法。

接下来，我们需要使用以下命令在指定的 socket 上监听传入连接：

```
hacker_socket.listen(5)
```

现在我们的程序配置几乎完成，我们可以开始监听传入的连接请求：

```
hacker_socket.listen(5)
print("listening for incoming connection requests")
hacker_socket, client_address = hacker_socket.accept()
```

程序的执行将在此处暂停。一旦客户端连接，此方法将返回两个参数。第一个是 hacker_socket，我们可以用它来发送和接收数据，第二个是受害者的地址。这将帮助程序知道哪个客户端已连接。

一旦连接被接受，我们就可以使用此 socket 通过网络发送消息。如前所述，accept 函数是阻塞的，这意味着执行在此处暂停，直到有人连接。为了演示这一点，让我们运行程序。你将看到以下输出：

![](img/d31345bbfd15f63be2920f0de5fa30dc_137_0.png)

图 6.3 - 等待传入连接

你会看到程序不会在此步骤之后继续。你可以按 Ctrl + C 退出程序。现在，让我们尝试从黑客向受害者发送一条简单消息。目前，我们将发送一个简单字符串，但在后面的章节中，我们将发送更高级的数据，例如文件：

```
message = "Message from hacker"
message_bytes = message.encode()
hacker_socket.send(message_bytes)
print("Message sent from hacker")
```

message.encode() 方法将消息字符串转换为字节，因为 socket.send() 方法只接受字节。

最后，我们将通过调用 close() 方法关闭此 socket。

黑客程序的完整代码如下所示：

```
import socket

if __name__ == "__main__":
    hacker_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    IP = "192.168.74.128"
    Port = 8008
    socket_address = (IP, Port)
    hacker_socket.bind(socket_address)
    hacker_socket.listen(5)
    print("listening for incoming connection requests")
    hacker_socket, client_address = hacker_socket.accept()
    message = "Message from hacker"
    message_bytes = message.encode()
    hacker_socket.send(message_bytes)
    print("Message sent")
    hacker_socket.close()
```

我们的黑客程序现已完成。接下来，我们将转向受害者程序，该程序将与黑客建立连接。

# 受害者的客户端

进入 Windows 10 机器并为受害者创建一个新项目。前几步将与黑客程序类似。看一下以下代码：

```
import socket

if __name__ == "__main__":
    victim_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    hacker_IP = "192.168.74.128"
```

```
hacker_port = 8008
```

```
hacker_address = (hacker_IP, hacker_port)
```

由于我们想要连接到黑客，我们将提供黑客的 IP 和黑客正在监听的相应端口。

接下来，我们将为 hacker_address 创建一个元组。

下一步是使用受害者的 socket 与黑客 connect()：

```
victim_socket.connect(hacker_address)
```

一旦调用此方法，如果服务器正在监听，我们将建立成功的连接，否则我们会看到错误消息。如果你现在运行程序，你会看到连接被拒绝的消息：

![](img/d31345bbfd15f63be2920f0de5fa30dc_139_0.png)

图 6.4 – 连接失败

这是因为，如果没有服务器监听某个端口，默认情况下所有传入流量都会被阻止。还记得在我们的黑客程序中，我们正在发送消息吗？我们需要在此处处理该消息，否则会出现错误。我们可以使用 recv 方法来接收消息：

```
data = victim_socket.recv(1024)
```

1024 是 socket 一次可以读取的字节数。来自黑客的超过此数量的任何数据都将被截断。我们可以使用循环来接收更多数据。目前，这个数字足够了。

最后，由于我们以字节形式接收数据，我们需要将它们解码为字符串以便打印，并在以后在程序中使用它们（如果我们想要）：

```
print(data.decode())
```

```
victim_socket.close()
```

