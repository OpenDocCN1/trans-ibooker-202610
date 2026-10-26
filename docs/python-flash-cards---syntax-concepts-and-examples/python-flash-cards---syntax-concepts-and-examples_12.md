## 关于函数

- 什么是函数？
- 如何定义函数？
- 如何调用函数？

函数是一段你为其命名的代码块，以便可以重复使用。调用它的名字就会运行整个代码块。

这个单行函数显示一条问候语：

```
def hello():
    """Greet everyone."""
    print("Hello, everyone!")

hello()
```

要创建函数，需要使用关键字 `def` 定义它，后面跟着函数名、一对圆括号，以及末尾的冒号。

下一行是文档字符串：用三引号括起来的对函数的简短描述。函数体包含调用函数时运行的 Python 代码。函数体没有行数限制，并在函数定义下方缩进。

要调用函数，只需使用函数名并加上一对圆括号。以下是调用 `hello()` 函数的输出：

```
Hello, everyone!
```

### 传递实参

- 什么是形参？
- 什么是实参？
- 如何编写一个接收信息的函数？
- 如何向函数传递实参？

形参是函数完成其工作所需的信息。实参是传递给函数的值。

要定义函数的形参，需要将它们放在函数定义的圆括号内。这会创建一个需要 `name` 数据的 `hello()` 函数：

```
def hello(name):
    """Greet someone by name."""
    print(f"Hello, {name}!")
```

要运行此函数，需要一个 `name` 实参，它会存储在 `name` 变量中，并在调用 `print()` 时使用。在调用函数时，通过在圆括号中包含一个值来向函数传递实参：

```
hello("Gretchen")
```

实参为形参 `name` 提供了一个值，使 `hello()` 函数能够生成个性化的问候语：

```
Hello, Gretchen!
```

有时形参和实参可以互换使用。

### 位置实参

- 什么是位置实参？
- 如何在函数调用中使用位置实参？
- 如果位置实参的顺序错误会发生什么？

当函数需要多个值时，它必须将接收到的值与形参匹配。使用*位置*实参时，函数会将接收到的第一个值与第一个形参匹配，以此类推。

这个函数接收一条消息和一个用户名，然后发布一条消息摘要：

```
def post_msg(msg, user):
    """Post a message from a user."""
    print(f'{user}: {msg}')

post_msg("I love the internet!", "Willie")
post_msg("Me too!", "Ever")
```

```
Willie: I love the internet!
Ever: Me too!
```

实参的顺序必须与函数定义中形参的顺序一致，否则输出将不具意义：

```
post_msg("Willie", "I love the internet!")
```

这会交换用户名和消息值：

```
I love the internet!: Willie
```

### 任意位置实参

- 如何编写一个接受未知数量实参的函数？
- 如何调用一个接受任意数量实参的函数？

在形参名前面加一个星号（`*`），可以让函数接收任意数量的位置实参。当你不知道函数会接收多少个实参时，可以使用它。

这个函数接收一个用户名以及用户发布的任意数量的消息。这些实参构成一个元组：

```
def post_messages(user, *msgs):
    """Post all of a user's messages."""
    print(f"{user} said:")
    for msg in msgs:
        print(f"  {msg}")

post_messages('Ahna', "I love mountains.",
    "I love rivers.",
    "I love the ocean.")
```

```
Ahna said:
  I love mountains.
  I love rivers.
  I love the ocean.
```

一个函数只能有一个形参用于收集任意数量的位置实参。

### 关键字实参

*关键字实参*指定值将被赋给哪个形参，因此你可以按任意顺序传递实参。

在函数调用中，包含形参名以及要赋给该形参的值：

```
def post_msg(msg, user):
    """Post a message from a user."""
    print(f'{user}: {msg}')

post_msg(user="Willie",
    msg="I love the internet!")
```

Python 会将值 `"Willie"` 赋给形参 `user`，将值 `"I love the internet!"` 赋给形参 `msg`：

```
Willie: I love the internet!
```

函数调用中关键字实参的顺序无关紧要。

### 任意关键字实参

- 如何编写一个接收任意数量关键字实参的函数？
- 如何调用一个带任意数量关键字实参的函数？

在形参前放置双星号（`**`），可以让函数接收任意数量的关键字实参。当你不知道函数会接收哪种信息时，可以使用它。

带 `**` 的形参会从函数调用中收集任何剩余的键值对，并将它们添加到字典中。这个函数接受任意数量的用户信息：

```
def desc_user(user, **desc):
    """Describe a user."""
    print(f"Description of {user}:")
    for key, value in desc.items():
        print(f"  {key}: {value}")

desc_user('anton', active=True,
    email='anton@example.com',
    num_posts=578)
```

第一个实参必须是用户名。然后任何剩余的关键字实参都会被打包到 `desc` 字典中：

```
Description of anton:
active: True
email: anton@example.com
num_posts: 578
```

每个函数只能有一个形参用于收集任意数量的关键字实参。

### 默认值

- 如何为形参指定默认值？
- 如何调用一个具有默认形参值的函数？





## 

这个函数提供一杯咖啡。你可以指定杯子的大小，如果不指定，你会得到一杯 12 盎司的咖啡：

```python
def coffee(size=12):
    """Serve a cup of coffee."""
    print(f"Here's your {size} oz coffee!")

coffee()
coffee(16)
```

这里我们得到一杯 12 盎司的咖啡和一杯 16 盎司的咖啡：

```text
Here's your 12 oz coffee!
Here's your 16 oz coffee!
```

默认值很有用，因为它们使某些参数成为可选的。
