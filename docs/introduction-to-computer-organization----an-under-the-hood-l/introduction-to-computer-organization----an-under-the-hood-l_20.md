# 第 18 章：面向对象编程

到目前为止，本书一直使用过程式编程范式（参见第 25 页的“用 C 探索数据格式”）。在本章中，我们将初步探讨面向对象编程在汇编语言层面是如何实现的。

在面向对象编程中，对象具有一组*属性*，即定义对象状态的数据项。这些属性可以通过对象的一组*方法*进行更改或查询。一个软件解决方案通常由构建对象的*实例*组成，然后编程向对象发送*消息*，对象使用方法作用于属性。

我们将使用 C++（C 的面向对象扩展）来说明其中一些概念。我们的讨论将展示记录如何用于存储对象的属性，以及方法如何实现为与记录关联的函数。

C++ 的许多其他特性对于创建良好的面向对象编程解决方案很重要，但本书不再深入。对于本书读者，我认为 Josh Lospinoso 的 *C++ Crash Course*（No Starch Press, 2019）是学习 C++ 的好途径。如果你在学会使用 C++ 后想深入了解其设计，我推荐 C++ 创始人 Bjarne Stroustrup 的著作 *The Design and Evolution of C++*（Addison-Wesley, 1994）。

照例，我们将从查看 C++ 编译器生成的汇编语言开始。

## C++ 中的对象

C++ 对象通过指定*类*来声明，这会向程序中添加一个程序员定义的类型。C++ 类非常类似于 C 记录，但除了定义对象属性的*数据成员*外，还可以包含作为类成员的函数。在 C++ 中，我们通过调用类的*成员函数*向对象发送消息，让它执行一个方法。

我们通过给出类名和对象名来*实例化*（创建实例）一个对象，就像定义变量一样。例如，在我们稍后将要查看的程序中，C++ 语句

```
fraction x;
```

实例化了一个名为 `x` 的对象，它属于 `fraction` 类。

C++ 允许我们编写两个特殊的成员函数。第一个是*构造函数*，用于初始化对象，使其在向对象发送消息之前处于已知状态。C++ 编译器会在实例化对象的位置自动生成调用构造函数的代码。构造函数与类同名。它不能有返回值，甚至不能是 `void`。*默认构造函数*不接受参数，但我们也可以编写接受参数的构造函数，这允许一个类中有多个构造函数。

我们还可以编写*析构函数*来释放构造函数分配的任何资源。例如，构造函数可能从堆中分配内存，析构函数则将其释放。（堆在第 10 章中描述。）只能有一个析构函数，其名称是类名前加上 `~` 字符。析构函数不能有返回值，也不接受参数。当程序流离开对象的作用域时，C++ 编译器会自动生成调用析构函数的代码。

我们将首先查看一个简单的 `fraction` 类，其属性为分子和分母，包含构造函数和析构函数。如果我们不提供构造函数或析构函数成员函数，C++ 编译器将提供适当的代码来执行对象的构造和析构。本章后面我们将探讨当我们不提供它们时编译器会为我们做什么。

我们将从 `fraction` 类的声明开始，将其放在头文件中，以便任何使用该类的文件都能包含它，如清单 18-1 所示。

### 清单 18-1：一个简单的 `fraction` 类

```
1 // fraction.hpp
// 简单的分数类。

#ifndef FRACTION_HPP
#define FRACTION_HPP
// 使用以下 C 函数
2 extern "C" int writeStr(char *);
extern "C" int getInt(int *);
extern "C" int putInt(int);

3 class fraction
{
  4 int num;        // 分子
    int den;        // 分母
5 public:
  6 fraction();     // 默认构造函数
    ~fraction();    // 默认析构函数
    void get();     // 获取用户的值
    void display(); // 显示分数
    void add(int);  // 加整数
};
#endif
```

作为 C 的扩展，C 中几乎所有能做的事情在 C++ 中也能做。C++ 的新增特性之一是用于注释的 `//` 语法 1。就像汇编语言中的 `#` 语法一样，该行剩余部分为注释，仅供人类阅读。




# C++ 类与对象

## C++ 中的类声明

我们将使用本书前面编写的一些遵循 C 调用约定的汇编语言函数。正如你将看到的，C++ 中调用函数的约定与 C 不同，因此我们需要告诉 C++ 编译器，我们将使用 C 约定来调用这些函数 2。

