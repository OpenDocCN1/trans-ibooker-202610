
## 清单 16-2：`toUpper` 函数的头文件

```c
/* toUpper.c
 * Converts alphabetic letters in a C string to uppercase.
 */

#include "toUpper.h"
#define UPMASK 0xdf
#define NUL '\0'

int toUpper(char *srcPtr, char *destPtr)
{
    int count = 0;
    while (*srcPtr != NUL)
    {
        *destPtr = *srcPtr & UPMASK;
        srcPtr++;
        destPtr++;
        count++;
    }
    *destPtr = *srcPtr;

    return count;
}
```

## 清单 16-3：`toUpper` 函数

为了确保第 5 位是 `0`，我们使用一个在第 5 位位置为 `0`、其他位置为 `1` 的掩码。只要不是 `NUL` 字符，我们就对该字符执行按位 AND 运算，这会屏蔽掉第 5 位，并让所有其他位在结果中保持不变。别忘了包含来自输入文本字符串的 `NUL` 字符！忘记这样做是一个常见的编程错误，在测试期间通常不会显现，因为内存中输出存储位置后面的字节碰巧就是 `0x00`（即 `NUL` 字符）。然后，如果你更改输入文本字符串的长度，内存中的下一个字节可能就不是 `0x00` 了。该错误可能会以一种看似随机的方式出现。

虽然此函数返回处理的字符数量，但我们的 `main` 函数并未使用该值。调用函数不需要使用返回值。我通常会在这样的函数中包含计数算法，以便在需要时进行调试。

## 清单 16-4：`toUpper` 函数的编译器生成汇编语言

```asm
        .file   "toUpper.c"
        .intel_syntax noprefix
        .text
        .globl  toUpper
        .type   toUpper, @function
toUpper:
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR -24[rbp], rdi ## save srcPtr
        mov     QWORD PTR -32[rbp], rsi ## save destPtr
        mov     DWORD PTR -4[rbp], 0    ## count = 0;
        jmp     .L2
.L3:
      1 mov     rax, QWORD PTR -24[rbp] ## load srcPtr
      2 movzx   eax, BYTE PTR [rax]     ## and char there
      3 and     eax, -33                ## and with 0xdf
        mov     edx, eax
        mov     rax, QWORD PTR -32[rbp] ## load destPtr
        mov     BYTE PTR [rax], dl      ## store new char
        add     QWORD PTR -24[rbp], 1   ## srcPtr++;
        add     QWORD PTR -32[rbp], 1   ## destPtr++;
        add     DWORD PTR -4[rbp], 1    ## count++;
.L2:
      4 mov     rax, QWORD PTR -24[rbp] ## load srcPtr
      5 movzx   eax, BYTE PTR [rax]     ## and char there
      6 test    al, al                  ## NUL char?
        jne     .L3                     ## no, loop back
        mov     rax, QWORD PTR -24[rbp] ## yes, load srcPtr
        movzx   edx, BYTE PTR [rax]     ## and char there
        mov     rax, QWORD PTR -32[rbp] ## load destPtr
        mov     BYTE PTR [rax], dl      ## store NUL
        mov     eax, DWORD PTR -4[rbp]  ## return count;
        pop     rbp
        ret
        .size   toUpper, .-toUpper
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0 "
        .section        .note.GNU-stack,"",@progbits
```

在进入 `while` 循环之前，`toUpper` 将源文本字符串的地址加载到 `rax` 中。`movzx` 指令用存储在该地址处的字节（`BYTE PTR`）覆盖 `rax` 中的地址，并将寄存器的高 56 位清零。`and` 指令使用立即数 `-33` = `0xdf`，这是将第 5 位置为 `0` 的掩码，从而确保字符为大写。对于输入文本字符串中每个不是 `NUL` 字符的字符，此代码序列都会在 `while` 循环中重复执行。

如前所述，将字符视为位模式而非数值，既适用于将小写字母转换为大写字母，也适用于让大写字母保持不变。

清单 16-4 引入了另一条逻辑指令 `and`。我们已经在第 11 章看到了 `xor` 指令。让我们再来看看两条逻辑指令：`and` 和 `or`。

### 逻辑指令

*逻辑指令*按位工作。也就是说，它们对两个操作数在对应位位置上的各个单独位进行操作。最常见的两条逻辑指令是 AND 和 OR。

#### `and` — 逻辑与

1. 对两个值执行按位 AND 运算。
2. `and reg1, reg2` 对寄存器 `reg1` 和 `reg2` 中的值执行按位 AND 运算，这两个寄存器可以是同一个，也可以是不同的。结果保留在 `reg1` 中。
3. `and reg, mem` 对寄存器中的值和内存中的值执行按位 AND 运算，结果保留在寄存器中。
4. `and mem, reg` 对内存中的值和寄存器中的值执行按位 AND 运算，结果保留在内存中。
5. `and reg, imm` 对寄存器中的值和常量 `imm` 执行按位 AND 运算，结果保留在寄存器中。
6. `and mem, imm` 对内存中的值和常量 `imm` 执行按位 AND 运算，结果保留在内存中。

`and` 指令对源值和目标值执行按位 AND 运算，结果保留在目标操作数中。`rflags` 寄存器中的 `SF`、`ZF` 和 `PF` 标志位根据结果设置，`OF` 和 `CF` 标志位被设置为 `0`，`AF` 标志位未定义。

#### `or` — 逻辑或

1. 对两个值执行按位包含或运算。
2. `or reg1, reg2` 对寄存器 `reg1` 和 `reg2` 中的值执行按位包含或运算，这两个寄存器可以是同一个，也可以是不同的。结果保留在 `reg1` 中。
3. `or reg, mem` 对寄存器中的值和内存中的值执行按位包含或运算，结果保留在寄存器中。
4. `or mem, reg` 对内存中的值和寄存器中的值执行按位包含或运算，结果保留在内存中。
5. `or reg, imm` 对寄存器中的值和常量 `imm` 执行按位包含或运算，结果保留在寄存器中。
6. `or mem, imm` 对内存中的值和常量 `imm` 执行按位包含或运算，结果保留在内存中。

`or` 指令对源值和目标值执行按位包含或运算，结果保留在目标操作数中。`rflags` 寄存器中的 `SF`、`ZF` 和 `PF` 标志位根据结果设置，`OF` 和 `CF` 标志位被设置为 `0`，`AF` 标志位未定义。

### 汇编语言中的位掩码

我们将在汇编语言版本中使用相同的掩码算法，但我们将使用标识符，以便更容易地看出正在发生什么。清单 16-5 展示了用汇编语言编写的 `main` 函数。
