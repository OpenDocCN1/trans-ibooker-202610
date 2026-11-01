
```
... a = a ^ b
... b = a ^ b
... a = a ^ b""")
0.13515726800000039
```

在我的电脑上，XOR 算法运行这段代码大约需要十分之一秒。这快吗？让我们把它与使用第三个临时变量的整数交换代码进行比较：

```
>>> import timeit
>>> timeit.timeit('a, b = 42, 101; temp = a; a = b; b = temp')
0.027540389999998638
```

真令人惊讶！使用第三个临时变量不仅更易读，而且速度也快了两倍！聪明的 XOR 技巧可能可以节省几字节的内存，但以牺牲速度和代码可读性为代价。牺牲代码可读性来减少几字节的内存使用或纳秒级的运行时间是不值得的。

更好的是，你可以使用多重赋值技巧，也称为可迭代解包，来交换两个变量，它也运行在很小的时间内：

```
>>> timeit.timeit('a, b = 42, 101; a, b = b, a')
0.024489236000007963
```

不仅这是最易读的代码，而且也是最快的。我们知道这一点不是因为我们假设它，而是因为我们客观地测量了它。

timeit.timeit() 函数还可以接受第二个字符串参数作为设置代码。设置代码在运行第一个字符串的代码之前只运行一次。你也可以通过为 number 关键字参数传递一个整数来更改默认的试验次数。例如，以下测试测量 Python 的 random 模块生成 1 到 100 之间的 10,000,000 个随机数有多快。（在我机器上，大约需要 10 秒。）

```
>>> timeit.timeit('random.randint(1, 100)', 'import random', number=10000000)
10.020913950999784
```

默认情况下，你传递给 timeit.timeit() 的字符串中的代码将无法访问程序其余部分中的变量和函数：

```
>>> import timeit
>>> spam = 'hello'  # We define the spam variable.
>>> timeit.timeit('print(spam)', number=1)  # We measure printing spam.
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Users\Al\AppData\Local\Programs\Python\Python37\lib\timeit.py", line 232, in timeit
    return Timer(stmt, setup, timer, globals).timeit(number)
  File "C:\Users\Al\AppData\Local\Programs\Python\Python37\lib\timeit.py", line 176, in timeit
    timing = self.inner(it, self.timer)
  File "<timeit-src>", line 6, in inner
NameError: name 'spam' is not defined
```

要修复这个问题，请将函数传递 globals() 的返回值作为 globals 关键字参数：

```
>>> timeit.timeit('print(spam)', number=1, globals=globals())
hello
0.000994909999462834
```

编写代码的一个好规则是先让它工作，然后再让它变快。只有在你有一个可工作的程序后，才应专注于使其更高效。

## cProfile 分析器

尽管 timeit 模块可用于测量小的代码片段，但 cProfile 模块更有效地用于分析整个函数或程序。分析会系统地分析程序的速度、内存使用情况和其他方面。cProfile 模块是 Python 的分析器，或可测量程序运行时间并为程序的各个函数调用构建运行时间分析的软件。这些信息提供了对代码的更细粒度的测量。

要使用 cProfile 分析器，将你想要测量的代码的字符串传递给 cProfile.run()。让我们看看 cProfile 如何测量并报告一个从 1 到 1,000,000 求和所有数字的短函数的执行情况：

```
import time, cProfile
def addUpNumbers():
    total = 0
    for i in range(1, 1000001):
        total += i

cProfile.run('addUpNumbers()')
```

运行此程序时，输出看起来会像这样：

```
4 function calls in 0.064 seconds

Ordered by: standard name

ncalls  tottime  percall  cumtime  percall  filename:lineno(function)
1       0.000    0.000    0.064    0.064    <string>:1(<module>)
1       0.064    0.064    0.064    0.064    test1.py:2(addUpNumbers)
1       0.000    0.000    0.064    0.064    {built-in method builtins.exec}
1       0.000    0.000    0.000    0.000    {method 'disable' of '_lsprof.Profiler' objects}
```

每一行代表一个不同的函数以及在该函数中花费的时间。cProfile.run() 输出中的列是：

**ncalls** 对函数进行的调用次数
**totttime** 在函数中花费的总时间，不包括子函数中的时间
percall 总时间除以调用次数
cumtime 在函数及其所有子函数中花费的累计时间
percall 累计时间除以调用次数
filename:lineno(function) 函数所在的文件名及行号

例如，从 https://nostarch.com/crackingcodes/ 下载 rsaCipher.py 和 al_sweigart_pubkey.txt 文件。此 RSA 密码程序曾出现在《用 Python 破解密码》（No Starch Press, 2018）中。输入以下内容到交互式 shell 中，以分析 encryptAndWriteToFile() 函数在加密由 'abc' * 100000 表达式创建的 300,000 字符消息时的性能：

