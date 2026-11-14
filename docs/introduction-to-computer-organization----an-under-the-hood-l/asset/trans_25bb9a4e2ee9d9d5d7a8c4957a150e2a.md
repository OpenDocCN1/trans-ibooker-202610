
在本章中，我们将分解这个过程。我们将通过讨论如何在 `main` 函数中向屏幕写入字符和从键盘读取字符来实现这一点。从本章开始，我们通常将绕过 C 标准库函数 `printf` 和 `scanf`，转而使用系统调用函数 `write` 向屏幕输出，使用 `read` 从键盘输入。

我们将首先讨论 `write` 和 `read` 函数。然后我们将研究如何通过 CPU 寄存器向函数传递参数。接下来我们将研究当需要向函数传递地址时，CPU 如何确定该地址。然后我们将研究称为 `call stack`（调用栈）的数据结构如何用于在函数内创建局部变量。

## `write` 和 `read` 系统调用函数

在第 2 章中，我们使用了 C 标准库中的 `printf` 和 `scanf` 来向屏幕写入和从键盘读取。如[图 2-1](#c02.xhtml#figure2-1)所示（在第 2 章中），`printf` 将数据从其内存存储格式转换为字符格式，并调用 `write` 系统调用函数在屏幕上显示字符。当从键盘读取字符时，`scanf` 调用 `read` 系统调用函数并将字符转换为内存存储格式。

Linux 将屏幕和键盘视为文件。当程序首次启动时，操作系统会打开三个文件——`standard in`（标准输入）、`standard out`（标准输出）和 `standard error`（标准错误）——并为每个文件分配一个称为 `file descriptor`（文件描述符）的整数。程序通过使用 `file descriptor` 与每个文件进行交互。调用 `read` 和 `write` 的 C 语言接口在 `Portable Operating System Interface`（可移植操作系统接口，`POSIX`）标准中规定。调用这两个函数的通用格式为

```c
int write(int fd, char *buf, int n);
int read(int fd, char *buf, int n);
```

其中 `fd` 是文件描述符，`buf` 是字符存储区的地址，`n` 是要读取或写入的字符数。你可以在 `write` 和 `read` 的 `man` 手册页中查看更多详细信息：

```
man 2 write
man 2 read
```

[表 11-1](#c11.xhtml#table11-1) 显示了我们将使用的文件描述符及其通常关联的设备。

**表 11-1：`write` 和 `read` 系统调用函数的文件描述符**

| 名称 | 编号 | 用途 |
|------|--------|-----|
| `STDIN_FILENO` | 0 | 从键盘读取字符 |
| `STDOUT_FILENO` | 1 | 向屏幕写入字符 |
| `STDERR_FILENO` | 2 | 向屏幕写入错误消息 |

这些名称定义在系统头文件 `unistd.h` 中，在我的 Ubuntu 系统上位于 `/usr/include/unistd.h`。（你的系统上的位置可能不同。）

让我们看看如何向 `write` 函数传递适当的参数以在屏幕上写入文本。

## 在寄存器中传递参数

在我们的环境中，最多可以通过寄存器从一个函数向另一个函数传递六个参数。我们将在第 14 章中研究如何传递超过六个参数，并在此指出 Windows C 环境只允许通过寄存器传递四个参数。

让我们从一个非常简单的程序开始。我们将使用 `write` 系统调用函数在屏幕上写入 "Hello, World!"（[清单 11-1](#c11.xhtml#listing11-1)）。

```c
/* helloWorld.c
 * Hello World program using the write() system call.
 */

#include <unistd.h>

int main(void)
{

  write(STDOUT_FILENO, "Hello, World!\n", 14);

  return 0;
}
```

**清单 11-1：使用 `write` 系统调用函数的 "Hello, World!" 程序**

此函数向 `write` 传递三个参数。原则上，C 编译器——或者你在编写汇编语言时——可以使用 16 个通用寄存器中的任意一个（`rsp` 除外）将参数从一个函数传递给另一个函数。（不能使用 `rsp` 的原因稍后会解释。）只需将参数存储在寄存器中并调用所需的函数即可。当然，编译器或编写汇编语言的人需要知道当进入被调用函数时，每个参数确切位于哪个寄存器中。

避免错误的最佳方法是制定一套标准规则并遵循它们。如果有不止一个人为程序编写代码，这一点尤为重要。其他人已经意识到制定此类标准的重要性，并在 `System V Application Binary Interface AMD64 Architecture Processor Supplement (with LP64 and ILP32 Programming Models) Version 1.0`（System V 应用程序二进制接口 AMD64 架构处理器补充（包含 LP64 和 ILP32 编程模型）版本 1.0）中给出了一套用于传递参数的良好标准。我在 [https://github.com/hjl-tools/x86-psABI/wiki/x86-64-psABI-1.0.pdf](https://github.com/hjl-tools/x86-psABI/wiki/x86-64-psABI-1.0.pdf) 找到了 2018 年 1 月 28 日版本的 PDF 格式。（最新版本以 LaTeX 源码形式维护在 [https://gitlab.com/x86-psABIs/x86-64-ABI](https://gitlab.com/x86-psABIs/x86-64-ABI)`*/*`，但你需要 `pdflatex` 来构建 PDF 版本。）我们使用的编译器 `gcc` 遵循 System V 标准中的规则，我们编写的汇编语言也将遵循相同的规则。

**表 11-2：通用寄存器用法**

| 寄存器 | 特殊用途 | 保存？ |
|----------|---------------|-------|
| `rax` | 从函数返回第一个值 | 否 |
| `rbx` | 通用 | 是 |
| `rcx` | 向函数传递第四个参数 | 否 |
| `rdx` | 向函数传递第三个参数；从函数返回第二个值 | 否 |
| `rsp` | 栈指针 | 是 |
| `rbp` | 可选帧指针 | 是 |
| `rdi` | 向函数传递第一个参数 | 否 |
| `rsi` | 向函数传递第二个参数 | 否 |
| `r8` | 向函数传递第五个参数 | 否 |
| `r9` | 向函数传递第六个参数 | 否 |
| `r10` | 传递函数的静态链指针 | 否 |
| `r11` | 无 | 否 |
| `r12` | 无 | 是 |
| `r13` | 无 | 是 |
| `r14` | 无 | 是 |
| `r15` | 无 | 是 |

“保存？”列显示被调用函数是否需要为调用函数保留该寄存器中的值。你将在接下来的几节中学习如何做到这一点。

前六个参数分别在寄存器 `rdi`、`rsi`、`rdx`、`rcx`、`r8` 和 `r9` 中传递，对应 C 函数中从左到右的顺序。[清单 11-2](#c11.xhtml#listing11-2) 显示了 `gcc` 为 [清单 11-1](#c11.xhtml#listing11-1) 中的 C 函数生成的汇编语言。这演示了如何向 `write` 函数传递所需的三个参数。

```nasm
        .file   "helloWorld.c"
        .intel_syntax noprefix
        .text
      1 .section  .rodata
2 .LC0:
        .string "Hello, World!\n"
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp
        mov     rbp, rsp
        mov     edx, 314         ## number of chars
        lea     rsi, 4.LC0[rip]  ## address of string
        mov     edi, 51          ## STDOUT_FILENO
        call  6 write@PLT
        mov     eax, 0
        pop     rbp
        ret
        .size   main, .-main
        .ident  " GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section  .note.GNU-stack,"",@progbits
```

**清单 11-2：`gcc` 为 [清单 11-1](#c11.xhtml#listing11-1) 中的程序生成的汇编语言**

在汇编语言编程中，通常从参数列表中的最后一个参数开始将参数存储在寄存器中，依次向前直到第一个参数。在[清单 11-2](#c11.xhtml#listing11-2)中，`write` 的第三个参数（字符数 3）首先存储在第三个参数的寄存器 `edx` 中。第二个参数是字符串 4 中第一个字符的地址，存入 `rsi`。第一个参数（要写入的设备 5）在调用 `write` 6 之前存入 `edi`。
