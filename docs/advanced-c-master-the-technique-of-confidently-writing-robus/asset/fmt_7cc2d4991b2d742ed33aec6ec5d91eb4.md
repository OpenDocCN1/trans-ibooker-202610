
Python 编程语言有一个名为 `in` 的成员运算符，可用于任何序列，即 `list`、`sequence`、`set`、`string` 等。尽管 C++ 拥有 100 多个算法，但没有等效的方法来实现相同功能。C++20 在 `std::set` 上引入了 `contains()` 方法，但这对我们来说还不够。我们需要创建一个 `contains()` 模板函数，使其适用于 `std::set`、`std::string`、`std::vector` 以及任何提供迭代器的容器。这取决于能否在其上调用 `end()`。我们旨在获得最佳性能，因此将在任何拥有 `find()` 成员方法的容器上调用它（这将是最高效的），否则回退到在容器上使用 `std::end()`。我们还需要区别对待 `std::string`，因为其 `find()` 方法返回一个特殊值。

我们可以使用一个通用模板和两个特化来实现这一点，但本活动旨在使用 SFINAE 和 `if constexpr` 技术使其工作。此外，此模板必须仅适用于支持 `end(C)` 的类。请遵循以下步骤实现此活动：

1.  从 `Lesson2B/Activity01` 文件夹加载准备好的项目。
2.  定义辅助模板函数和类，使用 `npos` 成员检测 `std::string` 情况。
3.  定义辅助模板函数和类，检测类是否有 `find()` 方法。
4.  定义 `contains` 模板函数，使用 `constexpr` 在三种实现中选择——字符串情况、有 `find` 方法的情况或一般情况。

实现上述步骤后，预期输出应如下所示：

![Figure 2B.35: Output from the successful implementation of contains](image/C14583_02B_35.jpg)

###### Figure 2B.35: Output from the successful implementation of contains

#### Note

该活动的解决方案见第 653 页。

## Summary

在本章中，我们学习了接口、继承和多态，这扩展了我们对类型能力的认识。我们首次涉足泛型编程，接触了 C++ 模板，并了解了 C++ 标准库（包括 STL）免费提供的功能。我们探索了 C++ 的一项开箱即用的特性——模板类型推导，它让使用模板变得更轻松。随后我们深入模板，学习了如何使用 SFINAE 和 `if constexpr` 控制编译器包含的模板部分。这些构成了我们 C++ 之旅的基石。在下一章中，我们将重新审视栈和堆，理解什么是异常、何时发生以及会发生什么。我们还将学习如何在异常发生时保护程序免受资源损失。

# 3. No Leaks Allowed - Exceptions and Resources

## Learning Objectives

本章结束时，你将能够：

*   开发管理资源的类
*   开发异常健壮的代码，使资源不通过 RAII 泄漏
*   实现可通过移动语义转移资源所有权的类
*   实现控制隐式转换的类

在本章中，你将学习如何使用类管理资源、防止泄漏，并防止复制大量数据。

## Introduction

在 *Chapter 2A, No Ducks Allowed – Types, and Deduction* 中，我们简要涉及了一些概念，如智能指针和移动语义。在本章中，我们将进一步探索它们。事实证明，这些主题与资源管理和编写健壮代码（即能长期、频繁运行且无问题的代码）密切相关。

为了理解发生了什么，我们将探讨变量在内存中的位置，以及它们何时、如何离开作用域。

我们将查看编译器为我们输入的内容生成的汇编代码，并探讨异常发生时所有这些如何受影响。

### Variable Scope and Lifetime

在 *Chapter 2B, No Ducks Allowed – Templates, and Deduction* 中，我们讨论了变量作用域和生命周期。让我们快速回顾其不同类型：

**Scope**：

*   **Local scope**（也称为 **block scope**）：适用于在由花括号（`{}`）包围的块中声明的任何内容。
*   **Global / file scope**：适用于在普通函数或类之外声明的变量，以及普通函数。

**Lifetime**：

*   **Automatic lifetime**：局部变量在声明时创建，在退出其所在作用域时销毁。这些由栈管理。
*   **Permanent lifetime**：全局和静态局部变量具有永久生命周期。
*   **Dynamic lifetime**：变量在程序员请求时创建和销毁（使用 `new` 和 `delete` 运算符）。这些变量的内存从堆分配。

我们将使用以下程序来理解 **local variables** 的行为——那些具有 **automatic lifetime** 和那些具有 **dynamic lifetime** 的：

![](image/C14583_03_01.jpg)

###### Figure 3.1: Test program for variable scope and lifetime

运行上述程序时，会生成以下输出：

![Figure 3.2: Output from Lifetime test program](image/C14583_03_02.jpg)

###### Figure 3.2: Output from Lifetime test program

上述输出中的十六进制数字（`0xNNNNNNNN`）是正在构造或析构的 `Int` 对象的地址。我们的程序从第 46 行的 `main()` 函数开始进入。此时，程序已经完成了大量初始化，以便一切随时可用。下图涉及两个栈——**PC Stack** 和 **Data Stack**。

这些是帮助我们解释幕后发生情况的抽象概念。**PC Stack**（**Program Counter Stack**）用于记住程序计数器的值（指向下一条需运行指令的寄存器），而 **Data Stack** 保存我们正在操作的值或地址。尽管这是两个独立的栈，但在真实 CPU 上，很可能实现为一个栈。让我们看看下表，其中我们用 **OLn** 表示前面程序输出中的行号：

![Figure 3.3: Detailed analysis of the test program's execution (part 1)](image/C14583_03_03.jpg)

###### Figure 3.3: Detailed analysis of the test program's execution (part 1)

下面是测试程序执行的详细分析第二部分：

![Figure 3.4: Detailed analysis of the test program's execution (part 2)](image/C14583_03_04.jpg)

###### Figure 3.4: Detailed analysis of the test program's execution (part 2)

下面是测试程序执行的详细分析第三部分：

![Figure 3.5: Detailed analysis of the test program's execution (part 3)](image/C14583_03_05.jpg)

###### Figure 3.5: Detailed analysis of the test program's execution (part 3)

从这个简单的程序中，我们学到了一些重要事实：

*   当我们按值传递时（本例中如此），会调用拷贝构造函数。
*   返回类型只会导致调用一次构造函数（不是两次——一次创建返回对象，一次为存储返回数据的变量）——C++ 称之为 **copy elision**，这在标准中现在是强制性的。
*   作用域结束时（右花括号 `}`），任何离开作用域的变量都会调用其析构函数。如果这是真的，那么为什么地址 `0x6000004d0` 没有显示析构函数调用（`~Int()`）？这引出了下一个事实。
