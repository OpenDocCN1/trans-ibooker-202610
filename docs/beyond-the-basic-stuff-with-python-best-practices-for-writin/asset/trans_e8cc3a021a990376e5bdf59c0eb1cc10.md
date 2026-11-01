
## 代码标签和 TODO 注释

程序员有时会留下简短的注释来提醒自己还有待完成的工作。这通常采用代码标签的形式：一条带有全大写标签的注释，例如 TODO，后面跟一个简短的描述。理想情况下，你应该使用项目管理工具来跟踪这类问题，而不是将它们深埋在源代码中。但对于不使用此类工具的较小、个人项目来说，偶尔的 TODO 注释可以作为有用的提醒。下面是一个示例：

```
_chargeIonFluxStream()  # TODO: Investigate why this fails every Tuesday.
```

你可以为这些提醒使用几种不同的代码标签：

- **TODO** 引入关于需要完成的工作的一般提醒
- **FIXME** 引入提醒，指出代码的这一部分并不完全正常工作
- **HACK** 引入提醒，指出代码的这一部分可以工作，也许勉强可以，但代码应该改进
- **XXX** 引入一般警告，通常严重程度较高

你应该用更具体的任务或问题的描述来跟随这些始终大写的标签。稍后，你可以搜索源代码中的标签来找到需要修复的代码。缺点是，除非你恰好阅读了包含这些提醒的代码部分，否则很容易忘记这些提醒。代码标签不应取代正式的问题跟踪器或错误报告工具。如果你要在代码中使用代码标签，我建议保持简单：只使用 TODO，放弃其他标签。

## 魔法注释和源文件编码

你可能在 .py 源文件顶部看到过类似以下行的内容：

```
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```

这些魔法注释始终出现在文件顶部，提供解释器或编码信息。Shebang 行 ①（在第 2 章中介绍）告诉你的操作系统使用哪个解释器来运行文件中的指令。

第二个魔法注释是编码定义行 ②。在这种情况下，该行将 UTF-8 定义为源文件使用的 Unicode 编码方案。你几乎永远不需要包含这一行，因为大多数编辑器和 IDE 已经以 UTF-8 编码保存源代码文件，而且从 Python 3.0 开始的 Python 版本默认将 UTF-8 视为定义的编码。以 UTF-8 编码的文件可以包含任何字符，因此你的 .py 源文件无论包含英语、中文还是阿拉伯字母都完全没问题。

关于 Unicode 和字符串编码的介绍，我强烈推荐 Ned Batchelder 的博客文章“实用 Unicode”，网址为 https://nedbatchelder.com/text/unipain.html。

## 文档字符串

文档字符串是多行注释，出现在模块的 .py 源代码文件顶部或直接跟在 class 或 def 语句之后。它们提供有关正在定义的模块、类、函数或方法的文档。自动文档生成工具使用这些文档字符串生成外部文档文件，例如帮助文件或网页。

文档字符串必须使用三引号的多行注释，而不是以井号 # 开头的单行注释。文档字符串应始终使用三个双引号作为其三引号字符串，而不是三个单引号。例如，以下是流行的 requests 模块中 sessions.py 文件的摘录：

```python
# -*- coding: utf-8 -*-
"""
requests.session
~~~~~~~~~~~~~~~~

This module provides a Session object to manage and persist settings across requests (cookies, auth, proxies).
"""
import os
import sys
--snip--
class Session(SessionRedirectMixin):
    """A Requests session.

    Provides cookie persistence, connection-pooling, and configuration.

    Basic Usage::

        >>> import requests
        >>> s = requests.Session()
        >>> s.get('https://httpbin.org/get')
        <Response [200]>
--snip--

def get(self, url, **kwargs):
    r"""Sends a GET request. Returns :class:`Response` object.

    :param url: URL for the new :class:`Request` object.
    :param \**kwargs: Optional arguments that ``request`` takes.
    :rtype: requests.Response
    """
--snip--
```

sessions.py 文件的摘录包含模块 ②、Session 类 ③ 以及 Session 类的 get() 方法 ④ 的文档字符串。请注意，虽然模块的文档字符串必须是模块中出现的第一个字符串，但它应位于任何魔法注释之后，例如 shebang 行或编码定义 ①。

