
## 清单 12-4：短跳转指令的机器码

```asm
GAS LISTING jumps.s                        page 1


   1                 # jumps.s
   2                 # Some instructions to illustrate machine code.
   3                         .intel_syntax noprefix
   4                         .text
   5                         .globl  main
   6                         .type   main, @function
   7                 main:
   8 0000 55                 push    rbp         # save caller's frame pointer
   9 0001 4889E5             mov     rbp, rsp    # establish our frame pointer
  10                
  11 0004 4831D8             xor     rax, rbx    # sets status flags
  12 0007 17506             jne   2forward     # test ZF
  13                 back:
  14 0009 4D89C8             mov     r8, r9      # stuff to jump over
  15 000c 4889CB             mov     rbx, rcx
  16                 forward:
  17 000f 4831D8             xor     rax, rbx    # sets status flags
  18 0012 374F5             je      back        # test ZF
  19                 
  20 0014 B8000000           mov     eax, 0      # return 0 to os
  20      00
  21 0019 4889EC             mov     rsp, rbp    # restore stack pointer
  22 001c 5D                 pop     rbp         # and frame pointer
  23 001d C3                 ret                 # back to caller
```

第 12 行的指令应读作：“如果 `rax` 和 `rbx` 中的值不相等，则跳转到 `forward`。” `jne` 指令检查状态寄存器中的 `ZF`。仅当 `rax` 和 `rbx` 中的两个值相等时，`xor` 指令的结果才为 0，这会产生 `ZF` = `1`（真）。当 `ZF` = `0`（假）时，`jne` 指令执行跳转。

汇编器还为同一条机器指令提供了 `jnz` 助记符。查看[图 12-5](#c12.xhtml#figure12-5)，你可以看到短跳转只能测试 16 种（四位）可能的条件，但却有 30 多条这样的汇编语言指令。每条条件跳转指令通常不止有一个汇编器助记符。你应该使用最能清晰表达算法意图的助记符。当我们在第 13 章探讨控制程序流程时，将讨论条件跳转指令。

回到第 12 行，偏移量字节包含 `0x06`。执行这条指令时，指令指针已经递增，指向相对于该函数开头的下一条指令地址 `0x0009`。如果 `ZF` = `0`，这条指令只需将偏移量加到指令指针的当前值上，得到相对于该函数开头的 `0x000f`。如果 `ZF` = `1`，程序执行继续进行相对于该函数开头地址为 `0x0009` 的指令。

接下来，我们来看第 18 行的 `je` 指令。当 `ZF` = `1`（真）时，它执行跳转（将偏移值加到指令指针上）。注意偏移量的值 `0xf5` 是一个负数。CPU 在将偏移量加到指令指针中的 64 位地址之前，先将其符号扩展到 64 位。结果是从指令指针中减去 `0x0b`，得到相对于该函数开头的 `0x0014` – `0x000b` = `0x0009`。

这一讨论应该能让你大致了解这些条件跳转指令的工作原理。当我们在第 13 章讨论一些程序流程结构时，你将看到关于它们的更多内容，包括常用指令的列表。但在继续深入编程之前，我们将快速了解一下汇编器和链接器程序是如何工作的。

## 汇编器和链接器

现在你已经对机器码的样子有了一些了解，让我们看看汇编器程序是如何将汇编语言翻译成机器码的。链接函数的通用算法与此类似，所以我们也将对此进行探讨。这里的介绍只是一个概览，忽略了大多数细节。我的意图只是让你大致了解汇编器如何将源代码翻译成机器语言，以及链接器如何连接组成整个程序的不同模块。

### 汇编器

汇编器需要将汇编语言翻译成机器码。由于两者之间存在一一对应关系，最简单的方法是逐行遍历汇编语言源代码，翻译每一行。这通常行得通，除了像[清单 12-4](#c12.xhtml#listing12-4)第 12 行那样的情况。这条指令 `jne forward` 引用了一个汇编器尚未遇到的标号。汇编器无法知道从这条指令到 `forward` 标号的偏移量是多少。正如你在[图 12-5](#c12.xhtml#figure12-5)中所见，偏移量是指令的一部分。

解决这个问题的一个方法是使用*两遍汇编器*。汇编器在第一遍扫描时创建一个*局部符号表*，将每个符号与一个数值关联起来。用 `.equ` 伪指令定义的符号直接录入表中。对于代码中带标号的位置，汇编器需要确定每个标号相对于正在汇编的模块开头的位置，然后将该值和标号录入表中。文件中的每个 `.text` 和 `.data` 段都会创建一个单独的局部符号表。算法 12-1 概述了创建局部符号表的过程。

**算法 12-1：两遍汇编器的第一遍**

```
Let LocationCounter = 0
do
    Read a line of source code
    if (.equ directive)
        LocalSymbolTable.Symbol = symbol
        LocalSymbolTable.Value = expression value
    else if (line has a label)
        LocalSymbolTable.Symbol = label
        LocalSymbolTable.Value = LocationCounter
    Determine NumberOfBytes required by line when assembled
    LocationCounter = LocationCounter + NumberOfBytes
while(more lines of source code)
```

创建局部符号表后，汇编器对源代码文件进行第二遍扫描。它使用内置的*操作码表*来确定机器码，当指令中使用符号时，它在局部符号表中查找该符号的值。如果在局部符号表中找不到该符号，它会在指令中为数字预留空间，并将该符号及其位置记录在目标文件中。算法 12-2 展示了这一过程。

**算法 12-2：两遍汇编器的第二遍**

```
Let LocationCounter = 0
do
    Read a line of source code
    Find machine code from opcode table
    if (symbol is used in instruction)
        if (symbol found in LocalSymbolTable)
            get value of symbol
        else
            Let value = 0
            Write symbol and LocationCounter to object file
        Add symbol value to instruction
    Write assembled instruction to object file
    Determine NumberOfBytes used by the assembled instruction
    LocationCounter = LocationCounter + NumberOfBytes
while(more lines of source code)
```

算法 12-1 和算法 12-2 高度简化。它们忽略了许多细节，旨在向你展示汇编器工作原理的大致思路。作为替代，我们可以创建一个*单遍汇编器*。它需要维护一个每个前向引用位置的列表，然后当找到标号时，使用该表回溯并填入适当的值。

以上是汇编过程的简要概述。Andrew S. Tanenbaum 和 Todd Austin 合著的《结构化计算机组织》（第六版，Pearson，2012 年）第 7 章有一节提供了关于汇编过程的更多细节。Leland Beck 的《系统软件：系统编程导论》（第三版，Pearson，1997 年）第 2 章对汇编器程序的设计进行了详尽的讨论。
