
## 注释放开的代码和死代码

临时措施下注释掉代码使其不运行是可以的。你可能想跳过某些行来测试其他功能，而注释它们可以让你以后轻松地加回来。但如果注释掉的代码仍然保留在原位，就完全不清楚为什么要移除它，以及在什么条件下它可能再次需要。请考虑以下示例：

```
doSomething()
#doAnotherThing()
doSomeImportantTask()
doAnotherThing()
```

这段代码引发了许多未解答的问题：为什么 doAnotherThing() 被注释掉了？我们还会再次包含它吗？为什么第二次调用 doAnotherThing() 没有被注释掉？原本是否有两个 doAnotherThing() 调用，还是有一个调用被移到了 doSomeImportantTask() 之后？是否有理由不删除注释掉的代码？这些问题都没有 readily available 的答案。

死代码是指无法到达或逻辑上永远无法运行的代码。例如，函数内部 return 语句之后的代码、if 语句块中始终为假条件下的代码，或从未被调用的函数中的代码都属于死代码。要在实践中看到这一点，请在交互式 shell 中输入以下内容：

```
>>> import random
>>> def coinFlip():
...     if random.randint(0, 1):
...         return 'Heads!'
...     else:
...         return 'Tails!'
...     return 'The coin landed on its edge!'
...
>>> print(coinFlip())
Tails!
```

return 'The coin landed on its edge!' 这一行是死代码，因为 if 和 else 块中的代码会在执行到达该行之前返回。死代码具有误导性，因为阅读它的程序员会假设它是程序的活跃部分，而实际上它与注释掉的代码相同。

占位符是这些代码异味规则的例外。它们是未来代码的占位符，例如尚未实现的功能或类。作为真实代码的替代，占位符包含一个 pass 语句，它什么也不做。（它也称为无操作或 no-op。）pass 语句的存在是为了让你在语言语法要求某些代码的地方创建占位符：

```
>>> def exampleFunction():
...     pass
...
```

当此函数被调用时，它什么也不做。相反，它表示最终会添加代码。

另外，为了避免意外调用未实现的功能，你可以用 raise NotImplementedError 语句对其进行占位。这将立即表明该函数尚未准备好被调用：

```
>>> def exampleFunction():
...     raise NotImplementedError
...
>>> exampleFunction()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in exampleFunction
NotImplementedError
```

引发 NotImplementedError 会在你的程序意外调用占位符函数或方法时发出警告。

注释掉的代码和死代码是代码异味，因为它们可能误导程序员认为代码是程序的可执行部分。相反，删除它们并使用版本控制系统，例如 Git 或 Subversion，来跟踪更改。版本控制在第 12 章中介绍。有了版本控制，你可以从程序中删除代码，如果需要，还可以轻松地稍后将其加回。

## Print Debugging

打印调试是在程序中放置临时的 print() 调用以显示变量的值，然后重新运行程序的做法。该过程通常遵循以下步骤：

- 1. 在你的程序中注意到一个错误。
- 2. 为某些变量添加 print() 调用以了解它们包含的内容。
- 3. 重新运行程序。
- 4. 因为之前的调用没有显示足够的信息，所以添加更多 print() 调用。
- 5. 重新运行程序。
- 6. 在最终找出错误之前，再重复前两个步骤几次。
- 7. 重新运行程序。
- 8. 意识到你忘记删除一些 print() 调用并将其删除。

打印调试看似快速简单。但在显示修复错误所需的信息之前，通常需要多次迭代重新运行程序。解决方案是使用调试器或为你的程序设置日志文件。通过使用调试器，你可以一次运行程序的一行代码并检查任何变量。使用调试器可能看起来比简单插入 print() 调用更慢，但从长远来看它可以节省你的时间。

日志文件可以记录程序的大量信息，以便你将一次运行与之前的运行进行比较。在 Python 中，内置的 logging 模块提供了你所需的功能，只需三行代码即可轻松创建日志文件：

```
import logging
logging.basicConfig(filename='log_filename.txt', level=logging.DEBUG, format='%(asctime)s - %(levelname)s - %(message)s')
logging.debug('This is a log message.')
```

