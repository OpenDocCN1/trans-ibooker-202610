

```python
scanner.scan(a_line)
    if unknown:
        print('Unrecognized input:',
              unknown)
        break
    except KeyError as e:
        print('Unrecognized symbol', e.args[0],
              'found in line', pc)
        print(a_list[pc])
        break

main()
```

当运行此源文件时，它会启动 main 函数，该函数控制程序的整体运行。首先，它会调用位于文件 rpn_io.py 中的 open_rpn_file 函数。

由于此文件不大且函数相对较少，这里使用了 import * 语法，以便使 rpn_io.py 中的所有符号名称直接可用。

单击此处查看代码图像

```python
#File rpn_io.py
---------------------------------------

def open_rpn_file():
    '''Open-source-file function.
    Open a named file and read lines into a list,
    which is then returned.
    '''
    while True:
        try:
            fname = input('Enter RPN source: ')
            if not fname:
                return None
            f = open(fname, 'r')
            break
        except:
            print('File not found. Re-enter.')
    a_list = f.readlines()
    return a_list

def do_prints(s):
    """Print string function.
    Print string argument s, without adding a
    newline.
    """
    a_str = get_str(s)
    print(a_str, end='')

def do_println(s=''):
    """Print Line function.
    Print an (optional) string and then add a
    newline.
    """
    if s:
        do_prints(s)
    print()

def get_str(s):
    """Get String helper function.
    Get the quoted portion of a string by getting
    text
    from the first quote mark to the last quote
    mark. If
    these aren't present, return an empty string.
    """
    a = s.find('"')
    b = s.rfind('"')
    if a == -1 or b == -1:
        return ''
    return s[a+1:b]

def do_printvar(s, sym_tab):
    """Print Variable function.
    Print named variable after looking it up in
    sym_tab, which was passed from main module.
    '''
    wrd = s.split()[0]
    print(sym_tab[wrd], end=' ')

def do_input(s, sym_tab):
    '''Get Input function.
    Get input from the end user and place it in
    the named variable, using a reference to the
    symbol table (sym_tab) passed in as a reference.
    '''
    wrd = input()
    if '.' in wrd:
        sym_tab[s] = float(wrd)
    else:
        sym_tab[s] = int(wrd)
```

## 第 14 章 总结

在本章中，我们探讨了在 Python 中使用 `import` 语句的各种方式，以创建可以包含任意数量源文件的多模块项目。

在 Python 中使用多个模块的方式与其他语言有所不同。特别是，在 Python 中，如果导入是 *单向* 的，则更安全，这意味着 `A.py` 可以导入 `B.py`，但如果是这样，`B` 不应该导入 `A`。你可以让 `A` 和 `B` 互相导入，但前提是你知道自己在做什么，并且小心不要创建相互依赖。

同样，你在从另一个模块导入模块级变量时也应当小心。最好使用其限定名称来引用这些变量，例如 `mod_a.x` 和 `mod_a.y`。否则，在创建该变量的模块之外对其进行任何赋值，都会导致创建一个“局部”于出现该变量的模块的新变量。

最后，本章完成了本书中一直在开发的 RPN 解释器应用程序的编程代码。本章添加了问号 (?) 作为非零跳转操作，比较符 (>) 以及感叹号 (!) 作为随机数生成器。添加这些操作极大地扩展了用 RPN 编写的脚本所能实现的功能。

但这些添加远非最终。还有许多其他重要功能你可能希望支持，例如行标签和更好的错误检查。这些留作本章末尾的练习。

## 第 14 章 复习问题

- 1 是否可以使用多个 `import` 语句多次导入同一个模块？这样做的目的是什么？你能想到什么场景会有用吗？

- 2 模块有哪些属性？（至少列举一个。）

- 3 使用循环导入——例如两个模块互相导入——可能会创建依赖关系和隐藏的错误。你如何设计程序以避免相互导入？

- 4 Python 中 `__all__` 的用途是什么？

- 5 在什么情况下引用 `__name__` 属性或字符串 `'__main__'` 是有用的？

- 6 在使用 RPN 解释器应用程序时——它逐行解释 RPN 脚本——添加程序计数器有哪些用途？

- 7 在设计一种简单的编程语言（如 RPN）时，你需要哪些最小的表达式或语句（或两者兼具）才能使该语言原始但完整——也就是说，理论上能够执行任何计算机任务？

## 第 14 章 建议问题

- 1 目前，两个模块 `rpn.py` 和 `rpn_io.py` 之间共享了一些数据。你能否修改应用程序，将共享数据放在第三个模块 `common.py` 中？

- 2 鉴于 RPN 解释器程序的编写方式，添加操作应该很容易，尤其是当它们对应于 operator 包中定义的操作之一时。作为一个小型项目，添加以下内容：小于测试和等于测试。你最大的挑战可能是找到足够的标点字符来表示所有不同的操作符。但是，如果你修改扫描中使用的正则表达式，你可以提出双字符操作符，例如 `==` 表示等于测试。

- 3 对于 RPN 脚本编写者来说，能够在脚本中添加注释将是很好的。你应该能够按照以下规则实现此功能：在 RPN 脚本的每一行中，忽略从井号 (#) 开始直到行尾的所有文本。

- 4 大于测试 (**>**) 是一个布尔运算符，产生 True 或 False（1 或 0）。RPN 脚本的编写者能否在没有实际提供小于 (**<**)、AND 或 OR 运算符的情况下，产生与其他逻辑运算符相同的效果？如果你仔细想想，乘法 (**\*** ) 完美地替代了 AND。加法 (**+**) 也能替代 OR 吗？大部分情况下可以；但是，结果有时是 **2** 而不是 **1** 或 **0**。那么，你能否创建一个逻辑 NOT 运算符，它接受输入 **0** 并产生 **1**，但接受任何正数并产生 **0**？我们真正想问的是，你能否想到几个算术运算，当组合在一起时，能做与逻辑 OR 相同的事情？

- 5 RPN 脚本语言中仍然缺少的最大部分是对行标签的支持。添加这些并不特别困难，但也并非微不足道。任何以 _label_: 开头的行都应被解释为标记一行代码。为了平滑地实现此功能，你应该进行几遍扫描。第一遍应该建立一个“代码表”，排除空行并编译第二个符号表，该表存储标签及其每个标签的值；该值应为代码表中的索引。例如，0 将表示第一行。

- 6 此应用程序中的错误检查可以进一步改进。例如，你能否添加错误检查，如果操作符过多则报告语法错误？（提示：在这种情况下，栈的状态会怎样？）

# 15. 从互联网获取财务数据

最后往往是最好的，我们把最好的留到了最后。用 Python 能做的最令人印象深刻的事情之一就是下载财务信息并以多种方式对其进行图表绘制。

本章将前面章节中使用的许多功能结合起来，展示它们的实际用途。你将看到如何去互联网上抓取信息，获取股票市场上的所需信息，并使用这些数据生成彩色图表，显示你喜爱的股票的表现。

这将是一次有趣的旅程。

## 15.1 本章计划

三个模块构成了本章的股票市场应用程序，如表 15.1 所示。这些文件可在 **brianoverland.com/books** 上以可下载形式获得，还有其他文件，包括 RPN 解释器应用程序。

**表 15.1.** 本章使用的模块

| 模块 | 描述 |
|---|---|

