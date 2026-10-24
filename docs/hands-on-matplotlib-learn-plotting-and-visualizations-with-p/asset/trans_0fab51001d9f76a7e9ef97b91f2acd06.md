

![](img/124f805c8155c7226d129ebbcc2842f0_29_0.png)

你甚至可以在操作系统的命令提示符下使用 Python 的解释器启动程序。打开操作系统的命令提示符并导航到程序存储的目录。在 Windows 命令提示符下，运行以下命令：

```text
python prog01.py
```

在 Linux 终端中，你必须运行以下命令：

```text
python3 prog01.py
```

然后解释器将在命令提示符下运行程序，输出（如果有）将显示在那里。

在 Linux 中，还有另一种无需显式使用解释器即可运行程序的方法。你可以在代码文件开头添加 shebang 行。例如，假设我们的代码文件如下所示：

```python
#!/usr/bin/python3
print('Hello World!')
```

第一行被称为 *shebang* 行。它告诉 shell 使用哪个解释器以及其位置。然后运行以下命令将文件权限更改为使其对所有者可执行，如下所示：

```text
chmod 755 prog01.py
```

然后你可以直接像其他任何可执行文件一样使用 `./` 启动你的 Python 程序文件，如下所示：

```text
./prog01.py
```

shell 将执行程序并在终端中打印输出。请注意，这仅适用于类 Unix 系统（Linux 和 macOS），因为它们支持以这种方式执行程序。你将在本书中继续学习更多关于 Python 编程的知识。

## 使用 Python IDE

你已经学习了如何使用 Python 解释器和 IDLE 来运行 Python 3 语句和程序。你可以使用其他免费可用的 IDE 和 IDE 插件来使用 Python。以下是一些著名的 Python 3 IDE 和插件的列表，以及它们主页的网址：

