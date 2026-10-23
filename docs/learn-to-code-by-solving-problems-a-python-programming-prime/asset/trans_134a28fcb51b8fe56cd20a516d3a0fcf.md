

函数对于代码的组织性和清晰度至关重要。所有程序员都会使用它们。如果没有函数，编写像游戏和文字处理器这样的大型软件系统将变得不可行。让我们来学习如何使用函数。

## 定义和调用函数

我们已经学习了如何调用 Python 自带的函数。例如，我们使用了 input 函数来读取输入。以下是一个没有参数的 input 调用示例：

```
```python
>>> s = input()
hello
>>> s
'hello'
```
```

我们还使用了 Python 的 print 函数来输出文本。以下是一个带有一个参数的 print 调用示例：

```
```python
>>> print('well, well')
well, well
```
```

Python 的内置函数是通用的，旨在用于各种广泛的环境。当我们希望一个函数解决特定任务时，我们就必须自己定义它。

### 无参函数

为了*定义*或创建一个函数，我们使用 Python 的 def 关键字。以下是一个输出三行内容的函数的定义：

```
```python
>>> def intro():
...     print('**********')
...     print('*WELCOME*')
...     print('**********')
...
```
```

函数定义的结构与 if 语句或循环的结构类似。def 后面的名称是我们正在定义的函数的名称；在这里，我们定义了一个名为 intro 的函数。在函数名称之后，有一对空括号 ()。稍后我们会看到，可以在这些括号中包含信息以向函数传递参数。这个 intro 函数不接收任何参数，这就是为什么括号是空的。括号后面是一个冒号；与 if 语句或循环一样，省略冒号会导致语法错误。在接下来的几行中，我们提供了一个缩进的语句块，该语句块将在每次调用函数时执行。

当你定义 intro 函数时，你可能期望看到以下输出：

```
**********
*WELCOME*
**********
```

但并没有：到目前为止，我们只是定义了该函数，并未调用它。定义函数没有可观察到的效果；它只是将函数存储在计算机内存中，以便我们稍后调用。我们调用自己定义的函数，就像调用 Python 的任何内置函数一样。由于这个 intro 函数不接收任何参数，我们在调用时使用一组空括号：

```
>>> intro()
**********
*WELCOME*
**********
```

你可以随意调用这个函数多次。只要我们需要，它就在那里。

## 带参数的函数

我们的 intro 函数不太灵活，因为它每次被调用时都执行相同的操作。我们可以修改函数，使其能够接收参数，并且传递的参数可以影响函数的行为。以下是 intro 函数的一个新版本，它允许我们传递单个参数：

```
>>> def intro2(message):
...     line_length = len(message) + 2
...     print('*' * line_length)
...     print(f'* {message} *')
...     print('*' * line_length)
...
```

要调用这个函数，我们需要提供一个字符串参数：

```
>>> intro2('HELLO')
*******
*HELLO*
*******
>>> intro2('WIN')
*****
*WIN*
*****
```

如果我们不传参数就无法调用这个 intro2 函数——如果我们尝试这样做，会得到一个错误：

```
>>> intro2()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: intro2() missing 1 required positional argument: 'message'
```

该错误提醒我们，我们没有为 message 提供参数。名称 message 被称为函数参数。当我们调用 intro2 时，Python 首先让 message 指向我们的参数所指向的内容；也就是说，message 成为我们参数的别名。

我们可以创建具有多个参数的函数。以下是一个接受两个参数的函数：一个要打印的信息和打印的次数：

```
>>> def intro3(message, num_times):
...     for i in range(num_times):
...         print(message)
...
```

要调用这个函数，我们需要提供两个参数。Python 从左到右处理，将第一个参数赋值给第一个参数名，将第二个参数赋值给第二个参数名。在下一次调用中，'high' 被赋值给 message 参数，5 被赋值给 num_times 参数：

```
>>> intro3('high', 5)
high
high
high
high
high
```

请务必提供正确数量的参数。对于 intro3，我们需要两个参数。其他任何情况都是错误：

```
>>> intro3()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: intro3() missing 2 required positional arguments: 'message' and 'num_times'
>>> intro3('high')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: intro3() missing 1 required positional argument: 'num_times'
```

我们还必须确保提供正确类型的值。错误类型不会阻止我们调用函数，但会导致函数内部出现错误：

```
>>> intro3('high', 'low')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in intro3
TypeError: 'str' object cannot be interpreted as an integer
```

这个 TypeError 出现是因为 intro3 使用了一个基于变量 num_times 的 range for 循环。如果为 num_times 提供的参数不是整数，range for 循环就会失败。

## 关键字参数

在调用函数时，可以覆盖参数与参数名之间的从左到右的对应关系。要做到这一点，我们可以按照自己喜欢的顺序使用参数名。使用参数名的参数称为关键字参数。以下是其工作方式：

```
>>> def intro3(message, num_times):
...   for i in range(num_times):
...     print(message)
...
>>> intro3(message='high', num_times=3)
high
high
high
>>> intro3(num_times=3, message='high')
high
high
high
```

这里的每个函数调用都使用了两个关键字参数。关键字参数由参数名、等号及其对应的参数组成。

你甚至可以以常规参数开始，并以关键字参数结束：

```
>>> intro3('high', num_times=3)
high
high
high
```

但一旦你使用了关键字参数，就不能再回到常规参数：

```
>>> intro3(message='high', 3)
  File "<stdin>", line 1
SyntaxError: positional argument follows keyword argument
```

在第 5 章的“排序列表”部分，我们在调用 sort 方法时使用了一个 reverse 关键字参数。Python 设计者决定 reverse 应该是一个仅关键字参数，这意味着如果不使用关键字参数，就无法填入它的值。Python 也允许我们在自己的函数中做到这一点，但在这本书中我们不需要这种程度的控制。

## 局部变量

参数名像常规变量一样工作，但仅限于定义它们的函数内部。也就是说，函数参数在其函数外部不存在：

```
>>> def intro2(message):
...   line_length = len(message) + 2
...   print('*' * line_length)
...   print(f'*{message}*')
...   print('*' * line_length)
>>> intro2('hello')
*******
*hello*
*******
>>> message
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'message' is not defined
```

那么 line_length 变量呢——它也是局部的吗？是的：

```
>>> line_length
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
NameError: name 'line_length' is not defined
```

如果你有一个变量，然后调用一个使用同名参数或局部变量的函数，会发生什么？你的值会丢失吗？让我们看看：

```
>>> line_length = 999
>>> intro2('hello')
*******
*hello*
*******
>>> line_length
999
```

松了一口气——它仍然是 999，正如我们离开时那样。局部变量在函数被调用时创建，在函数终止时销毁，这一切都不会影响其他同名的变量。

一个函数可以访问在该函数外部创建的变量。尽管不建议依赖于此，因为这会使该函数不再自包含，而是期望它需要的变量实际存在。在这本书中，我们将编写仅使用局部变量的函数。函数所需的所有信息将通过其参数提供给函数。

## 可变参数

由于参数是其对应参数的别名，因此它可以用于修改变量值。以下是一个从列表 lst 中移除所有 value 出现的函数：

```
>>> def remove_all(lst, value):
...     while value in lst:
...         lst.remove(value)
...
>>> lst = [5, 10, 20, 5, 45, 5, 9]
>>> remove_all(lst, 5)
>>> lst
[10, 20, 45, 9]
```

