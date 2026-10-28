## 2 VARIABLES AND SIMPLE DATA TYPES

![](img/8f15563c16d2a98c4d3115150679b092_68_0.png)

In this chapter you’ll learn about the different kinds of data you can work with in your Python programs. You’ll also learn how to use variables to represent data in your programs.

### What Really Happens When You Run hello_world.py

Let’s take a closer look at what Python does when you run *hello_world.py*. As it turns out, Python does a fair amount of work, even when it runs a simple program:

***hello_world.py***

```python
print("Hello Python world!")
```

When you run this code, you should see the following output:

```text
Hello Python world!
```

When you run the file *hello_world.py*, the ending *.py* indicates that the file is a Python program. Your editor then runs the file through the *Python interpreter*, which reads through the program and determines what each word in the program means. For example, when the interpreter sees the word `print` followed by parentheses, it prints to the screen whatever is inside the parentheses.

As you write your programs, your editor highlights different parts of your program in different ways. For example, it recognizes that `print()` is the name of a function and displays that word in one color. It recognizes that “Hello Python world!” is not Python code, and displays that phrase in a different color. This feature is called *syntax highlighting* and is quite useful as you start to write your own programs.

# Variables

Let’s try using a variable in *hello_world.py*. Add a new line at the beginning of the file, and modify the second line:

*hello_world.py*

```python
message = "Hello Python world!"
print(message)
```

Run this program to see what happens. You should see the same output you saw previously:

```text
Hello Python world!
```

We’ve added a *variable* named `message`. Every variable is connected to a *value*, which is the information associated with that variable. In this case the value is the "Hello Python world!" text.

Adding a variable makes a little more work for the Python interpreter. When it processes the first line, it associates the variable `message` with the "Hello Python world!" text. When it reaches the second line, it prints the value associated with `message` to the screen.

Let’s expand on this program by modifying *hello_world.py* to print a second message. Add a blank line to *hello_world.py*, and then add two new lines of code:

```
message = "Hello Python world!"
print(message)

message = "Hello Python Crash Course world!"
print(message)
```

Now when you run *hello_world.py*, you should see two lines of output:

```
Hello Python world!
Hello Python Crash Course world!
```

You can change the value of a variable in your program at any time, and Python will always keep track of its current value.

### Naming and Using Variables

When you’re using variables in Python, you need to adhere to a few rules and guidelines. Breaking some of these rules will cause errors; other guidelines just help you write code that’s easier to read and understand. Be sure to keep the following rules in mind when working with variables:

## Avoiding Name Errors When Using Variables

Every programmer makes mistakes, and most make mistakes every day. Although good programmers might create errors, they also know how to respond to those errors efficiently. Let's look at an error you're likely to make early on and learn how to fix it.

We'll write some code that generates an error on purpose. Enter the following code, including the misspelled word message, which is shown in bold:

```
message = "Hello Python Crash Course reader!"
print(message)
```

When an error occurs in your program, the Python interpreter does its best to help you figure out where the problem is. The interpreter provides a traceback when a program cannot run successfully. A traceback is a record of where the interpreter ran into trouble when trying to execute your code. Here's an example of the traceback that Python provides after you've accidentally misspelled a variable's name:

```
Traceback (most recent call last):
  File "hello_world.py", line 2, in <module>
    print(message)
          ^^^^^^^
NameError: name 'message' is not defined. Did you mean: 'mess age'?
```

The output reports that an error occurs in line 2 of the file *hello_world.py* ❶. The interpreter shows this line ❷ to help us spot the error quickly and tells us what kind of error it found ❸. In this case it found a *name error* and reports that the variable being printed, `message`, has not been defined. Python can't identify the variable name provided. A name error usually means we either forgot to set a variable's

value before using it, or we made a spelling mistake when entering the variable's name. If Python finds a variable name that's similar to the one it doesn't recognize, it will ask if that's the name you meant to use.

In this example we omitted the letter s in the variable name message in the second line. The Python interpreter doesn't spellcheck your code, but it does ensure that variable names are spelled consistently. For example, watch what happens when we spell *message* incorrectly in the line that defines the variable:

```
message = "Hello Python Crash Course reader!"
print(message)
```

In this case, the program runs successfully!

```
Hello Python Crash Course reader!
```

The variable names match, so Python sees no issue. Programming languages are strict, but they disregard good and bad spelling. As a result, you don't need to consider English spelling and grammar rules when you're trying to create variable names and writing code.

Many programming errors are simple, single-character typos in one line of a program. If you find yourself spending a long time searching for one of these errors, know that you're in good company. Many experienced and talented programmers spend hours hunting down these kinds of tiny errors. Try to laugh about it and move on, knowing it will happen frequently throughout your programming life.