- PyCharm Community Edition ([https://www.jetbrains.com/pycharm/](https://www.jetbrains.com/pycharm/))
- Spyder IDE ([https://www.spyder-ide.org/](https://www.spyder-ide.org/))
- Thonny Python Editor ([https://thonny.org/](https://thonny.org/))
- Mu Editor ([https://codewith.mu/](https://codewith.mu/))
- PyDev plugin for Eclipse ([https://www.pydev.org/](https://www.pydev.org/))

所有这些 IDE 和插件都可免费下载和使用。作为本章的练习，你可能想探索它们以找到你最舒适的 IDE。

## 探索科学 Python 生态系统

科学 Python 生态系统是一组用于科学计算的开源 Python 库。它具有以下核心组件：

- *Python*: 这是一种编程语言。
- *NumPy*: 这是数值计算的基础库。科学 Python 生态系统中的几乎所有库都基于 NumPy。它提供了一种多功能的数据结构，称为 *Ndarray*（“N 维数组”）。
- *SciPy*: 该库有许多用于科学计算的例程。
- *Matplotlib*: 这是一个用于可视化的库。其 pyplot 模块具有 Matlab 风格可视化的例程。

所有这些组件一起提供了类似 Matlab 的功能：

- *Pandas*: 这是一个用于数据科学的库，提供高性能、易于使用的数据结构，如 series 和 dataframes 用于存储数据。
- *SymPy*: 这用于符号数学和代数。
- *NetworkX*: 这是一个用于表示和可视化图和网络的库。
- *Scikit-image*: 这是一个用于图像处理的库。
- *Scikit-learn*: 这是一个用于机器学习和人工智能的库。

除了这些库之外，IPython 还为 Python 解释器提供了一个更好的交互式环境。IPython 的交互式环境可以通过使用 Jupyter Notebook 的基于 Web 的笔记本进行访问。

本章的其余部分重点介绍 Jupyter Notebook。

## 介绍 Jupyter Notebook

在本章前面，你学习了运行 Python 语句的各种方法。你在脚本中以及解释器的交互模式中运行了 Python 语句。使用交互模式的主要优点是即时反馈。这种模式的主要缺点是，如果你在输入的语句中犯了任何错误，你必须重写整个语句才能重新执行它。此外，很难将其保存为程序。保存要运行在解释器上的语句的选项可以在菜单的 File 选项中找到。但是，所有语句及其输出都将以 .py 扩展名的纯文本格式保存。如果有任何图形输出，它会单独显示并且无法与语句一起存储。

由于解释器交互模式的局限性，我们将使用一个更好的工具在 Web 浏览器中交互式运行 Python 语句。该工具称为 Jupyter Notebook。它是一个服务器程序，可以在 Web 浏览器中创建交互式笔记本。

Jupyter Notebook 是一个基于 Web 的笔记本，用于交互式编程各种编程语言，如 Python、Octave、Julia 和 R。它在从事研究领域的人们中很受欢迎。Jupyter Notebook 可以在一个文件中保存代码、可视化、输出和富文本。与 Python 自身的交互式提示相比，Jupyter Notebook 的优点是你可以在编辑代码的同时立即看到新的输出，而在 Python 的交互模式中这是不可能的。另一个优点是，你可以在磁盘上的同一个文件中拥有代码、富文本元素以及代码的输出（可以是图形或富文本格式）。这使得分发变得容易。你可以通过互联网或使用便携式存储设备保存和共享这些笔记本。网上有许多服务可以帮助你在云服务器上存储和执行你的笔记本脚本。

## 设置 Jupyter Notebook

你可以通过在命令提示符下运行以下命令在任何计算机上轻松安装 Jupyter Notebook 服务器程序：

```bash
pip3 install jupyter
```

让我们看看现在如何使用 Jupyter Notebook 编写和执行 Python 语句。在操作系统的命令提示符中运行以下命令以在那里启动 Jupyter Notebook 服务器进程：

```bash
jupyter notebook
```

Jupyter Notebook 服务器进程将被启动，命令提示符窗口显示服务器日志，如图 1-13 所示。

![](img/124f805c8155c7226d129ebbcc2842f0_33_0.png)

**Figure 1-13.** *Launching a new Jupyter Notebook process*

它还在操作系统的默认浏览器中启动一个网页。如果浏览器窗口已打开，则它将在同一浏览器窗口的新标签页中启动页面。另一种打开页面的方法（以防你关闭正在运行 Jupyter Notebook 的此浏览器窗口）是在浏览器中访问 `http://localhost:8888/`。它显示如图 1-14 所示的页面。

![](img/124f805c8155c7226d129ebbcc2842f0_34_0.png)

**Figure 1-14.** *Logging in with a token*

以下是服务器日志的文本。

要访问笔记本，请在浏览器中打开此文件：

```text
file:///C:/Users/Ashwin/AppData/Roaming/jupyter/runtime/nbserver-8420-open.html
```

或复制并粘贴以下其中一个 URL：

```text
http://localhost:8888/?token=e4a4fab0d8c22cd01b6530d5daced19d32d7e0c3a56f925c
http://127.0.0.1:8888/?token=e4a4fab0d8c22cd01b6530d5daced19d32d7e0c3a56f925c
```

在之前的日志中，你可以看到几个 URL。它们引用的是同一页面（localhost 和 127.0.0.1 是相同的 hosts）。你可以直接复制并粘贴这些 URL 中的任何一个到浏览器标签的地址栏中并打开 Jupyter Notebook 主页，或者你可以访问 http://localhost:8888/ 如前所述，然后粘贴服务器日志中的 token（在我们的例子中是 e4a4fab0d8c22cd01b6530d5daced19d32d7e0c3a56f925c）并登录。这将带你到同一个主页。

请注意，Jupyter Notebook 服务器的每个实例都有自己的 token，因此书中显示的 token 将无法与你的笔记本一起使用。token 仅对该服务器进程有效。

因此，如果你遵循前面解释的任何一条路径，你将在浏览器窗口中看到一个主页标签，如图 1-15 所示。

![](img/124f805c8155c7226d129ebbcc2842f0_35_0.png)

**Figure 1-15.** *A new home page tab of Jupyter Notebook*

正如你所见，网页上有三个标签：Files、Running 和 Clusters。Files 标签显示从命令提示符启动笔记本服务器时所在的目录中的目录和文件。在之前的示例中，我从我的 Raspberry Pi 的 lxterminal 执行了命令 jupyter notebook。并且当前工作目录是 pi 用户的主目录 /home/pi。这就是为什么你在图 1-15 中可以看到我的 Raspberry Pi 计算机主目录中的所有文件和目录。

在右上角，你可以看到 Quit 和 Logout 按钮。如果你点击 Logout 按钮，则会从当前会话注销，并且要登录，你再次需要来自笔记本服务器日志的 token 或带有嵌入 token 的 URL，如前所述。如果你点击 Quit 按钮，则会停止在命令提示符下运行的笔记本服务器进程并显示如图 1-16 所示的模态消息框。

