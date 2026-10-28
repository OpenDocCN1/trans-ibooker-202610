## 10 文件和异常

![](img/8f15563c16d2a98c4d3115150679b092_314_0.png)

现在你已经掌握了编写结构清晰、易于使用的程序所需的基本技能，是时候思考如何让你的程序更相关、更易用了。在本章中，你将学习如何处理文件，以便你的程序能够快速分析大量数据。

你将学习如何处理错误，以便你的程序在遇到意外情况时不会崩溃。你将学习关于 *exceptions*，它们是 Python 在程序运行期间出现错误时创建的特殊对象，用于管理错误。你还将学习关于 `json` 模块，它允许你保存用户数据，以便在程序停止运行后数据不会丢失。

学习如何处理文件和保存数据将使你的程序更易于人们使用。用户将能够选择输入什么数据以及何时输入。人们将

```text
3.1415926535
8979323846
2643383279
```

要自己尝试以下示例，你可以在编辑器中输入这些行并将文件保存为 *pi_digits.txt*，或者你可以从书籍资源中通过 [https://ehmatthes.github.io/pcc_3e](https://ehmatthes.github.io/pcc_3e) 下载该文件。将文件保存在与本章程序相同的目录中。

下面是一个打开此文件、读取它并将文件内容打印到屏幕上的程序：

```python
# file_reader.py
from pathlib import Path

path = Path('pi_digits.txt')
contents = path.read_text()
print(contents)
```

要处理文件的内容，我们需要告诉 Python 文件的路径。*path* 是系统上文件或文件夹的确切位置。Python 提供了一个名为 `pathlib` 的模块，使在任何操作系统上处理文件和目录变得更容易，无论你或你的程序用户使用的是哪个操作系统。提供此类特定功能的模块通常被称为 *library*，因此得名 `pathlib`。

我们首先从 `pathlib` 导入 `Path` 类。指向文件的 `Path` 对象可以做很多事情。例如，你可以在处理文件之前检查文件是否存在，读取文件的内容，或向文件写入新数据。在这里，我们构建一个表示文件 *pi_digits.txt* 的 `Path` 对象，并将其分配给变量 `path` ❶。由于此文件保存在我们正在编写的 .py 文件所在的同一目录中，`Path` 只需要文件名即可访问该文件。

> **NOTE**
>
> *VS Code 会在最近打开的文件夹中查找文件。如果你使用 VS Code，请先打开存储本章程序的文件夹。例如，如果你将程序文件保存在名为 chapter_10 的文件夹中，请按 CTRL-O（macOS 上为 ⌘-O），并打开该文件夹。*

一旦我们有了表示 *pi_digits.txt* 的 Path 对象，我们就使用 `read_text()` 方法读取文件的全部内容 ➋。文件的内容作为单个字符串返回，我们将其分配给变量 `contents`。当我们打印 `contents` 的值时，我们会看到文本文件的全部内容：

```text
3.1415926535
  8979323846
  2643383279
```

此输出与原始文件之间的唯一区别是输出末尾多出的空白行。空白行出现是因为 `read_text()` 在到达文件末尾时返回一个空字符串；这个空字符串显示为一条空白行。

我们可以通过对 `contents` 字符串使用 `rstrip()` 来删除多余的空白行：

```python
from pathlib import Path

path = Path('pi_digits.txt')
contents = path.read_text()
contents = contents.rstrip()
print(contents)
```

回想一下第 2 章，Python 的 `rstrip()` 方法会删除或剥离字符串右侧的任何空白字符。现在输出与原始文件的内容完全匹配：

```text
3.1415926535
8979323846
2643383279
```

我们可以在读取文件内容时剥离尾部的换行符，通过在调用 `read_text()` 后立即应用 `rstrip()` 方法：

```python
contents = path.read_text().rstrip()
```

这一行告诉 Python 在我们正在处理的文件上调用 `read_text()` 方法。然后它对 `read_text()` 返回的字符串应用 `rstrip()` 方法。清理后的字符串随后被分配给变量 `contents`。这种方法称为方法链，你将在编程中经常看到它使用。

### 相对和绝对文件路径

当你向 `Path` 传递像 `pi_digits.txt` 这样的简单文件名时，Python 会在当前正在执行的那个文件（即你的 .py 程序文件）所在的目录中查找。

有时，根据你组织工作的方式，你想要打开的文件可能不在与程序文件相同的目录中。例如，你可能将程序文件存储在名为 `python_work` 的文件夹中；在 `python_work` 内部，你可能还有另一个名为 `text_files` 的文件夹，以区分你的程序文件和它们所操作的文件。即使 `text_files` 在 `python_work` 中，仅向 `Path` 传递 `text_files` 中的文件名也不会起作用，因为 Python 只会在 `python_work` 中查找，然后停止；它不会继续去查找 *text_files*。要让 Python 从与程序文件存储的目录不同的目录中打开文件，你需要提供正确的路径。

在编程中指定路径主要有两种方式。*relative file path* 告诉 Python 相对于当前运行程序文件所在的目录查找给定位置。由于 *text_files* 在 *python_work* 内部，我们需要构建一个以目录 *text_files* 开头、以文件名结尾的路径。以下是构建此路径的方法：

```python
path = Path('text_files/filename.txt')
```

你也可以告诉 Python 文件在你的计算机上的确切位置，而不管正在执行的程序存储在哪里。这称为 *absolute file path*。如果相对路径不起作用，你可以使用绝对路径。例如，如果你将 *text_files* 放在 *python_work* 以外的某个文件夹中，那么仅传递路径 `'text_files/filename.txt'` 就不会起作用，因为 Python 只会在 `python_work` 内部查找该位置。你需要写出绝对路径以明确你希望 Python 查找的位置。

绝对路径通常比相对路径长，因为它们从系统的根文件夹开始：

```python
path = Path('/home/eric/data_files/text_files/filename.txt')
```

使用绝对路径，你可以从系统上的任何位置读取文件。现在最简单的方法是将文件存储在与程序文件相同的目录中，或存储在程序文件目录内的 *text_files* 等文件夹中。

> **NOTE**
>
> *Windows 系统在显示文件路径时使用反斜杠 (\\) 而不是正斜杠 (/)，但即使在 Windows 上，你也应在代码中使用正斜杠。pathlib 库在与你的系统或任何用户系统交互时会自动使用路径的正确表示形式。*

### 访问文件行

当你处理文件时，你通常希望检查文件的每一行。你可能在文件中寻找某些信息，或者你可能想以某种方式修改文件中的文本。例如，你可能想读取一份天气数据文件并处理描述当天天气包含 *sunny* 词的任何行。在新闻报道中，你可能会查找包含标签 `<headline>` 的任何行，并以特定格式重写该行。

你可以使用 `splitlines()` 方法将长字符串转换为一组行，然后使用 `for` 循环一次检查文件中的每一行：

```python
# file_reader.py
from pathlib import Path

path = Path('pi_digits.txt')
contents = path.read_text()

lines = contents.splitlines()
for line in lines:
    print(line)
```

我们首先读取文件的全部内容，就像之前做的那样 ❶。如果你计划处理




文件中的每一行，你在读取文件时不需要去除任何空白。`splitlines()` 方法返回文件中所有行的列表，我们将这个列表赋值给变量 `lines`。然后我们遍历这些行并打印每一行：

```text
3.1415926535
  8979323846
  2643383279
```

由于我们没有修改任何行，输出与原始文本文件完全一致。

## 处理文件的内容

将文件内容读入内存后，你就可以对这些数据做任何你想做的事情，所以让我们简要探索一下 *圆周率* 的数字。首先，我们将尝试构建一个包含文件中所有数字且不含任何空白的单一字符串：

**pi_string.py**

```python
from pathlib import Path

path = Path('pi_digits.txt')
contents = path.read_text()

lines = contents.splitlines()
pi_string = ''
for line in lines:
    pi_string += line.strip()

print(pi_string)
print(len(pi_string))
```

我们首先读取文件，并将每一行数字存储在一个列表中，就像我们在前一个示例中所做的那样。然后我们创建一个变量 `pi_string`，用来存储 *圆周率* 的数字。我们写下

我一直很好奇我的生日是否出现在 *圆周率* 的数字中。让我们用刚刚编写的程序来找出某人的生日是否出现在 *圆周率* 的前一百万位数字中。我们可以通过将每个生日表示为一个数字字符串，然后检查该字符串是否出现在 pi_string 中的任何位置来实现：

**pi_birthday.py**

```python
--snip--
for line in lines:
    pi_string += line.strip()

birthday = input("Enter your birthday, in the form mmddyy: ")
if birthday in pi_string:
    print("Your birthday appears in the first million digits of pi!")
else:
    print("Your birthday does not appear in the first million digits of pi.")
```

我们首先提示用户输入生日，然后检查该字符串是否在 pi_string 中。让我们试一试：

```text
Enter your birthday, in the form mmddyy: 120372
Your birthday appears in the first million digits of pi!
```

我的生日确实出现在 *圆周率* 的数字中！一旦你从文件中读取了内容，你就可以用你能想到的任何方式分析其内容。

## 试一试

- **10-1. 学习 Python：** 在你的文本编辑器中打开一个空白文件，写几行总结你到目前为止学到的关于 Python 的知识。每一行以 *In Python you can.* 开头…… 将该文件保存为 *learning_python.txt*，放在与本章练习相同的目录中。编写一个程序，读取该文件并打印你写的内容两次：一次通过读取整个文件来打印内容，另一次通过将行存储在列表中然后遍历每一行来打印。

- **10-2. 学习 C：** 你可以使用 `replace()` 方法将字符串中的任何单词替换为另一个单词。下面是一个快速示例，展示如何在句子中将 'dog' 替换为 'cat'：

  ```text
  >>> message = "I really like dogs."
  >>> message.replace('dog', 'cat')
  'I really like cats.'
  ```

  读取你刚刚创建的文件 *learning_python.txt* 中的每一行，并将单词 *Python* 替换为另一种语言的名称，例如 *C*。将每一行修改后的内容打印到屏幕上。

- **10-3. 更简洁的代码：** 本节的程序 *file_reader.py* 使用了一个临时变量 *lines* 来展示 `splitlines()` 是如何工作的。你可以跳过临时变量，直接循环遍历 `splitlines()` 返回的列表：

  ```text
  for line in contents.splitlines():
  ```

  从本节的每个程序中移除临时变量，使它们更加简洁。

## 写入文件

保存数据最简单的方法之一就是将其写入文件。当你将文本写入文件时，即使关闭了包含程序输出的终端，输出仍然可用。你可以在程序运行结束后检查输出，也可以将输出文件与他人共享。你还可以编写程序将文本重新读入内存，并在之后再次使用。

### 写入单行

一旦你定义了路径，就可以使用 `write_text()` 方法将内容写入文件。为了了解它是如何工作的，让我们编写一条简单的消息并将其存储到文件中，而不是打印到屏幕上：

**write_message.py**

```python
from pathlib import Path

path = Path('programming.txt')
path.write_text("I love programming.")
```

`write_text()` 方法接受一个参数：你要写入文件的字符串。这个程序没有终端输出，但如果你打开文件 *programming.txt*，你会看到一行内容：

**programming.txt**

```text
I love programming.
```

这个文件的行为与计算机上的任何其他文件一样。你可以打开它、在其中写入新文本、复制它、粘贴到它，等等。

> **注意**
>
> Python 只能将字符串写入文本文件。如果你想在文本文件中存储数值数据，你必须首先使用 `str()` 函数将数据转换为字符串格式。

### 写入多行

`write_text()` 方法在幕后做了几件事。如果 path 指向的文件不存在，它会创建该文件。此外，在将字符串写入文件后，它会确保文件被正确关闭。未正确关闭的文件可能导致数据丢失或损坏。

要向文件写入多行，你需要构建一个包含文件全部内容的字符串，然后调用 `write_text()` 并传入该字符串。让我们向 *programming.txt* 文件写入几行：

```python
from pathlib import Path

contents = "I love programming.\n"
contents += "I love creating new games.\n"
contents += "I also love working with data.\n"

path = Path('programming.txt')
path.write_text(contents)
```

我们定义了一个名为 `contents` 的变量，它将保存文件的全部内容。在下一行，我们使用 `+=` 运算符向这个字符串添加内容。你可以根据需要多次执行此操作，以构建任意长度的字符串。在这种情况下，我们在每一行的末尾都包含了换行符，以确保每条语句都显示在单独的行上。

如果你运行此程序然后打开 *programming.txt*，你会看到文本文件中有这些行：

```text
I love programming.
I love creating new games.
I also love working with data.
```

你也可以使用空格、制表符和空行来格式化输出，就像你一直在处理基于终端的输出一样。你的字符串长度没有限制，许多计算机生成的文档就是这样创建的。

> **注意**
>
> 在路径对象上调用 `write_text()` 时要小心。如果文件已经存在，`write_text()` 将擦除文件的当前内容并写入新内容。在本章后面，你将学习如何使用 `pathlib` 检查文件是否存在。

## 试一试

- **10-4. 访客：** 编写一个程序，提示用户输入姓名。当他们响应时，将他们的姓名写入一个名为 guest.txt 的文件。

- **10-5. 访客簿：** 编写一个 while 循环，提示用户输入姓名。收集所有输入的姓名，然后将这些姓名写入一个名为 guest_book.txt 的文件。确保每个条目都出现在文件中的新行上。

## 异常

Python 使用称为异常的特殊对象来管理程序执行期间出现的错误。每当发生错误使 Python 不确定下一步该做什么时，它就会创建一个异常对象。如果你编写了处理异常的代码，程序将继续运行。如果你不处理异常，程序将停止并显示 traceback，其中包含引发的异常报告。

异常使用 try-except 块来处理。try-except 块要求 Python 做一些事情，同时也告诉 Python 如果引发异常该怎么做。当你使用 try-except 块时，即使事情开始出错，你的程序也会继续运行。不再是 traceback，而可能是



confusing for users to read, users will see friendly error messages that you’ve written.

## Handling the ZeroDivisionError Exception

Let’s look at a simple error that causes Python to raise an exception. You probably know that it’s impossible to divide a number by zero, but let’s ask Python to do it anyway:

division_calculator.py

```
print(5/0)
```

Python can’t do this, so we get a traceback:

```
Traceback (most recent call last):
    File "division_calculator.py", line 1, in <module>
        print(5/0)
          ^^
ZeroDivisionError: division by zero
```

The error reported in the traceback, ZeroDivisionError, is an exception object ①. Python creates this kind of object in response to a situation where it can’t do what we ask it to. When this happens, Python stops the program and tells us the kind of exception that was raised. We can use this information to modify our program. We’ll tell Python what to do when this kind of exception occurs; that way, if it happens again, we’ll be prepared.

## Using try-except Blocks

When you think an error may occur, you can write a try-except block to handle the exception that might be raised. You tell Python to try running some code, and you tell it what to do if the code results in a particular kind of exception.

Here’s what a try-except block for handling the ZeroDivisionError exception looks like:

```
try:
    print(5/0)
except ZeroDivisionError:
    print("You can't divide by zero!")
```

We put print(5/0), the line that caused the error, inside a try block. If the code in a try block works, Python skips over the except block. If the code in the try block causes an error, Python looks for an except block whose error matches the one that was raised, and runs the code in that block.

In this example, the code in the try block produces a ZeroDivisionError, so Python looks for an except block telling it how to respond. Python then runs the code in that block, and the user sees a friendly error message instead of a traceback:

```
You can't divide by zero!
```

If more code followed the try-except block, the program would continue running because we told Python how to handle the error. Let’s look at an example where catching an error can allow a program to continue running.

## Using Exceptions to Prevent Crashes

Handling errors correctly is especially important when the program has more work to do after the error occurs. This happens often in programs that prompt users for input. If the program responds to invalid input appropriately, it can prompt for more valid input instead of crashing.

Let’s create a simple calculator that does only division:

division_calculator.py

## The else Block

We can make this program more error resistant by wrapping the line that might produce errors in a `try-except` block. The error occurs on the line that performs the division, so that’s where we’ll put the `try-except` block. This example also includes an `else` block. Any code that depends on the `try` block executing successfully goes in the `else` block:

```
```python
--snip--
while True:
    --snip--
    if second_number == 'q':
        break
    try:
        answer = int(first_number) / int(second_number)
    except ZeroDivisionError:
        print("You can't divide by 0!")
    else:
        print(answer)
```
```

We ask Python to try to complete the division operation in a `try` block ❶, which includes only the code that might cause an error. Any code that depends on the `try` block succeeding is added to the `else` block. In this case, if the division operation is successful, we use the `else` block to print the result ❸.

The `except` block tells Python how to respond when a `ZeroDivisionError` arises ❷. If the `try` block doesn’t succeed because of a division-by-zero error, we print a friendly message telling the user how to avoid this kind of error. The program continues to run, and the user never sees a traceback:

```
```text
Give me two numbers, and I'll divide them.
Enter 'q' to quit.

First number: 5
```

```
Second number: 0
You can't divide by 0!

First number: 5
Second number: 2
2.5

First number: q
```

The only code that should go in a try block is code that might cause an exception to be raised. Sometimes you'll have additional code that should run only if the try block was successful; this code goes in the else block. The except block tells Python what to do in case a certain exception arises when it tries to run the code in the try block.

By anticipating likely sources of errors, you can write robust programs that continue to run even when they encounter invalid data and missing resources. Your code will be resistant to innocent user mistakes and malicious attacks.

## Handling the FileNotFoundError Exception

One common issue when working with files is handling missing files. The file you're looking for might be in a different location, the filename might be misspelled, or the file might not exist at all. You can handle all of these situations with a try-except block.

Let's try to read a file that doesn't exist. The following program tries to read in the contents of *Alice in Wonderland*, but I haven't saved the file *alice.txt* in the same directory as *alice.py*:

***alice.py***

```
from pathlib import Path

path = Path('alice.txt')
contents = path.read_text(encoding='utf-8')
```

Note that we’re using `read_text()` in a slightly different way here than what you saw earlier. The `encoding` argument is needed when your system’s default encoding doesn’t match the encoding of the file that’s being read. This is most likely to happen when reading from a file that wasn’t created on your system.

Python can’t read from a missing file, so it raises an exception:

```
Traceback (most recent call last):
  File "alice.py", line 4, in <module>
    contents = path.read_text(encoding='utf-8')
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/.../pathlib.py", line 1056, in read_text
    with self.open(mode='r', encoding=encoding, errors=errors) as f:
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/.../pathlib.py", line 1042, in open
    return io.open(self, mode, buffering, encoding, errors, newline)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
FileNotFoundError: [Errno 2] No such file or directory: 'alice.txt'
```

This is a longer traceback than the ones we’ve seen previously, so let’s look at how you can make sense of more complex tracebacks. It’s often best to start at the very end of the traceback. On the last line, we can see that a `FileNotFoundError` exception was raised ❸. This is important because it tells us what kind of exception to use in the `except` block that we’ll write.

Looking back near the beginning of the traceback ❶, we can see that the error occurred at line 4 in the file *alice.py*. The next line shows the line of code that caused the error ❷. The rest of the traceback shows some code from the

libraries that are involved in opening and reading from files. You don’t usually need to read through or understand all of these lines in a traceback.

To handle the error that’s being raised, the try block will begin with the line that was identified as problematic in the traceback. In our example, this is the line that contains read_text():

```
from pathlib import Path

path = Path('alice.txt')
try:
    contents = path.read_text(encoding='utf-8')
except FileNotFoundError:
    print(f"Sorry, the file {path} does not exist.")
```

In this example, the code in the try block produces a FileNotFoundError, so we write an except block that matches that error ①. Python then runs the code in that block when the file can’t be found, and the result is a friendly error message instead of a traceback:

```
Sorry, the file alice.txt does not exist.
```

The program has nothing more to do if the file doesn’t exist, so this is all the output we see. Let’s build on this example and see how exception handling can help when you’re working with more than one file.

## Analyzing Text

You can analyze text files containing entire books. Many classic works of literature are available as simple text files because they are in the public domain. The texts used in this section come from Project Gutenberg (https://gutenberg.org). Project Gutenberg maintains a collection of literary works that are available in the public domain, and it’s a



如果你对在编程项目中使用文学文本感兴趣，这是一个很棒的资源。

让我们引入 *爱丽丝梦游仙境* 的文本并尝试统计文本中的单词数。为此，我们将使用字符串方法 `split()`，默认情况下它会在遇到任何空白字符时拆分字符串：

```python
from pathlib import Path

path = Path('alice.txt')
try:
    contents = path.read_text(encoding='utf-8')
except FileNotFoundError:
    print(f"Sorry, the file {path} does not exist.")
else:
    # Count the approximate number of words in the file:
    words = contents.split()
    num_words = len(words)
    print(f"The file {path} has about {num_words} words.")
```

我已将文件 *alice.txt* 移动到正确的目录，因此 try 块这次会正常工作。我们获取字符串 contents，它现在包含 *爱丽丝梦游仙境* 的全部文本作为一个长字符串，并使用 `split()` 生成书中所有单词的列表 ❶。对该列表使用 `len()` ❷ 可以很好地近似原始文本中的单词数。最后，我们打印一条语句报告在文件中找到的单词数。此代码放在 else 块中，因为只有在 try 块的代码成功执行时它才有效。

输出告诉我们 *alice.txt* 中有多少个单词：

```
The file alice.txt has about 29594 words.
```

计数有点偏高，因为此处使用的文本文件中出版商提供了额外的信息，但它是对 *爱丽丝梦游仙境* 长度的一个良好近似。

## 处理多个文件

让我们添加更多书籍进行分析，但在这样做之前，让我们将此程序的大部分内容移到一个名为 `count_words()` 的函数中。这样可以更轻松地对多本书进行分析：

### word_count.py

```python
from pathlib import Path

def count_words(path):
    """Count the approximate number of words in a file."""
    try:
        contents = path.read_text(encoding='utf-8')
    except FileNotFoundError:
        print(f"Sorry, the file {path} does not exist.")
    else:
        # Count the approximate number of words in the file:
        words = contents.split()
        num_words = len(words)
        print(f"The file {path} has about {num_words} words.")

path = Path('alice.txt')
count_words(path)
```

大部分代码保持不变。只是进行了缩进，并移入了 `count_words()` 的主体。修改程序时保持注释更新是一个好习惯，因此注释也已更改为 docstring 并略有改写 ❶。

现在我们可以编写一个简短的循环来统计我们想要分析的任何文本的单词数。我们通过将要分析的文件名存储在列表中，然后对列表中的每个文件调用 `count_words()` 来实现。我们将尝试统计 *爱丽丝梦游仙境*、*悉达多*、*白鲸* 和 *小妇人* 的单词数，这些书都可在公共领域获得。我故意将 *siddhartha.txt* 从包含 *word_count.py* 的目录中移出，以便我们可以看到程序处理缺失文件的情况有多好：

```python
from pathlib import Path

def count_words(filename):
    --snip--

filenames = ['alice.txt', 'siddhartha.txt', 'moby_dick.txt',
             'little_women.txt']
for filename in filenames:
    path = Path(filename)
    count_words(path)
```

文件名以简单的字符串形式存储。每个字符串然后在调用 `count_words()` 之前转换为 `Path` 对象 ❶。缺失的 *siddhartha.txt* 文件不会影响程序其余部分的执行：

```text
The file alice.txt has about 29594 words.
Sorry, the file siddhartha.txt does not exist.
The file moby_dick.txt has about 215864 words.
The file little_women.txt has about 189142 words.
```

在此示例中使用 `try-except` 块提供了两个显著的优势。我们防止用户看到回溯，并让程序继续分析它能够找到的文本。如果我们不捕获 *siddhartha.txt* 引发的 `FileNotFoundError`，用户将看到完整的回溯，并且程序在尝试分析 *悉达多* 后就会停止运行。它永远不会分析 *白鲸* 或 *小妇人*。

## 静默失败

在前面的示例中，我们通知用户其中一个文件不可用。但你不必报告捕获的每个异常。有时，你希望程序在发生异常时静默失败并继续执行，就像什么都没发生一样。要使程序静默失败，你照常编写 try 块，但在 except 块中明确告诉 Python 什么都不做。Python 有一个 pass 语句，告诉它在块中什么都不做：

```python
def count_words(path):
    """Count the approximate number of words in a file."""
    try:
        --snip--
    except FileNotFoundError:
        pass
    else:
        --snip--
```

此列表与前一个列表的唯一区别是 except 块中的 pass 语句。现在当引发 FileNotFoundError 时，except 块中的代码会运行，但什么也不会发生。不会产生回溯，也不会对引发的错误做出任何输出。用户会看到每个存在的文件的单词计数，但他们看不到任何文件未找到的迹象：

```
The file alice.txt has about 29594 words.
The file moby_dick.txt has about 215864 words.
The file little_women.txt has about 189142 words.
```

pass 语句也充当占位符。它提醒你在程序执行的特定点上选择什么都不做，并且你可能以后想在那里做些什么。例如，在此程序中，我们可能会决定将任何缺失的文件名写入名为 missing_files.txt 的文件。用户看不到此文件，但我们可以读取该文件并处理任何缺失的文本。

## 决定报告哪些错误

你如何知道何时向用户报告错误，何时让程序静默失败？如果用户知道应该分析哪些文本，他们可能会欣赏一条信息，告知他们为何某些文本未被分析。如果用户期望看到一些结果但不知道应该分析哪些书籍，他们可能不需要知道某些文本不可用。向用户提供他们不寻找的信息会降低程序的可用性。Python 的错误处理结构使你能够精细控制在出现问题时与用户分享多少信息；分享多少信息由你决定。

编写良好、经过充分测试的代码不太容易出现内部错误，例如语法错误或逻辑错误。但每当你的程序依赖于某些外部因素，例如用户输入、文件的存在或网络连接的可用性时，就有可能引发异常。一些经验将帮助你知道在程序中何处包含异常处理块，以及向用户报告多少错误信息。

## 试试看

- **10-6. 加法：** 在提示输入数值时出现的一个常见问题是人们提供文本而不是数字。当你尝试将输入转换为 `int` 时，你会得到一个 `ValueError`。编写一个程序提示输入两个数字。将它们相加并打印结果。如果任一输入值不是数字，请捕获 `ValueError` 并打印友好的错误消息。通过输入两个数字然后输入文本而不是数字来测试你的程序。

- **10-7. 加法计算器：** 将练习 10-6 中的代码包装在一个 `while` 循环中，以便用户即使在输入文本而不是数字时犯了错误也能继续输入数字。

- **10-8. 猫和狗：** 创建两个文件，*cats.txt* 和 *dogs.txt*。在第一个文件中存储至少三个猫的名字，在第二个文件中存储三个狗的名字。编写一个程序尝试读取这些文件并将文件内容打印到屏幕上。用 `try-except` 块包装你的代码以捕获 `FileNotFoundError`，如果文件丢失则打印友好的消息。将其中一个文件移动到系统上的不同位置，并确保 `except` 块中的代码正确执行。

- **10-9. 静默的猫和狗：** 修改练习 10-8 中的 `except` 块，如果任一文件丢失则静默失败。

- **10-10. 常见词：** 访问 Project Gutenberg（[https://gutenberg.org](https://gutenberg.org)）并找到你想分析的几篇文本。下载这些作品的文本文件，或将原始文本从浏览器复制到计算机上的文本文件。

  你可以使用 `count()` 方法找出某个单词或短语在字符串中出现的次数。例如，以下代码统计字符串中 `'row'` 出现的次数：

  ```python
  >>> line = "Row, row, row your boat"
  >>> line.count('row')
  2
  >>> line.lower().count('row')
  3
  ```

  注意，使用 `lower()` 将字符串转换为小写可以捕获你正在寻找的单词的所有出现情况，而不管其格式如何。

  编写一个程序读取你在 Project Gutenberg 找到的文件，并确定单词 `'the'` 在每篇文本中出现的次数。这是一个近似值，因为它也会统计诸如 `'then'` 和 `'there'` 这样的词。尝试统计 `'the '`，字符串中带有一个空格，看看你的计数会低多少。





## 存储数据

你的许多程序都会要求用户输入某些类型的信息。你可能允许用户在游戏中存储偏好，或者为可视化提供数据。无论你的程序关注什么，你都会将用户提供的信息存储在列表和字典等数据结构中。当用户关闭程序时，你几乎总是希望保存他们输入的信息。一种简单的方法就是使用 `json` 模块来存储数据。

`json` 模块允许你将简单的 Python 数据结构转换为 JSON 格式的字符串，然后在程序下次运行时从该文件加载数据。你也可以使用 `json` 在不同 Python 程序之间共享数据。更棒的是，JSON 数据格式并非 Python 专用，因此你可以将存储为 JSON 格式的数据与使用许多其他编程语言的人共享。这是一种有用且可移植的格式，而且易于学习。

> **注意**
>
> *JSON（JavaScript 对象表示法）格式最初是为 JavaScript 开发的。然而，它后来已成为许多语言（包括 Python）使用的通用格式。*

### 使用 `json.dumps()` 和 `json.loads()`

让我们编写一个短程序来存储一组数字，再编写另一个程序将这些数字读回内存。第一个程序将使用 `json.dumps()` 来存储这组数字，第二个程序将使用 `json.loads()`。

`json.dumps()` 函数接受一个参数：一个需要转换为 JSON 格式的数据。该函数返回一个字符串，然后我们可以将其写入数据文件：

**number_writer.py**

```python
from pathlib import Path
import json

numbers = [2, 3, 5, 7, 11, 13]

path = Path('numbers.json')
contents = json.dumps(numbers)
path.write_text(contents)
```

我们首先导入 `json` 模块，然后创建要处理的数字列表。接着选择一个文件名来存储这个数字列表 ❶。通常使用 `.json` 文件扩展名来表示文件中的数据是以 JSON 格式存储的。接下来，我们使用 `json.dumps()` ❷ 函数生成一个字符串，其中包含我们正在处理的数据的 JSON 表示形式。一旦我们有了这个字符串，就使用之前使用过的 `write_text()` 方法将它写入文件。

这个程序没有输出，但让我们打开 `numbers.json` 文件看看。数据存储的格式看起来就像 Python：

```text
[2, 3, 5, 7, 11, 13]
```

现在我们将编写一个单独的程序，使用 `json.loads()` 将列表读回内存：

**number_reader.py**

```python
from pathlib import Path
import json

① path = Path('numbers.json')
② contents = path.read_text()
③ numbers = json.loads(contents)

print(numbers)
```

我们确保读取的是我们写入的同一个文件 ①。由于数据文件只是一个具有特定格式的文本文件，我们可以使用 `read_text()` 方法读取它 ②。然后将文件内容传递给 `json.loads()` ③。这个函数接受一个 JSON 格式的字符串，并返回一个 Python 对象（在此处是一个列表），我们将其赋值给 `numbers`。最后，打印恢复的数字列表，可以看到它与 `number_writer.py` 中创建的列表相同：

```text
[2, 3, 5, 7, 11, 13]
```

这是在两个程序之间共享数据的一种简单方式。

### 保存和读取用户生成的数据

使用 `json` 保存数据在处理用户生成的数据时非常有用，因为如果你不以某种方式存储用户的信息，程序停止运行时你将丢失这些信息。让我们看一个示例：用户第一次运行程序时提示输入姓名，然后当用户再次运行程序时记住他们的名字。

我们先来存储用户名：

**remember_me.py**

```python
from pathlib import Path
import json

① username = input("What is your name? ")

② path = Path('username.json')
contents = json.dumps(username)
```

我们可以适当地处理 *username.json* 不存在的情况，但这里我们将使用 `pathlib` 模块中的一个便捷方法：

**remember_me.py**

```python
from pathlib import Path
import json

path = Path('username.json')
① if path.exists():
    contents = path.read_text()
    username = json.loads(contents)
    print(f"Welcome back, {username}!")
② else:
    username = input("What is your name? ")
    contents = json.dumps(username)
    path.write_text(contents)
    print(f"We'll remember you when you come back, {username}!")
```

有许多有用的方法可以与 `Path` 对象一起使用。`exists()` 方法在文件或文件夹存在时返回 `True`，不存在时返回 `False`。这里我们使用 `path.exists()` 来判断用户名是否已被存储 ①。如果 *username.json* 存在，我们加载用户名并向用户打印个性化的问候语。

如果文件 *username.json* 不存在 ②，我们提示输入用户名并存储用户输入的值。我们还会打印熟悉的提示消息，告诉用户他们回来时我们会记住他们。

无论执行哪个代码块，结果都是一个用户名和适当的问候语。如果这是程序第一次运行，输出如下：

```text
What is your name? Eric
We'll remember you when you come back, Eric!
```

否则：

```text
Welcome back, Eric!
```

如果程序至少已经运行过一次，你会看到这样的输出。虽然本节中的数据只是一个字符串，但该程序对于任何可以转换为 JSON 格式字符串的数据也同样适用。

### 重构

通常，你会遇到代码可以正常工作，但你认为可以通过将代码分解为一系列具有特定功能的函数来改进它的情况。这个过程称为重构。重构使你的代码更整洁、更易于理解且更易于扩展。

我们可以通过将 *remember_me.py* 的大部分逻辑移入一个或多个函数来重构它。*remember_me.py* 的重点是向用户问好，所以让我们将所有现有代码移入一个名为 `greet_user()` 的函数中：

**remember_me.py**

```python
from pathlib import Path
import json

def greet_user():
    """Greet the user by name."""
    path = Path('username.json')
    if path.exists():
        contents = path.read_text()
        username = json.loads(contents)
        print(f"Welcome back, {username}!")
    else:
        username = input("What is your name? ")
        contents = json.dumps(username)
        path.write_text(contents)
        print(f"We'll remember you when you come back, {username}!")

greet_user()
```

因为我们现在使用函数，所以将注释重写为反映程序当前工作方式的文档字符串 ①。这个文件更简洁一些，但 `greet_user()` 函数所做的不仅仅是向用户问好——它还负责在存在存储的用户名时检索它，并在不存在时提示输入新用户名。

让我们重构 `greet_user()`，使它不要承担这么多不同的任务。我们先从将检索存储用户名的代码移到一个单独的函数开始：

```python
from pathlib import Path
import json

def get_stored_username(path):
    """Get stored username if available."""
    if path.exists():
        contents = path.read_text()
        username = json.loads(contents)
        return username
    else:
        return None

def greet_user():
    """Greet the user by name."""
    path = Path('username.json')
    username = get_stored_username(path)
    if username:
        print(f"Welcome back, {username}!")
    else:
        username = input("What is your name? ")
        contents = json.dumps(username)
        path.write_text(contents)
        print(f"We'll remember you when you come back, {username}!")

greet_user()
```

新函数 `get_stored_username()` ① 目的明确，正如文档字符串所述。这个函数检索存储的用户名，如果找到则返回用户名。如果



如果传递给 `get_stored_username()` 的路径不存在，该函数会返回 `None` ❷。这是一个良好的实践：一个函数要么返回你期望的值，要么返回 `None`。这使我们能够对函数的返回值进行简单的测试。如果成功检索到用户名，我们会向用户打印欢迎回来信息 ❸，如果没有，则提示输入新的用户名。

我们还应该从 `greet_user()` 中提取另一块代码。如果用户名不存在，我们应该将提示输入新用户名的代码移到专门为此目的而设的函数中：

```
```python
from pathlib import Path
import json

def get_stored_username(path):
    """Get stored username if available."""
    --snip--

def get_new_username(path):
    """Prompt for a new username."""
    username = input("What is your name? ")
    contents = json.dumps(username)
    path.write_text(contents)
    return username

def greet_user():
    """Greet the user by name."""
    path = Path('username.json')
    username = get_stored_username(path)
    if username:
        print(f"Welcome back, {username}!")
    else:
        username = get_new_username(path)
        print(f"We'll remember you when you come back, {username}!")

greet_user()
```
```

在这个最终版本的 remember_me.py 中，每个函数都有一个单一、清晰的目的。我们调用 greet_user()，该函数会打印合适的消息：它要么欢迎回来的现有用户，要么问候新用户。它通过调用 get_stored_username() 来实现这一点，后者仅负责在存在时检索存储的用户名。最后，如果需要，greet_user() 会调用 get_new_username()，后者仅负责获取新用户名并将其存储起来。这种工作的 compartmentalization 是编写清晰代码以便易于维护和扩展的重要组成部分。

## TRY IT YOURSELF

- **10-11. Favorite Number:** 编写一个程序，提示用户输入其最喜欢的数字。使用 json.dumps() 将此数字存储在文件中。编写一个单独的程序，读取此值并打印消息 “I know your favorite number! It’s _____。”

- **10-12. Favorite Number Remembered:** 将你在练习 10-11 中编写的两个程序合并到一个文件中。如果数字已存储，则向用户报告最喜欢的数字。如果没有，则提示用户输入最喜欢的数字并将其存储在文件中。运行程序两次以查看其是否正常工作。

- **10-13. User Dictionary:** remember_me.py 示例仅存储一个信息，即用户名。通过询问用户另外两条信息来扩展此示例，然后将收集到的所有信息存储在字典中。使用 json.dumps() 将此字典写入文件，并使用 json.loads() 将其读回。打印摘要，显示程序关于用户的确切记忆内容。

- **10-14. Verify User:** remember_me.py 的最终列表假设用户已经输入了其用户名，或者程序正在首次运行。我们应该对其进行修改，以防当前用户不是上次使用程序的人。

在 greet_user() 中打印欢迎回来消息之前，询问用户此用户名是否正确。如果不是，则调用 get_new_username() 以获取正确的用户名。

## Summary

在本章中，你学习了如何处理文件。你学习了如何读取文件的全部内容，然后在需要时逐行处理内容。你学习了如何向文件写入任意多的文本。你还阅读了关于异常及其在程序中可能遇到的异常处理方法。最后，你学习了如何存储 Python 数据结构，以便保存用户提供的信息，防止他们每次运行程序时都必须从头开始。

在第 11 章中，你将学习高效测试代码的方法。这将帮助你确信你开发的代码是正确的，并帮助你在继续构建已编写的程序时识别引入的错误。

*[OceanofPDF.com](http://OceanofPDF.com)*