`class` 声明 3 的整体语法类似于记录声明，但包含将类的方法作为成员函数 6 包含在内的能力。默认情况下，在 `class` 中声明的数据成员和成员函数处于 `private` 作用域 4：它们只能被同一个类的成员函数访问。我们将 `fraction` 类的属性 `num` 和 `den` 放在私有作用域中，在那里它们被定义为变量 4。它们只能被成员函数访问。

`class` 也可以拥有 `public` 作用域，用于类外部实体需要访问的项 5。我们将在公有作用域中声明成员函数，以便它们可以从类外部被调用 6。C++ 类可以有其他访问级别，但本书不予介绍。

`struct` 关键字也可用于声明 C++ 类，但它没有默认的作用域访问保护。我们需要显式声明 `private` 作用域，如 清单 18-2 所示。

```cpp
struct fraction
{
  private:
    int num;        // 分子
    int den;        // 分母
  public:
    fraction();     // 构造函数
    ~fraction();    // 析构函数
    void get();     // 获取用户的值
    void display(); // 显示分数
    void add(int);  // 加整数
};
```

清单 18-2：使用 `struct` 关键字声明的 `fraction` 类

我更倾向于使用 `class` 关键字，因为这强调了它不仅仅是一个简单的 C 语言记录，但这属于个人选择。接下来，我们将探讨如何创建对象以及如何向它们发送消息。

## C++ 中使用对象

为了说明如何创建对象并向其发送消息，我们将使用一个简单的程序，该程序允许用户输入分数的分子和分母值，然后给该分数加 1，如 清单 18-3 所示。程序会在获取用户输入值之前显示分数的状态，然后在给用户的分数加 1 后再次显示。

```cpp
// incFraction.cpp
// 从用户获取分数并加 1。

#include "fraction.hpp"
int main(void)
{
1 fraction x;

2 x.display();
  x.get();
  x.add(1);
  x.display();
  return 0;
} 
```

清单 18-3：给分数加 1 的程序

通过使用类名并为对象提供名称来实例化对象，就像定义变量一样 1。点运算符 (`.`) 用于向类中的方法发送消息 2，这会调用该对象所属类中的相应成员函数。

接下来，我们将查看 C++ 编译器生成的汇编语言，以实现 清单 18-3 中的 `main` 函数。C++ 编译器名为 `g++`。我使用以下命令生成 清单 18-4 中的汇编语言：

```bash
g++ -fno-asynchronous-unwind-tables -fno-exceptions -fcf-protection=none -S \
-masm=intel incFraction.cc
```

这与我们用于 C 代码的命令相同，只是添加了 `-fno-exceptions` 选项。C++ 提供了 `exception` 机制，用于在检测到运行时错误时进行处理。编译器通过汇编指令提供此功能的信息，这会倾向于模糊此处关于对象如何实现的讨论。`-fno-exceptions` 选项关闭此功能。

```asm
        .file   "incFraction.cpp"
        .intel_syntax noprefix
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp
        mov     rbp, rsp
        push    rbx
      1 sub     rsp, 24
        mov     rax, QWORD PTR fs:40
        mov     QWORD PTR -24[rbp], rax
        xor     eax, eax
      2 lea     rax, -32[rbp]     ## 对象的地址
        mov     rdi, rax
        call    _ZN8fractionC1Ev@PLT  ## 构造
        lea     rax, -32[rbp]     ## 对象的地址
        mov     rdi, rax
        call    _ZN8fraction7displayEv@PLT
        lea     rax, -32[rbp]     ## 对象的地址
        mov     rdi, rax
        call    _ZN8fraction3getEv@PLT
        lea     rax, -32[rbp]     ## 对象的地址
        mov     esi, 1            ## 要加的整数
        mov     rdi, rax
        call    _ZN8fraction3addEi@PLT
        lea     rax, -32[rbp]     ## 对象的地址
        mov     rdi, rax
        call    _ZN8fraction7displayEv@PLT
        mov     ebx, 0            ## 返回值
        lea     rax, -32[rbp]     ## 对象的地址
        mov     rdi, rax
      3 call    _ZN8fractionD1Ev@PLT
        mov     eax, ebx          ## return 0;
        mov     rdx, QWORD PTR -24[rbp]
        xor     rdx, QWORD PTR fs:40
        je      .L3
        call    __stack_chk_fail@PLT
.L3:
        add     rsp, 24
        pop     rbx
        pop     rbp
        ret
        .size   main, .-main
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section        .note.GNU-stack,"",@progbits
```

