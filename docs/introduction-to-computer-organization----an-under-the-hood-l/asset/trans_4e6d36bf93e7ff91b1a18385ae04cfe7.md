
## 清单 16-24：`intToUDec` 函数的汇编语言版本

```assembly
# intToUDec.s
# 创建表示无符号 32 位整数的字符串。
# 调用序列：
#   rdi <- 指向结果字符串的指针
#   esi <- 无符号整数
        .intel_syntax noprefix

# 栈帧
        .equ    reverseArray,-32
        .equ    canary,-8
        .equ    localSize,-32
# 有用的常量
        .equ    DECIMAL,10
        .equ    ASCII,0x30
        .equ    NUL,0

# 代码
        .text
        .globl  intToUDec
        .type   intToUDec, @function
intToUDec:
        push    rbp                     # 保存帧指针
        mov     rbp, rsp                # 设置新帧指针
        add     rsp, localSize          # 为局部变量分配空间
        mov     rax, qword ptr fs:40    # 获取金丝雀值
        mov     qword ptr canary[rbp], rax

        lea     rcx, reverseArray[rbp]  # 指针
        mov     byte ptr [rcx], NUL     # 字符串终止符
        inc     rcx                     # 已存储一个字符
        
        mov     eax, esi                # 待表示的整数
        mov     r8d, DECIMAL            # 所用的基数
convertLoop:
      1 mov     edx, 0                  # 用于余数
        div     r8d                     # 商和余数
      2 or      dl, ASCII               # 转换为字符
      3 mov     byte ptr [rcx], dl      # 追加到字符串
        inc     rcx                     # 下一个字符位置
        cmp     eax, 0                  # 全部完成？
        ja      convertLoop             # 否，继续
reverseLoop:        
        dec     rcx                     # 是，反转字符串
        mov     dl, byte ptr [rcx]      # 一次一个字符
        mov     byte ptr [rdi], dl
        inc     rdi                     # 指向目标字符串
        cmp     dl, NUL                 # 是 NUL 吗？
        jne     reverseLoop             # 否，继续
        
        mov     eax, 0                  # 返回 0;
        mov     rcx, canary[rbp]        # 取回保存的金丝雀值
        xor     rcx, fs:40              # 并检查它
        je      goodCanary
        call    __stack_chk_fail@PLT    # 金丝雀值错误
goodCanary:
        mov     rsp, rbp                # 恢复栈指针
        pop     rbp                     # 和调用者的帧指针
        ret
```

### 说明

我们汇编语言版本的主要区别在于，我们利用了 `div` 指令将商留在 `eax` 中、余数留在 `edx` 中这一已知事实 2。由于转换基数为 10，我们知道余数将始终在 0–9 范围内。因此，余数可以很容易地转换为其对应的数字 ASCII 码 2。将新转换的字符追加到我们正在创建的文本字符串后 3，`edx` 寄存器会在下一次执行 `div` 指令前被清零 1。

除法比乘法耗时更长。人们发明了不使用除法即可确定低位十进制数字的算法。当除以常数时，一种技术是利用移位比除法快得多这一事实。例如，在我们的 `intToUDec` 函数中，我们要除以 10。当把一个数 *x* 除以 10 时，考虑以下方程：

<p class="MathGraphic"><img src="image_fi/500099c16/c16m0001.png" alt="c16m0001"/></p>

![](image_fi/500099c16/c16eq001.png)

<p class="MathEquation"><span class="MText">x</span><span class="mathvariant">10</span><span class="MText"> </span><span class="Malign"/><span class="mathalign">= </span><span class="MEquation"><span class="mathvariant">2</span></span><sup class="MathSuperscript">n</sup><span class="mathvariant">10</span><span class="mathvariant"> × </span><span class="mathalign">x</span><span class="MEquation"><span class="mathvariant">2</span></span><sup class="MathSuperscript">n</sup><br/><span class="Malign"/><span class="mathalign">= </span><span class="Md"><span class="MDelimiter">(</span><span class="MEquation"><span class="MEquation"><span class="mathvariant">2</span></span><sup class="MathSuperscript">n</sup><span class="mathvariant">10</span><span class="mathvariant"> × </span><span class="MText">x</span></span><span class="MDelimiter">)</span></span><span class="MEquation"><span class="mathvariant">2</span></span><sup class="MathSuperscript">n</sup></p>

现在，如果我们计算常数 2^(*n*)/10，就可以用这个新常数乘以 *x*，然后通过将乘法结果右移 *n* 位来进行除法。详细内容超出了本书范围，但你可以通过查看编译器为 [清单 16-21](#c16.xhtml#listing16-21) 中的 `intToUDec` 函数生成的汇编语言（使用 `-O1` 优化选项）来了解其工作原理。

## 你学到了什么

1.  **位掩码** 我们可以使用位逻辑指令直接更改变量中的位模式。
2.  **位移动** 变量中的位可以左移或右移，相当于乘以或除以 2 的倍数。
3.  **乘法** 有符号乘法指令有多种形式，比只有一种形式的无符号乘法指令更灵活。
4.  **除法** 有符号和无符号除法指令都会产生商和余数。有符号整数除法稍微复杂一些。
5.  **在二进制存储与字符显示之间转换数字** 当数字以二进制系统存储时，算术运算更容易，但键盘输入和屏幕显示使用相应的字符格式。

我们已经介绍了组织程序流程和对数据项执行算术或逻辑运算的方法。组织数据是设计计算算法的另一个重要部分。在下一章中，我们将介绍组织数据的两种最基本方式：数组和记录。

# 第 17 章：数据结构

![](image_fi/book_art/chapterart.png)

编程的一个重要部分是确定如何组织数据。在本章中，我们将介绍组织数据的两种最基本方式：数组，可用于仅分组相同数据类型的数据项；记录，可用于分组不同数据类型的数据项。

正如你将看到的，这些数据组织方式决定了我们如何访问其中的各个数据项。两者都需要两个寻址项来定位一个数据项。由于数组中的数据项类型相同，我们可以通过数组名称加上该项的索引号来访问单个数据项。访问记录中的单个数据项则需要记录名称和记录中该数据项的名称。

## 数组

*数组* 是由相同数据类型的数据元素按顺序排列组成的集合。我们可以通过数组名称配合一个 *索引* 值来访问数组中的单个元素，该索引值指定了该元素相对于数组开头的编号。在前几章中，我们使用过 `char` 数组来存储作为文本字符串的 ASCII 字符。数组中的每个元素类型相同，都是 `char`，占用一个字节。在我们的应用中，我们是按顺序访问每个字符的，所以我们从指向第一个 `char` 的指针开始，只需将其加 1 即可访问后续的每个 `char`。我们不需要索引来定位文本字符串数组中的每个 `char`。

在本章中，我们将介绍 `int` 数组，其数组中的每个数据元素占用四字节。如果我们从指向第一个元素的指针开始，则需要将其加 4 才能访问后续的每个元素。但使用数组索引号来访问每个单独元素要容易得多。你将看到索引号如何转换为地址偏移量，以相对于数组开头访问数组元素。你还将看到 C 语言传递数组给其他函数的方式与其他数据项不同。

### C 语言中的数组
