

在本例中，变量 `name` 指向小写字符串 "ada lovelace"。方法 `title()` 出现在 `print()` 调用中的变量之后。*方法*是 Python 可以对数据执行的操作。在 `name.title()` 中，`name` 后面的点（`.`）告诉 Python 让 `title()` 方法作用于变量 `name`。每个方法后面都跟着一对圆括号，因为方法通常需要额外信息来完成工作。这些信息就提供在圆括号内。`title()` 方法不需要任何额外信息，所以它的圆括号是空的。

`title()` 方法会将每个单词转换为标题大小写，即每个单词以大写字母开头。这很有用，因为你经常想把名字视为一条信息。例如，你可能希望程序将输入值 `Ada`、`ADA` 和 `ada` 视为同一个名字，并将它们全部显示为 `Ada`。

另外，还有一些有用的方法可用于处理大小写。例如，你可以像下面这样将字符串全部改为大写或全部改为小写：

```python
name = "Ada Lovelace"
print(name.upper())
print(name.lower())
```

这将显示以下内容：

```text
ADA LOVELACE
ada lovelace
```

`lower()` 方法对于存储数据尤其有用。你通常不会信任用户提供的大小写形式，所以在存储字符串之前会将其转换为小写。之后当你想显示这些信息时，再使用对每个字符串最有意义的大小写形式。

## 在字符串中使用变量

在某些情况下，你会希望把变量的值用在字符串中。例如，你可能想分别用两个变量表示名字和姓氏，然后将这些值组合起来显示某人的全名：

`full_name.py`

```python
first_name = "ada"
last_name = "lovelace"
full_name = f"{first_name} {last_name}"
print(full_name)
```

要将变量的值插入字符串，请在开始引号前紧挨着放置字母 f①。将你想在字符串中使用的任意变量的名称用花括号括起来。当字符串显示时，Python 会将每个变量替换为其值。

这些字符串称为 f-字符串。f 代表 format（格式），因为 Python 通过将花括号中任何变量的名称替换为其值来格式化字符串。上面代码的输出是：

```text
ada lovelace
```

你可以用 f-字符串做很多事情。例如，你可以使用 f-字符串，利用与变量关联的信息来构造完整消息，如下所示：

```python
first_name = "ada"
last_name = "lovelace"
full_name = f"{first_name} {last_name}"
print(f"Hello, {full_name.title()}!")
```

全名用于问候用户的句子中①，并且 title() 方法将名字转换为标题大小写。

这段代码返回一个简单但格式友好的问候：

```text
Hello, Ada Lovelace!
```

你也可以使用 f-字符串构造一条消息，然后将整条消息赋给一个变量：

```python
first_name = "ada"
last_name = "lovelace"
full_name = f"{first_name} {last_name}"
message = f"Hello, {full_name.title()}!"  # ①
print(message)  # ②
```

这段代码同样显示消息 Hello, Ada Lovelace!，但通过将消息赋给变量①，最终的 print() 调用会简单得多②。

## 使用制表符或换行符向字符串添加空白

在编程中，空白指任何非打印字符，例如空格、制表符和行尾符号。你可以使用空白组织输出，让用户更容易阅读。

要在文本中添加制表符，请使用字符组合 \t：

```python
>>> print("Python")
Python
>>> print("\tPython")
    Python
```

要在字符串中添加换行符，请使用字符组合 \n：

```python
>>> print("Languages:\nPython\nC\nJavaScript")
Languages:
Python
C
JavaScript
```

你也可以在单个字符串中组合制表符和换行符。字符串"\n\t"告诉 Python 换到新行，并用制表符开始下一行。下面的示例展示了如何使用单行字符串生成四行输出：

```python
>>> print("Languages:\n\tPython\n\tC\n\tJavaScript")
Languages:
    Python
    C
    JavaScript
```

在接下来的两章中，当你开始用几行代码生成多行输出时，换行符和制表符会非常有用。

## 剥离空白

程序中的多余空白可能会造成混淆。对程序员来说，'python' 和 'python ' 看起来几乎一样。但对程序来说，它们是两个不同的字符串。Python 会检测到 'python ' 中的多余空格，并认为它有意义，除非你告诉它不要这样。

考虑空白非常重要，因为你经常需要比较两个字符串以确定它们是否相同。例如，一个重要的例子可能是检查用户登录网站时的用户名。在简单得多的情况下，多余空白也可能造成混淆。幸运的是，Python 可以轻松地消除人们输入数据中的多余空白。

Python 可以查找字符串右侧和左侧的多余空白。要确保字符串右侧不存在空白，请使用 rstrip() 方法：

```python
>>> favorite_language = 'python '
>>> favorite_language
'python '
>>> favorite_language.rstrip()
'python'
>>> favorite_language
'python '
```

与 `favorite_language` 关联的值❶在字符串末尾包含多余空白。在终端会话中向 Python 询问这个值时，你会看到值末尾的空格❷。当 `rstrip()` 方法作用于变量 `favorite_language` ❸时，这个多余空格会被删除。但是，它只是被暂时删除。如果你再次询问 `favorite_language` 的值，字符串看起来与输入时一样，包括多余空白❹。

要从字符串中永久删除空白，你必须将剥离后的值与变量名关联：

```python
>>> favorite_language = 'python '
>>> favorite_language = favorite_language.rstrip()
>>> favorite_language
'python'
```

要从字符串中删除空白，你需要从字符串右侧剥离空白，然后将这个新值与原始变量关联❶。在编程中，更改变量值的操作非常常见。这就是变量值在程序执行期间或响应用户输入时被更新的方式。

你也可以使用 `lstrip()` 方法剥离字符串左侧的空白，或使用 `strip()` 同时剥离两侧的空白：

```python
>>> favorite_language = ' python '  # ①
>>> favorite_language.rstrip()  # ②
' python'
>>> favorite_language.lstrip()  # ③
'python '
>>> favorite_language.strip()  # ④
'python'
```

在这个示例中，我们从开头和结尾都有空白的值①开始。然后我们分别从右侧②、左侧③以及两侧④删除多余空格。尝试这些剥离函数可以帮助你熟悉字符串操作。在现实中，这些剥离函数最常用于在用户输入存储到程序之前清理输入。

## 删除前缀

在处理字符串时，另一个常见任务是删除前缀。考虑一个带有常见前缀 *https://* 的 URL。我们希望删除这个前缀，从而专注于用户需要在地址栏中输入的 URL 部分。操作方法如下：

```python
>>> nostarch_url = 'https://nostarch.com'
>>> nostarch_url.removeprefix('https://')
'nostarch.com'
```

输入变量名，后跟一个点，然后是方法 `removeprefix()`。在圆括号内，输入要从原始字符串中删除的前缀。

与删除空白的方法一样，`removeprefix()` 会保持原始字符串不变。如果你想保留删除前缀后的新值，可以将其重新赋给原始变量，或者赋给一个新变量：

```python
>>> simple_url = nostarch_url.removeprefix('https://')
```

