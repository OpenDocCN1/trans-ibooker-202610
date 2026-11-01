
例如，WizCoin 类中的 total() 方法返回该对象以 knuts 为单位的值。我们可以将它从一个普通方法改为只读属性，因为没有合理的方式来设置 WizCoin 对象的 total。毕竟，如果你将 total 设置为整数 1000，这是指 1,000 knuts？还是指 1 galleon 和 493 knuts？还是指其他某种组合？因此，我们将通过向 wizcoin.py 文件中添加加粗的代码来使 total 成为只读属性：

```
@property
def total(self):
    """Total value (in knuts) of all the coins in this WizCoin object."""
    return (self.galleons * 17 * 29) + (self.sickles * 29) + (self.knuts)

# Note that there is no setter or deleter method for `total`.
```

在 total() 前面添加 @property 函数装饰器后，每当访问 total 时，Python 都会调用 total() 方法。因为没有 setter 或 deleter 方法，如果任何代码尝试通过赋值或 del 语句分别修改或删除 total，Python 会引发 AttributeError。请注意，total 属性的值取决于 galleons、sickles 和 knuts 属性中的值：该属性不是基于名为 _total 的备份变量。 在交互式 shell 中输入以下内容：

```
>>> import wizcoin
>>> purse = wizcoin.WizCoin(2, 5, 10)
>>> purse.total
1141
>>> purse.total = 1000
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: can't set attribute
```

你可能不喜欢程序在尝试更改只读属性时立即崩溃，但这种行为比允许更改只读属性更可取。你的程序能够修改只读属性肯定会在程序运行期间在某个时刻导致错误。如果该错误在修改只读属性之后很久才发生，就很难追踪到原始原因。立即崩溃可以让你更早地注意到问题。

不要将只读属性与常量变量混淆。常量变量全部大写，依赖于程序员不去修改它们。它们的值 supposed 在程序运行期间保持恒定不变。只读属性与任何属性一样，与对象相关联。只读属性不能直接设置或删除。但它可能会计算出一个变化的值。我们的 WizCoin 类的 total 属性会随着其 galleons、sickles 和 knuts 属性的变化而变化。

## 何时使用属性

正如你在前面的章节中看到的，属性提供了对如何使用类的属性的更多控制，并且它们是编写代码的 Pythonic 方式。名称类似于 `getSomeAttribute()` 或 `setSomeAttribute()` 的方法表明你可能应该改用属性。

这并不是说以 *获取* 或 *设置* 开头的 *每个* 方法实例都应该立即被属性替换。有些情况下，即使方法名称以 *获取* 或 *设置* 开头，你也应该使用方法。以下是一些示例：

- 对于耗时超过一两秒的慢速操作——例如，下载或上传文件
- 对于具有副作用的操作，例如对其他属性或对象的更改
- 对于需要向 get 或 set 操作传递额外参数的操作——例如，在像 `emailObj.getFileAttachment(filename)` 这样的方法调用中

程序员通常将方法视为动词（因为方法执行某些操作），而将属性和属性视为名词（因为它们代表某些项目或对象）。如果你的代码似乎更像是在执行获取或设置的操作，而不是获取或设置一个项目，那么最好使用 getter 或 setter 方法。最终，这个决定取决于作为程序员的你认为听起来是否正确。

使用 Python 属性的巨大优势在于，你在最初创建类时不必使用它们。你可以使用普通属性，如果以后需要属性，可以在不破坏类外部任何代码的情况下将属性转换为属性。当我们使用属性的名称创建属性时，我们可以使用下划线前缀重命名属性，我们的程序仍将像以前一样工作。

## Python 的 Dunder 方法

Python 有几个以双下划线开头和结尾的特殊方法名称，简称为 *双下划线*。这些方法被称为 *双下划线方法*、*特殊方法* 或 *魔法方法*。你已经熟悉 `__init__()` dunder 方法名称，但 Python 还有更多。我们经常将它们用于 *运算符重载*——即添加自定义行为，允许我们使用类的对象与 Python 运算符，如 + 或 >=。其他 dunder 方法让类的对象能够与 Python 的内置函数一起工作，例如 `len()` 或 `repr()`。

与 `__init__()` 或属性的 getter、setter 和 deleter 方法一样，你几乎从不直接调用 dunder 方法。当你使用运算符或内置函数使用对象时，Python 会在幕后调用它们。例如，如果你为类创建名为 `__len__()` 或 `__repr__()` 的方法，当该类的对象分别传递给 len() 或 repr() 函数时，它们会在幕后被调用。这些方法在官方 Python 文档 https://docs.python.org/3/reference/datamodel.html 中在线记录。

随着我们探索不同类型的 dunder 方法，我们将扩展我们的 WizCoin 类以利用它们。

### 字符串表示 Dunder 方法

