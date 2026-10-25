

there being support for random access causes an exception to be raised.

The **seek** method is sometimes useful even in programs that don’t use random access. When you read a file, in either text or binary mode, reading starts at the beginning and goes sequentially forward.

What if you want to read a file again, from the beginning? Usually you won’t need to do that, but we’ve found it useful in testing, in which you want to rerun the file-read operations. You can always use **seek** to return to the beginning.

Click here to view code image

```
file_obj.seek(0, 0)  # Go back to beginning of the file.
```

This statement assumes that **file_obj** is a successfully opened file object. The first argument is an offset. The second argument specifies the origin value 0, which indicates the beginning of the file. Therefore, the effect of this statement is to reset the file pointer to the beginning of the file.

The possible values for _offset_ are 0, 1, and 2, indicating the beginning, current position, and end of the file.

Moving the file pointer also affects writing operations, which could cause you to write over data you’ve already written. If you move to the end of the file, any writing operations effectively append data.

Otherwise, random access is often most useful in binary files that have a series of fixed-length records. In that case, you can directly access a record by using its zero-based index and multiplying by the record size:

Click here to view code image

```
file_obj.seek(rec_size * rec_num, 0)
```

The **tell** method is the converse of **seek**. It returns an offset number that tells the number of bytes from the beginning of the file. A value of 0 indicates that the beginning of the file is in fact your current position.

Click here to view code image

```
file_pointer = file_obj.tell()
```

## 8.9 将文本读入 RPN 项目

有了读写文本文件的能力，我们可以为逆波兰表示法（RPN）项目添加新功能。经过本节的更改后，你将能够打开一个由 RPN 语句组成的文本文件，逐个执行这些语句，并打印结果。

在添加文本文件读取能力后，我们将朝着构建功能齐全的语言解释器迈出重要一步。

### 8.9.1 迄今为止的 RPN 解释器

程序的当前版本继承自第 7 章，使用正则表达式语法和 Scanner 对象对 RPN 语句进行词法分析和解析。程序一次评估一个 RPN 语句，遇到空行时退出。

针对输入的每一行 RPN，程序输入该行代码并打印该 RPN 语句的最终数值。

Click here to view code image

```
import re
import operator

stack = []   # Stack to hold the values.

# Scanner object. Isolate each token and take
# appropriate action: push a numeric value, but
# perform
# operation on top two elements on stack if an
# operator
# is found.
scanner = re.Scanner([
    (r"[ \t\n]", lambda s, t: None),
    (r"-?(\d*\.)?\d+", lambda s, t:
        stack.append(float(t))),
    (r"\d+", lambda s, t: stack.append(int(t))),
    (r"[+]", lambda s, t: bin_op(operator.add)),
    (r"[-]", lambda s, t: bin_op(operator.sub)),
    (r"[*]", lambda s, t: bin_op(operator.mul)),
    (r"[/]", lambda s, t: bin_op(operator.truediv)),
    (r"[\^]", lambda s, t: bin_op(operator.pow)),
])

# Binary Operator function. Pop top two elements
from
# stack and push the result back on the stack.

def bin_op(action):
    op2, op1 = stack.pop(), stack.pop()
    stack.append(action(op1, op2))

def main():
    while True:
        input_str = input('Enter RPN line: ')
        if not input_str:
            break
        try:
            tokens, unknown =
scanner.scan(input_str)
            if unknown:
                print('Unrecognized input:',
unknown)
            else:
                print(str(stack[-1]))
        except IndexError:
            print('Stack underflow.')

main()
```

以下是示例会话：

> Click here to view code image

```
Enter RPN line: 25 4 *
100.0
Enter RPN line: 25 4 * 50.75-
49.25
Enter RPN line: 3 3* 4 4* + .5^
5.0
Enter RPN line:
```

每一行 RPN 代码——尽管间距不同且不一致——都能被程序正确读取和评估。例如，第三行输入（加粗部分）是勾股定理的一个示例，因为它正在计算

