
# 循环构造

## for 循环

### for 循环语法

```
for (initialize loop control variable; expression; change loop control variable)
{
  body
}
next_statement
```

但将所有控制元素放在括号内并非必须。事实上，我们也可以按如下方式编写 `for` 循环：

```
initialize loop control variable
for (; expression;)
{
  body
  change loop control variable
}
next_statement
```

`for` 循环语法要求括号内必须包含两个分号。

我们可以使用 `for` 循环重写 [Listing 13-1](#c13.xhtml#listing13-1) 中的程序，如 [Listing 13-4](#c13.xhtml#listing13-4) 所示。

```c
/* helloWorld-for.c
 * Hello World program using the write() system call
 * one character at a time.
 */
#include <unistd.h>
#define NUL '\x00'

int main(void)
{
  char *stringPtr;

  for (stringPtr = "Hello, World!\n"; *stringPtr != NUL; stringPtr++)
  {
    write(STDOUT_FILENO, stringPtr, 1);
  }

  return 0;
}
```

Listing 13-4: 使用 `for` 循环在终端窗口中逐字符写入文本字符串

你可能会想知道两种循环构造哪个更好。这就是你的汇编语言知识派上用场的地方。当我用 `gcc` 为 [Listing 13-4](#c13.xhtml#listing13-4) 生成汇编语言时，得到的汇编代码与 [Listing 13-1](#c13.xhtml#listing13-1) 中 `while` 循环版本的相同。由于 `for` 循环的汇编语言已在 [Listing 13-2](#c13.xhtml#listing13-2) 中展示，此处不再重复。

`for` 循环常用于*计数控制循环*，即在循环开始前已知迭代次数。稍后我们在查看选择构造时将看到此用法的示例。

通过对比 `for` 循环与 `while` 循环，我们得出的结论是：应使用最适合你正在解决的问题的高级语言循环构造。是的，这通常是一个主观选择。

C 语言中的第三种循环构造提供了不同的行为。`while` 循环和 `for` 循环构造都会在循环控制变量的初始值满足终止条件时跳过循环体。而 `do-while` 循环将始终至少执行一次循环体。

## do-while 循环

在某些情况下，算法将至少执行一次循环体。在这些情况下，`do-while` *循环*可能更自然。其一般形式如下：

```
initialize loop control variable
do
{
  body
  change loop control variable
} while (expression)
next_statement
```

在 `do-while` 循环构造中，表达式的值在执行完循环体后计算。循环持续进行，直到该计算结果为布尔假。我们可以使用 `do-while` 循环重写“Hello, World!”程序，如 [Listing 13-5](#c13.xhtml#listing13-5) 所示。

```c
/* helloWorld-do.c
 * Hello World program using the write() system call
 * one character at a time.
 */
#include <unistd.h>
#define NUL '\x00'

int main(void)
{
  char *stringPtr = "Hello, World!\n";

  do
  {
    write(STDOUT_FILENO, stringPtr, 1);
    stringPtr++;
  }
  while (*stringPtr != NUL);

  return 0;
}
```

Listing 13-5: 使用 `do-while` 循环在屏幕上逐字符输出

`gcc` 生成的汇编语言 [Listing 13-6](#c13.xhtml#listing13-6) 展示了 `do-while` 与 `while` 和 `for` 构造的区别。

```asm
        .file   "helloWorld-do.c"
        .intel_syntax noprefix
        .text
        .section  .rodata
.LC0:
        .string "Hello, World!\n"
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     QWORD PTR -8[rbp], 0
        lea     rax, .LC0[rip]          ## address of text string
        mov     QWORD PTR -8[rbp], rax
1 .L2:
        mov     rax, QWORD PTR -8[rbp]  ## address of current character
        mov     edx, 1                  ## one character
        mov     rsi, rax                ## pass address
        mov     edi, 1                  ## STDOUT_FILENO
        call    write@PLT
        add     QWORD PTR -8[rbp], 1    ## increment pointer
        mov     rax, QWORD PTR -8[rbp]  ## address of current characte
        movzx   eax, BYTE PTR [rax]     ## load character
      2 test    al, al                  ## NUL character?
        jne     .L2                     ## no, continue looping
        mov     eax, 0
        leave
        ret
        .size   main, .-main
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section  .note.GNU-stack,"",@progbits
```

Listing 13-6: [Listing 13-5](#c13.xhtml#listing13-5) 中 `do-while` 循环的汇编语言

将 [Listing 13-6](#c13.xhtml#listing13-6) 中的汇编语言与 [Listing 13-2](#c13.xhtml#listing13-2) 中的（`while` 和 `for` 循环）比较，唯一的区别是 `do-while` 在第一次执行循环 1 之前不会跳转下去执行循环控制检查 2。`do-while` 似乎更高效，但从汇编语言来看，唯一的节省只是第一次执行循环时的一次跳转。

接下来，我们将探讨如何选择是否执行一块代码。

## 选择

另一种常见的流程构造是选择，即确定是否执行一块代码。我们将从最简单的情况开始——基于布尔条件语句决定是否执行单个代码块。然后我们将探讨使用布尔条件语句在两个代码块中选择一个。本章最后将讨论基于整数值在多个代码块中选择的方法。

### if 条件语句

C 语言中 `if` *条件语句*的一般形式如下：

```
if (expression)
{
  block
}
next_statement
```

`expression` 在布尔上下文中求值。如果其求值为假（C 语言中为 `0`），控制流继续执行 `next_statement`。如果 `expression` 求值为真（C 语言中为非零），则执行代码块，随后控制流继续执行 `next_statement`。

[Listing 13-7](#c13.xhtml#listing13-7) 展示了一个 `if` 语句示例，模拟抛硬币 10 次并显示正面朝上的情况。

```c
/* coinFlips1.c
 * Flips a coin, heads.
 */

#include <stdio.h>
#include <stdlib.h>

int main()
{
  register int randomNumber;
  register int i;

1 for (i = 0; i < 10; i++)
  {
    randomNumber = 2 random();
    if 3 (randomNumber < RAND_MAX/2)
    {
    4 puts("heads");
    }
  }

  return 0;
}
```

Listing 13-7: 模拟抛硬币，显示正面朝上的情况

该程序使用计数控制的 `for` 循环模拟抛硬币 10 次 1。模拟过程涉及调用 C 标准库中的 `random()` 函数 2。如果随机数处于 `random()` 函数所有可能值的下半部分 3，我们称之为正面。我们使用 C 标准库中的 `puts()` 函数，它在屏幕上打印简单文本字符串并追加换行符 4。编译器生成的汇编语言如 [Listing 13-8](#c13.xhtml#listing13-8) 所示。
