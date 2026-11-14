
# 将十进制文本字符串转换为整数

让我们修改清单 [16-7](#c16.xhtml#listing16-7)、[16-8](#c16.xhtml#listing16-8) 和 [16-9](#c16.xhtml#listing16-9) 中的 C 程序，以将数字文本字符串转换为十进制整数。当从十六进制文本字符串转换时，我们将累积值向左移动四位，从而将其乘以 16。我们将对转换十进制文本字符串使用相同的算法，但改为乘以 10 而不是 16。清单 [16-13](#c16.xhtml#listing16-13)、[16-14](#c16.xhtml#listing16-14) 和 [16-15](#c16.xhtml#listing16-15) 展示了该 C 程序。

## 清单 16-13：主程序 (`convertDec.c`)

```c
/* convertDec.c
 * 从键盘读取十进制数并显示其
 * 在十六进制中的存储方式。
 */

#include <stdio.h>
#include "writeStr.h"
#include "readLn.h"
#include "decToUInt.h"
#define MAX 20
int main()
{
  char theString[MAX];
  unsigned int theInt;
   
  writeStr("Enter an unsigned integer: ");
  readLn(theString, MAX);

  decToUInt(theString, &theInt);
  printf("\"%s\" is stored as 0x%x\n", theString, theInt);
   
  return 0;
}
```

此十进制转换程序的 `main` 函数与上一节中十六进制转换程序的函数几乎相同。主要区别在于我们显示用户输入的原始文本字符串，并展示生成的 `unsigned int` 如何以十六进制存储（参见清单中的标注 1）。

## 清单 16-14：头文件 (`decToUInt.h`)

执行转换的函数 `decToUInt` 接受一个指向文本字符串的指针和一个指向用于主要输出的变量的指针，并返回转换的字符数。

```c
/* decToUInt.h
 * 将十进制字符字符串转换为无符号整数。
 * 返回字符数。
 */
 
#ifndef DECTOUINT_H
#define DECTOUINT_H
int decToUInt(char *stringPtr, unsigned int *intPtr);
#endif
```

## 清单 16-15：实现 (`decToUInt.c`)

```c
/* decToUInt.c
 * 将十进制字符字符串转换为无符号整数。
 * 返回字符数。
 */

#include <stdio.h>
#include "decToUInt.h"
#define INTMASK 0xf

int decToUInt(char *stringPtr, unsigned int *intPtr)
{
  int radix = 10;
  char current;
  int count = 0;
  
  *intPtr = 0;
  current = *stringPtr;
  while (current != '\0')
  {
    current = current & INTMASK;        // 1
    *intPtr = *intPtr * radix;          // 2
    *intPtr += current;                 // 3
    stringPtr++;
    count++;
    current = *stringPtr;
  }
  return count;
}
```

该算法与十六进制转换算法的第一个区别在于，我们不需要检查字母字符，因为数字字符的 ASCII 码从 0 到 9 是连续的。由于数字的 ASCII 码的低四位与其所代表的整数值相同，我们只需屏蔽掉高四位（标注 1）。

正如我们所见，在处理十六进制时，我们可以通过移位累积结果轻松为新值腾出空间，但这仅在新值的最大值是 2 的幂时有效。在处理十进制时，我们需要将累积结果乘以 10（标注 2），然后加上新值（标注 3）。

## 清单 16-16：编译器生成的汇编代码 (`decToUInt.s`)

编译器为 `decToUInt` 函数生成的汇编语言：

```asm
        .file   "decToUInt.c"
        .intel_syntax noprefix
        .text
        .globl  decToUInt
        .type   decToUInt, @function
decToUInt:
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR -24[rbp], rdi     ## save stringPtr
        mov     QWORD PTR -32[rbp], rsi     ## save intPtr
        mov     DWORD PTR -4[rbp], 10       ## radix = 10;
        mov     DWORD PTR -8[rbp], 0        ## count = 0;
        mov     rax, QWORD PTR -32[rbp]
        mov     DWORD PTR [rax], 0          ## *intPtr = 0;
        mov     rax, QWORD PTR -24[rbp]
        movzx   eax, BYTE PTR [rax]
        mov     BYTE PTR -9[rbp], al        ## load character
        jmp     .L2                         ## go to bottom
.L3:
        and     BYTE PTR -9[rbp], 15        ## convert to int
        mov     rax, QWORD PTR -32[rbp]
        mov     edx, DWORD PTR [rax]        ## load current value
        mov     eax, DWORD PTR -4[rbp]      ## load radix
        imul    edx, eax                    ## 1: times 10
        mov     rax, QWORD PTR -32[rbp]
        mov     DWORD PTR [rax], edx        ## store 10 times current
        mov     rax, QWORD PTR -32[rbp]
        mov     edx, DWORD PTR [rax]        ## load 10 times current
        movsx   eax, BYTE PTR -9[rbp]       ## 2: byte to 32 bits
        add     edx, eax                    ## 3: add in latest value
        mov     rax, QWORD PTR -32[rbp]
        mov     DWORD PTR [rax], edx        ## *intPtr += current;
        add     QWORD PTR -24[rbp], 1       ## stringPtr++;
        add     DWORD PTR -8[rbp], 1        ## count++;
        mov     rax, QWORD PTR -24[rbp]
        movzx   eax, BYTE PTR [rax]
        mov     BYTE PTR -9[rbp], al        ## load next character
.L2:
        cmp     BYTE PTR -9[rbp], 0         ## NUL?
        jne     .L3                         ## no, keep going
        mov     eax, DWORD PTR -8[rbp]      ## yes, return count;
        pop     rbp
        ret
        .size   decToUInt, .-decToUInt
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section        .note.GNU-stack,"",@progbits
```

`decToUInt` 函数的汇编语言与清单 [16-12](#c16.xhtml#listing16-12) 中所示的 `hexToInt` 函数的汇编语言相似。主要区别在于累积结果乘以我们要转换到的基数（十进制为 10），使用 `imul` 指令（标注 1），并且在将新值加到累积结果之前（标注 3），将其类型转换为 32 位 `int`（标注 2）。

x86-64 指令集既包含无符号乘法指令 `mul`，也包含有符号乘法指令 `imul`。编译器使用有符号乘法指令将数字文本字符串转换为 `unsigned int`，这似乎有点奇怪。在查看这两条指令的细节后，我们将了解原因。

### 乘法指令

#### `imul`——有符号乘法

有符号乘法指令可以有一个、两个或三个操作数：

1.  执行有符号乘法。

2.  `imul reg` 将 `al`、`ax`、`eax` 或 `rax` 中的整数与 `reg` 中的整数相乘，结果分别留在 `ax`、`dx:ax`、`edx:eax` 或 `rdx:rax` 中。

3.  `imul mem` 将 `al`、`ax`、`eax` 或 `rax` 中的整数与 `mem` 中的整数相乘，结果分别留在 `ax`、`dx:ax`、`edx:eax` 或 `rdx:rax` 中。

4.  `imul reg1, reg2` 将 `reg1` 中的整数与 `reg2` 中的整数相乘，结果留在 `reg1` 中。`reg1` 和 `reg2` 可以是同一个寄存器。

5.  `imul reg, mem` 将 `reg` 中的整数与 `mem` 中的整数相乘，结果留在 `reg` 中。

6.  `imul reg1, reg2, imm` 将 `reg2` 中的整数与立即数 `imm` 相乘，结果留在目标寄存器 `reg1` 中。`reg1` 和 `reg2` 可以是同一个寄存器。

7.  `imul reg, mem, imm` 将 `mem` 中的整数与立即数 `imm` 相乘，结果留在 `reg` 中。

    寄存器或内存中整数的宽度必须相同。在第一种形式中，结果的宽度将是被乘数宽度的两倍，并且高位部分将进行符号扩展。在第二种和第三种形式中，*n* 位目标操作数乘以 *n* 位源操作数，低 *n* 位留在目标操作数中。在最后两种形式中，–128 ≤ `imm` ≤ +127，在乘法前将符号扩展为与源操作数和目标寄存器相同的宽度，低 *n* 位留在目标寄存器中。

