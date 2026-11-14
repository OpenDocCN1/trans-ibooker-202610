

# 图 14-5：C 语言变量三大类的作用域与生命周期

[图 14-5](#c14.xhtml#figure14-5) 展示了该程序的一次运行。

![图 14-5：C 语言变量三大类的作用域与生命周期](image_fi/500099c14/f14005.png)

该程序显示 `main` 中的 `x` 和 `y` 变量与 `addConst` 中的 `x` 和 `y` 变量是不同的。我们还可以看到，`addConst` 中的 `x` 每次函数被调用时都会重新初始化。但 `addConst` 中的 `y` 变量仅在第一次调用函数时获得其初始值，且对该变量加 1,000 的操作会在两次函数调用之间持续保留。我们还可以看到，`main` 和 `addConst` 都在使用同一个（全局）`z`。

我对 `varLife` 程序的汇编语言解决方案，清单 [14-23](#c14.xhtml#listing14-23) 和 [14-24](#c14.xhtml#listing14-24)，与编译器生成的代码相似，但我使用了更有意义的标签和名称，使代码更易于人类阅读。

## 清单 14-23：`varLife` 程序中 `main` 函数的汇编语言版本

```
# varLife.s
# 比较自动、静态和全局变量的作用域与生命周期。
        .intel_syntax noprefix

# 栈帧
        .equ    x,-8
        .equ    y,-4
        .equ    localSize,-16
# 有用的常量
        .equ    INITx,12
        .equ    INITy,34
        .equ    INITz,56
        .section    .rodata
        .align  8
tableHead1:
        .string "           automatic   static   global"
tableHead2:
        .string "                   x        y        z"
format:
        .string "In main:%12i %8i %8i\n"
# 定义全局变量
        .data
        .align  4
        .globl  z
        .type   z, @object
z:
      1 .int    INITz   # 初始化全局变量
# 代码
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp                     # 保存帧指针
        mov     rbp, rsp                # 设置新帧指针
        add     rsp, localSize          # 为局部变量预留空间

        mov   2 dword ptr x[rbp], INITx # 初始化局部变量
        mov     dword ptr y[rbp], INITy

        lea     rdi, tableHead1[rip]    # 打印表头
        call    puts@plt
        lea     rdi, tableHead2[rip]
        call    puts@plt
        mov   3 ecx, z[rip]             # 打印变量
        mov     edx, y[rbp]
        mov     esi, x[rbp]
        lea     rdi, format[rip]
        mov     eax, 0
        call    printf@plt

        call    addConst                # 对变量进行加法
        call    addConst

        mov     ecx, z[rip]             # 打印变量
        mov     edx, y[rbp]
        mov     esi, x[rbp]
        lea     rdi, format[rip]
        mov     eax, 0
        call    printf@plt

        mov     eax, 0                  # return 0;
        mov     rsp, rbp                # 恢复栈指针
        pop     rbp                     # 并恢复调用者帧指针
        ret
```

`.int` 指令与编译器使用的 `.long` 相同。在我们的环境中，两者都以小端序发射 32 位整数。我倾向于使用 `.int` 指令而非 `.long` 来指定 `int` 值 1，但这只是个人风格。

由于此函数中的 `x` 和 `y` 是自动局部变量，它们位于栈帧中并相对于帧指针 2 访问，而 `z` 是全局变量，因此相对于指令指针 3 访问。

## 清单 14-24：`varLife` 程序中 `addConst` 函数的汇编语言版本

当我们用汇编语言编写 `addConst` 函数时，我们将使用一个更有意义的标识符来修饰静态局部变量的名称，如 [清单 14-24](#c14.xhtml#listing14-24) 所示。

```
# addConst.s
# 向自动、静态、全局变量添加常量。
        .intel_syntax noprefix

# 栈帧
        .equ  1 x,-4
        .equ    localSize,-16
# 有用的常量
        .equ    ADDITION,1000
        .equ    INITx,78
        .equ    INITy,90
# 常量数据
        .section  .rodata
        .align  8
format:
        .string "In addConst:%8i %8i %8i\n"
# 定义静态变量
        .data
        .align  4
        .type 2 y_addConst, @object
y_addConst:
      3 .int    INITy

# 代码
        .text
        .globl  addConst
        .type   addConst, @function
addConst:
        push    rbp                     # 保存帧指针
        mov     rbp, rsp                # 设置新帧指针
        add     rsp, localSize          # 为局部变量预留空间
        mov     dword ptr x[rbp], INITx # 初始化

        add     dword ptr x[rbp], ADDITION # 对变量加法
        add   4 dword ptr y_addConst[rip], ADDITION
        add     dword ptr z[rip], ADDITION

        mov     ecx, z[rip]             # 打印变量
        mov     edx, y_addConst[rip]
        mov     esi, x[rbp]
        lea     rdi, format[rip]
        mov     eax, 0                  # 无浮点数
        call    printf@plt

        mov     eax, 0                  # return 0;
        mov     rsp, rbp                # 恢复栈指针
        pop     rbp                     # 并恢复调用者帧指针
        ret
```

请仔细注意，这里的自动局部变量 `x` 是在 `addConst` 的栈帧中创建的 1，因此它不同于在 `main` 的栈帧中创建的 `x`。我选择通过将函数名追加到变量名后面来修饰静态局部变量 `y`，从而确保该变量拥有唯一的名称 2。由于 `y` 是静态的，它被放置在 `.data` 节 3 中，并相对于指令指针 4 访问。

## 表 14-1：程序某些组件的内存特征

[表 14-1](#c14.xhtml#table14-1) 总结了程序中一些最常见组件的内存特征。

| 在程序中的角色 | 内存段 | 访问权限 | 生命周期 |
| --- | --- | --- | --- |
| 自动局部变量 | 栈 | 读和写 | 函数 |
| 常量 | 文本段 | 只读 | 程序 |
| 指令 | 文本段 | 只读 | 程序 |
| 静态局部变量 | 数据段 | 读和写 | 程序 |
| 全局变量 | 数据段 | 读和写 | 程序 |

[表 14-1](#c14.xhtml#table14-1) 中的内存段是指由操作系统创建的段。常量被放置在文本段中，因为操作系统禁止程序向文本段写入。变量需要位于可读可写的段中。

## 表 14-2：一些常见的汇编器内存指令

[表 14-2](#c14.xhtml#table14-2) 总结了一些用于控制程序组件在内存中位置的更常见的汇编器指令。

| 指令 | 内存段 | 效果 |
| --- | --- | --- |
| `.text` | 文本段 | 后跟指令 |
| `.rodata` | 文本段 | 后跟常量数据 |
| `.string "string"`, … | 文本段 | 字符数组，每个以 `NUL` 结尾 |
| `.ascii "string"`, … | 文本段 | 字符数组 |
| `.asciz "string"`, … | 文本段 | 字符数组，每个以 `NUL` 结尾 |
| `.data` | 数据段 | 后跟变量数据 |
| `.bss` | 数据段 | 后跟数据内存初始化为零 |
| `.comm label, size` | 数据段 | 分配 `size` 字节的未初始化数据内存 |
| `.byte expression`, … | 数据段 | 初始化内存，每个表达式占一字节 |
| `.int expression`, … | 数据段 | 初始化内存，每个表达式占一个 `int` |
| `.long expression`, … | 数据段 | 初始化内存，每个表达式占一个 `int` |

`.string`、`.ascii` 和 `.asciz` 指令可以分配多个文本字符串，每个字符串用逗号分隔。`.string` 和 `.asciz` 指令在文本字符串末尾添加一个 `NUL` 字符，而 `.ascii` 不添加。

