
# GDB 调试会话：递归阶乘

现在我们到了 `factorial` 的递归调用传入基本情况值 `0` 1 的地步。我们可以看到 `factorial` 创建了三个栈帧，一个叠在另一个上面。最近的两个栈帧显示返回地址指向同一位置 23，该位置位于 `factorial` 中。最旧的栈帧显示返回地址指向 `main` 4。

继续执行四次会展开栈帧，带我们回到 `main` 调用 `factorial` 时创建的第一个栈帧。

```
(gdb) c
Continuing.

Breakpoint 2, done () at factorial.s:29
29             mov     rsp, rbp            # restore stack pointer
(gdb) c
Continuing.

Breakpoint 2, done () at factorial.s:29
29             mov     rsp, rbp            # restore stack pointer
(gdb) c
Continuing.

Breakpoint 2, done () at factorial.s:29
29             mov     rsp, rbp            # restore stack pointer 
(gdb) c
Continuing.

Breakpoint 2, done () at factorial.s:29
29             mov     rsp, rbp            # restore stack pointer
(gdb) i r rax rdi rbp rsp
rax            0x6               16
rdi          2 0x0                 0
rbp            0x7fffffffde40      0x7fffffffde40
rsp            0x7fffffffde30      0x7fffffffde30
(gdb) x/4xg 0x7fffffffde30
0x7fffffffde30: 0x00007ffff7fb1fc8   0x00000003555551b0
0x7fffffffde40: 0x00007fffffffde60   0x0000555555555156
(gdb) c
Continuing.
3! = 6
[Inferior 1 (process 2373) exited normally]
```

现在我们回到了 `factorial` 的第一次调用，即将返回 `main` 的那次调用，该调用的参数是基本情况 `0` 2，它正在返回结果 `6` 1。

正如你所见，递归函数调用会使用大量栈空间。而且因为它们在每次重复中都会调用一个函数，所以可能很耗时。每个递归解决方案都有一个等效的迭代解决方案，后者通常在时间和栈使用方面更高效。例如，计算整数阶乘的迭代算法很简单。但虽然在这个简单的例子中看不出来，许多问题（例如某些排序算法）更自然地适合递归解决方案。在这类问题中，代码的简洁性往往值得付出递归的代价。

现在我们知道了如何在函数中存储数据项以及如何在汇编语言中在函数之间来回移动数据，我们将探讨如何使用汇编语言访问我们正在使用的高级语言中可能无法访问的硬件特性。

# 在汇编语言中访问 CPU 特性