清单 18-4：编译器生成的汇编语言，展示了对象的构造、向其发送消息及其销毁

首先要注意的是，`x` 是一个自动局部变量 (清单 18-3)，因此在函数的序言中为这个 `fraction` 对象在栈上分配了内存空间 1。然后，在实例化对象的那一点，这块内存区域的地址 2 被传递给一个函数 `_ZN8fractionC1Ev`。（注意，在为 `fraction` 对象分配 24 字节之前 `rbx` 被压入了栈，使得此处的正确偏移量为 -32。）

如果你回顾 清单 18-1，可以看到 C++ 编译器已经将我们的构造函数名 `fraction` 修饰为 `_ZN8fractionC1Ev`。我们在第 15 章见过 C 编译器对静态局部变量进行名字修饰。那里的目的是区分在同一文件中定义的不同函数中同名的不同静态局部变量。

C++ 进行名字修饰是为了将成员函数与其类关联起来。如果你查看 清单 18-4 中对类成员函数的调用，可以看到它们都以 `_ZN8fraction` 开头。由于函数名在全局作用域内，包含类名允许我们在程序中定义其他拥有相同成员函数名的类。例如，程序中可能有多个类都拥有 `display` 成员函数。名字修饰将每个 `display` 成员函数与其所属的类标识开来。

C++ 名字修饰还允许函数重载。如果你仔细观察编译器如何修饰我们的 `add` 成员函数 `_ZN8fraction3addEi`，你大概能推断出编译器的名字修饰包含了参数的数量和类型。在这个例子中，修饰名末尾的 `i` 表明该函数接受单个 `int` 参数。这允许我们拥有多个同名但参数数量和类型不同的类成员函数，这被称为函数重载。当轮到你的练习时，你将有机会重载默认构造函数。

关于名字修饰如何进行没有标准，所以不同编译器的做法可能不同。这意味着程序中的所有 C++ 代码必须使用兼容的编译器和链接器进行编译和链接。




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




我之前说过，我们可以在成员函数中访问对象的地址。C++ 提供了一个名为 `this` 的特殊指针变量，它包含对象的地址。大多数时候，我们不需要显式使用 `this` 指针。当我们在成员函数中使用对象的数据成员时，编译器会假定我们指的是当前对象，并隐式地使用 `this` 指针。如果我们在成员函数内部调用另一个成员函数，这一假设同样成立。一些更高级的 C++ 编程技巧需要显式使用 `this` 指针，但这超出了本书的范围。

Listing 18-6 中的其余代码对你来说应该很熟悉，所以我们将继续展示何时不需要编写构造函数或析构函数。

### 让编译器编写构造函数和析构函数

如果我们只需要一个默认构造函数来初始化数据成员，甚至不需要编写构造函数或析构函数。Bjarne Stroustrup 和 Herb Sutter 维护了一份优秀的 C++ 编写建议清单。他们的建议 C.45 ([`isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-default/`](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-default/)) 指出：“不要定义仅初始化数据成员的默认构造函数；应改用类内成员初始化器。”他们指出，编译器会为我们“生成该函数”，这“可能更高效”。我读过的大多数 C++ 书籍给出的建议基本相同。

在本节中，我们将遵循这一建议。我们将通过删除构造函数和析构函数，修改来自 Listing 18-1、Listing 18-3 和 Listing 18-5 的代码。然后我们将利用汇编语言知识来查看编译器为我们生成了什么。Listing 18-7 展示了没有构造函数或析构函数的 `fraction` 类，改用*类内成员初始化器*。

```
// fraction.hpp
// Simple fraction class.

#ifndef FRACTION_HPP
#define FRACTION_HPP
// Uses the following C functions
extern "C" int writeStr(char *);
extern "C" int getInt(int *);
extern "C" int putInt(int);

class fraction
{
  1 int num{0};              // numerator
    int den{1};              // denominator
  public:
    void get();              // gets user's values
    void display();           // displays fraction
    void add(int theValue);   // adds integer
};
#endif
```

Listing 18-7：来自 Listing 18-1 的 `fraction` 类，移除了构造函数和析构函数

