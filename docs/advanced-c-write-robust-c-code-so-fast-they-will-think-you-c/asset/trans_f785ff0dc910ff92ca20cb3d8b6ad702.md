

## 练习 2：实现异常处理器

在本练习中，我们将实现一个异常处理器层次结构来管理异常的处理方式。按照以下步骤实现本练习：

+   1. 在 Eclipse 中打开 **Lesson3** 项目。然后在 **Project Explorer** 中展开 **Lesson3** 然后 **Exercise02** 并双击 **exceptions.cpp** 以在编辑器中打开该练习的文件。此文件包含以下代码：

```
```cpp
#include <exception>
#include <iostream>

void run_exceptions()
{
    try
    {
        throw std::domain_error("We got one!!!!");
    }
    catch(...)
    {
        std::cout << "Exception caught by default handler\n";
    }
    catch(const std::exception& e)
    {
        std::cout << "Exception '" << e << "' caught by std::exception handler\n";
    }
    catch(const std::logic_error& e)
    {
        std::cout << "Exception '" << e << "' caught by std::logic_error handler\n";
    }
    catch(const std::domain_error& e)
    {
        std::cout << "Exception '" << e << "' caught by std::domain_error handler\n";
    }
}

int main()
{
    std::cout << "\n\n------ Exercise 2 ------\n";
    run_exceptions();
```

```
std::cout << "Complete.\n";
return 0;
}
```

> **注意**
> 
> 所有异常处理器都使用了相同的异常参数名称，即 *e*。该变量的作用域仅限于声明它的 catch 块。

+   2. 点击 **Launch Configuration** 下拉菜单并选择 **New Launch Configuration....** 从 **Search Project** 菜单配置 **L3Exercise2** 应用程序以使用名称 **L3Exercise2** 运行它。
+   3. 完成后，它将成为当前选定的 **Launch Configuration**。
+   4. 点击 **Run** 按钮。练习 2 将运行并产生以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_239_0.png)

图 3.18：练习 2 输出 – 默认处理器捕获了异常

+   5. 在 Console 窗口中，点击 **Display Selected Console** 按钮，并选择 **CDT Global Build Console**。滚动浏览窗口。你会发现（如果使用的是 GCC 编译器）与我们放置异常处理器的顺序相关的五条警告信息。（实际上，第一条警告通常会是错误，只是 **CMake** 文件在编译此目标时设置了 **-fpermissive** 标志。）
+   6. 在编辑器中，将默认异常处理器 **catch(...)** 移到 **std::domain_error** 处理器之后。点击 **Run** 按钮。练习 2 将运行并产生以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_239_1.png)

图 3.19：std::exception 处理器已被使用

+   7. 在编辑器中，将 **std::exception** 处理器移到 **std::domain_error** 处理器之后。点击 **Run** 按钮。这次，它将报告 **std::logic_error** 处理器被执行。
+   8. 在编辑器中，将 `std::logic_error` 处理器移到 `std::domain_error` 处理器之后。点击 **Run** 按钮。这次，它将报告 `std::domain_error` 处理器被执行，这正是我们所期望的。
+   9. 现在将 `throw` 行改为 `std::logic_error` 异常。点击 **Run** 按钮。这次，它将报告 `std::logic_error` 处理器被执行，如预期。
+   10. 现在将 `throw` 行改为 `std::underflow_error` 异常。点击 **Run** 按钮，这次它将报告异常被 `std::exception` 处理器捕获，如预期。`std::exception` 是所有标准库异常的基类。

在本练习中，我们实现了一系列异常处理器并观察了异常处理器的顺序如何影响异常的捕获方式，以及如何使用异常层次结构。

## CMake 生成器表达式

在使用 **CMake** 时，有时需要调整变量的值。**CMake** 是一个构建生成系统，可以为许多构建工具和编译器工具链生成构建文件。由于这种灵活性，如果你想在编译器中启用某个功能，只需将其应用于特定类型即可。这是因为不同厂商之间的命令行选项不同。例如，启用 g++ 编译器 C++17 支持的命令行选项是 **-std=c++17**，但对于 **msvc** 则是 **/std:c++17**。如果打开 **CMakeLists.txt** 文件并定位 **L3Exercise2 add_executable**，则其后会有以下行：

```
target_compile_options(L3Exercise2 PRIVATE $<$<CXX_COMPILER_ID:GNU>:-fpermissive>)
```

这使用了 **$<CXX_COMPILER_ID:GNU>** 变量查询来检查是否为 GCC 编译器。如果是，则生成 1（真），否则为 0（假）。它还使用了 **$<condition:true_string>** 条件表达式，仅为 **L3Exercise2** 目标的 gcc 编译器将 **-fpermissive** 添加到编译器选项中。这些可以为每种编译器类型分别通过对 **target_compile_options** 的调用添加，也可以通过一次调用完成。

> **注意**
> 
> 有关生成器表达式的更多信息，请查看以下链接：https://cmake.org/cmake/help/v3.15/manual/cmake-generator-expressions.7.html。

## 异常使用指南

在使用 C++ 代码中的异常时，请记住以下几点：

+   - 原则：**按值抛出，按引用捕获**
+   - **不要使用异常进行正常程序流程。** 如果函数遇到异常条件且无法满足其（功能）义务，则仅在此时才抛出异常。如果函数可以解决异常条件并履行其义务，则这不是异常。它们被称为异常是有原因的，如果你不使用它们，就不会产生任何处理开销。
+   - **不要在析构函数中抛出异常。** 请记住，由于栈展开，局部变量析构函数将被执行。如果在栈展开过程中调用析构函数并抛出异常，则程序将终止。
+   - **不要吞掉异常。** 不要使用默认的 catch 处理器而不对异常做任何处理。异常被抛出是为了指示存在问题，你应该对此采取行动。忽略异常可能导致以后难以排查的故障。这是因为吞掉的异常会真正丢失任何有用的信息。
+   - **异常对象是从 throw 处复制的。**

## 资源管理（在异常世界中）

到目前为止，我们已经研究了局部变量作用域，以及当变量超出作用域时 `automatic` 和 `dynamic lifetime variables` 如何被处理 – 自动生命周期变量（放置在栈上的变量）会被完全析构，而 `dynamic lifetime variables`（由程序员分配到堆上的变量）不会被析构：我们只是丢失了对它们的访问。我们还看到，当抛出异常时，会找到最近匹配的处理程序，并且在抛出点和处理程序之间的所有局部变量都将通过栈展开过程被析构。

我们可以利用这一知识编写健壮的资源管理类，使我们无需跟踪资源（动态生命周期变量、文件句柄、系统句柄等），以确保在我们使用完后将其释放（归还）。用于在正常操作和异常情况下管理资源的技巧被称为 **资源获取即初始化（RAII）**。

// 响应异常
}
}

这段代码显示了资源管理存在的两个潜在问题：

+   - 最重要的是，在打开和关闭文件之间发生异常会导致资源泄漏。如果这是系统资源，许多此类泄漏可能导致系统不稳定或应用程序性能受损，因为资源被耗尽。
+   - 此外，在一个方法中管理多个资源可能导致因错误处理而产生深层嵌套子句。这不利于代码的可读性，从而影响其理解和可维护性。很容易忘记释放资源，尤其是在存在多个退出点时。

那么，我们如何管理资源以获得异常安全且更简单的代码？这个问题并不独特于 C++，不同语言以不同方式处理它。Java、C# 和 Python 使用垃圾回收方法，在对象不再被引用时对其进行清理。但 C++ 没有垃圾回收，那么解决方案是什么？

考虑以下类：

```
class File {
public:
    File(const char* name, const char* access) {
        m_file = fopen(name, access);
        if (m_file == nullptr) {
            throw std::ios_base::failure("failed to open file");
        }
    }

    ~File() {
        fclose(m_file);
    }

    operator FILE*() {
        return m_file;
```

