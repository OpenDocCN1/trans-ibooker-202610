

**[captures]** 之后的所有内容与普通函数定义相同，只是没有函数名。Lambda 是一种便捷的方式，可以在使用的位置定义一段短小的函数（只需几行）。Lambda 通常作为参数传递，并且一般不会重复使用。还应注意，lambda 可以赋值给变量（通常使用 `auto` 声明）。

我们可以重写之前使用 **Scaler** 类的程序，改用 lambda 来实现相同的效果：

```
#include <iostream>

#include <vector>

#include <algorithm>

void PrintVector(const char* prefix, std::vector<int>& values)
{
    const char* sep = "";
    std::cout << prefix << " = [";
    for(auto n : values)
    {
        std::cout << sep << n;
        sep = ", ";
    }
    std::cout << "]\n";
}

int main()
{
    std::cout << "\n\n------ Lambdas with STL ------\n";
    std::vector<int> values{1,2,3,4,5};

    PrintVector("Before transform", values);
    std::transform(values.begin(), values.end(), values.begin(),
    [] (int n) {return 5*n;}
    );
```

```
PrintVector("After transform", values);

std::cout << "Complete.\n";
return 0;
}
```

该程序运行时，输出显示向量已按比例缩放 5 倍：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_329_0.png)

图 4.24：使用 lambda 进行缩放变换

程序中的 lambda 是 `[] (int n) {return 5*n;}`，它有一个空的捕获子句 `[]`。空的捕获子句意味着该 lambda 函数不会访问周边作用域中的任何变量。如果 lambda 不接受任何参数，则参数子句 `()` 是可选的。

## 将数据捕获到 Lambda 中

**捕获子句**（或按照 C++ 规范称为 **lambda 引入器**）允许匿名函数从周边作用域捕获数据以供后续使用。函数与解析函数变量所用作用域的组合被称为**闭包**。此处的“作用域”是捕获子句中指定的一组变量绑定。在前一节中，我们说过 lambda 是匿名函数。通过闭包实现变量捕获后，lambda 更准确地应被称为匿名函数对象。编译器会创建一个匿名类，其中包含一个内联构造函数来捕获变量，以及一个函数调用运算符 **operator()**。

捕获子句是一个以逗号分隔的列表，包含零个或多个被捕获的变量。还存在默认捕获的概念——按引用捕获或按值捕获。因此，捕获的基本语法为：

- `[&]` – 按引用捕获作用域内所有自动存储期变量
- `[=]` – 按值捕获作用域内所有自动存储期变量（生成副本）
- `[&x, y]` – 按引用捕获 `x`，按值捕获 `y`

19. 点击 **Run** 按钮，输出现在正确报告了计数：

```
------ Exercise 4 ------
Initial Vector = [46, 27, 26, 27, 32, 1, 18, 50, 21, 49]
After sort = [1, 18, 21, 26, 27, 27, 32, 46, 49, 50]
There are 7 values > 25
There are 3 values > 40
Complete.
```

图 4.32：重用 pred lambda 得到正确输出

在本练习中，我们实现了多个 lambda，使用了 lambda 表达式语法的各种特性，包括 init 捕获和 `mutable`。

> **使用 lambda**
> 
> 尽管 lambda 是 C++ 的一项强大特性，但应当合理使用。目标始终是生成可读性强的代码。因此，虽然 lambda 可能简洁明了，但有时为了便于维护，最好还是将功能提取为命名良好的方法。

## 可变参数模板

在 *第 2B 章，切勿生搬硬套——模板与推导* 中，我们介绍了泛型编程和模板。模板在 C++03 之前就已经是 C++ 的一部分。在 C++11 之前，模板的参数数量是固定的。在某些需要可变数量参数的情况下，必须为每种所需参数数量编写一个模板。另外，也有像 **printf()** 这样的可变参数函数，可以接受可变数量的参数。可变参数函数的问题在于它们不是类型安全的，因为对参数的访问是通过 `va_arg` 宏进行类型转换实现的。C++11 通过引入可变参数模板改变了这一切，单个模板可以接受任意数量的参数。C++17 通过引入 **constexpr if** 结构改进了可变参数模板的编写方式，该结构允许将基准情形模板与“递归”模板合并。

最好的方法是实现一个可变参数模板并解释其工作原理。

```
#include <iostream>

#include <string>

template<typename T, typename... Args>
T summer(T first, Args... args) {
```

```
T summer(T first, Args... args) {
    if constexpr(sizeof...(args) > 0)
        return first + summer(args...);
    else
        return first;
}
```

在上述代码中：

- `typename... Args` 将 `Args` 声明为**模板参数包**。
- `Args... args` 是一个**函数参数包**，即一个参数包，其类型由 `Args` 给出。
- `sizeof...(args)` 返回 `args` 中包元素的数量。它是包展开的一种特殊形式。
- `args...` 在递归调用 `summer()` 时展开参数包。

或者，你可以将模板看作是等效于：

```
template<typename T, typename T1, typename T2, ..., typename Tn>
T summer(T first, T1 t1, T2, ..., Tn tn) {
    if constexpr(sizeof...( t1, t2, ..., tn) > 0)
        return first + summer(t1, t2, ..., tn);
    else
        return first;
}
```

当编译器处理示例程序中的 `summer(1, 3, 5, 7, 9, 11)` 时，它会执行以下步骤：

- 它推导出 `T` 是 `int`，而 `Args...` 是包含 `<int, int, int, int, int>` 的参数包。
- 由于包中的参数数量大于零，编译器生成 `first + summer(args...)`，其中省略号解包模板实参，将 `summer(args...)` 转换为 `summer(3,5,7,9,11)`。

- 然后编译器为 `summer(3,5,7,9,11)` 生成代码。同样，会应用 `first + summer(args...)`，即 `summer(5,7,9,11)`。
- 此过程不断重复，直到编译器必须为 `summer(11)` 生成代码。在这种情况下，会触发 `constexpr if` 语句的 `else` 分支，仅返回 `first`。

由于类型由模板的参数决定，我们并不局限于所有参数具有相同类型。我们已经在 STL 中见过几个可变参数模板——`std::function` 和 `std::bind`。

还有另一种类型的可变参数模板，它将参数转发给另一个函数或模板。这类模板本身做得很少，但提供了一种标准的处理方式。一个例子是 `make_unique` 模板，它可以实现为：

```
template<typename T, typename... Args>
unique_ptr<T> make_unique(Args&&... args)
{
    return unique_ptr<T>(new T(std::forward<Args>(args)...));
}
```

`make_unique` 必须调用 `new` 运算符来分配内存，然后调用适合该类型的构造函数。调用构造函数所需的参数数量可能差异很大。这种形式的可变参数模板引入了额外的包展开：

- `Args&&...` 表示我们有一组转发引用。
- `std::forward<Args>(args)...` 包含两个同时展开的参数包，它们必须具有相同数量的元素——`Args` 是模板参数包，`args` 是函数参数包。

每当我们需要在可变参数模板中将一个函数调用转发给另一个函数调用时，就会使用这种模式。

