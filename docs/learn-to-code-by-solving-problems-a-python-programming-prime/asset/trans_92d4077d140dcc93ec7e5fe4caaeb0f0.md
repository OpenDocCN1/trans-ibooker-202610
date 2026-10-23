

```python
>>> output_file = open('word.out', 'w')
>>> output_file
<_io.TextIOWrapper name='word.out' mode='w' encoding='cp1252'>
```

## 从文件读取

要从打开的文件中读取一行，我们使用文件对象的 `readline` 方法。该方法返回一个包含文件下一行内容的字符串。就这一点而言，它与 `input` 函数类似。然而，与 `input` 不同，`readline` 是从文件读取而不是从标准输入读取。

让我们打开 `word.in` 并读取它的两行：

```python
>>> input_file = open('word.in', 'r')
>>> input_file.readline()
'12 13\n'
>>> input_file.readline()
'perhaps better poetry will be written in the language of digital computers\n'
```

这里令人意外的是每个字符串末尾的 `\n`。我们在使用 `input` 读取一行时肯定没有看到那个。字符串中的 `\` 符号是一个 *转义字符*。它跳出字符的标准解释并改变其含义。我们不会把 `\n` 当作两个独立的字符 `\` 和 `n` 来处理。相反，`\n` 只是一个字符：换行符。文件中所有行，或许除了最后一行，都以换行符结束。如果没有的话，一切都会在一行上！`readline` 方法字面地给了我们整行，包括其终止的换行符。

下面是如何在我们自己的字符串中嵌入换行符：

```python
>>> 'one\ntwo\nthree'
'one\ntwo\nthree'
>>> print('one\ntwo\nthree')
one
two
three
```

Python shell 不会处理转义字符的效果，但 `print` 会。

`\n` 序列在字符串中很有用，因为它帮助我们添加多行。但我们很少希望从文件中读取的行中保留这些换行符。为了去掉它们，我们可以使用字符串 `rstrip` 方法。此方法与 `strip` 类似，只是它仅从字符串右侧移除空白（而不是左侧）。就它而言，换行符与空格一样是空白：

```python
>>> 'hello\nthere\n\n'
'hello\nthere\n\n'
>>> 'hello\nthere\n\n'.rstrip()
'hello\nthere'
```

让我们再次从文件中读取，这次去掉换行符：

```python
>>> input_file = open('word.in', 'r')
>>> input_file.readline().rstrip()
'12 13'
>>> input_file.readline().rstrip()
'perhaps better poetry will be written in the language of digital computers'
```

此时，我们已经读取了两行，所以文件里没有东西可读了。`readline` 方法通过返回一个空字符串来表示这一点。

```python
>>> input_file.readline().rstrip()
''
```

空字符串意味着我们已到达文件末尾。如果我们想再次读取行，必须重新打开文件以从开头开始。

让我们这样做，这次使用变量保存每一行：

```python
>>> input_file = open('word.in', 'r')
>>> first = input_file.readline().rstrip()
>>> second = input_file.readline().rstrip()
>>> first
'12 13'
>>> second
'perhaps better poetry will be written in the language of digital computers'
```

如果我们需要读取文件中的所有行，无论有多少行，我们可以使用 `for` 循环。Python 中的文件充当行序列，因此我们可以像遍历字符串和列表一样遍历它们：

```python
>>> input_file = open('word.in', 'r')
>>> for line in input_file:
...     print(line.rstrip())
...
```

不过，与字符串或列表不同，我们不能第二次遍历文件，因为第一次遍历会把我们带到文件末尾。如果我们尝试，就什么也得不到：

```python
>>> for line in input_file:
...   print(line.rstrip())
...
```

### 概念检查

我们想使用 `while` 循环输出打开文件 `input_file` 的每一行。 （该文件可以是任意文件；我没有假设它与论文格式有关。）以下哪段代码能正确做到这一点？

**A.**

```python
while input_file.readline() != "":
    print(input_file.readline().rstrip())