我们将不再使用构造函数来初始化 `fraction` 对象中的数据成员，而是使用 C++ 的*大括号初始化*语法 1。C++ 还允许以下语法进行数据成员初始化：

```
int num = 0;
int num = {0};
```

我喜欢大括号初始化语法，因为它传达了这样的信息：直到对象被实例化时，实际的赋值操作才会发生，我们稍后就会看到。这些差异在本章开头引用的 Josh Lospinoso 的书中有讨论。

我们不会更改 Listing 18-3 中的代码，并将从 Listing 18-5 中移除构造函数和析构函数（`fraction()` 和 `~fraction()`）。这些更改为我们提供了 Listing 18-8 中所示 `main` 函数的编译器生成汇编语言。

```
        .file   "incFraction.cpp"
        .intel_syntax noprefix
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp
        mov     rbp, rsp
      1 sub     rsp, 16
        mov     rax, QWORD PTR fs:40
        mov     QWORD PTR -8[rbp], rax
        xor     eax, eax
      2 mov     DWORD PTR -16[rbp], 0     ## num = 0;
        mov     DWORD PTR -12[rbp], 1     ## den = 1;
        lea     rax, -16[rbp]
        mov     rdi, rax
        call    _ZN8fraction7displayEv@PLT
        lea     rax, -16[rbp]
        mov     rdi, rax
        call    _ZN8fraction3getEv@PLT
        lea     rax, -16[rbp]
        mov     esi, 1
        mov     rdi, rax
        call    _ZN8fraction3addEi@PLT
        lea     rax, -16[rbp]
        mov     rdi, rax
        call    _ZN8fraction7displayEv@PLT
        mov     eax, 0                    ## return 0;
      3 mov     rdx, QWORD PTR -8[rbp]
        xor     rdx, QWORD PTR fs:40
        je      .L3
        call    __stack_chk_fail@PLT
.L3:
        leave
        ret
        .size   main, .-main
        .ident  "GCC: (Ubuntu 9.3.0-17ubuntu1~20.04) 9.3.0"
        .section        .note.GNU-stack,"",@progbits
```

Listing 18-8：展示 `g++` 编译器如何为我们的 `fraction` 对象生成构造函数的汇编语言

C++ 在栈帧中创建对象，就像我们编写自己的构造函数时一样 1。但编译器不会编写单独的构造函数。相反，它会在实例化对象的函数中直接初始化数据成员 2。

由于 `fraction` 对象使用的唯一资源是栈上的内存，函数尾声中的正常栈清理会删除该对象。因此，编译器不需要做任何特殊操作来析构我们的 `fraction` 对象 3。

将 Listing 18-8 中的代码与 Listing 18-4 中的代码进行比较，我们可以看到编译器节省了八条指令，包括两次函数调用。所以它确实为我们创建了一个更高效的构造函数。但汇编语言也向我们展示了，说编译器生成了一个*内联构造函数*而不是构造函数功能更为准确。当然，C++ 语言规范允许其他编译器以不同方式处理此事。

最后，我想指出的是，如果我们需要另一个接受参数的构造函数，那么我们也必须提供自己的默认构造函数。编译器不会为我们处理默认构造。

Listing 18-8 中的其余代码对你来说应该很熟悉，所以我们将继续探讨如何直接在汇编语言中实现对象。

## 汇编语言中的对象

我们可能不会使用汇编语言进行面向对象编程，但本节的讨论将有助于确保我们清楚地了解 C++ 如何实现对象。

我们从 `fraction` 对象中数据成员的偏移量开始，如 Listing 18-9 所示。

```
# fraction
# Declaration of fraction attributes.
# This object takes 8 bytes.
        .equ    num,0
        .equ    den,4
```

Listing 18-9：`fraction` 类中属性值的偏移量

对象的属性被实现为记录，因此对象数据成员的偏移量声明方式与记录中的字段偏移量相同。没有汇编指令使它们成为私有的。私有和公有的区别由高级语言（在我们的例子中是 C++）来定义。我们需要编写汇编语言，确保只有类成员函数访问数据成员。

Listing 18-10 展示了我们 `main` 函数的汇编语言版本。





# 清单 18-10：将 1 加到分数的汇编语言程序

