

follows:

```python
def pr_named_vals( **kwargs ):
    for k in kwargs:
        print(k, ':', kwargs[k])
```

该函数遍历由 **kwargs** 表示的字典，打印键值（对应参数名）以及传递给参数的相应值。

例如：

```python
>>> pr_named_vals(a=10, b=20, c=30)
a : 10
b : 20
c : 30
```

函数定义可以将任意数量的命名参数（即 **kwargs**）与任意数量的未命名参数（即 **args**）结合起来。下面是一个正是这样做的函数定义。以下示例定义了这样的函数，然后调用它。

```python
def pr_vals_2( *args, **kwargs ):
    for i in args:
        print(i)
    for k in kwargs:
        print(k, ':', kwargs[k])
pr_vals_2(1, 2, 3, -4, a=100, b=200)
```

这个小程序作为脚本运行时，将打印以下内容：

```
1
2
3
-4
a : 100
b : 200
```

> 注意
> 虽然 args 和 kwargs 分别被展开为列表和字典，
> 但这些符号可以传递给另一个函数，如下一节所示。

# 4.9 装饰器和函数分析器

当你开始改进 Python 程序时，最有用的事情之一就是计时单个函数的运行速度。你可能想知道，当程序执行一个生成一千个随机数的函数时，会经过多少秒和秒的分数。

装饰后的函数可以分析代码的运行速度，并提供其他信息，因为函数是一等对象。装饰概念的核心是包装函数，它执行原函数的所有操作，但还会添加其他要执行的语句。

下面是一个示例，如图 4.3 所示。装饰器以函数 F1 作为输入，并返回另一个函数 F2 作为输出。第二个函数 F2 是通过包含对 F1 的调用并添加其他语句生成的。F2 是一个包装函数。

![](img/6e45221a8147f2442c653a3d4a866570_264_0.png)

下面是一个装饰器函数的示例，它将函数作为参数，并通过添加对 time.time 函数的调用来包装它。请注意，time 是一个包，必须在调用 time.time 之前导入它。

```python
import time

def make_timer(func):
    def wrapper():
        t1 = time.time()
        ret_val = func()
        t2 = time.time()
        print('Time elapsed was', t2 - t1)
        return ret_val
```

这个简单示例涉及几个函数（顺便说一句，它还不完整！），让我们回顾一下。

- 有一个函数作为输入；我们将其称为 _原始_ 函数（本例中为 F1）。我们希望能够输入任何想要的函数，并对其进行装饰——即获取一些额外的语句。
- _包装_ 函数是将这些额外语句添加到原始函数的结果。在本例中，这些添加的语句报告原始函数执行所花费的秒数。
- _装饰器_ 是执行创建包装函数并返回它的工作的函数。装饰器能够做到这一点，是因为它在内部使用 **def** 关键字来定义一个新函数。
- 最终，包装后的版本旨在替换原始版本，如你将在本节中看到。这是通过重新分配函数名来完成的。

如果你查看这个装饰器函数，应该注意到它有一个重要的遗漏：原始函数 _func_ 的参数被忽略了。因此，包装函数如果涉及参数，将无法正确调用 _func_。

解决方案涉及上一节中引入的 **\*args** 和 **\*\*kwargs** 语言特性。下面是完整的装饰器：

```python
import time

def make_timer(func):
    def wrapper(*args, **kwargs):
        t1 = time.time()
        ret_val = func(*args, **kwargs)
        t2 = time.time()
        print('Time elapsed was', t2 - t1)
        return ret_val
    return wrapper
```

请记住，新函数将是 wrapper。正是 wrapper（或者更准确地说，暂时命名为 wrapper 的函数）最终将替代 func 被调用；因此，这个包装函数必须能够接受任意数量的参数，包括任意数量的关键字参数。正确的做法是将所有这些参数传递给原始函数 func。具体做法如下：

```python
ret_val = func(*args, **kwargs)
```

返回值也在此处处理；包装函数返回与 func 相同的值，正如它应该做的那样。如果 func 不返回值怎么办？这不是问题，因为 Python 函数默认返回 None。因此，在这种情况下，值 None 会被简单地传递。（你不需要测试返回值是否存在；总是存在一个！）

定义了这个装饰器 make_timer 后，我们可以对任何函数生成一个包装后的版本。然后——这几乎是最后的技巧——我们重新分配函数名，使其指向函数的包装版本。

```python
def count_nums(n):
    for i in range(n):
        for j in range(1000):
            pass

count_nums = make_timer(count_nums)
```

由 `make_timer` 生成的包装函数定义如下（除了标识符 `func` 将被重新分配，如你稍后将看到）。

```python
def wrapper(*args, **kwargs):
    t1 = time.time()
    ret_val = func(*args, **kwargs)
    t2 = time.time()
    print('Time elapsed was', t2 - t1)
    return ret_val
```

我们现在重新分配名称 `count_nums`，使其指向 *这个* 函数——`wrapper`——它将调用原始的 `count_nums` 函数，但还会做其他事情。

感到困惑了吗？诚然，起初这有点绕脑子。但实际发生的事情是：(1) 在运行时创建原始函数的一个更复杂的版本，以及 (2) 这个更复杂的版本就是名称 `count_nums` 今后将指向的对象。Python 符号可以引用任何对象，包括函数（可调用对象）。因此，我们可以随意重新分配函数名。

```python
count_nums = wrapper
```

或者，更准确地说，

```python
count_nums = make_timer(count_nums)
```

所以现在，当你运行 `count_nums`（它现在指向函数的包装版本）时，你会得到类似以下的输出，报告以秒为单位的执行时间。

```python
>>> count_nums(33000)
Time elapsed was 1.063697338104248
```

`count_nums` 的原始版本除了进行一些计数之外什么也不做；这个包装版本除了调用 `count_nums` 的原始版本之外，还会报告时间的流逝。

作为最后一步，Python 提供了一小段方便的语法来自动化函数名的重新分配。

![](img/6e45221a8147f2442c653a3d4a866570_268_0.png)

```python
@decorator
def func(args):
    statements
```

此语法被翻译为以下内容：

```python
def func(args):
    statements
    func = decorator(func)
```

在任一情况下，都假设 _decorator_ 是一个已经定义的函数。这个装饰器必须将函数作为其参数，并返回函数的包装版本。假设所有这些都已正确完成，下面是一个使用 @ 符号的完整示例。

```python
@make_timer
def count_nums(n):
    for i in range(n):
        for j in range(1000):
            pass
```

在 Python 执行此定义后，_count_num_ 可以被调用，它将按定义执行 _count_num_，但还会添加（作为包装的一部分）一个 **print** 语句，告诉经过的秒数。

请记住，这个技巧的部分（实际上是最后的技巧）是让名称 _count_nums_ 指向 _count_nums_ 的 _新_ 版本，在通过装饰过程添加了新语句之后。

# 4.10 生成器

关于 Python 的主题，没有哪个比生成器更容易引起困惑的了。

