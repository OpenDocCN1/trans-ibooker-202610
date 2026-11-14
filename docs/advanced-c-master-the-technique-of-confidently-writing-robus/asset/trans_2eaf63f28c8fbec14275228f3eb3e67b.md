

# PIMPL 惯用法

解决这种耦合问题的一个方案是使用 **Pimpl 惯用法**（代表 **Pointer to Implementation Idiom**，即指向实现的指针惯用法）。这也被称为不透明指针、编译器防火墙惯用法，甚至称为 **柴郡猫技术**。以 **Qt 库**为例，特别是 **Qt 平台抽象 (QPA)**。它是一个抽象层，用于隐藏 Qt 应用程序所托管的操作系统和/或平台的细节。实现这种层的一种方法是使用 PIMPL 惯用法，即向应用程序开发者暴露通用接口，但隐藏功能交付的实现细节。Qt 实际上使用了一种称为 d-pointer 的 PIMPL 变体。

> **注意**：有关 Qt 平台抽象 (QPA) 的更多信息，请访问以下链接：<https://doc.qt.io/qt-5/qpa.html#>。

## 示例：对话框实现

例如，GUI 的一项功能是使用对话框，它是一个弹出窗口，用于显示信息或提示用户输入。它可以在 `dialog.hpp` 中声明如下：

### 头文件 (`dialog.hpp`)

```cpp
#pragma once

class Dialog
{
public:
    Dialog();
    ~Dialog();
    void create(const char* message);
    bool show();

private:
    struct DialogImpl;
    DialogImpl* m_pImpl;
};
```

用户可以访问使用 `Dialog` 所需的所有函数，但不知道它是如何实现的。请注意，我们声明了 `DialogImpl` 但未定义它。通常，对于像 `DialogImpl` 这样的类，我们能做的事情不多。但有一件事是允许的，那就是声明一个指向它的指针。C++ 的这一特性允许我们将实现细节隐藏在实现文件中。这意味着在该简单情况下，我们不需要为此声明包含任何头文件。

### 实现文件 (`dialogImpl.cpp`)

```cpp
#include "dialog.hpp"
#include <iostream>
#include <string>

struct Dialog::DialogImpl
{
    void create(const char* message)
    {
        m_message = message;
        std::cout << "Creating the Dialog\n";
    }

    bool show()
    {
        std::cout << "Showing the message: '" << m_message << "'\n";
        return true;
    }

    std::string m_message;
};

Dialog::Dialog() : m_pImpl(new DialogImpl)
{
}

Dialog::~Dialog()
{
    delete m_pImpl;
}

void Dialog::create(const char* message)
{
    m_pImpl->create(message);
}

bool Dialog::show()
{
    return m_pImpl->show();
}
```

我们从中注意到几点：

*   我们在定义 `Dialog` 所需的方法之前定义了实现类 `DialogImpl`。这是必要的，因为 `Dialog` 需要通过 `m_pImpl` 来调用这些方法，这意味着它们需要先被定义。
*   `Dialog` 的构造函数和析构函数负责内存管理。
*   我们仅在实现文件中包含实现所需的所有必要头文件。这通过最小化 `Dialog.hpp` 中包含的头文件数量来减少耦合。

### 客户端代码 (`main.cpp`)

程序可以按如下方式执行：

```cpp
#include <iostream>
#include "dialog.hpp"

int main()
{
    std::cout << "\n\n------ Pimpl ------\n";
    Dialog dialog;
    dialog.create("Hello World");
    if (dialog.show())
    {
        std::cout << "Dialog displayed\n";
    }
    else
    {
        std::cout << "Dialog not displayed\n";
    }
    std::cout << "Complete.\n";
    return 0;
}
```

### 输出

执行时，上述程序产生以下输出：

![图 4.2：PIMPL 实现示例输出](image/C14583_04_02.jpg)

**图 4.2：PIMPL 实现示例输出**

## PIMPL 的优缺点

