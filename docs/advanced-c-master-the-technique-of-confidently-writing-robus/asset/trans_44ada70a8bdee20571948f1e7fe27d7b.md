

## `test_find` 的第二个模板

`test_find` 的第二个模板创建了一个特化版本，用于测试一个类是否拥有名为 `find()` 的成员函数，且其返回类型为 `std::true_type`。请注意，该版本的参数类型为 `int`。

`has_find<T,A0>` 模板通过从 `test_find()` 函数的返回类型派生自身来工作。如果 `T` 类没有 `find()` 方法，则只会生成 `std::false_type` 版本的 `test_find()`，因此 `has_find<T,A0>::value` 的值将为 false，并可用于 `if constexpr()` 中。

有趣的部分在于：如果 `T` 类拥有 `find()` 方法，那么两个 `test_find()` 方法都会被生成。但特化版本接受 `int` 类型的参数，而默认版本接受 `long` 类型的参数。当我们用零 (0) “调用”该函数时，它将匹配特化版本并使用它。参数差异很重要，因为不能有两个参数类型相同、仅返回类型不同的函数。如果想验证此行为，可将参数从 0 改为 0L 以强制调用 long 版本。

## 类模板

到目前为止，我们只涉及函数模板。但模板也可用于为类提供蓝图。模板类声明的通用结构如下：

```cpp
template<class T>
class MyClass {
    // 使用 T 的变量和方法。
};
```

函数模板允许我们生成通用算法，而类模板允许我们生成通用数据类型及其相关行为。

当我们介绍标准模板库 (STL) 时，强调了它包含容器模板——`vector`、`deque`、`stack` 等。这些模板允许我们存储和管理任意想要的数据类型，但仍能按预期表现。

### 练习 4：编写类模板

计算机科学中最常用的两种数据结构是栈和队列。两者在 STL 中都有现成实现。但为了让我们熟悉模板类，我们将编写一个可用于任何类型的栈模板类。开始吧：

1.  在 Eclipse 中打开 `Lesson2B` 项目，然后在 **Project Explorer（项目资源管理器）** 中展开 `Lesson2B`，再展开 `Exercise04`，双击 `Exercise4.cpp` 在编辑器中打开该练习文件。

2.  配置一个新的 **Launch Configuration（启动配置）**，名为 `L2BExercise4`，设置运行名称为 **Exercise4**。

3.  同时，配置一个新的 C/C++ Unit Run Configuration（C/C++ 单元测试运行配置），名为 `L2BEx4Tests`，用于运行 `L2BEx4tests`。设置 **Google Tests Runner（Google Test 运行器）**。

4.  点击测试的 **Run（运行）** 选项，首次运行测试：

    ![Figure 2B.32: Initial unit test for stacks](image/C14583_02B_32.jpg)

    *Figure 2B.32: 栈的初始单元测试*

5.  在编辑器中打开 `Stack.hpp`。你将看到以下代码：

    ```cpp
    #pragma once

    #include <vector>
    #include <cstddef>

    #define EXERCISE4_STEP  1

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

    模板定义的第一个要点是：它必须放在头文件中，以便在需要复用的地方通过 `#include` 包含。其次，我们使用了 pragma 指令 (`#pragma once`)，它告诉编译器：如果再次遇到此文件被 `#include`，则无需再次处理。虽然这不严格属于标准，但几乎所有现代 C++ 编译器都支持它。最后请注意，为了本练习的目的，我们选择使用 STL vector 来存储项。

6.  在编辑器中，在 `Stack` 类的 **public（公有）** 部分添加以下声明：

    ```cpp
    bool empty() const
    {
        return m_stack.empty();
    }
    ```

7.  在文件顶部，将 `EXERCISE4_STEP` 的值改为 `10`。点击 **Run（运行）** 按钮。练习 4 的测试应运行并失败：

    ![Figure 2B.33: Jumping to a Failing test](image/C14583_02B_33.jpg)

    *Figure 2B.33: 跳转到失败的测试*

8.  点击失败测试的名称，即 `DefaultConstructorInitsEmpty`。它会在右侧 Messages（消息）区显示失败原因。双击该消息。它将打开包含失败测试的文件并跳转到出错行，如上图所示。该测试存在错误。测试期望栈为空。然而我们可以看到 `empty()` 报告为 false。

9.  将 `ASSERT_FALSE` 改为 `ASSERT_TRUE` 并重新运行测试。这次测试通过，因为它测试的是正确的逻辑。

10. 接下来我们将添加一些类型别名，以便后续几个方法使用。在编辑器中，在 `empty()` 方法上方添加以下行：

    ```cpp
    using value_type = T;
    using reference = value_type&;
    using const_reference = const value_type&;
    using size_type = std::size_t;
    ```

11. 点击 **Run（运行）** 按钮重新运行测试。它们应该通过。在测试驱动开发中，口诀是：编写一个小测试并见其失败，然后编写刚好足够的代码使其通过。在这种情况下，我们实际上测试了别名定义是否正确，因为编译失败也是一种测试失败形式。我们现在准备好添加 push 函数了。

12. 在编辑器中，通过在 `empty()` 方法下方添加以下代码来修改 `Stack.hpp`：

    ```cpp
    void push(const value_type& value)
    {
        m_stack.push_back(value);
    }
    ```

13. 在文件顶部，将 `EXERCISE4_STEP` 的值改为 `15`。点击 **Run（运行）** 按钮。现在有两个测试运行并通过。`StackTests.cpp` 中的新测试 `PushOntoStackNotEmpty` 证明了 push 操作使栈不再为空。我们需要添加更多方法以确保其按预期工作。

14. 在编辑器中，通过在 `push()` 方法下方添加以下代码并将 `EXERCISE4_STEP` 改为 `16` 来修改 `Stack.hpp`：

    ```cpp
    size_type size() const
    {
        return m_stack.size();
    }
    ```

15. 点击 **Run（运行）** 按钮运行测试。现在应该有三个通过的测试。

16. 在编辑器中，通过在 `push()` 方法下方添加以下代码并将 `EXERCISE4_STEP` 改为 `18` 来修改 `Stack.hpp`：

    ```cpp
    void pop()
    {
        m_stack.pop_back();
    }
    ```

17. 点击 **Run（运行）** 按钮运行测试。现在应该有四个通过的测试。

18. 在编辑器中，通过在 `pop()` 方法下方添加以下代码并将 `EXERCISE4_STEP` 改为 `20` 来修改 `Stack.hpp`：

    ```cpp
    reference top()
    {
        m_stack.back();
    }

    const_reference top() const
    {
        m_stack.back();
    }
    ```

19. 点击 **Run（运行）** 按钮运行测试。现在有五个通过的测试，我们已实现了一个栈。

20. 从 Launch Configuration（启动配置）下拉菜单中选择 `L2BExercise4` 并点击 **Run（运行）** 按钮。练习 4 将运行并产生类似以下的输出：

    ![Figure 2B.34: Exercise 4 output](image/C14583_02B_34.jpg)

    *Figure 2B.34: 练习 4 输出*

检查 `Stack.hpp` 文件中的代码。在类内部定义类型的方法在 STL 中很常见（尽管由于历史原因它们可能使用 `typedef`）。`std::stack` 模板接受两个参数，第二个参数定义要使用的容器——vector 可以作为第一个选择。检查 `StackTests.cpp` 中的测试。测试应以表明其测试目的的方式命名，并应专注于完成该目的。

### 实践 1：开发通用的 "contains" 模板函数

