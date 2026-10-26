

![](img/6507d435e2edfa3c00c220d0ccfcaf94_209_0.png)

### 图2B.32：栈的初始单元测试

- 5. 在编辑器中打开 **Stack.hpp**。你将看到以下代码：

```cpp
#pragma once
#include <vector>
#include <cstddef>

#define EXERCISE4_STEP    1

namespace acpp
{

template <typename T>
class Stack
{
public:

private:
    std::vector<T> m_stack;
};

} // namespace acpp
```

关于模板定义，首先需要注意的是，它必须放在头文件中，以便在需要复用时可以包含该文件。其次，我们使用了一个编译指示指令（**#pragma once**），它告诉编译器：如果再次遇到需要 #include 这个文件的情况，则不必重复包含。虽然这并非严格的标准内容，但几乎所有现代 C++ 编译器都支持它。最后要注意的是，出于本练习的目的，我们选择使用 STL 的 vector 来存储元素。

- 6. 在编辑器中，在 **Stack** 类的 **public** 部分添加以下声明：

```cpp
bool empty() const
{
    return m_stack.empty();
}
```

- 7. 在文件顶部，将 EXERCISE4_STEP 改为 10。单击 Run 按钮。Exercise 4 的测试应该会运行并失败：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_210_0.png)

### 图2B.33：跳转到失败的测试

- 8. 单击失败测试的名称，即 DefaultConstructorInitsEmpty。它会在右侧的 Messages 部分显示失败原因。双击该消息，它会打开包含失败测试的文件并跳转到有问题的行，如前面的截图所示。这个测试有一个错误。在测试中，我们期望栈是空的。然而，我们可以看到 empty() 返回 false。

- 9. 将 ASSERT_FALSE 改为 ASSERT_TRUE，然后重新运行测试。这次测试会通过，因为它测试的是正确的内容。

- 10. 接下来，我们要添加一些类型别名，以备接下来的几个方法使用。在编辑器中，将以下行添加到 empty() 方法的正上方：

```cpp
using value_type = T;
using reference = value_type&;
using const_reference = const value_type&;
using size_type = std::size_t;
```

- 11. 单击 Run 按钮重新运行测试。它们应该会通过。在进行测试驱动开发时，口诀是先编写一个小测试并看到它失败，然后编写刚好足够的代码使其通过。在这种情况下，我们实际上验证了别名定义的正确性，因为编译失败也是测试失败的一种形式。现在我们可以准备添加 push 函数了。

- 12. 在编辑器中，通过修改 **Stack.hpp**，在 **empty()** 方法正下方添加以下代码：

```cpp
void push(const value_type& value)
{
    m_stack.push_back(value);
}
```

- 13. 在文件顶部，将 **EXERCISE4_STEP** 改为 **15**。单击 **Run** 按钮。现在有两个测试会运行并通过。在 **StackTests.cpp** 中的新测试 **PushOntoStackNotEmpty** 证明了 push 操作确实让栈不再为空。我们需要添加更多方法来确保它做了预期的事情。

- 14. 在编辑器中，通过修改 **Stack.hpp**，在 **push()** 方法正下方添加以下代码，并将 **EXERCISE4_STEP** 的值改为 **16**：

```cpp
size_type size() const
{
    return m_stack.size();
}
```

- 15. 单击 **Run** 按钮运行测试。现在应该有三个测试通过。

- 16. 在编辑器中，通过修改 **Stack.hpp**，在 **push()** 方法正下方添加以下代码，并将 **EXERCISE4_STEP** 的值改为 **18**：

```cpp
void pop()
{
    m_stack.pop_back();
}
```

- 17. 单击 **Run** 按钮运行测试。现在应该有四个测试通过。

- 18. 在编辑器中，通过修改 **Stack.hpp**，在 **pop()** 方法正下方添加以下代码，并将 **EXERCISE4_STEP** 的值改为 **20**：

```cpp
reference top()
{
    return m_stack.back();
}

const_reference top() const
{
    return m_stack.back();
}
```

- 19. 单击 Run 按钮运行测试。现在有五个测试通过，我们已经实现了一个栈。

- 20. 从“启动配置”下拉菜单中选择 L2BExercise4，并单击 Run 按钮。Exercise 4 将运行并产生类似下面的输出：

```
------ Exercise 4 ------
Stack is empty
Pushing 0.0F onto stack
Stack is not empty
Pushing 3.14159F onto stack
Stack has 2 items
Top item is 3.14159
Complete.
```

### 图2B.34：Exercise 4 的输出

检查当前 Stack.hpp 文件中的代码。在类内部定义类型的方法在整个 STL 中都很常见（尽管由于历史原因，它们可能使用 typedef）。std::stack 模板接受两个参数，第二个参数定义了要使用的容器——vector 可能是第一个想到的容器。检查 StackTests.cpp 中的测试。测试的命名应当表明其测试目标，并且测试应当专注于验证该目标。

## 活动1：开发一个通用的“contains”模板函数

Python 编程语言有一个名为 "in" 的成员运算符，可用于任何序列，即列表、序列、集合、字符串等。尽管 C++ 有超过 100 个算法，但它并没有一个等效的方法来实现同样的功能。C++20 在 std::set 上引入了 contains() 方法，但这还不够。我们需要创建一个 contains() 模板函数，使其适用于 std::set、std::string、std::vector 以及任何其他提供迭代器的容器。这一点可以通过该容器是否支持调用 end() 来判断。我们追求最佳性能，因此对于任何拥有 find() 成员方法的容器，我们都会调用该方法（这是最有效的方式），否则就回退到使用容器的 std::end()。我们还需要特殊处理 std::string，因为它的 find() 方法返回一个特殊值。

我们可以使用一个通用模板和两个特化来实现这一点，但本活动的目的是让我们使用 SFINAE 和 if constexpr 的技术使其正常工作。此外，该模板必须只适用于支持 end(C) 的类。请按照以下步骤来实现此活动：

- 1. 从 Lesson2B/Activity01 文件夹加载准备好的项目。
- 2. 定义辅助模板函数和类，使用 npos 成员来检测 std::string 的情况。
- 3. 定义辅助模板函数和类，检测类是否具有 `find()` 方法。
- 4. 定义 contains 模板函数，使用 constexpr 在三种实现之间选择——string 情况、具有 find 方法的情况或一般情况。

完成上述步骤后，预期输出应如下所示：

```
------ Activity 1 ------
Set contains 5: true
Set contains 15: false
Vector contains 5: true
Vector contains 15: false
String contains 'times': true
String contains 'Light': false
String contains 'm': true
String contains 'z': false

Complete.
```

### 图2B.35：contains 成功实现的输出

> **注意**
> 
> 本活动的解决方案可在第 653 页找到。

## 总结

在本章中，我们学习了接口、继承和多态，这些扩展了我们对类型所能执行操作的技能。我们第一次涉足了使用 C++ 模板的泛型编程，并了解了 C++ 标准库（包括 STL）为我们免费提供的内容。我们探索了 C++ 中一个开箱即用的特性，即模板类型推导，它使我们在使用模板时更加轻松。随后，我们进一步深入模板，学习了如何使用 SFINAE 和 if constexpr 来控制编译器包含的模板部分。这些构成了我们 C++ 学习之旅的基础。在下一章中，我们将重新审视栈和堆，并理解什么是异常、异常发生时会发生什么以及何时会发生。我们还将学习如何在发生异常时保护我们的程序免受资源损失。

