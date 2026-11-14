
## GDB 命令参考

4. `i r` 显示 CPU 寄存器内容（`info registers`）。（您将在第 9 章了解 CPU 寄存器。）

5. `l` `line_number` 列出以指定 `line_number` 为中心的 10 行源代码。

6. `print` `expression` 对 `expression` 求值并打印结果。

7. `printf` `"format", var1, var2, ...` 以给定格式显示 `var1, var2, ...` 的值。`"format"` 字符串遵循与 C 标准库中 `printf` 相同的规则。

8. `r` 开始执行已在 `gdb` 控制下加载的程序。

9. `x/nfs memory_address` 从 `memory_address` 开始，以格式 `f`、大小 `s` 显示（检查）内存中的 `n` 个值。

## 使用调试器

让我们使用 `gdb` 演练[清单 2-1](#c02.xhtml#listing2-1)中的程序，探索迄今为止涵盖的一些概念。建议您在电脑上跟随操作：亲自使用 `gdb` 更容易理解。请注意，您在电脑上看到的地址可能与示例中的不同。

首先使用 `gcc` 命令编译程序：

```
$ gcc -g -Wall -masm=intel -o intAndString intAndString.c
```

`-g` 选项告诉编译器在可执行程序中包含调试信息。`-Wall` 选项告诉编译器对代码中正确但可能非预期的 C 代码发出警告。例如，它会警告您在函数中声明了从未使用的变量，这可能意味着您遗漏了什么。

本书后面编写汇编语言时，我们将使用 Intel 和 AMD 文档中指定的语法，并使用 `masm=intel` 选项告诉编译器使用相同语法。目前不需要此选项，但建议习惯使用，因为稍后会用到。

`-o` 选项指定输出文件名，即可执行程序。

编译完成后，可使用以下命令在 `gdb` 控制下运行程序：

```
$ gdb ./intAndString
--snip--
Reading symbols from ./intAndString…
(gdb) l
1	/* intAndString.c
2	 * Using printf to display an integer and a text string.
3	 */
4	
5	#include <stdio.h>
6	
7	int main(void)
8	{
9	  unsigned int anInt;
10	  char aString[10];
(gdb) 
11	
12	  printf("Enter a number in hexadecimal: ");
13	  scanf("%x", &anInt);
14	  printf("Enter it again: ");
15	  scanf("%s", aString);
16	  
17	  printf("The integer is %u and the string is %s\n", anInt, aString);
18	
19	  return 0;
20	}
(gdb)
```

`gdb` 启动信息（为节省空间已从上述输出中移除）包含有关调试器的信息，并引导您查阅其使用文档。

`l` 命令列出 10 行源代码，然后将控制权返回给 `gdb` 程序，如 `(gdb)` 提示符所示。按回车键重复上一条命令，`l` 将显示接下来的（最多）10 行。

*断点* 用于停止程序并将控制权返回给调试器。我喜在函数即将调用另一个函数的地方设置断点，以便在参数变量传递给被调用函数前检查其值。`main` 函数在第 17 行调用 `printf`，所以我在那里设置断点。由于我已经在想要设置断点的函数的源代码中，因此无需指定文件名：

```
(gdb) b 17
Breakpoint 1 at 0x11f6: file intAndString.c, line 17.
```

如果 `gdb` 在执行程序时到达此语句，它将在*语句执行前*暂停并将控制权返回给调试器。

设置好断点后，我运行程序：

```
(gdb) r
Starting program: /home/bob/progs/chapter_02/intAndString/intAndString
Enter a hexadecimal value: 123abc
Enter it again: 123abc

Breakpoint 1, main () at intAndString.c:17
1 17	  printf("The integer is %u and the string is %s\n", anInt, aString);
```

`r` 命令从头开始执行程序。当程序到达断点时，控制权返回给 `gdb`，它显示即将执行的下一条程序语句 1。在继续执行前，我将显示传递给 `printf` 函数的两个变量的内容：

```
(gdb) print anInt
$1 = 1194684
(gdb) print aString
$2 = "123abc\000\177\000>
```

可使用 `print` 命令显示变量中当前存储的值。`gdb` 从源代码了解每个变量的数据类型。它以十进制显示 `int` 变量。显示 `char` 变量时，`gdb` 会尽力显示对应代码点值的字符字形。当没有对应的字符字形时，`gdb` 以 `\` 后跟三个*八进制*数字显示代码点。（参考[表 2-2](#c02.xhtml#table2-2)。）例如，`NUL` 没有字符字形，因此 `gdb` 在我们输入的文本字符串末尾显示 `\000`。

`printf` 命令可格式化显示的值。格式化字符串与 C 标准库中 `printf` 函数的相同：

```
(gdb) printf "anInt = %u = %#x\n", anInt, anInt
anInt = 1194684 = 0x123abc
(gdb) printf "aString = 0x%s\n", aString
aString = 0x123abc
```

`gdb` 提供另一个命令 `x` 用于直接检查内存内容——即检查实际位模式。其帮助信息简短，但包含所需的一切：

```
(gdb) h x
Examine memory: x/FMT ADDRESS.
ADDRESS is an expression for the memory address to examine.
FMT is a repeat count followed by a format letter and a size letter.
Format letters are o(octal), x(hex), d(decimal), u(unsigned decimal),
 t(binary), f(float), a(address), i(instruction), c(char) and s(string).
Size letters are b(byte), h(halfword), w(word), g(giant, 8 bytes).
The specified number of objects of the specified size are printed
according to the format.

Defaults for format and size letters are those previously used.
Default count is 1. Default address is following last thing printed
with this command or "print".
```

`x` 命令需要要显示的内存区域地址。可使用 `print` 命令查找变量地址：

```
(gdb) print &anInt
$3 = (unsigned int *) 0x7fffffffde88
```

我们将用 `x` 命令以三种不同方式显示 `anInt` 的内容：一个十进制字（`1dw`）、一个十六进制字（`1xw`）和四个十六进制字节（`4xb`）。

```
(gdb) x/1dw 0x7fffffffde88
0x7fffffffde88: 1194684
(gdb) x/1xw 0x7fffffffde88
0x7fffffffde88: 0x00123abc
(gdb) x/4xb 0x7fffffffde88
0x7fffffffde88: 0xbc  0x3a  0x12  0x00
```

四个字节的显示对您来说可能看起来顺序颠倒。第一个字节 1 位于行左侧显示的地址处。行中下一个字节位于后续地址 `0x7fffffffde89`。因此，该行从左到右显示存储在四个内存地址 `0x7fffffffde88`、`0x7fffffffde89`、`0x7fffffffde8a` 和 `0x7fffffffde8b` 中的各个字节，它们组成变量 `anInt`。单独显示这同四个字节时，最低有效字节在内存中*最先*出现。这称为*小端序*存储顺序；在完成这趟 `gdb` 之旅后我将进一步解释。

同样，我们将通过先获取地址来显示 `aString` 变量的内容：

```
(gdb) print &aString
$4 = (char (*)[50]) 0x7fffffffde8e
```

接下来，我们将以两种方式查看 `aString` 的内容：10 个字符（`10c`）和 10 个十六进制字节（`10xb`）：

```
(gdb) x/10c 0x7fffffffde8e
0x7fffffffde8e: 49 '1'  50 '2'  51 '3'  97 'a'  98 'b'  99 'c'  0 '\000'  127 '\177'
0x7fffffffde96: 0 '\000'  0 '\000'
(gdb) x/10xb 0x7fffffffde8e
0x7fffffffde8e: 0x31  0x32  0x33  0x61  0x62  0x63  0x00  0x7f
0x7fffffffde96: 0x00  0x00
```
