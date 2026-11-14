

# C++ 类与对象

## C++ 中的类声明

我们将使用本书前面编写的一些遵循 C 调用约定的汇编语言函数。正如你将看到的，C++ 中调用函数的约定与 C 不同，因此我们需要告诉 C++ 编译器，我们将使用 C 约定来调用这些函数 2。

`class` 声明 3 的整体语法类似于记录声明，但包含将类的方法作为成员函数 6 包含在内的能力。默认情况下，在 `class` 中声明的数据成员和成员函数处于 `private` 作用域 4：它们只能被同一个类的成员函数访问。我们将 `fraction` 类的属性 `num` 和 `den` 放在私有作用域中，在那里它们被定义为变量 4。它们只能被成员函数访问。

`class` 也可以拥有 `public` 作用域，用于类外部实体需要访问的项 5。我们将在公有作用域中声明成员函数，以便它们可以从类外部被调用 6。C++ 类可以有其他访问级别，但本书不予介绍。

`struct` 关键字也可用于声明 C++ 类，但它没有默认的作用域访问保护。我们需要显式声明 `private` 作用域，如 [清单 18-2](#c18.xhtml#listing18-2) 所示。

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

为了说明如何创建对象并向其发送消息，我们将使用一个简单的程序，该程序允许用户输入分数的分子和分母值，然后给该分数加 1，如 [清单 18-3](#c18.xhtml#listing18-3) 所示。程序会在获取用户输入值之前显示分数的状态，然后在给用户的分数加 1 后再次显示。

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

接下来，我们将查看 C++ 编译器生成的汇编语言，以实现 [清单 18-3](#c18.xhtml#listing18-3) 中的 `main` 函数。C++ 编译器名为 `g++`。我使用以下命令生成 [清单 18-4](#c18.xhtml#listing18-4) 中的汇编语言：

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

首先要注意的是，`x` 是一个自动局部变量 ([清单 18-3](#c18.xhtml#listing18-3))，因此在函数的序言中为这个 `fraction` 对象在栈上分配了内存空间 1。然后，在实例化对象的那一点，这块内存区域的地址 2 被传递给一个函数 `_ZN8fractionC1Ev`。（注意，在为 `fraction` 对象分配 24 字节之前 `rbx` 被压入了栈，使得此处的正确偏移量为 -32。）

如果你回顾 [清单 18-1](#c18.xhtml#listing18-1)，可以看到 C++ 编译器已经将我们的构造函数名 `fraction` 修饰为 `_ZN8fractionC1Ev`。我们在第 15 章见过 C 编译器对静态局部变量进行名字修饰。那里的目的是区分在同一文件中定义的不同函数中同名的不同静态局部变量。

C++ 进行名字修饰是为了将成员函数与其类关联起来。如果你查看 [清单 18-4](#c18.xhtml#listing18-4) 中对类成员函数的调用，可以看到它们都以 `_ZN8fraction` 开头。由于函数名在全局作用域内，包含类名允许我们在程序中定义其他拥有相同成员函数名的类。例如，程序中可能有多个类都拥有 `display` 成员函数。名字修饰将每个 `display` 成员函数与其所属的类标识开来。

C++ 名字修饰还允许函数重载。如果你仔细观察编译器如何修饰我们的 `add` 成员函数 `_ZN8fraction3addEi`，你大概能推断出编译器的名字修饰包含了参数的数量和类型。在这个例子中，修饰名末尾的 `i` 表明该函数接受单个 `int` 参数。这允许我们拥有多个同名但参数数量和类型不同的类成员函数，这被称为函数重载。当轮到你的练习时，你将有机会重载默认构造函数。

关于名字修饰如何进行没有标准，所以不同编译器的做法可能不同。这意味着程序中的所有 C++ 代码必须使用兼容的编译器和链接器进行编译和链接。