你可以使用 __repr__() 和 __str__() dunder 方法来创建 Python 通常不知道如何处理的对象的字符串表示。通常，Python 以两种方式创建对象的字符串表示。repr（读作 "repper"）字符串是一段 Python 代码字符串，运行后会创建对象的副本。str（读作 "stir"）字符串是一个人类可读的字符串，提供关于对象的清晰、有用信息。repr 和 str 字符串分别由 repr() 和 str() 内置函数返回。例如，输入以下内容到交互式 shell 以查看 datetime.date 对象的 repr 和 str 字符串：

```
>>> import datetime
>>> newyears = datetime.date(2021, 1, 1)
>>> repr(newyears)
'datetime.date(2021, 1, 1)'
>>> str(newyears)
'2021-01-01'
>>> newyears
datetime.date(2021, 1, 1)
```

在这个示例中，datetime.date 对象 ② 的 'datetime.date(2021, 1, 1)' repr 字符串 literally 是一段创建该对象 ① 副本的 Python 代码字符串。这个副本提供了对象的精确表示。另一方面，datetime.date 对象 ③ 的 '2021-01-01' str 字符串是以人类易读的方式表示对象值的字符串。如果我们 simply 将对象输入到交互式 shell ④，它会显示 repr 字符串。对象的 str 字符串通常显示给用户，而对象的 repr 字符串用于技术上下文，例如错误消息和日志文件。

Python 知道如何显示其内置类型的对象，例如整数和字符串。但它无法知道如何显示我们创建的类的对象。如果 repr() 不知道如何为对象创建 repr 或 str 字符串，按惯例字符串将被括在角括号中并包含对象的内存地址和类名：'<wizcoin.WizCoin object at 0x00000212B4148EE0>'。要为 WizCoin 对象创建这种字符串，请在交互式 shell 中输入以下内容：

```
>>> import wizcoin
>>> purse = wizcoin.WizCoin(2, 5, 10)
>>> str(purse)
'<wizcoin.WizCoin object at 0x00000212B4148EE0>'
>>> repr(purse)
'<wizcoin.WizCoin object at 0x00000212B4148EE0>'
>>> purse
<wizcoin.WizCoin object at 0x00000212B4148EE0>
```

这些字符串不太可读或有用，因此我们可以通过实现 __repr__() 和 __str__() dunder 方法来告诉 Python 使用什么字符串。__repr__() 方法指定当对象传递给 repr() 内置函数时 Python 应返回什么字符串，而 __str__() 方法指定当对象传递给 str() 内置函数时 Python 应返回什么字符串。将以下内容添加到 wizcoin.py 文件的末尾：

```
--snip--
def __repr__(self):
    """Returns a string of an expression that re-creates this object."""
    return f'{self.__class__.__qualname__}({self.galleons}, {self.sickles}, {self.knuts})'

def __str__(self):
    """Returns a human-readable string representation of this object."""
    return f'{self.galleons}g, {self.sickles}s, {self.knuts}k'
```

当我们将 purse 传递给 repr() 和 str() 时，Python 调用 __repr__() 和 __str__() dunder 方法。我们在代码中不调用 dunder 方法。请注意，包含对象在花括号中的 f-string 将隐式调用 str() 来获取对象的 str 字符串。例如，在交互式 shell 中输入以下内容：

```
>>> import wizcoin
>>> purse = wizcoin.WizCoin(2, 5, 10)
>>> repr(purse)  # Calls WizCoin's __repr__() behind the scenes.
'WizCoin(2, 5, 10)'
>>> str(purse)  # Calls WizCoin's __str__() behind the scenes.
'2g, 5s, 10k'
>>> print(f'My purse contains {purse}.')  # Calls WizCoin's __str__().
My purse contains 2g, 5s, 10k.
```

当我们将 purse 中的 WizCoin 对象传递给 repr() 和 str() 函数时，在幕后 Python 调用 WizCoin 类的 __repr__() 和 __str__() 方法。我们编程这些方法以返回更可读和有用的字符串。如果你将 'WizCoin(2, 5, 10)' repr 字符串的文本输入到交互式 shell，它将创建一个具有与 purse 中的对象相同属性的 WizCoin 对象。str 字符串是对象值更人类可读的表示：'2g, 5s, 10k'。如果你在 f-string 中使用 WizCoin 对象，Python 使用对象的 str 字符串。

如果 WizCoin 对象复杂到无法通过单个构造函数调用创建其副本，我们会将 repr 字符串括在角括号中以表示它不是 Python 代码。这就是通用表示字符串，例如 '<wizcoin.WizCoin object at 0x00000212B4148EE0>' 所做的。将此字符串输入到交互式 shell 会引发 SyntaxError，因此它不可能被误认为是创建对象副本的 Python 代码。

在 __repr__() 方法内部，我们使用 self.__class__.__qualname__ 而不是硬编码字符串 'WizCoin'；因此，如果我们对 WizCoin 进行子类化，继承的 __repr__() 方法将使用子类名称而不是 'WizCoin'。此外，如果我们重命名 WizCoin 类，__repr__() 方法将自动使用更新后的名称。