Click here to view code image

```
square_root((3 * 3) + (4 * 4))
```

的值。

### 8.9.2 从文本文件读取 RPN

下一步是让程序打开一个文本文件并从中读取 RPN 语句。“语句”可以由一系列运算符和数字组成，就像前面示例中所示，但程序在评估每个语句后应该做什么呢？

目前，让我们采用一条简单的规则：如果要读取的文件中的某行文本为空，则不执行任何操作。但如果该行有任何输入，则执行该行 RPN 代码并打印结果，该结果应可作为 `stack[-1]`（栈顶）获得。

程序的新版本如下。请注意，加粗的行表示新增或大幅修改的行。此外，`open_rpn_file` 定义是新的。

Click here to view code image

```
import re
import operator

stack = []   # Stack to hold the values.

# Scanner object. Isolate each token and take
# appropriate action: push a numeric value, but
perform
# operation on top two elements on stack if an
operator
# is found.

scanner = re.Scanner([
    (r"[ \t\n]", lambda s, t: None),
    (r"-?(\d*\.)?\d+", lambda s, t:
        stack.append(float(t))),
    (r"\d+", lambda s, t: stack.append(int(t))),
    (r"[+]", lambda s, t: bin_op(operator.add)),
    (r"[-]", lambda s, t: bin_op(operator.sub)),
    (r"[*]", lambda s, t: bin_op(operator.mul)),
    (r"[/]", lambda s, t: bin_op(operator.truediv)),
    (r"[^]", lambda s, t: bin_op(operator.pow)),
])
# Binary Operator function. Pop top two elements
from
# stack and push the result back on the stack.

def bin_op(action):
    op2, op1 = stack.pop(), stack.pop()
    stack.append(action(op1, op2))

def main():
    a_list = open_rpn_file()
    if not a_list:
        print('Bye!')
        return

    for a_line in a_list:
        a_line = a_line.strip()
        if a_line:
            tokens, unknown = scanner.scan(a_line)
            if unknown:
                print('Unrecognized input:',
                      unknown)
            else:
                print(str(stack[-1]))

def open_rpn_file():
    ''"Open-source-file function. Open a named
    file and read lines into a list, which is
    returned.
    ''"
    while True:
        try:
            fname = input('Enter RPN source: ')
            f = open(fname, 'r')
            if not f:
                return None
            else:
                break
        except:
            print('File not found. Re-enter.')
    a_list = f.readlines()
    return a_list

main()
```

让我们进一步假设，同一目录下有一个名为 `rpn.txt` 的文件，其内容如下：

```
3 3 * 4 4 * + .5 ^
1 1 * 1 1 * + .5 ^
```

有了这个文件和新版本的 RPN 解释器程序，以下是示例会话。

```
Enter RPN source: rppn.txt
File not found. Re-enter.
Enter RPN source: rpn.txt
5.0
1.4142135623730951
```

该程序的行为完全符合设计。当输入 RPN 文件名（rpn.txt）时，程序会按需评估每一行。

请注意，rpn.txt 的第一行被故意留空，作为测试。程序如预期般跳过了它。

此版本程序的基本操作是打开一个文本文件，该文件理想情况下包含语法正确的 RPN 语言语句。当成功打开有效文本文件时，open_rpn_file 函数返回一个文本行列表。main 函数随后逐个评估该列表中的每个成员。

但我们才刚刚开始。下一步是扩展 RPN 语言的语法，使其能够将值赋给变量，就像 Python 本身所做的那样。

### 8.9.3 为 RPN 添加赋值运算符

RPN“语言”即将变得更加有趣。我们将使其能够识别和存储符号名称。如何实现这一点？

我们需要一个符号表。Python 提供了一种特别方便、快速且简单的方法来实现这一点：使用数据字典。记住，你可以通过赋值 { } 来创建一个空字典。

```
sym_tab = { }
```

现在我们可以向符号表添加条目。以下