稍后，你可以通过检查相应对象的 __doc__ 属性来检索模块、类、函数或方法的文档字符串。例如，这里我们检查文档字符串以了解更多关于 sessions 模块、Session 类和 get() 方法的信息：

```
>>> from requests import sessions
>>> sessions.__doc__
'\nrequests.session\n~~~~~~~~~~~~~~\n\nThis module provides a Session object to manage and persist settings across\nrequests (cookies, auth, proxies).\n'
>>> sessions.Session.__doc__
"A Requests session.\n\n    Provides cookie persistence, connection-pooling, and configuration.\n\n    Basic Usage::\n\n        >>> import requests\n--snip--
>>> sessions.Session.get.__doc__
'Sends a GET request. Returns :class:`Response` object.\n\n    :param url: URL for the new :class:`Request` object.\n        :param \*\*kwargs:
--snip--
```

自动文档工具可以利用文档字符串提供上下文相关的信息。其中一种工具是 Python 内置的 help() 函数，它以比直接原始 __doc__ 字符串更易读的格式显示你传递给它的对象的文档字符串。当你在交互式 shell 中进行实验时，这很有用，因为你可以立即调出你正在尝试使用的任何模块、类或函数的信息：

```
>>> from requests import sessions
>>> help(sessions)
Help on module requests.sessions in requests:

NAME
    requests.sessions

DESCRIPTION
    requests.session
    ~~~~~~~~~~~~~~~~

    This module provides a Session object to manage and persist settings
-- More --
```

如果文档字符串太大而无法在屏幕上显示，Python 会在窗口底部显示 -- More --。你可以按 ENTER 滚动到下一行，按空格键滚动到下一页，或按 Q 退出查看文档字符串。

一般来说，文档字符串应包含一行总结模块、类或函数的描述，后跟一个空行和更详细的信息。对于函数和方法，这可以包括有关其参数、返回值和副作用的信息。我们为其他程序员而不是软件用户编写文档字符串，因此它们应包含技术信息，而不是教程。

文档字符串提供的第二个关键好处是它们将文档集成到源代码中。当你将文档与代码分开编写时，你常常会完全忘记它。相反，当你将文档字符串放在模块、类和函数的顶部时，信息仍然易于审查和更新。

你可能并不总是能够立即编写文档字符串，如果你仍在处理它所描述的代码。在这种情况下，在文档字符串中包含一个 TODO 注释作为提醒，提醒你填写其余细节。例如，以下虚构的 reverseCatPolarity() 函数有一个糟糕的文档字符串，说明了显而易见的事情：

```
def reverseCatPolarity(catId, catQuantumPhase, catVoltage):
    """Reverses the polarity of a cat.

    TODO Finish this docstring."""
--snip--
```

因为每个类、函数和方法都应该有文档字符串，你可能会想只写最少的文档然后继续。没有 TODO 注释，很容易忘记这个文档字符串最终需要重写。

PEP 257 在 https://www.python.org/dev/peps/pep-0257/ 包含有关文档字符串的进一步文档。

## 类型提示

许多编程语言具有静态类型，这意味着程序员必须在源代码中声明所有变量、参数和返回值的 数据类型。这允许解释器或编译器在程序运行之前检查代码是否正确使用所有对象。Python 具有动态类型：变量、参数和返回值可以是任何数据类型，甚至在程序运行时更改数据类型。动态语言通常更容易编程，因为它们需要更少的正式规范，但它们缺乏静态语言所具有的防止错误的好处。如果你写一行 Python 代码，例如 round('forty two')，你可能不会意识到你在向一个只接受 int 或 float 参数的函数传递字符串，直到你运行代码并导致错误。静态类型语言会在你分配错误类型的值或传递错误类型的参数时给出早期警告。

Python 的类型提示提供可选的静态类型。在以下示例中，类型提示以粗体显示：

```
def describeNumber(number: int) -> str:
    if number % 2 == 1:
        return 'An odd number. '
```