在导入 logging 模块并设置其基本配置后，你可以调用 logging.debug() 将信息写入文本文件，而不是使用 print() 在屏幕上显示它。与打印调试不同，调用 logging.debug() 使得很明显哪些输出是调试信息，哪些输出是程序正常运行的结果。你可以在《Automate the Boring Stuff with Python》第 2 版（No Starch, 2019）第 11 章中找到有关调试的更多信息，你可以在 https://autbor.com/2e/c11/ 在线阅读。

## Variables with Numeric Suffixes

编写程序时，你可能需要多个存储相同类型数据的变量。在这种情况下，你可能会想通过为其添加数字后缀来重用变量名。例如，如果你正在处理一个注册表单，要求用户输入两次密码以防止打字错误，你可能会将这些密码字符串存储在名为 password1 和 password2 的变量中。这些数字后缀并不是对变量包含的内容或它们之间的差异的良好描述。它们也无法表明这些变量有多少个：是否还有 password3 或 password4？尝试创建不同的名称，而不是懒惰地添加数字后缀。对于这个密码示例，更好的名称集合是 password 和 confirm_password。

让我们看另一个示例：如果有一个处理起点和目的坐标的函数，你可能有参数 x1、y1、x2 和 y2。但数字后缀名称传达的信息不如名称 start_x、start_y、end_x 和 end_y 多。与 x1 和 y1 相比，start_x 和 start_y 变量彼此相关也更清晰。

如果你的数字后缀超过 2，你可能想使用列表或集合数据结构将数据存储为集合。例如，你可以将 pet1Name、pet2Name、pet3Name 等的值存储在一个名为 petNames 的列表中。

这种代码异味并不适用于每个简单以数字结尾的变量。例如，拥有一个名为 enableIPv6 的变量是完全可以的，因为“6”是“IPv6”专有名词的一部分，而不是数字后缀。但如果你为一系列变量使用数字后缀，请考虑用数据结构（如列表或字典）替换它们。

## Classes That Should Just Be Functions or Modules

使用 Java 等语言的程序员习惯于创建类来组织程序的代码。例如，让我们看一下这个示例 Dice 类，它有一个 roll() 方法：

```
>>> import random
>>> class Dice:
...     def __init__(self, sides=6):
...         self.sides = sides
...     def roll(self):
...         return random.randint(1, self.sides)
...
>>> d = Dice()
>>> print('You rolled a', d.roll())
You rolled a 1
```

这看起来像是组织良好的代码，但想想我们的实际需求是什么：1 到 6 之间的随机数。我们可以用一个简单的函数调用替换整个类：

```
>>> print('You rolled a', random.randint(1, 6))
You rolled a 6
```

与其他语言相比，Python 使用一种随意的方式来组织代码，因为它的代码不需要存在于类或其他样板结构中。如果你发现自己只是为了进行一次函数调用而创建对象，或者如果你编写的类只包含静态方法，这些都是代码异味，表明你可能最好改写函数。

在 Python 中，我们使用模块而不是类来分组函数。因为类无论如何都必须在模块中，所以把这段代码放在类中只是在你的代码中添加了一个不必要的组织层。第 15 章到第 17 章更详细地讨论了这些面向对象设计原则。Jack Diederich 在 PyCon 2012 的演讲“Stop Writing Classes”涵盖了你可能过度复杂化 Python 代码的其他方式。

## List Comprehensions Within List Comprehensions

列表推导式是创建复杂列表值的一种简洁方式。例如，要创建一个 0 到 100 的数字的字符串列表，排除所有 5 的倍数，你通常需要一个 for 循环：

```
>>> spam = []
>>> for number in range(100):
...     if number % 5 != 0:
...         spam.append(str(number))
...
>>> spam
['1', '2', '3', '4', '6', '7', '8', '9', '11', '12', '13', '14', '16', '17',
--snip--
'86', '87', '88', '89', '91', '92', '93', '94', '96', '97', '98', '99']
```