使用 PIMPL 最大的优势在于它打破了类的客户端代码与其实现之间的编译时依赖。这使得构建时间更快，因为 PIMPL 消除了定义（头）文件中大量的 `#include` 指令，而是将它们仅推送到实现文件中按需包含。

它还将实现与客户端代码解耦。我们现在可以自由地更改 PIMPL 类的实现，并且只需重新编译该文件。这防止了编译级联，即对隐藏成员的更改触发客户端代码的重新构建。这被称为编译防火墙。

PIMPL 惯用法的其他一些优点如下：

*   **数据隐藏** – 实现的内部细节真正隔离在实现类中。如果这是库的一部分，则可用于防止信息泄露，例如知识产权。
*   **二进制兼容性** – 类的二进制接口现在独立于私有字段。这意味着我们可以向实现添加字段而不影响客户端代码。这也意味着我们可以将实现类部署在共享库（`DLL` 或 `.so` 文件）中，并可自由更改它而不影响客户端代码。

这些优势是有代价的。缺点如下：

*   **维护工作量** – 可见类中存在将调用转发给实现类的额外代码。这增加了一层间接性，并带来了少量的复杂性增加。
*   **内存管理** – 为实现添加指针现在要求我们管理内存。它还需要额外的存储空间来保存指针，在内存受限的系统中（例如物联网设备），这可能至关重要。

## 使用 `std::unique_ptr<>` 实现 PIMPL

我们目前的 `Dialog` 实现使用原始指针来保存 PIMPL 实现的引用。在 *第 3 章*、*能与该做的距离——对象、指针和继承* 中，我们讨论了对象的所有权，并介绍了智能指针和 RAII。PIMPL 指针指向的隐藏对象是一种需要管理的资源，应使用 **RAII** 和 `std::unique_ptr` 来执行。正如我们将看到的，使用 `std::unique_ptr` 实现 **PIMPL** 有一些注意事项。

让我们更改 `Dialog` 实现以使用智能指针。首先，头文件更改为引入 `#include <memory>` 行，并且可以移除析构函数，因为 `unique_ptr` 会自动删除实现类。

### 更新后的头文件 (`dialog.hpp`)

```cpp
#pragma once
#include <memory>

class Dialog
{
public:
    Dialog();
    void create(const char* message);
    bool show();

private:
    struct DialogImpl;
    std::unique_ptr<DialogImpl> m_pImpl;
};
```

显然，我们从实现文件中移除了析构函数，并修改构造函数以使用 `std::make_unique`。

### 更新后的构造函数

```cpp
Dialog::Dialog() : m_pImpl(std::make_unique<DialogImpl>())
{
}
```

当重新编译我们的新版本时，`Dialog.hpp` 和 `DialogImpl.cpp` 文件没有问题，但我们的客户端 `main.cpp` 报告了以下错误（使用 gcc 编译器），如下所示：

![图 4.3：使用 unique_ptr 编译 Pimpl 失败](image/C14583_04_03.jpg)

**图 4.3：使用 unique_ptr 编译 Pimpl 失败**

第一个错误报告 **对不完整类型 'Dialog::DialogImpl' 无效地应用了 'sizeof'**。问题在于在 `main.cpp` 中，当 `main()` 函数结束时，编译器试图为我们调用 `Dialog` 的析构函数。正如我们在 *第 2A 章*、*不允许鸭子——类型与推导* 中讨论的，编译器将为我们生成一个析构函数（因为我们移除了它）。这个生成的析构函数将调用 `unique_ptr` 的析构函数，这是错误的原因。如果我们查看 `unique_ptr.h` 的 **第 76 行**，会发现 `unique_ptr` 使用的默认 `deleter` 的 `operator()` 函数具有以下实现（`deleter` 是 `unique_ptr` 在销毁其指向的对象时调用的函数）：

```cpp
void operator()(_Tp* __ptr) const
{
    static_assert(!is_void<_Tp>::value, "can't delete pointer to incomplete type");
}
```

