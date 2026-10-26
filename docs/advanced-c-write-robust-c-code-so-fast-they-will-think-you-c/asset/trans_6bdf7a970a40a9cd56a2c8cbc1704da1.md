

```
void create(const char* message);
bool show();

private:
    struct DialogImpl;
    DialogImpl* m_pImpl;
};
```

用户可以使用使用对话框所需的所有函数，但并不了解其实现方式。注意，我们声明了DialogImpl但没有定义它。一般来说，对于像DialogImpl这样的类，我们能做的并不多。但有一件事是允许的，那就是声明指向它的指针。C++的这个特性允许我们在实现文件中隐藏实现细节。这意味着在这个简单的情况下，我们不需要为这个声明包含任何头文件。

实现文件dialogImpl.cpp可以实现如下：

```
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
```

```
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

我们从中注意几点：

+   - 我们在定义Dialog所需的方法之前定义实现类**DialogImpl**。这是必要的，因为Dialog需要通过**m_pImpl**调用这些方法，这意味着它们必须先被定义。
+   - **Dialog**的构造函数和析构函数负责内存管理。
+   - 我们只在实现文件中包含实现所需的所有头文件。这通过最小化**Dialog.hpp**中需要包含的头文件数量来减少耦合。

## PIMPL的优点与缺点

使用PIMPL的最大优点是它打破了类的客户端与其实现之间的编译时依赖。这可以实现更快的构建时间，因为PIMPL消除了定义（头）文件中的大量#include指令，而是将它们推迟到仅在实现文件中才需要。

它还使实现与客户端解耦。我们现在可以自由更改PIMPL类的实现，并且只需要重新编译该文件。这避免了因隐藏成员的变化而触发客户端重建的编译级联。这被称为编译防火墙。

PIMPL惯用法的其他一些优点如下：

+   - **数据隐藏** – 实现的内部细节真正隔离在实现类中。如果这是库的一部分，那么它可以用来防止信息披露，例如知识产权。
+   - **二进制兼容性** – 类的二进制接口现在独立于私有字段。这意味着我们可以向实现中添加字段而不影响客户端。这也意味着我们可以将实现类部署在共享库（DLL或.so文件）中，并可以自由更改它而不影响客户端代码。

这些优点是有代价的。缺点如下：

+   - **维护成本** – 可见类中有额外的代码将调用转发给实现类。这增加了一层间接性，并略微增加了复杂性。
+   - **内存管理** – 为实现添加一个指针现在要求我们管理内存。它还需要额外的存储来保存指针，并且在内存受限的系统（例如：物联网设备）中这可能是关键的。

## 使用unique_ptr<>实现PIMPL

我们当前的Dialog实现使用原始指针来持有PIMPL实现引用。在第3章“能与应该之间的距离——对象、指针和继承”中，我们讨论了对象的所有权，并引入了智能指针和RAII。PIMPL指针指向的隐藏对象是需要管理的资源，应该使用RAII和std::unique_ptr来管理。正如我们将看到的，使用std::unique_ptr实现PIMPL有一些注意事项。

让我们更改Dialog实现以使用智能指针。首先，头文件需要引入**#include <memory>**行，并且可以移除析构函数，因为**unique_ptr**会自动删除实现类。

```
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

显然，我们从实现文件中移除了析构函数，并修改构造函数以使用**std::make_unique**。

```
Dialog::Dialog() : m_pImpl(std::make_unique<DialogImpl>())
{
}
```

重新编译新版本时，**Dialog.hpp**和**DialogImpl.cpp**文件没有问题，但我们的客户端**main.cpp**报告了以下错误（使用gcc编译器），如下所示：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_304_0.png)

第一个错误报告**对不完整类型'Dialog::DialogImpl'进行了无效的'sizeof'应用**。问题在于，在**main.cpp**文件中，当**main()**函数结束时，编译器试图为我们调用**Dialog**的析构函数。正如我们在*第2A章，不许鸭子——类型与推导*中讨论的那样，编译器会为我们生成一个析构函数（因为我们移除了它）。这个生成的析构函数将调用**unique_ptr**的析构函数，这就是错误的原因。如果查看**unique_ptr.h**文件的**第76行**，我们会发现以下用于**unique_ptr**的默认删除器的**operator()**函数的实现（删除器是**unique_ptr**在销毁其指向的对象时调用的函数）：

```cpp
void

operator()(_Tp* __ptr) const
{
    static_assert(!is_void<_Tp>::value, "can't delete pointer to incomplete type");
    static_assert(sizeof(_Tp)>0, "can't delete pointer to incomplete type");
    delete __ptr;
}
```

我们的代码在第二个static_assert()语句上失败，该语句以错误终止编译。问题在于编译器试图为std::unique_ptr<DialogImpl>生成析构函数，而DialogImpl是一个不完整类型。所以，要解决这个问题，我们控制析构函数的生成在DialogImpl是完整类型的地方。

为此，我们将析构函数的声明放回类中，并将其实现添加到DialogImpl.cpp文件中。

```
Dialog::~Dialog()
{
}
```

当我们编译并运行程序时，它会产生与之前完全相同的输出。实际上，如果我们只需要一个空的析构函数，可以用以下代码替换上面的代码：

```
Dialog::~Dialog() = default;
```

如果我们编译并运行程序，将产生以下输出：

```
------ Pimpl ------
Creating the Dialog
Showing the message: 'Hello World'
Dialog displayed
Complete.
```

图4.4：示例unique_ptr Pimpl实现输出

## unique_ptr<> PIMPL 特殊函数

由于PIMPL通常意味着可见接口类拥有实现类，移动语义是自然契合的。然而，就像编译器生成的析构函数实现是正确的，编译器生成的移动构造函数和移动赋值运算符将给出预期的行为，即对成员unique_ptr执行移动。两个移动操作在赋值转移的值之前都可能需要执行delete，因此会遇到与不完整类型的析构函数相同的问题。解决方案与析构函数相同——在头文件中声明方法，并在类型完整时实现——即在实现文件中实现。所以，我们的头文件如下所示：

```
class Dialog
{
public:
```