### **Variables Are Labels**

Variables are often described as boxes you can store values in. This idea can be helpful the first few times you use a variable, but it isn't an accurate way to describe how

Because most programs define and gather some sort of data and then do something useful with it, it helps to classify different types of data. The first data type we’ll look at is the string. Strings are quite simple at first glance, but you can use them in many different ways.

A *string* is a series of characters. Anything inside quotes is considered a string in Python, and you can use single or double quotes around your strings like this:

```python
"This is a string."
'This is also a string.'
```

This flexibility allows you to use quotes and apostrophes within your strings:

```python
'I told my friend, "Python is my favorite language!"'
"The language 'Python' is named after Monty Python, not the snake."
"One of Python's strengths is its diverse and supportive community."
```

Let’s explore some of the ways you can use strings.

### Changing Case in a String with Methods

One of the simplest tasks you can do with strings is change the case of the words in a string. Look at the following code, and try to determine what’s happening:

**name.py**

```python
name = "ada lovelace"
print(name.title())
```

Save this file as *name.py* and then run it. You should see this output:

```text
Ada Lovelace
```





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


```python
File "apostrophe.py", line 1
    message = 'One of Python's strengths is its diverse community.'
              ^
SyntaxError: unterminated string literal (detected at line 1)
```

在输出中你可以看到错误发生在最后一个单引号之后❶。这个语法错误表明解释器不认为代码中的某些内容是有效的 Python 代码，并且它认为问题可能是一个引号不正确且未闭合的字符串。错误可能来自多种来源，我会在出现时指出一些常见问题。在你学习编写正确的 Python 代码时，你可能会经常看到语法错误。语法错误也是最不具体的错误类型，因此识别和纠正起来可能很困难且令人沮丧。如果你卡在一个特别顽固的错误上，请参阅附录 C 中的建议。

> **NOTE**
>
> *你的编辑器的语法高亮功能应该能帮助你在编写程序时快速发现一些语法错误。如果你看到 Python 代码被高亮得像英语一样，或者英语被高亮得像 Python 代码一样，你很可能在文件中的某个地方有不匹配的引号。*

## Try It Yourself

将以下每个练习保存为单独的文件，文件名类似 *name_cases.py*。如果卡住了，请休息一下或参阅附录 C 中的建议。

- **2-3. Personal Message:** 使用一个变量来表示一个人的名字，并向那个人打印一条消息。你的消息应很简单，例如，“Hello Eric, would you like to learn some Python today?”
- **2-4. Name Cases:** 使用一个变量来表示一个人的名字，然后以小写、大写和标题格式打印该人的名字。
- **2-5. Famous Quote:** 找一句你钦佩的名人说过的话。打印这句话和作者的名字。你的输出应看起来像以下这样，包括引号：

  > *Albert Einstein once said, “A person who never made a mistake never tried anything new.”*

- **2-6. Famous Quote 2:** 重复练习 2-5，但这次使用名为 `famous_person` 的变量来表示名人的名字。然后组合你的消息并用名为 `message` 的新变量表示它。打印你的消息。
- **2-7. Stripping Names:** 使用一个变量来表示一个人的名字，并在名字的开头和结尾包含一些空白字符。确保至少使用一次每个字符组合，`\t` 和 `\n`。

  打印一次名字，以便显示名字周围的空白。然后使用三个去除函数 `lstrip()`、`rstrip()` 和 `strip()` 打印名字。
- **2-8. File Extensions:** Python 有一个 `removesuffix()` 方法，其工作原理与 `removeprefix()` 完全相同。将值 `'python_notes.txt'` 分配给名为 `filename` 的变量。然后使用 `removesuffix()` 方法显示不带文件扩展名的文件名，就像某些文件浏览器那样。

## Numbers

数字在编程中经常使用，用来在游戏中计分、在可视化中表示数据、在 Web 应用程序中存储信息等等。Python 根据它们的使用方式以几种不同的方式处理数字，具体取决于它们的使用方式

这些示例中的间距不会影响 Python 对表达式的求值方式；它只是帮助你在阅读代码时更快地发现具有优先级的运算。

### Floats

Python 将任何带小数点的数字称为 *float*。这个术语在大多数编程语言中使用，它指的是小数点可以出现在数字的任何位置。每种编程语言都必须精心设计，以正确管理十进制数字，以便无论小数点出现在哪里，数字都能适当表现。

在大多数情况下，你可以使用浮点数而无需担心它们的行为。只需输入你想要使用的数字，Python 很可能会按你期望的方式执行：

