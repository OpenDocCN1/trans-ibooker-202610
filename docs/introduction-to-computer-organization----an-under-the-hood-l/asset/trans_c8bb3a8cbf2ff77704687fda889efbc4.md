
# 定点运算的汇编清单

## 清单 19-2：以英寸和十六分之一英寸显示长度

### 源代码：`displayLength.s`

```assembly
# displayLength.s
# 以英寸和 1/16 英寸显示长度。
# 调用序列：
#   edi <- 低 4 位存放十六分之一英寸的值
        .intel_syntax noprefix
# 有用的常量
        .equ  1 fractionMask, 0xf
# 栈帧
        .equ    length,-16
        .equ    localSize,-16
# 常量数据
        .section	.rodata
        .align  8
link:
        .string " "
over:
        .string "/16"
msg:
        .string "Total = "
endl:
        .string "\n"
# 代码
        .text
        .globl  displayLength
        .type   displayLength, @function
displayLength:
        push    rbp                 # 保存帧指针
        mov     rbp, rsp            # 设置新帧指针
        add     rsp, localSize      # 为局部变量分配空间
        
        mov     length[rbp], rdi    # 保存输入长度
        lea     rdi, msg[rip]       # 友好提示信息
        call    writeStr
        
        mov     edi, length[rbp]    # 原始值
      2 shr     edi, 4              # 整数部分
        call    putUInt             # 写入屏幕
        lea     rdi, link[rip]
        call    writeStr
        
        mov     edi, length[rbp]    # 原始值
      3 and     edi, fractionMask   # 小数部分
        call    putUInt             # 写入屏幕
      4 lea     rdi, over[rip]
        call    writeStr

        lea     rdi, endl[rip]
        call    writeStr

        mov     rsp, rbp            # 恢复栈指针
        pop     rbp                 # 恢复调用者帧指针
        ret
```

### 说明

我们将数字向右移动四位，以便将整数部分作为整数显示 2。使用四位掩码 1，我们屏蔽掉整数部分，并将小数部分作为另一个整数显示 3。我们添加一些文本以表明第二个整数是小数部分 4。

[清单 19-3](#c19.xhtml#listing19-3) 展示了 `main` 函数。

## 清单 19-3：将两个尺子测量值相加

### 源代码：`rulerAdd.s`

```assembly
# rulerAdd.s
# 将两个尺子测量值相加，精确到 1/16 英寸。
        .intel_syntax noprefix
# 栈帧
        .equ    x,-16
        .equ    y, -12
        .equ    canary,-8
        .equ    localSize,-16
# 常量数据
        .section	.rodata
        .align  8
endl:
        .string "\n"
# 代码
        .text
        .globl	main
        .type	main, @function
main:
        push    rbp                     # 保存帧指针
        mov     rbp, rsp                # 设置新帧指针
        add     rsp, localSize          # 为局部变量分配空间
        mov     rax, qword ptr fs:40    # 获取金丝雀值
        mov     qword ptr canary[rbp], rax

        lea     rdi, x[rbp]             # x 长度
        call    getLength
        
        lea     rdi, y[rbp]             # y 长度
        call    getLength

        mov     edi, x[rbp]             # 取回 x 长度
      1 add     edi, y[rbp]             # 加上 y 长度
        call    displayLength

        mov     eax, 0                  # 返回 0;
        mov     rcx, qword ptr canary[rbp]
        xor     rcx, qword ptr fs:40
        je      goodCanary
        call    __stack_chk_fail@plt
goodCanary:
        mov     rsp, rbp                # 恢复栈指针
        pop     rbp                     # 恢复调用者帧指针
        ret
```

### 说明

清单 19-3：将两个以英寸和十六分之一英寸为单位的测量值相加的程序

如果你查看上一节中用二进制表示小数值的公式，你大概能说服自己，整数 `add` 指令适用于整个数字，包括小数部分 1。

这个示例非常适合二进制数，但我们在计算中主要使用十进制数。正如我们在本章前面看到的，大多数十进制小数无法转换为有限位数的二进制，需要四舍五入。在下一节中，我们将讨论在使用十进制小数时如何避免舍入错误。

## 当小数部分为十进制时

让我们思考一下在这里如何处理定点格式中的小数部分。当我们从键盘读取整数部分时，我们将其向左移动四个位位置，为将十六分之一的数量加到这个 `int` 中留出空间。

我们实际上创建了一个 32 位数字，其二进制点位于第 5 位和第 4 位之间（位编号为 4 和 3）。这之所以可行，是因为小数部分是 2 的负幂之和。

另一种思考之前如何处理分数的方法是，四位移位将数字乘以 16。处理十进制时我们将采用这种方法：将数字乘以 10 的倍数，使最小值变为整数。

我们将通过一个程序来探索这种方法，该程序将两个美元值相加，精确到 1/100 美元。与清单 [19-1](#c19.xhtml#listing19-1)、[19-2](#c19.xhtml#listing19-2) 和 [19-3](#c19.xhtml#listing19-3) 中的尺子测量程序一样，我们将从从键盘读取货币值的函数 `getMoney` 开始，如 [清单 19-4](#c19.xhtml#listing19-4) 所示。

## 清单 19-4：读取货币值（美元和美分）

### 源代码：`getMoney.s`

```assembly
# getMoney.s
# 获取美元和美分。
# 输出 32 位值，以美分表示货币。
# 调用序列：
#   rdi <- 指向长度的指针
        .intel_syntax noprefix
# 有用的常量
        .equ  1 dollar2cents, 100
# 栈帧
        .equ    moneyPtr,-16
        .equ    dollars,-8
        .equ    cents,-4
        .equ    localSize,-16
# 常量数据
        .section	.rodata
        .align  8
prompt:
        .string "Enter amount\n"
dollarsPrompt:
        .string "    Dollars: "
centsPrompt:
        .string "      Cents: "
# 代码
        .text
        .globl  getMoney
        .type   getMoney, @function
getMoney:
        push    rbp                     # 保存帧指针
        mov     rbp, rsp                # 设置新帧指针
        add     rsp, localSize          # 为局部变量分配空间
        
        mov     moneyPtr[rbp], rdi      # 保存输出指针

        lea     rdi, prompt[rip]        # 提示用户
        call    writeStr

        lea     rdi, dollarsPrompt[rip] # 询问美元
        call    writeStr
        lea     rdi, dollars[rbp]       # 获取美元
      2 call    getUInt
        lea     rdi, centsPrompt[rip]   # 询问美分
        call    writeStr
        lea     rdi, cents[rbp]         # 获取美分
      3 call    getUInt
        
        mov     eax, dword ptr dollars[rbp] # 取回美元
        mov     ecx, dollar2cents           # 将美元缩放为美分
      4 mul     ecx
        mov     ecx, dword ptr cents[rbp]   # 取回美分
        add     eax, ecx                    # 加上美分
        mov     rcx, moneyPtr[rbp]          # 加载输出指针
        mov     [rcx], eax                  # 输出

        mov     rsp, rbp                    # 恢复栈指针
        pop     rbp                         # 恢复调用者帧指针
        ret
```

### 说明

清单 19-4：从键盘读取美元和美分并转换为美分的函数

与尺子测量程序一样，我们将整数部分 2 和小数部分 3 作为整数读取。如解释所述，由于缩放因子是 10 的倍数，我们需要将整数部分 4 乘以缩放因子 1，而不仅仅是移位。

显示缩放数字的函数需要逆转该过程以分离整数和小数部分。我们将在 `displayMoney` 函数中执行此操作，如 [清单 19-5](#c19.xhtml#listing19-5) 所示。
