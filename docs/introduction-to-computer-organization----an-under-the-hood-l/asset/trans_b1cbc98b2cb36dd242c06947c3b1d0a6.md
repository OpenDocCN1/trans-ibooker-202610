
# 栈保护与汇编编程

## 栈金丝雀检查

在[清单 11-5](#c11.xhtml#listing11-5)中，检查栈是否损坏的代码首先检索保存在栈上的值，即栈金丝雀，然后将其与程序启动时生成的原始值（位于内存位置 `fs:40`）执行按位异或运算。如果这两个值相同，异或运算结果为 `0`，这会将零状态标志 `ZF` 设置为 `1`（真），导致 `je .L3` 指令将程序流程转移到 `leave` 指令，从而跳过对 `__stack_chk_fail@PLT` 函数的调用。如果异或运算不产生 `0`，则跳转不会发生，程序将调用 `__stack_chk_fail@PLT` 函数，该函数将报告栈损坏错误并终止程序。

## `leave` 指令

你在该程序中看到了另一条新指令 `leave`。

**`leave`—离开函数**

1.  删除栈帧。
2.  `leave` 恢复调用者的帧指针，并将栈指针置于返回地址处。

`leave` 指令执行与以下两条指令相同的操作：

```
mov     rsp, rbp
pop     rbp
```

参考[图 11-4](#c11.xhtml#figure11-4)，你可以看到这将栈指针移动到存储调用者 `rbp` 的位置，然后将其弹回 `rbp` 寄存器，使栈指针保留在返回地址处。

## 汇编器数据项大小符号

`gcc` 在[清单 11-5](#c11.xhtml#listing11-5) 中生成的汇编语言包含一些额外的符号，即 `QWORD PTR`。在大多数情况下，汇编器可以从指令的上下文中推断出操作数的大小——字节、字、双字或四字。如果操作数之一是寄存器，则寄存器名称决定了操作数的大小。但如果一个操作数是内存地址而另一个是字面常量，则操作数大小无法确定。例如，在[清单 11-5](#c11.xhtml#listing11-5) 中，如果第 3 处的指令如下所示，

```
mov     -8[rbp], 123
```

整数 123 可以存储在从一字节及以上的任何大小中。在这种情况下，你需要使用第 10 章[表 10-1](#c10.xhtml#table10-1) 中的符号（此处复制为[表 11-3](#c11.xhtml#table11-3)）告诉汇编器数据项的大小。

**表 11-3：汇编器数据项大小符号**

| 修饰符 | 数据类型 | 位数 |
| --- | --- | --- |
| `byte ptr` | 字节 | 8 |
| `word ptr` | 字 | 16 |
| `dword ptr` | 双字 | 32 |
| `qword ptr` | 四字 | 64 |

因此，指令

```
mov     byte ptr -8[rbp], 123
```

会将 123 作为 8 位值存储，而指令

```
mov     qword ptr -8[rbp], 123
```

会将 123 作为 64 位值存储。我不知道编译器编写者为何选择在[清单 11-5](#c11.xhtml#listing11-5) 中使用这种符号，因为汇编器可以从所用寄存器的名称 `rax` 推断出数据项大小，但这种冗余并无害处。

## 汇编语言示例：echoChar 程序（清单 11-6）

让我们把这些内容结合起来，直接用汇编语言编写 `echoChar` 程序。我们将为标签使用更有意义的名称，让汇编器计算文本字符串的长度，并为代码添加注释，如[清单 11-6](#c11.xhtml#listing11-6) 所示。

```
# echoChar.s
# 提示用户输入一个字符，然后回显响应
.intel_syntax noprefix
# 有用的常量
.equ    STDIN,0
.equ    STDOUT,1
# 栈帧
.equ    aLetter,-1
.equ    localSize,-16

# 常量数据
.section  .rodata
prompt:
.string "Enter one character: "
.equ    promptSz,.-prompt-1
msg:
.string "You entered: "
.equ    msgSz,.-msg-1
.text
# 代码 
.globl  main
.type   main, @function
main:
push    rbp           # 保存调用者的帧指针
mov     rbp, rsp      # 建立我们的帧指针
add     rsp, localSize # 为局部变量分配空间

mov     rax, fs:40    # 获取栈金丝雀
mov     -8[rbp], rax  # 并保存它

mov     edx, promptSz # 提示符大小
lea     rsi, prompt[rip] # 提示符文本字符串的地址
mov     edi, STDOUT   # 标准输出
call    write@plt     # 调用 write 函数

mov     edx, 1        # 1 个字符
lea     rsi, 1aLetter[rbp] # 存储字符的位置
mov     edi, STDOUT   # 标准输出
call    write@plt     # 调用 write 函数

mov     edx, 1        # 1 个字符
lea     rsi, aLetter[rbp] # 存储字符的位置
mov     edi, STDIN    # 标准输入
call    read@plt      # 调用 read 函数

mov     edx, msgSz    # 消息大小
lea     rsi, msg[rip] # 消息文本字符串的地址
mov     edi, STDOUT   # 标准输出
call    write@plt     # 调用 write 函数

mov     edx, 1        # 1 个字符
lea     rsi, aLetter[rbp] # 存储字符的位置
mov     edi, STDOUT   # 标准输出
call    write@plt     # 调用 write 函数

mov     eax, 0        # 返回 0

mov     rcx, -8[rbp]  # 检索保存的金丝雀值
xor     rcx, fs:40    # 并检查它
je      goodCanary
call    __stack_chk_fail@PLT    # 金丝雀值错误
goodCanary:
mov     rsp, rbp      # 删除局部变量
pop     rbp           # 恢复调用者的帧指针
ret                   # 返回调用函数
```

**清单 11-6：直接用汇编语言编写的回显单个字符的程序**

阅读[清单 11-6](#c11.xhtml#listing11-6) 中的代码时，我认为你会发现，为栈帧中变量的偏移量命名会使代码更易读。我还显式地撤销栈帧，而不是使用 `leave` 指令，以强调正在发生的事情。

在后续章节中，你将学习如何为更大、更复杂的变量使用栈帧。你还将学习如何使用栈来传递超出寄存器可传递的六个参数的参数。

## 不使用 C 运行时环境

本书的主要目的是展示在使用高级语言编写代码时，在指令集级别发生了什么，因此我们将在本书的其余部分继续使用 C（以及后来的 C++）运行时环境和 POSIX `write` 和 `read` 系统调用函数。

当然，也可以编写不使用 C 运行时环境的独立程序。你将在第 20 章看到如何做到这一点。

## 你学到的内容

1.  `write` 和 `read` **函数** — 绕过 C 标准库的系统调用函数。
2.  向子函数传递参数 — 最多六个参数通过寄存器传递。
3.  位置无关可执行文件 — 操作系统可以将程序加载到内存的任何位置，它都能正确执行。
4.  调用栈 — 用于存储程序数据和地址的内存区域，根据需要增长和收缩。
5.  函数序言 — 为从调用函数到被调用函数的转换设置调用栈。
6.  函数尾声 — 函数序言的补充，将调用栈恢复到调用函数时的状态。
7.  自动变量 — 每次调用函数时新建。它们可以轻松地在调用栈上创建。
8.  栈金丝雀 — 一个随机数被放置在当前函数栈区域的开头，可显示调用栈上的重要信息何时被更改。

在下一章中，我们将暂时放下编写程序，转而观察汇编程序执行的向机器码的翻译过程。

# 第 12 章：指令细节

![](image_fi/book_art/chapterart.png)
