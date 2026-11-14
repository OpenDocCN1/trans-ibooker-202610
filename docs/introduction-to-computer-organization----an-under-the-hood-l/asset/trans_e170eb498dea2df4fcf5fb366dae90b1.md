
# 汇编语言示例

## 列表 16-5：`main` 函数 (`upperCase.s`)

```assembly
# upperCase.s
# 将用户输入的字母文本字符串转为全大写
        .intel_syntax noprefix
# 栈帧
        .equ    myString,-64
        .equ    canary,-8
        .equ    localSize,-64
# 有用的常量
        .equ    upperMask,0xdf
        .equ    MAX,50                # 字符缓冲区限制
        .equ    NUL,0
# 常量数据
        .section  .rodata
        .align  8
prompt:
        .string "Enter up to 50 alphabetic characters: "
message:
        .string "All upper: "
newLine:
        .string "\n"
# 代码
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp                   # 保存帧指针
        mov     rbp, rsp              # 设置新帧指针
        add     rsp, localSize        # 为局部变量预留空间
        mov     rax, qword ptr fs:40  # 获取 canary
        mov     qword ptr canary[rbp], rax

        lea     rdi, prompt[rip]      # 提示用户
        call    writeStr
        
        mov     esi, MAX              # 限制用户输入长度
        lea     rdi, myString[rbp]    # 存放输入的位置
        call    readLn

        lea     rsi, myString[rbp]    # 目标字符串
        lea     rdi, myString[rbp]    # 源字符串
        call    toUpper
        
        lea     rdi, message[rip]     # 提示用户
        call    writeStr

        lea     rdi, myString[rbp]    # 结果
        call    writeStr
        lea     rdi, newLine[rip]     # 格式化换行
        call    writeStr

        mov     eax, 0                # 返回 0;
        mov     rcx, canary[rbp]      # 取回保存的 canary
        xor     rcx, fs:40            # 并检查它
        je      goodCanary
        call    __stack_chk_fail@PLT  # canary 损坏
goodCanary:
        mov     rsp, rbp              # 恢复栈指针
        pop     rbp                   # 恢复调用者帧指针
        ret
```

列表 16-5：将小写字母字符转换为大写的 `main` 函数的汇编语言版本。

我们为50个字符预留了足够的内存空间。为 `canary` 值再预留8字节共计58字节，但我们需要保持栈指针为16的倍数，因此为局部变量分配了64字节。我们将 `char` 数组的地址同时作为源和目标传递给 `toUpper` 函数，因此它会用新值替换数组中的原始值。

## 列表 16-6：`toUpper` 函数 (`toUpper.s`)

```assembly
# toUpper.s
# 将 C 字符串中的字母字符转换为大写。
# 调用约定：
#   rdi <- 指向源字符串的指针
#   rsi <- 指向目标字符串的指针
#   返回处理的字符数。
        .intel_syntax noprefix

# 栈帧
        .equ    count,-4
        .equ    localSize,-16
# 有用的常量
        .equ    upperMask,0xdf
        .equ    NUL,0
# 代码
        .text
        .globl  toUpper
        .type   toUpper, @function
toUpper:
        push    rbp                   # 保存帧指针
        mov     rbp, rsp              # 设置新帧指针
        add     rsp, localSize        # 为局部变量预留空间
        
        mov     dword ptr count[rbp], 0
whileLoop:
        mov     al, byte ptr [rdi]    # 从源读取字符
        and     al, upperMask         # 不，确保它是大写
        mov     byte ptr [rsi], al    # 字符写入目标
        cmp     al, NUL               # 是否为结束符？
        je      allDone               # 是，全部完成
        inc     rdi                   # 增加
        inc     rsi                   #      指针
        inc     dword ptr count[rbp]  #      和计数器
        jmp     whileLoop             # 继续循环
allDone:
        mov     byte ptr [rsi], al    # 以 NUL 结尾
        mov     eax, dword ptr count[rbp] # 返回计数

        mov     rsp, rbp              # 恢复栈指针
        pop     rbp                   # 恢复调用者帧指针
        ret
```

列表 16-6：用汇编语言编写的 `toUpper` 函数。

编译器使用栈上的红区存放局部变量，但我更倾向于创建显式的栈帧。我们不必将传递给该函数的源地址和目标地址保存在栈的局部变量区域，而是可以直接使用传递地址时所用的寄存器。

编译器使用 `movzx` 指令将 `rax` 寄存器中未用于处理每个字符的部分清零。我更倾向于使用 `rax` 寄存器的字节部分 `al` 来处理字符，因为这是正确的大小。请注意，这会保留 `rax` 中的高56位原样，但如果我们在算法处理字符时始终只使用 `al`，这将无关紧要。在我看来，使用 `cmp` 指令而不是 `test` 来检查终止字符 `NUL` 似乎更自然。而且，如前所述，别忘了包含 `NUL` 字符。

# 位移操作

有时能够将变量中的所有位向左或向右移动很有用。如果变量是整数，将所有位向左移动一位相当于将整数乘以二，向右移动一位相当于除以二。使用左/右移位来实现乘以/除以2的幂次非常高效。

## C语言中的位移操作

我们将通过一个程序来讨论移位操作，该程序从键盘读取以十六进制输入的整数并将其存储为 `long int`。程序最多读取八个字符：`'0'`…`'f'`。每个字符采用8位ASCII码，代表一个4位整数（0-15）。我们的程序从一个值为0的64位整数开始。从最高位的十六进制字符（即用户输入的第一个字符）开始，程序将8位ASCII码转换为对应的4位整数。我们将64位整数的累积值向左移动四位，为下一个4位整数腾出空间，然后将新的4位整数值加到累积值上。

列表 16-7、16-8 和 16-9 展示了该程序。

## 列表 16-7：`convertHex.c`

```c
/* convertHex.c
 * 从用户获取十六进制数并将其存储为 long int。
 */
#include <stdio.h>
#include "writeStr.h"
#include "readLn.h"
#include "hexToInt.h"

#define MAX 20

int main()
{
  char theString[MAX];
  long int theInt;
   
  writeStr("Enter up to 16 hex characters: ");
  readLn(theString, MAX);

  hexToInt(theString, &theInt);
  printf("%lx = %li\n", theInt, theInt);
  return 0;
}
```

列表 16-7：将十六进制转换为 `long int` 的程序。

程序分配一个 `char` 数组用于存储用户输入的字符串，以及一个 `long int` 用于存放转换后的值。`long int` 数据类型的大小取决于运行环境的操作系统和硬件。在我们的环境中，它是64位。读取用户输入字符串后，`main` 函数调用 `hexToInt` 进行实际转换，传递输入文本字符串的地址和存放结果变量的地址。

`printf` 函数将 `theInt` 转换回字符格式以在屏幕上显示。`%lx` 格式代码指示 `printf` 以十六进制显示整个 `long int`（在我们的环境中为64位）。`%li` 格式代码以十进制显示 `long int`。

## 列表 16-8：`hexToInt.h`

```c
/* hexToInt.h
 * 将十六进制字符字符串转换为 long int。
 * 返回转换的字符数。
 */
 
#ifndef HEXTOINT_H
#define HEXTOINT_H
int hexToInt(char *stringPtr, long int *intPtr);
#endif
```

列表 16-8：`hexToInt` 函数的头文件。