```assembly
# incFraction.s
# Gets numerator and denominator of a fraction
# from user and adds 1 to the fraction.
        .intel_syntax noprefix
# Stack frame
        .equ    x,-16
        .equ    canary,-8
        .equ    localSize,-16
# Constant data
        .section	.rodata
        .align 8
# Code
        .text
        .globl  main
        .type   main, @function
main:
        push    rbp                     # save frame pointer
        mov     rbp, rsp                # set new frame pointer
        add     rsp, localSize          # for local var.
        mov     rax, qword ptr fs:40    # get canary
        mov     canary[rbp], rax

        lea     rdi, x[rbp]             # address of object
      1 call    fraction_construct      # construct it
        
        lea     rdi, x[rbp]             # address of object
        call    fraction_display        # display fraction
        
        lea     rdi, x[rbp]             # address of object
        call    fraction_get            # get fraction values
        
        mov     esi, 1                  # amount to add
        lea     rdi, x[rbp]             # address of object
        call    fraction_add            # add it

        lea     rdi, x[rbp]             # address of object
        call    fraction_display        # display fraction
        
        lea     rdi, x[rbp]             # address of object
        call    fraction_destruct       # delete fraction
        
        mov     eax, 0                  # return 0;
      2 mov     rcx, canary[rbp]
        xor     rcx, qword ptr fs:40
        je      goodCanary
        call    __stack_chk_fail@plt
goodCanary:
        mov     rsp, rbp                # restore stack pointer
        pop     rbp                     # and caller frame pointer
        ret
```

在 C++ 中编写代码时，构造函数和析构函数会被隐式调用，但在汇编语言中我们必须显式地进行调用 1。我们不会像编译器那样对名称进行大量修饰。我们只需在每个成员函数名前加上 `fraction_` 前缀。

除了函数名修饰之外，这个 `main` 函数没有什么特别之处。请注意，我们在调用析构函数后检查栈金丝雀，因为该调用会使用栈 2。

接下来，我们将编写成员函数的汇编语言，从清单 18-11 中的构造函数开始。

# 清单 18-11：我们的 `fraction` 类构造函数的汇编语言实现

```assembly
# fraction_construct.s
# Initializes fraction to 0/1.
# Calling sequence:
#   rdi <- address of object
        .intel_syntax noprefix
        .include    "fraction"
# Code
        .text
        .globl  fraction_construct
        .type   fraction_construct, @function
fraction_construct:
        push    rbp                     # save frame pointer
        mov     rbp, rsp                # set new frame pointer
      1 mov     dword ptr num[rdi], 0   # initialize
        mov     dword ptr den[rdi], 1   #     fraction
        mov     rsp, rbp                # restore stack pointer
        pop     rbp                     # and caller frame pointer
        ret
```

由于我们不会从该函数中调用任何其他函数，因此可以使用 `rdi` 寄存器作为 `this` 指针来访问对象中的数据成员 1。

在我们的 `fraction` 类中，析构函数不执行任何操作，但为了完整性，我们还是会编写一个，如清单 18-12 所示。

# 清单 18-12：我们的 `fraction` 类的析构函数

```assembly
# fraction_destruct.s
# Nothing to do here.
# Calling sequence:
#   rdi <- address of object
        .intel_syntax noprefix
        .include    "fraction"
# Code
        .text
        .globl  fraction_destruct
        .type   fraction_destruct, @function
fraction_destruct:
        push    rbp         # save frame pointer
        mov     rbp, rsp    # set new frame pointer
# Has nothing to do
        mov     rsp, rbp    # restore stack pointer
        pop     rbp         # and caller frame pointer
        ret
```

接下来，我们将编写 `fraction_display` 函数，如清单 18-13 所示。

# 清单 18-13：我们的 `fraction` 类的 `fraction_display` 函数

```assembly
# fraction_display.s
# Displays fraction.
# Calling sequence:
#   rdi <- address of object
        .intel_syntax noprefix
        .include    "fraction"
# Text for fraction_display
        .data
over:
        .string "/"
endl:
        .string "\n"
# Stack frame
      1 .equ    this,-16
        .equ    localSize,-16
# Code
        .text
        .globl  fraction_display
        .type   fraction_display, @function
fraction_display:
        push    rbp               # save frame pointer
        mov     rbp, rsp          # set new frame pointer
        add     rsp, localSize    # for local var.
        mov     this[rbp], rdi    # this pointer

        mov     rax, this[rbp]    # load this pointer
        mov     edi, num[rax]
        call    putInt
        
        lea     rdi, over[rip]    # slash
        call    writeStr

        mov     rax, this[rbp]    # load this pointer
        mov     edi, den[rax]
        call    putInt
        
        lea     rdi, endl[rip]    # newline
        call    writeStr

        mov     rsp, rbp          # restore stack pointer
        pop     rbp               # and caller frame pointer
        ret
```

