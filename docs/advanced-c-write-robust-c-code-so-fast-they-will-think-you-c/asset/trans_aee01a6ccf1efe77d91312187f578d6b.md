

完成这些步骤后，你应该会收到以下输出：

```
------ Activity 2 ------
There are 358 days between 1-Jan-2019 and 25-Dec-2019
There are 0.980172 years between 1-Jan-2019 and 25-Dec-2019

There are 364 days between 1-Jan-2019 and 31-Dec-2019
There are 0.996599 years between 1-Jan-2019 and 31-Dec-2019

There are 18255 days between 1-Jan-1970 and 25-Dec-2019
There are 49.9805 years between 1-Jan-1970 and 25-Dec-2019

There are 36525 days between 1-Jan-1970 and 1-Jan-2070
There are 100.002 years between 1-Jan-1970 and 1-Jan-2070

Complete.
```

图 3.51：一个成功的 Date 示例应用程序的输出

> Note
> 本活动的解决方案可在第 664 页找到。

# Summary

在本章中，我们探讨了变量的生命周期——自动变量和动态变量，它们存储的位置，以及何时被析构。然后我们利用这些信息开发了 RAII 技术，使我们几乎可以忽略资源管理，因为自动变量会在析构时清理它们，即使在出现异常的情况下也是如此。然后，我们研究了抛出异常和捕获异常，以便在正确的层级处理异常情况。从 RAII 出发，我们进入了关于资源所有权以及 STL 智能指针如何帮助我们的讨论。我们发现几乎所有东西都被视为函数调用，从而允许运算符重载和隐式转换。我们发现了令人惊叹的（或可怕的？）参数依赖查找（ADL）世界，以及它如何可能在未来使我们陷入困境。我们现在对 C++ 的基本特性有了良好的理解。在下一章中，我们将开始探索函数对象以及它们如何使用 lambda 函数实现和实现。我们将进一步深入 STL 的功能，并探索 PIMPL，因为我们将重新审视封装。

# 4
关注点分离 - 软件架构、函数和可变参数模板

# Learning Objectives

到本章结束时，你将能够：

+   - 使用 PIMPL 惯用法开发类以实现对象级封装
+   - 使用仿函数、std::function 和 lambda 表达式实现回调系统
+   - 使用适合情况的正确捕获技术实现 lambda
+   - 开发可变参数模板以实现用于事件处理的 C# 风格委托。

本章将向你展示如何实现 PIMPL 惯用法，以及如何为自己的程序开发回调机制。

# ## Introduction

在上一章中，我们学习了如何使用 RAII 实现类，以便即使在出现异常时也能正确管理资源。我们还学习了 ADL（**Argument Dependent Lookup**）以及它如何确定要调用的函数。最后，我们谈到了 explicit 关键字如何用于防止编译器进行类型之间的自动转换，即隐式转换

在本章中，我们将研究依赖关系，既有物理的也有逻辑的，并了解它们如何对构建时间产生不利影响。我们还将学习如何将可见的接口类与实现细节分离以提高构建速度。然后我们将学习如何捕获函数和上下文，以便稍后使用 **functors**、**std::function** 和 **lambda expressions** 调用它们。最后，我们将实现一个可变参数模板来提供基于事件的回调机制。

# ## The Pointer to Implementation (PIMPL) Idiom

随着用 C++ 实现的项目变得越来越大，构建时间增长速度超过文件数量的可能性就越大。这是因为 C++ 构建模型使用的文本包含模型。这样做是为了让编译器能够确定类的大小和布局，从而在 **caller** 和 **callee** 之间产生耦合，但允许优化。请记住，一切都必须在被使用之前定义好。未来的一项特性 **Modules** 承诺解决这个问题，但现在我们需要了解这个问题以及用于解决该问题的技术。

# ## Logical and Physical Dependencies

当我们希望从另一个类访问一个类时，我们就有了逻辑依赖。一个类在逻辑上依赖于另一个类。如果我们考虑 **Graphics** 类，我们在 *Chapter 2A, No Ducks Allowed – Types and Deduction* 和 *Chapter 3, The Distance between Can and Should – Objects, Pointers and Inheritance* 中开发的 **Point3d** 和 **Matrix3d**，我们有两个逻辑上独立的类 **Matrix3d** 和 **Point3d**。然而，由于我们对两者之间乘法运算符的实现方式，我们创建了一个编译时或 **physical dependency**。

![](img/6507d435e2edfa3c00c220d0ccfcaf94_297_0.png)

图 4.1：Matrix3d 和 Point3d 的物理依赖关系

正如我们所见，对于这些相对简单的类，头文件和实现文件之间的物理依赖关系会迅速变得复杂。正是这种复杂性导致了大型项目的构建时间增加，因为物理（和逻辑）依赖的数量增长到数千个。前面的图表中仅显示了 13 个依赖关系，如箭头所示。但实际上还有更多，因为包含一个标准库头文件通常会引入一系列包含文件。这意味着如果一个头文件被修改，那么所有直接或间接依赖它的文件都需要重新编译以应对更改。即使更改是对用户甚至无法访问的私有类成员定义的更改，重建的触发也会发生。

为了帮助加快编译时间，我们使用了保护技术来防止头文件被多次处理：

```
```cpp
#if !defined(MY_HEADER_INCLUDED)

#define MY_HEADER_INCLUDED

// definitions

#endif // !defined(MY_HEADER_INCLUDED)
```
```

最近，大多数编译器现在都支持 **#pragma once** 指令，可实现相同的结果。

实体（文件、类等）之间的这些关系被称为 **coupling**。如果对一个文件/类的更改导致另一个文件/类发生更改，则该文件/类与另一个文件/类 **高度耦合**。如果对一个文件/类的更改不会导致另一个文件/类发生更改，则该文件/类与另一个文件/类 **松散耦合**。

高度耦合的代码（文件/类）在项目中引入了问题。高度耦合的代码难以更改（不灵活）、难以测试且难以理解。另一方面，松散耦合的代码更容易更改（只需修改一个类）、高度可测试（只需被测试的类）且更易于阅读和理解。耦合反映了逻辑和物理依赖关系，并且与之相关。

# The Pointer to Implementation (PIMPL) Idiom

解决耦合问题的解决方案之一是使用“**Pimpl Idiom**”（代表 **Pointer to Implementation Idiom**）。这也被称为不透明指针、编译器防火墙惯用法甚至 **Cheshire Cat technique**。考虑 **Qt library**，特别是 **Qt Platform Abstraction (QPA)**。它是一个抽象层，隐藏了 Qt 应用程序所托管的操作系统和/或平台的细节。实现此类层的一种方法是使用 PIMPL 惯用法，其中公共接口暴露给应用程序开发人员，但功能交付的实现方式被隐藏。Qt 实际上使用了一种称为 d-pointer 的 PIMPL 变体。

例如，GUI 的一个功能是使用对话框，即弹出窗口，用于显示信息或提示用户输入。它可以在 **dialog.hpp** 中如下声明：

> **Note**
> 
> 有关 QT Platform Abstraction (QPA) 的更多信息，请访问以下链接：
> https://doc.qt.io/qt-5/qpa.html#

```
#pragma once

class Dialog
{
public:
    Dialog();
    ~Dialog();
```

