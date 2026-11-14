

std::cout << "\n\n------ Functors with STL ------\n";

std::vector<int> values{1,2,3,4,5};

PrintVector("Before transform", values);

std::transform(values.begin(), values.end(), values.begin(), Scaler(3));

PrintVector("After transform", values);

std::cout << "Complete.\n";

return 0;

}

如果运行此程序，生成的输出将如下所示：

![图 4.11：显示 Scaler 变换向量的程序输出](image/C14583_04_11.jpg)

*图 4.11：显示 Scaler 变换向量的程序输出*

## 练习 2：实现函数对象

在本练习中，我们将实现两个不同的函数对象，它们可以与 STL 算法 `for_each` 配合使用。

1.  在 Eclipse 中打开 **Lesson4** 项目，然后在 **Project Explorer** 中展开 **Lesson4** 再展开 **Exercise02**，双击 `Exercise2.cpp` 以在编辑器中打开该练习文件。

2.  由于这是一个基于 CMake 的项目，请将当前构建器更改为 **CMake Build (portable)**。

3.  点击 **Launch Configuration** 下拉菜单并选择 **New Launch Configuration…**。配置 `L4Exercise2` 以 **Exercise2** 为名称运行。

4.  点击 **Run** 按钮。练习 2 将运行并产生以下输出：

    ![图 4.12：练习 2 初始输出](image/C14583_04_12.jpg)

    *图 4.12：练习 2 初始输出*

    我们要做的第一件事是通过引入一个函数对象来修复输出格式。

5.  在编辑器中，在 `main()` 函数定义之前，添加以下类定义：

    ```cpp
    struct Printer
    {
        void operator()(int n)
        {
            std::cout << m_sep << n;
            m_sep = ", ";
        }
        const char* m_sep = "";
    };
    ```

6.  在 `main()` 方法中，将以下代码：

    ```cpp
    std::cout << "Average of [";
    for (auto n : values)
        std::cout << n << ", ";
    std::cout << "] = ";
    ```

    **改为**

    ```cpp
    std::cout << "Average of [";
    std::for_each(values.begin(), values.end(), Printer());
    std::cout << "] = ";
    ```

7.  点击 **Run** 按钮。练习将运行并产生以下输出：

    ![图 4.13：练习 2 改进后的输出格式](image/C14583_04_13.jpg)

    *图 4.13：练习 2 改进后的输出格式*

8.  `Printer` 类的内部状态允许我们修复格式。现在，引入一个 **聚合器** 类来计算 **平均值**。将以下类定义添加到文件顶部：

    ```cpp
    struct Averager
    {
        void operator()(int n)
        {
            m_sum += n;
            m_count++;
        }
        float operator()() const
        {
            return static_cast<float>(m_sum) / (m_count == 0 ? 1 : m_count);
        }
        int m_count{0};
        int m_sum{0};
    };
    ```

9.  修改 `main()` 方法以使用 `Averager` 类，如下所示：

    ```cpp
    int main(int argc, char** argv)
    {
        std::cout << "\n------ Exercise 2 ------\n";
        std::vector<int> values{1,2,3,4,5,6,7,8,9,10};
        Averager averager = std::for_each(values.begin(), values.end(), Averager());
        std::cout << "Average of [";
        std::for_each(values.begin(), values.end(), Printer());
        std::cout << "] = ";
        std::cout << averager() << "\n";
        std::cout << "Complete.\n";
        return 0;
    }
    ```

10. 点击 **Run** 按钮。练习将运行并产生以下输出：

    ![图 4.14：带有平均值的练习 2 输出](image/C14583_04_14.jpg)

    *图 4.14：带有平均值的练习 2 输出*

请注意，`std::for_each()` 返回传入它的 `Averager` 实例。该实例被复制到变量 `averager` 中，后者随后包含计算平均值所需的数据。在本练习中，我们实现了两个函数对象或 **函数对象** 类：`Averager` 和 `Printer`，当它们被传递给 STL 算法 `for_each` 时，可以作为函数使用。

## std::function<> 模板

C++11 引入了一个通用的多态函数包装器模板 `std::function<>`，它使得实现回调和其他函数相关功能变得更加容易。`std::function` 保存一个称为 **目标** 的可调用对象。如果它不包含目标，则称为 **空**。调用一个空的 `std::function` 将导致抛出 `std::bad_function_call` 异常。

函数对象可以存储、复制或调用以下任意可调用对象作为目标：函数、函数对象（定义了 `operator()`）、成员函数指针或 Lambda 表达式。我们将在 *What is a Lambda Expression?* 主题中详细介绍相关内容。

实例化 `std::function` 对象时，只需提供函数签名而无需提供用于初始化的值，从而得到一个空实例。实例化方式如下：

![图 4.15：std::function 声明的结构](image/C14583_04_15.jpg)

*图 4.15：std::function 声明的结构*

传递给模板的参数定义了由 `variable` 存储的目标的 **函数签名**。签名以返回类型开始（可以是 void），随后在括号中列出调用该函数时将使用的类型列表。

将自由函数和 **函数对象** 与 `std::function` 配合使用非常直接。只要签名与传递给 `std::function` 模板的参数匹配，我们就可以简单地将自由函数或 **函数对象** 赋值给实例。

```cpp
void FreeFunc(int value);

struct Functor
{
    void operator()(int value);
};

std::function<void(int)> func;
Functor functor;

func = FreeFunc;          // 将目标设为 FreeFunc
func(32);                 // 用参数 32 调用 FreeFunc
func = functor;           // 将目标设为 functor
func(42);                 // 用参数 42 调用 Functor::operator()
```

然而，如果我们想在对象实例上使用成员方法，则需要使用另一个 STL 辅助模板 `std::bind()`。如果运行以下程序：

```cpp
#include <iostream>
#include <functional>

struct Binder
{
    void method(int a, int b)
    {
        std::cout << "Binder::method(" << a << ", " << b << ")\n";
    }
};

int main()
{
    std::cout << "\n\n------ Member Functions using bind ------\n";
    Binder binder;
    std::function<void(int,int)> func;
    auto func1 = std::bind(&Binder::method, &binder, 1, 2);
    auto func2 = std::bind(&Binder::method, &binder, std::placeholders::_1, std::placeholders::_2);
    auto func3 = std::bind(&Binder::method, &binder, std::placeholders::_2, std::placeholders::_1);

    func = func1;
    func(34,56);
    func = func2;
    func(34,56);
    func = func3;
    func(34,56);

    std::cout << "Complete.\n";
    return 0;
}
```

然后我们将得到以下输出：

![图 4.16：使用 std::bind() 和 std::function 的程序输出](image/C14583_04_16.jpg)

*图 4.16：使用 std::bind() 和 std::function 的程序输出*

有几点需要注意：

*   函数 `method()` 通过类名作为作用域指定符来引用；
*   `Binder` 实例的地址作为第二个参数传递给 `std::bind()`，这使其成为传递给 `method()` 的第一个参数。这是必要的，因为所有非静态成员都有一个隐式的 `this` 指针作为第一个参数传递。
*   使用 `std::placeholders` 定义，我们可以绑定调用绑定方法时使用的参数，甚至可以改变传递的顺序（如 `func3` 所示）。