```

**B.**

```python
line = 'x'
while line != "":
    line = input_file.readline()
    print(line.rstrip())
```

**C.**

```python
line = input_file.readline()
while line != "":
    line = input_file.readline()
    print(line.rstrip())
```

- D. 以上全部
- E. 以上皆非

在查看答案之前，我鼓励你创建一个包含四到五行内容的文件并在该文件上尝试每段代码。你也可以考虑在输出的每一行开头添加一个字符如 `*`，以便能看到否则为空的空白行。

**答案：** E. 每段代码都有细微的错误。

代码 A 只输出文件的每隔一行。例如，`while` 循环的布尔表达式导致第一行被读取……然后丢失，因为它没有被赋给变量。因此循环的第一次迭代输出的是文件的第二行。

代码 B 非常接近正确。它输出文件的全部行，但也在末尾输出了一行多余的空白行。

代码 C 未打印文件的第一行。那是因为第一行在循环之前被读取，但循环随后读取第二行而没有打印第一行。它也像代码 B 一样在末尾产生了一行多余的空白行。

下面是正确读取并打印每一行的代码：

```python
line = input_file.readline()
while line != "":
    print(line.rstrip())
    line = input_file.readline()
```

## 写入文件

要向打开的文件写入一行，我们使用文件对象的 `write` 方法。我们传递一个字符串，该字符串会被添加到文件末尾。

为了解决论文格式问题，我们将写入 `word.out`。我们还不准备解决那个问题，所以让我们改写到 `blah.out`。下面是如何向该文件写入一行的示例：

```python
>>> output_file = open('blah.out', 'w')
>>> output_file.write('hello')
5
```

这里的 5 是做什么的？`write` 方法返回写入的字符数。这是一个很好的确认，表明我们写入了预期数量的文本。

如果你在文本编辑器中打开 `blah.out`，你应该在其中看到文本 `hello`。

让我们尝试向文件写入三行。开始：

```python
>>> output_file = open('blah.out', 'w')
>>> output_file.write('sq')
2
>>> output_file.write('ui')
2
>>> output_file.write('sh')
2
```

根据我迄今为止告诉你的内容，你可能会期望 `blah.out` 看起来像这样：

```text
sq
ui
sh
```

但如果你在文本编辑器中打开 `blah.out`，你应该会看到以下内容：

```text
squish
```

字符之所以在一行上，是因为 `write` 不会为我们添加换行符！如果我们想要单独的行，我们需要明确指定，像这样：

```python
>>> output_file = open('blah.out', 'w')
>>> output_file.write('sq\n')
3
>>> output_file.write('ui\n')
3
>>> output_file.write('sh\n')
3
```

注意在每种情况下 `write` 都写入了三个字符，而不是两个。换行符算作一个字符。现在如果你打开 `blah.out`，你应该会看到文本分布在三行上：

与 `print` 不同，`write` 仅在你用字符串调用它时才有效。要向文件写入数字，先将其转换为字符串：

```python
>>> num = 7788
>>> output_file = open('blah.out', 'w')
>>> output_file.write(str(num) + '\n')
5
```

## 关闭文件

完成后关闭文件是良好的实践。它向代码的阅读者表明文件不再被使用。

关闭文件也有助于操作系统管理计算机的资源。当你使用 `write` 方法时，你写入的内容可能不会立即进入文件。相反，Python 或操作系统可能会等待积累一批写入请求，然后一次性写入。关闭你写入的文件可保证你写入文件的内容已安全存储在文件中。

要关闭文件，调用其 `close` 方法。下面是打开文件、读取一行并关闭它的示例：

```python
>>> input_file = open('word.in', 'r')
>>> input_file.readline()
'12 13\n'
>>> input_file.close()
```

一旦你关闭了文件，就不能再从中读取或写入：

```python
>>> input_file.readline()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: I/O operation on closed file.
```

## 解决问题

回到论文格式。现在我们知道如何从 `word.in` 读取并写入 `word.out`。这解决了输入和输出要求。

