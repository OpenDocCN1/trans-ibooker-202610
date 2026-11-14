
接下来，看看每次调用成员函数之前的两条指令。我们可以看到对象的地址被传递给了它们每一个。这是一个 `hidden argument`（隐藏参数），不会出现在 C++ 代码中。当我们在本章后面深入成员函数内部时，将会看到如何在成员函数中访问该地址。

虽然在我们编写的 C++ 代码中看不出来，但编译器会在程序流离开对象作用域的那一点生成对析构函数的调用。在一些更高级的编程技巧中，我们会显式调用析构函数，但本书不涉及这些内容。大多数时候，我们让编译器决定何时调用析构函数。

接下来我们将查看这个 `fraction` 类的构造函数、析构函数和其他成员函数。

## 定义类成员函数

虽然通常将每个 C 函数放在各自的文件中，但 C++ 源文件通常组织为包含一个类的所有函数。清单 18-5 展示了我们 `fraction` 类成员函数的定义。

```
// fraction.cpp
// Simple fraction class.

#include "fraction.hpp"
// Use char arrays because writeStr is C-style function.
char numMsg[] = "Enter numerator: ";
char denMsg[] = "Enter denominator: ";
char over[] = "/";
char endl[] = "\n";

fraction::fraction()
{
  num = 0;
  den = 1;
}

fraction::~fraction()
{
  // Nothing to do for this object
}

void fraction::get()
{
  writeStr(numMsg);   
  getInt(&num);
   
  writeStr(denMsg);
  getInt(&den);
}

void fraction::display()
{
  putInt(num);
  writeStr(over);
  putInt(den);
  writeStr(endl);
}

void fraction::add(int theValue)
{
  num += theValue * den;
}
```

清单 18-5：`fraction` 类的成员函数定义

虽然 C++ 包含用于写入屏幕的库函数，但调用它们的汇编语言有些复杂。相反，我们将使用我们的汇编语言 `writeStr` 函数，它遵循 C 调用约定。它适用于 C 风格的文本字符串，我们将把它们放在全局区域。这将使我们能够专注于对象在 C++ 中是如何实现的。

通过给出类名后跟两个冒号（`::`）来指定类成员身份。构造函数与类同名，用于执行任何必要的初始化。例如，我们的构造函数将 `fraction` 对象初始化为 0/1。在某些设计中，它们可能需要做其他事情，例如从堆分配内存或打开文件。它们不能有返回值。

让我们看看 C++ 编译器为这些成员函数生成的汇编语言，如清单 18-6 所示。

```
        .file   "fraction.cpp"
        .intel_syntax noprefix
        .text
        .globl  numMsg
        .data
        .align 16
        .type   numMsg, @object
        .size   numMsg, 18
numMsg:
        .string "Enter numerator: "
        .globl  denMsg
        .align 16
        .type   denMsg, @object
        .size   denMsg, 20
denMsg:
        .string "Enter denominator: "
        .globl  over
        .type   over, @object
        .size   over, 2
over:
        .string "/"
        .globl  endl
        .type   endl, @object
        .size   endl, 2
endl:
        .string "\n"
        .text
        .align 2
        .globl  _ZN8fractionC2Ev
        .type   _ZN8fractionC2Ev, @function
_ZN8fractionC2Ev:                         ## constructor
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR -8[rbp], rdi    ## this pointer
        mov     rax, QWORD PTR -8[rbp]    ## load addr of object
        mov     DWORD PTR [rax], 0        ## num= 0;
        mov     rax, QWORD PTR -8[rbp]
        mov     DWORD PTR 4[rax], 1       ## den = 1;
        nop
        pop     rbp
        ret
        .size   _ZN8fractionC2Ev, .-_ZN8fractionC2Ev
        .globl  _ZN8fractionC1Ev
        .set    _ZN8fractionC1Ev,_ZN8fractionC2Ev
        .align 2
        .globl  _ZN8fractionD2Ev
        .type   _ZN8fractionD2Ev, @function
_ZN8fractionD2Ev:                         ## destructor
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR -8[rbp], rdi    ## this pointer
        nop
        pop     rbp
        ret
        .size   _ZN8fractionD2Ev, .-_ZN8fractionD2Ev
        .globl  _ZN8fractionD1Ev
        .set    _ZN8fractionD1Ev,_ZN8fractionD2Ev
        .align 2
        .globl  _ZN8fraction3getEv
        .type   _ZN8fraction3getEv, @function
_ZN8fraction3getEv:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     QWORD PTR -8[rbp], rdi    ## this pointer
        lea     rdi, numMsg[rip]
        call    writeStr@PLT
        mov     rax, QWORD PTR -8[rbp]
        mov     rdi, rax
        call    getInt@PLT
        lea     rdi, denMsg[rip]
        call    writeStr@PLT
        mov     rax, QWORD PTR -8[rbp]
        add     rax, 4
        mov     rdi, rax
        call    getInt@PLT
        nop
        leave
        ret
        .size   _ZN8fraction3getEv, .-_ZN8fraction3getEv
        .align 2
        .globl  _ZN8fraction7displayEv
        .type   _ZN8fraction7displayEv, @function
_ZN8fraction7displayEv:
        push    rbp
        mov     rbp, rsp
        sub     rsp, 16
        mov     QWORD PTR -8[rbp], rdi    ## this pointer
        mov     rax, QWORD PTR -8[rbp]
        mov     eax, DWORD PTR [rax]
        mov     edi, eax
        call    putInt@PLT
        lea     rdi, over[rip]
        call    writeStr@PLT
        mov     rax, QWORD PTR -8[rbp]
        mov     eax, DWORD PTR 4[rax]
        mov     edi, eax
        call    putInt@PLT
        lea     rdi, endl[rip]
        call    writeStr@PLT
        nop
        leave
        ret
        .size   _ZN8fraction7displayEv, .-_ZN8fraction7displayEv
        .align 2
        .globl  _ZN8fraction3addEi
        .type   _ZN8fraction3addEi, @function
_ZN8fraction3addEi:
        push    rbp
        mov     rbp, rsp
        mov     QWORD PTR -8[rbp], rdi    ## this pointer
        mov     DWORD PTR -12[rbp], esi
        mov     rax, QWORD PTR -8[rbp]
        mov     edx, DWORD PTR [rax]
        mov     rax, QWORD PTR -8[rbp]
        mov     eax, DWORD PTR 4[rax]
        imul    eax, DWORD PTR -12[rbp]
        add     edx, eax
        mov     rax, QWORD PTR -8[rbp]
        mov     DWORD PTR [rax], edx
        nop
        pop     rbp
        ret
        .size   _ZN8fraction3addEi, .-_ZN8fraction3addEi
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section        .note.GNU-stack,"",@progbits
```

清单 18-6：编译器为 `fraction` 类成员函数生成的汇编语言

`main` 函数调用 `_ZN8fractionC1Ev` 作为 `fraction` 对象的构造函数（清单 18-4），但编译器将其命名为 `_ZN8fractionC2Ev`。然后编译器将 `_ZN8fractionC1Ev` 设置为等于 `_ZN8fractionC2Ev`。（汇编指令 `.set` 与 `.equ` 相同。）这两个不同的构造函数用于实现 C++ 的更高级功能。在我们的简单示例中，它们是相同的。

我们的 `main` 函数调用 `_ZN8fractionD1Ev` 作为析构函数。与构造函数类似，编译器将我们的析构函数命名为 `_ZN8fractionD2Ev`，然后使 `_ZN8fractionD1Ev` 等于 `_ZN8fractionD2Ev`。同样，这是为了允许更复杂的析构函数。
