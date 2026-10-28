

## 9.8 量子门的普适性

对于一本编程书籍来说，量子计算中通用门集的存在性大多是学术性的，但值得简单讨论一下，以与经典情况互补。

与经典情况不同，1量子比特门有无限多个。RX_φ、RY_φ和RZ_φ就给出了这样一个集合，其中0 ≤ φ < 2π。如果我们无法精确地生成这些门，就需要尽量接近地逼近它们。我们修改定义：如果我们可以用这些门构建一个电路，并以任意精度逼近任何n量子比特门，那么这组量子门就是普适的。[TCN]

我们需要量子比特纠缠，这在经典中没有对应物。

最后，在X、Y、Z、H和CNOT这些优雅的门之下，我们需要了解在任何给定的量子计算技术中，工程师在硬件中实现了哪些门。例如，这些门在“超导transmon”、“离子阱”、“量子点”和“光子”量子比特方法中有所不同，并且可能在每个类别内部也有差异。[DRQ] 我们能否从硬件门出发近似生成我们想要的任何门？

研究人员已经发现了几个量子门的通用集，包括：

-   所有1量子比特门加上CNOT，
-   Toffoli CCX、Hadamard H和S

以及

-   CNOT、Hadamard H和T。

在实践中，通过Qiskit可用或你可以用电路自行构建的门集非常大。我们重点讨论这些门。

## 9.9 小结

本章探讨了经典计算的门与电路基础。它展示了底层编码者如何构建位加法和更高级别的函数。然后，我们转向量子门与电路。虽然我们使用了相同的术语，但编程模型有着根本性的不同。特别是，叠加和纠缠带来了仅靠经典技术无法获得的行为和工具。

下一章，我们将研究测试和优化代码的技术，无论是经典代码还是量子代码。我们已经通过使用Qiskit的transpile函数为通用和硬件特定用途优化量子门时看到了一些相关内容。

## 10

## 优化和测试你的代码

> *经验是每个人赋予自己错误的名称。*
> 
> ——奥斯卡·王尔德，*《温德米尔夫人的扇子》*

如果我们的代码完美无缺，那将是多么美妙。它会以最有效的方式运行，并且会完全按照我们的意图从第一次就表现出来。不会有任何一行或一个表达式是无用的。它会在标准情况下以及软件用户扔给我们的每一个怪异的转折中正确执行。

在本章中，我们面对现实。代码在开始时通常有一个或多个bug。你的编程编辑器（例如Visual Studio Code）可能会帮助你处理语法错误，但语义错误可能更难发现。我们首先学习广泛使用的测试代码的技术，从简单的健全性检查到有组织的、可重复的过程。然后，我们查看代码运行所需的时间，并给出提高性能的建议。你的所有代码都被使用了吗？我们考察一下“覆盖率”的概念，看看我们编写的所有代码是否都被测试和执行。最后，我们研究Python装饰器，学习如何在不修改原始代码的情况下向函数和方法添加额外功能。

## 本章涵盖的主题

-   10.1 测试你的代码  384
-   10.2 测量代码运行时间  391
-   10.3 优化你的代码  393
-   10.4 查找孤立代码  400
-   10.5 定义和使用装饰器  403
-   10.6 小结  408

## 10.1 测试你的代码

你编写的代码必须在所有可能的情况下对所有可能的输入都能工作。因此，你必须设计一个测试系统，确保所有可能性都被覆盖，并且结果是正确的。

这是一个强有力的说法，如果你编写“快速而粗糙”的一次性代码或仅供自己使用，你可能会放宽规则。如果你要分享你的代码，或者它将作为生产环境的一部分，测试是必不可少的。

### 10.1.1 __debug__

默认情况下，系统变量__debug__为True。此设置允许你编写如下代码：

```
from src.code.unipoly import UniPoly

x = UniPoly(1, "x", 1)

def square_poly(p):
    if __debug__:
        print(f"Argument {p = }\n")
    return p*p

square_poly(x**2 - 3*x + 7)
```

