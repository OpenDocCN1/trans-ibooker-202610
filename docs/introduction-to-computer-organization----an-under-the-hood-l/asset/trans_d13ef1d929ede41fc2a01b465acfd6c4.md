
# 清单 14-15：`addNine` 函数的编译器生成汇编语言

```
.file   "addNine.c"
.intel_syntax noprefix
.text
.globl  addNine
.type   addNine, @function
addNine:
        push    rbp
      1 mov     rbp, rsp
      2 mov     DWORD PTR -20[rbp], edi   ## store a locally
        mov     DWORD PTR -24[rbp], esi   ## store b locally
        mov     DWORD PTR -28[rbp], edx   ## store c locally
        mov     DWORD PTR -32[rbp], ecx   ## store d locally
        mov     DWORD PTR -36[rbp], r8d   ## store e locally
        mov     DWORD PTR -40[rbp], r9d   ## store f locally
        mov     edx, DWORD PTR -20[rbp]   ## sum = a
        mov     eax, DWORD PTR -24[rbp]
        add     edx, eax                  ## sum += b
        mov     eax, DWORD PTR -28[rbp]
        add     edx, eax                  ## sum += c
        mov     eax, DWORD PTR -32[rbp]
        add     edx, eax                  ## sum += d
        mov     eax, DWORD PTR -36[rbp]
        add     edx, eax                  ## sum += e
        mov     eax, DWORD PTR -40[rbp]
        add     edx, eax                  ## sum += f
      3 mov     eax, DWORD PTR 16[rbp]    ## from arg list
        add     edx, eax                  ## sum += g
        mov     eax, DWORD PTR 24[rbp]
        add     edx, eax                  ## sum += h
        mov     eax, DWORD PTR 32[rbp]
        add     eax, edx                  ## sum += i
        mov     DWORD PTR -4[rbp], eax
        mov     eax, DWORD PTR -4[rbp]
        pop     rbp
        ret
        .size   addNine, .-addNine
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section        .note.GNU-stack,"",@progbits
```

在 `addNine` 函数的序言建立其帧指针 1 后，其栈帧处于 [图 14-3](#c14.xhtml#figure14-3) 所示的状态。

![图 14-3：addNine 函数的栈帧](image_fi/500099c14/f14003.png)

**图 14-3：`addNine` 函数的栈帧**

`addNine` 函数将寄存器中传递的参数存储在栈的红区 2 中。然后它开始计算总和。当处理通过栈传递的参数时，它会根据需要 3 将每个参数加载到 `eax` 寄存器中。每个栈参数相对于 `rbp` 寄存器的偏移量是从 [图 14-3](#c14.xhtml#figure14-3) 的图示中获得的。

你已经看到，子函数直接访问栈上的参数，而不是将它们弹出。调用函数可以直接将它们放在栈上，而不是压入它们。在下一节中，我将展示如何在直接用汇编语言编写 `sumNine` 程序时实现这一点。

## 直接在栈上存储参数

`push` 操作有些低效。它执行两个操作：从 `rsp` 寄存器减去 8，并在更新后的 `rsp` 寄存器地址处存储一个值。当在栈上放置多个值时，从栈指针减去足够的空间以容纳所有值，然后直接将每个值存储在栈上，效率会稍高一些。我们将在直接用汇编语言编写 `sumNine` 程序时使用此技术。

[清单 14-16](#c14.xhtml#listing14-16) 展示了我们汇编语言版本 `sumNine` 程序的 `main` 函数。

# 清单 14-16：直接用汇编语言编写的 `sumNine` 程序的 `main` 函数

```
# sum9Ints.s
# Sums the integers 1 - 9.
.intel_syntax noprefix

# Stack frame
#   passing arguments on stack (rsp)
#     need 3x8 = 24 -> 32 bytes
      1 .equ    seventh,0
        .equ    eighth,8
        .equ    ninth,16
        .equ    argSize,-32
#   local vars (rbp)
#     need 10x4 = 40 -> 48 bytes for alignment
        .equ    i,-4
        .equ    h,-8
        .equ    g,-12
        .equ    f,-16
        .equ    e,-20
        .equ    d,-24
        .equ    c,-28
        .equ    b,-32
        .equ    a,-36
        .equ    total,-40
        .equ    localSize,-48
# Read only data
.section  .rodata
format:
        .string "The sum is %i\n"
# Code
.text
.globl  main
.type   main, @function
main:
        push    rbp                 # save frame pointer
        mov     rbp, rsp            # set new frame pointer
        add     rsp, localSize      # for local var.

        mov     dword ptr a[rbp], 1 # initialize values
        mov     dword ptr b[rbp], 2 #     etc...
        mov     dword ptr c[rbp], 3
        mov     dword ptr d[rbp], 4
        mov     dword ptr e[rbp], 5
        mov     dword ptr f[rbp], 6
        mov     dword ptr g[rbp], 7
        mov     dword ptr h[rbp], 8
        mov     dword ptr i[rbp], 9

      2 add     rsp, argSize        # space for arguments
        mov     eax, i[rbp]         # load i
      3 mov     ninth[rsp], rax     #   9th argument
        mov     eax, h[rbp]         # load h
        mov     eighth[rsp], rax    #   8th argument
        mov     eax, g[rbp]         # load g
        mov     seventh[rsp], rax   #   7th argument
        mov     r9d, f[rbp]         # f is 6th
        mov     r8d, e[rbp]         # e is 5th
        mov     ecx, d[rbp]         # d is 4th
        mov     edx, c[rbp]         # c is 3rd
        mov     esi, b[rbp]         # b is 2nd
        mov     edi, a[rbp]         # a is 1st
        call    addNine
      4 sub     rsp, argSize        # remove arguments
        mov     total[rbp], eax     # total = sumNine(...)

        mov     esi, total[rbp]     # show result
        lea     rdi, format[rip]
        mov     eax, 0
        call    printf@plt

        mov     eax, 0              # return 0;
        mov     rsp, rbp            # restore stack pointer
        pop     rbp                 # and caller frame pointer
        ret
```

我们将通过使用 [图 14-2](#c14.xhtml#figure14-2) 中的栈图来确定 `seventh`、`eighth` 和 `ninth` 参数的标识符值 1 来开始设计。为我们在栈上为参数所需的空间量创建一个标识符也很方便，确保它是 16 的倍数。

我们通过首先从 `rsp` 2 减去适当的数量，在栈上为超出我们将通过寄存器传递的六个参数之外的三个参数分配空间，来开始对 `addNine` 的调用。为每个参数创建了标识符后，直接将这三个参数中的每一个存储在我们刚刚分配的区域中 3 就变得很简单。然后，当 `addNine` 子函数返回到我们的函数时，我们需要通过加上开始调用序列时从 `rsp` 减去的相同数量 4，有效地从栈顶删除参数区域。

接下来，让我们看看 `addNine` 函数 [清单 14-17](#c14.xhtml#listing14-17) 是如何访问通过栈传递的参数的。