`fraction_display` 函数会调用其他函数，这些函数中的任何一个都可能改变 `rdi` 的内容，因此我们需要在栈帧中保存 `this` 指针 1。

清单 18-13 展示了我们遵循惯例、在汇编语言中为每个函数使用单独文件的原因之一。由 `.equ` 指令定义的标识符具有文件作用域。根据栈帧中还需要放置什么其他内容，`this` 指针在不同函数中的相对位置可能会有所不同。如果我们将所有成员函数放在同一个文件中，就需要进行一些名称修饰，以将每个 `this` 偏移量与其相应的成员函数关联起来。C++ 编译器会分别计算每个成员函数中 `this` 指针的数值偏移量，因此它生成的汇编语言中不会使用 `this` 这个名称。

清单 18-14 和 18-15 展示了 `fraction_get` 和 `fraction_add` 函数。

# 清单 18-14：我们的 `fraction` 类的 `fraction_get` 函数

```assembly
# fraction_get.s
# Gets numerator and denominator from keyboard.
# Calling sequence:
#   rdi <- address of object
        .intel_syntax noprefix
        .include    "fraction"
# Messages
        .data
numMsg:
        .string "Enter numerator: "
denMsg:
        .string "Enter denominator: "
# Stack frame
        .equ    this,-16
        .equ    localSize,-16
# Code
        .text
        .globl  fraction_get
        .type   fraction_get, @function
fraction_get:
        push    rbp               # save frame pointer
        mov     rbp, rsp          # set new frame pointer
        add     rsp, localSize    # for local var.
        mov     this[rbp], rdi    # this pointer

        lea     rdi, numMsg[rip]  # prompt message
        call    writeStr
        mov     rax, this[rbp]    # load this pointer
        lea     rdi, num[rax]
        call    getInt
        
        lea     rdi, denMsg[rip]
        call    writeStr
        mov     rax, this[rbp]    # load this pointer
        lea     rdi, den[rax]
        call    getInt
        mov     rsp, rbp          # restore stack pointer
        pop     rbp               # and caller frame pointer
        ret
```




# 清单 18-15：`fraction` 类的 `fraction_add` 函数

```asm
# fraction_add.s
# 将整数加到分数上
# 调用序列：
#   rdi <- 对象指针
#   esi <- 要加的整数
        .intel_syntax noprefix
        .include    "fraction"
# 代码
        .text
        .globl  fraction_add
        .type   fraction_add, @function
fraction_add:
        push    rbp             # 保存帧指针
        mov     rbp, rsp        # 设置新帧指针
        mov     eax, den[rdi]   # 加载分母
        imul    eax, esi        # 分母 X 要加的整数
        add     num[rdi], eax   # 加到分子上
        mov     rsp, rbp        # 恢复栈指针
        pop     rbp             # 并恢复调用者的帧指针
        ret
```

`fraction_get` 或 `fraction_add` 没有什么特别之处。`fraction_get` 函数调用其他函数，因此我们需要将 `this` 指针放在栈帧中。`rdi` 寄存器在 `fraction_add` 中可安全用作 `this` 指针，因为它不调用任何其他函数。

## 所学内容

1.  **类** — 定义对象状态的数据成员的声明，以及用于访问这些数据成员的任何成员函数。
2.  **C++ 中的对象** — 一个命名的内存区域，包含类中声明的数据成员。
3.  **方法或成员函数** — 类中声明的成员函数可被调用以访问同一类对象的状态。
4.  **名称修饰** — 编译器创建的成员函数名称包含函数名、所属类，以及函数参数的数量和类型。
5.  **构造函数** — 用于初始化对象的成员函数。
6.  **析构函数** — 用于清理不再需要的资源的成员函数。

这简要介绍了 C++ 实现基本面向对象编程特性的方式。

到目前为止，本书中的程序只使用了整数值。在下一章中，我们将探讨小数值如何在内存中表示，以及用于操作它们的一些 CPU 指令。