Argument p = x**2 - 3*x + 7

x**4 - 6*x**3 + 23*x**2 - 42*x + 49

注意，我从特定文件夹路径“src.code.unipoly”导入了UniPoly。你可能需要根据你对该模块的使用情况将其更改为其他路径。我在第7章“将对象组织成类”中介绍了UniPoly。

```
iterative_factorial(1.3)

File "<stdin>", line 1, in "<module>"
File "<stdin>", line 2, in iterative_factorial
AssertionError
```

更具体地说，在条件后面包含一条消息：

```
def iterative_factorial(n):
    assert isinstance(n, int), "n not an int"
    assert n >= 0, "n < 0"

    product = 1
    for i in range(1, n + 1):
        product *= i
    return product
```

```
iterative_factorial(-1.3)

File "<stdin>", line 1, in "<module>"
File "<stdin>", line 2, in iterative_factorial
AssertionError: n not an int
```

```
iterative_factorial(-17)

File "<stdin>", line 1, in "<module>"
File "<stdin>", line 3, in iterative_factorial
AssertionError: n < 0
```

你可以在一个assert语句中测试多个条件，但我更喜欢将它们分开，以便查看哪个测试失败了。

使用assert是有开销的，因为Python必须计算每个条件。如果你在代码开发早期添加了断言，一旦你确信一切正常，就可以删除它们。启动Python时使用-O或-OO会关闭所有assert条件检查。

### 10.1.3 pytest

假设你编写了一个函数或类。通常，它们不会单独存在，而是包含在模块、包或项目中。尽管如此，对每个组件进行正确行为测试是合理的。运行并强调一段代码的软件称为单元测试。

```
"""Tests that a multiplication fails."""
x = UniPoly(1, 'x', 1)
p = x + 1
q = x - 1
>     assert p * q == x * x + 1
E     assert x**2 - 1 == x**2 + 1
E       Use -v to get the full diff

unipoly.py:732: AssertionError
=========================== short test summary info ===========================
FAILED unipoly.py::test_bad_multiplication - assert x**2 - 1 == x**2 + 1
=========================== 1 failed, 1 passed in 0.10s ============================
```

这里有很多信息，包括操作系统、pytest版本、已安装的测试插件、断言失败的代码、原始断言以及求值后的断言。输出中未提及test_good_addition，因为断言条件为True。

pytest会查看输入文件，并处理所有名称以“test_”开头的函数。

通常，我不会定义像test_bad_multiplication这样的函数，而是会编写类似这样的代码：

```
def test_multiplication():
    """Tests polynomial multiplication."""
    x = UniPoly(1, 'x', 1)
    p = x + 1
    q = x - 1
    assert 0 * p == 0
    assert p * 0 == 0
    assert 1 * p == p
    assert -1 * p == -p
    assert p * 1 == p
    assert 2 * p == p * 2
    assert p * q == q * p
    assert p * q == x * x - 1
    # ... more multiplication tests
```

我会对每个函数和方法都这样做。

## 练习10.1

在图10.1中，对于60 ≤ n ≤ 65的2ⁿ的*timeit*时间曲线中，当n = 64时出现了奇怪的现象。解释为什么我们会看到执行时间的大幅增加。（提示：参见5.2节。）

![](img/4ffeafa3e3f1373f02971e43b0755ea6_420_0.png)

你可以通过定义一个函数，来评估比容易放入`stmt`字符串中更多的代码。

```
def f(n):
    assert n > 6

    the_list = []
    for x in range(n):
        the_list.append(x)
    for _ in range(10):
        the_list += the_list
        the_list = the_list[5:]
```

*f*是一个低效的无意义函数，它创建一个列表，将其追加到自身，进行切片，并重复十次。使用`globals`关键字参数，让*timeit*可以使用你的局部定义。

```
count = 1000
timeit.timeit(stmt="f(100)", number=count, globals=globals())
```

0.9215112999999997