```
>>> 0.1 + 0.1
0.2
>>> 0.2 + 0.2
0.4
>>> 2 * 0.1
0.2
>>> 2 * 0.2
0.4
```

然而，请注意，有时你的答案中会出现任意数量的小数位：

```
>>> 0.2 + 0.1
0.30000000000000004
>>> 3 * 0.1
0.30000000000000004
```

这在所有语言中都会发生，问题不大。Python 试图尽可能精确地表示结果，这有时很困难，因为计算机必须在内部以特定方式表示数字。现在只需忽略这些额外的小数位；当你在第二部分项目中需要时，你将学习如何处理这些额外位数。

### Integers and Floats

当你除以任意两个数字时，即使它们是结果为整数的整数，你也总是会得到一个浮点数：

```
>>> 4/2
2.0
```

如果你在任何其他运算中混合使用整数和浮点数，你也会得到浮点数：

```
>>> 1 + 2.0
3.0
>>> 2 * 3.0
6.0
>>> 3.0 ** 2
9.0
```

Python 在使用浮点数的任何运算中默认使用浮点数，即使输出是整数。

### Underscores in Numbers

当你编写长数字时，可以使用下划线对数字进行分组，以使大数字更易读：

```
>>> universe_age = 14_000_000_000
```

当你打印使用下划线定义的数字时，Python 只打印数字：

```
>>> print(universe_age)
14000000000
```

## Try It Yourself

- **2-9. Number Eight:** 编写加法、减法、乘法和除法运算，使其结果均为数字 8。确保将你的运算包含在 `print()` 调用中以查看结果。你应该创建四行，看起来像这样：

```
print(5+3)
```

你的输出应为四行，每行出现一次数字 8。

- **2-10. Favorite Number:** 使用一个变量来表示你的最喜欢的数字。然后，使用该变量创建一个揭示你最喜欢的数字的消息。打印该消息。

## Comments

注释是在大多数编程语言中非常有用的功能。到目前为止，你在程序中编写的所有内容都是 Python 代码。随着程序变得更长、更复杂，你应该在程序中添加注释，描述你解决问题的整体方法。*注释* 允许你在程序中用你所说的语言写下笔记。

### How Do You Write Comments?

在 Python 中，井号（#）表示注释。在代码中井号后面的任何内容都会被 Python 解释器忽略。例如：

`comment.py`

```python
# Say hello to everyone.
print("Hello Python people!")
```

Python 忽略第一行并执行第二行。

### What Kinds of Comments Should You Write?

编写注释的主要原因是解释你的代码 supposed to do 以及你如何使其工作。当你在项目中间工作时，你了解所有部分如何组合在一起。但当你在一段时间后返回项目时，你可能会忘记一些细节。你总可以研究你的代码一段时间并弄清楚各段 supposed to 如何工作，但编写良好的注释可以通过清晰地总结你的整体方法为你节省时间。

如果你想成为一名专业程序员或与其他程序员合作，你应该编写有意义的注释。如今，大多数软件都是协作编写的，无论是由一家公司的员工团队还是在开源项目上一起工作的团队。熟练的程序员期望在代码中看到注释，因此最好现在就开始为你的程序添加描述性注释。在代码中编写清晰简洁的注释是你作为新程序员可以养成的最有益的习惯之一。

当你决定是否要编写注释时，问问自己是否在想出合理的方法使某事工作之前考虑了几种方法；如果是这样，就写下关于你的解决方案的注释。以后删除多余的注释要比返回为稀疏注释的程序编写注释容易得多。从现在开始，我将在本书中的示例中使用注释来帮助解释代码段。



无论是改进那个项目的代码，还是继续做一些新的事情。

当你继续学习下一章并开始深入研究更复杂的主题时，请牢记这种简单清晰的理念。经验丰富的程序员会更加尊重你的代码，并乐于为你提供反馈，与你合作完成有趣的项目。

> **动手试一试**
> 
> **2-12. Python 之禅：** 在 Python 终端会话中输入 `import this`，并浏览其中其他原则。

## 本章小结

在本章中，你学习了如何使用变量。你学会了使用描述性的变量名，并在出现名称错误和语法错误时解决它们。你了解了什么是字符串，以及如何使用小写、大写和首字母大写形式来显示它们。你开始使用空白来整齐地组织输出，并学会了如何从字符串中移除不需要的元素。你开始使用整数和浮点数，并学习了处理数值数据的一些方法。你还学会了编写解释性注释，使你的代码更容易让你和他人阅读。最后，你了解了一种理念：尽可能让代码保持简单。

在第 3 章中，你将学习如何将信息集合存储到名为*列表*的数据结构中。你还将学习如何遍历列表，并操作列表中的任何信息。