```
>>> import cProfile, rsaCipher
>>> cProfile.run("rsaCipher.encryptAndWriteToFile('encrypted_file.txt', 'al_sweigart_pubkey.txt', 'abc'*100000)")
11749 function calls in 28.900 seconds

Ordered by: standard name

ncalls tottime percall cumtime percall filename:lineno(function)
1 0.001 0.001 28.900 28.900 <string>:1(<module>)
2 0.000 0.000 0.000 0.000 _bootlocale.py:11(getpreferredencoding)
--snip--
1 0.017 0.017 28.900 28.900 rsaCipher.py:104(encryptAndWriteToFile)
1 0.248 0.248 0.249 0.249 rsaCipher.py:36(getBlocksFromText)
1 0.006 0.006 28.873 28.873 rsaCipher.py:70(encryptMessage)
1 0.000 0.000 0.000 0.000 rsaCipher.py:94(readKeyFile)
--snip--
2347 0.000 0.000 0.000 0.000 {built-in method builtins.len}
2344 0.000 0.000 0.000 0.000 {built-in method builtins.min}
2344 28.617 0.012 28.617 0.012 {built-in method builtins.pow}
2 0.001 0.001 0.001 0.000 {built-in method io.open}
4688 0.001 0.000 0.001 0.000 {method 'append' of 'list' objects}
--snip--
```

你可以看到传递给 cProfile.run() 的代码花费了 28.9 秒完成。请注意总时间最高的函数；在这种情况下，Python 内置的 pow() 函数占用了 28.617 秒。这几乎是整个代码的运行时间！我们无法更改此代码（它是 Python 的一部分），但也许我们可以更改代码以减少对它的依赖。

在这种情况下这是不可能的，因为 rsaCipher.py 已经相当优化了。即使如此，对此代码的分析也为我们提供了 pow() 是主要瓶颈的见解。因此，几乎没有必要尝试改进，例如 readKeyFile() 函数（运行时间如此之短，以至于 cProfile 报告其运行时间为 0）。

这一思想被阿姆达尔定律所概括，阿姆达尔定律是一个公式，用于计算在改进程序的某个组件后整个程序加速多少。公式为 speed-up of whole task = 1 / ((1 - p) + (p / s))，其中 s 是对组件的加速，p 是该组件占整体程序的比例。所以，如果你将占程序总运行时间 90% 的组件的速度提高一倍，你将得到 1 / ((1 − 0.9) + (0.9 / 2)) = 1.818，即整体程序加速 82%。这比例如将仅占总运行时间 25% 的组件速度提高三倍要好，后者仅为 1 / ((1 − 0.25) + (0.25 / 2)) = 1.143，即整体加速 14%。你不需要记住公式。只需记住，加快代码中缓慢或冗长的部分比加快已经快速或短小的部分更有成效。这很常见：昂贵的房子打 10% 的折扣比便宜的一双鞋打 10% 的折扣更好。

## Big O 算法分析

Big O 是一种算法分析形式，描述代码如何扩展。它将代码分类为若干个阶，总体描述随着需要完成的工作量增加，代码的运行时间会多长。Python 开发者 Ned Batchelder 将 big O 描述为“代码随数据增长而变慢”的分析，这也是他信息丰富的 PyCon 2018 演讲的标题，该演讲可在 https://youtu.be/duvZ-2UK0fc/ 获取

让我们考虑以下场景。假设你有一项工作需要一个小时完成。如果工作量加倍，需要多长时间？你可能会想当然地认为需要两倍的时间，但实际上，答案取决于工作的类型。

如果阅读一本短书需要一个小时，那么阅读两本短书大约需要两个小时。但如果你可以在一个小时内按字母顺序排列 500 本书，那么排列 1,000 本书很可能需要超过两个小时，因为你需要在更大的书籍集合中找到每本书的正确位置。另一方面，如果你只是检查书架是否为空，那么书架上有 0 本、10 本或 1,000 本书都无关紧要。看一眼你就会立即知道答案。无论有多少本书，运行时间大致保持不变。尽管有些人读或整理书的速度可能更快或更慢，但这些总体趋势保持不变。

算法的 big O 描述了这些趋势。算法可以在快速或慢速的计算机上运行，但我们仍然可以使用 big O 来描述算法在总体上的表现如何，而不管执行算法的实际硬件如何。Big O 不使用秒或 CPU 周期等具体单位来描述算法的运行时间，因为这些在不同的计算机或编程语言之间会有所不同。

## Big O 阶

Big O 记号通常定义以下阶。这些从较低的阶开始，描述代码随着数据量的增长，