在第 14 章中，创建一个完整的子函数仅仅为了将两个整数相加（参见 [清单 14-8](#c14.xhtml#listing14-8)）可能显得有点愚蠢，因为这可以用一条指令完成。但正如我们在第 3 章中看到的，即使是简单的加法也会产生进位或溢出，这由 CPU 中 `rflags` 寄存器中的标志位指示。

C 和 C++ 都没有提供检查 `rflags` 寄存器中溢出标志或进位标志的方法。在本节中，我们将探讨两种方法来告知我们的 C 函数加法是否产生了溢出：我们可以编写一个可从 C 代码调用的单独汇编语言函数，或者在 C 代码中嵌入汇编语言。

## 用汇编语言编写的单独函数

我们将从用 C 语言重写 `sumInts` 程序开始，使其在加法产生溢出时警告用户。我们将在子函数 `addTwo` 中检查溢出，并通过返回机制将结果传回 `main` 函数。

[清单 15-6](#c15.xhtml#listing15-6) 展示了我们修改后的 `main` 函数，它检查返回值以判断溢出。

```c
/* sumInts.c
 * Adds two integers using local variables
 * Checks for overflow
 */

#include <stdio.h>
#include "addTwo.h"

int main(void)
{
  int x = 0, y = 0, z;
  int overflow;
  
  printf("Enter an integer: ");
  scanf("%i", &x);
  printf("Enter an integer: ");
  scanf("%i", &y);
  overflow = addTwo(x, y, &z);
  printf("%i + %i = %i\n", x, y, z);
  if (overflow)
  {
    printf("    *** Overflow occurred ***\n");
  }  

  return 0;
}
```

**清单 15-6：** 用于求两个整数之和并检查溢出的程序

我们将重写 `addTwo` 函数，使其在无溢出时返回 `0`，有溢出时返回 `1`，我们将该值赋给变量 `overflow` 1。在 C 语言中，零在逻辑上为假，而非零为真 2。

[清单 15-7](#c15.xhtml#listing15-7) 展示了我们新 `addTwo` 函数的头文件。

```c
/* addTwo.h
 * Adds two integers and determines overflow.
 */

#ifndef ADDTWO_H
#define ADDTWO_H
int addTwo(int a, int b, int *c);
#endif
```

**清单 15-7：** 检查溢出的 `addTwo` 函数的头文件

函数声明的唯一变化是返回 `int` 而不是 `void`。我们需要在 `addTwo` 函数的定义中添加溢出检查，如 [清单 15-8](#c15.xhtml#listing15-8) 所示。

```c
/* addTwo.c
 * Adds two integers and determines overflow.
 */

#include "addTwo.h"

int addTwo(int a, int b, int *c)
{
  int temp;
  int overflow = 0;   /* assume no overflow */
  
  temp = a + b;
  if (((a > 0) && (b > 0) && (temp < 0)) ||
        ((a < 0) && (b < 0) && (temp > 0)))
  {
    overflow = 1;
  }
  *c = temp;
  return overflow;
}
```

**清单 15-8：** 将两个整数相加并检查溢出

我们在第 3 章学到，如果将两个同号整数相加得到的结果符号相反，则发生溢出，因此我们使用此逻辑作为溢出检查 1。[清单 15-9](#c15.xhtml#listing15-9) 展示了编译器从此 C 源代码生成的汇编语言。

```asm
        .file   "addTwo.c"
        .intel_syntax noprefix
        .text
        .globl  addTwo
        .type   addTwo, @function
addTwo:
        push    rbp
        mov     rbp, rsp
        mov     DWORD PTR -20[rbp], edi   ## store a
        mov     DWORD PTR -24[rbp], esi   ## store b
        mov     QWORD PTR -32[rbp], rdx   ## address of c
      1 mov     DWORD PTR -8[rbp], 0      ## overflow = 0;
        mov     edx, DWORD PTR -20[rbp]
        mov     eax, DWORD PTR -24[rbp]
        add     eax, edx                  ## a + b
        mov     DWORD PTR -4[rbp], eax    ## temp = a + b;
      2 cmp     DWORD PTR -20[rbp], 0     ## a <= 0?
        jle     .L2                       ## yes
        cmp     DWORD PTR -24[rbp], 0     ## b <= 0?
        jle     .L2                       ## yes
        cmp     DWORD PTR -4[rbp], 0      ## temp < 0?
        js      .L3                       ## yes, overflow
.L2:
        cmp     DWORD PTR -20[rbp], 0     ## a == 0?
      3 jns     .L4                       ## yes, no overflow
        cmp     DWORD PTR -24[rbp], 0     ## b == 0?
        jns     .L4                       ## yes, no overflow
        cmp     DWORD PTR -4[rbp], 0      ## temp == 0?
        jle     .L4                       ## yes, no overflow
.L3:
      4 mov     DWORD PTR -8[rbp], 1
.L4:
        mov     rax, QWORD PTR -32[rbp]   ## address of c
        mov     edx, DWORD PTR -4[rbp]    ## temp
        mov     DWORD PTR [rax], edx      ## *c = temp;
        mov     eax, DWORD PTR -8[rbp]    ## return overflow;
        pop     rbp
        ret
        .size   addTwo, .-addTwo
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section        .note.GNU-stack,"",@progbits
```

**清单 15-9：** 编译器为 [清单 15-8](#c15.xhtml#listing15-8) 中的 `addTwo` 生成的汇编语言

该算法首先假设不会发生溢出 1。在将两个整数相加后，它检查 `a ≤ 0` 2。如果是，则通过将 `a` 与 `0` 比较并检查 `rflags` 寄存器中的符号标志来查看 `a` 是否为负数 3。如果为 `0`，则没有溢出，因此算法跳过会将溢出变量改为 `1` 的指令 4。
