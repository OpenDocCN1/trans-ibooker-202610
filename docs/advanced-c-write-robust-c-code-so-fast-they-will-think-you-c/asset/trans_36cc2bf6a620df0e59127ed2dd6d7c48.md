

+   10. 现在，将 `IntVector::iterator` 的唯一出现更改为 `IntVectorIter`。
+   11. 单击 **Run** 按钮。输出应与之前相同。

在本练习中，typedef 和 using 别名之间似乎几乎没有区别。在任何一种情况下，使用一个命名良好的别名都会使代码更易于阅读和理解。当涉及更复杂的别名时，**using** 提供了更简便的写法。**using** 于 C++11 中引入，现在已成为定义别名的首选方法。它相对于 **typedef** 还有其他优势，例如可以在模板内部使用。

## Templates – More than Generic Programming

模板不仅能提供通用编程（一种带类型的 cookie cutter）。在通用编程的情况下，模板作为一个不可更改的蓝图运行，并为指定的类型或类型生成编译后的模板版本。

模板可以编写为基于所涉及类型的函数或算法特化。这被称为 **template specialization**，并且在我们之前使用的意义上并不是通用编程。只有当它使某些类型在给定上下文中按我们期望的方式表现时，才能称之为通用编程。当为所有类型使用的算法被修改时，就不能称之为通用编程。

请检查以下特化代码的示例：

```
```cpp
#include <iostream>

#include <type_traits>

template <typename T, std::enable_if_t<sizeof(T) == 1, int> = 0>
void print(T val)
{
    printf("%c\n", val);
}

template <typename T, std::enable_if_t<sizeof(T) == sizeof(int), int> = 0>
void print(T val)
{
    printf("%d\n", val);
}
```
```

```
}

template <typename T, std::enable_if_t<sizeof(T) == sizeof(double), int> = 0>
void print(T val)
{
    printf("%f\n", val);
}

int main(int argc, char** argv)
{
    print('c');
    print(55);
    print(32.1F);
    print(77.3);
}
```

它定义了一个模板，该模板根据使用 **std::enable_if_t<>** 和 **sizeof()** 的模板特化调用 **printf()**，使用不同的格式字符串。当我们运行它时，会生成以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_186_0.png)

**Figure 2B.14: Erroneous print template program output**

## Substitution Failure Is Not An Error – SFINAE

打印的 **32.1F (-1073741824)** 值与该数字没有任何相似之处。如果我们检查编译器为以下程序生成的代码，就会发现它好像我们写了以下内容（以及更多）：

```
template<typename int, int=0>
void print<int,0>(int val)
{
    printf("%d\n",val);
```

第一个 **enable_if** 模板将导致一个空结构体（或类）的定义。第二个 **enable_if** 模板是针对第一个模板参数为 true 的特化，将导致带有 typedef 定义的类。**enable_if_t** 的定义是一个辅助模板，它消除了我们在使用它时在模板末尾输入 `::type` 的需求。那么，这如何工作？请考虑以下代码：

```
```cpp
template <typename T, std::enable_if_t<condition, int> = 0>

void print(T val) { … }
```
```

如果在编译时评估的条件结果为 **true**，则 **enable_if_t** 模板将导致如下所示的模板：

```
```cpp
template <typename T, int = 0>

void print(T val) { … }
```
```

这是有效的语法，并且该函数作为候选函数添加到符号表中。如果在编译时计算的条件结果为 **false**，则 **enable_if_t** 模板将导致如下所示的模板：

```
```cpp
template <typename T, = 0>

void print(T val) { … }
```
```

这是 **malformed code**，现在被丢弃 – SFINAE 正在发挥作用。

**std::is_floating_point_v<T>** 是另一个辅助类，它访问 **std::is_floating_point<T>** 模板的 `::value` 成员。它的名字说明了一切 – 如果 T 是浮点类型（float、double、long double），它将为 true；否则为 false。如果我们进行此更改，则编译器（GCC）会生成以下错误：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_188_0.png)

*Figure 2B.15: Compiler error for the modified print template program*

现在的问题是，我们有两个模板可以满足类型为 float 时的条件：

```
```cpp
template <typename T, std::enable_if_t<sizeof(T) == sizeof(int), int> = 0>

void print(T val)
{
    printf("%d\n", val);
}
```
```

如果我们现在编译程序，它会完成编译并链接程序。如果我们运行它，它现在会产生以下（预期）输出：

```
C
55
32.099998
77.300000
```

**Figure 2B.17: Corrected print template program output**

## Floating-Point Representations

**32.099998** 不应该是 **32.1** 吗？那就是传递给函数的值。在计算机上执行浮点运算的问题在于，表示会自动引入误差。实数形成一个连续（无限）的域。如果你考虑实数域中的 1 和 2，那么它们之间有无限多个实数。不幸的是，计算机对浮点数的表示对其值进行了量化，无法表示所有无限多个数字。用于存储数字的位数越多，其在实数域上的表示就越好。因此，long double 比 double 好，比 float 好。它真正取决于你的问题域，什么才适合存储数据。回到 **32.099998**。计算机将单精度数字存储为 2 的幂之和，然后按幂因子移动它们。整数通常很容易，因为它们可以用 **2^n 幂（n>=0）** 的和轻松表示。小数部分，在本例中为 0.1，必须表示为 **2^(-n)（n>0）** 的和。我们添加更多幂−2 分数来尝试使数字更接近目标，直到我们用完单精度浮点数中用于精度的 24 位。

> **Note**
> 
> 如果你想了解计算机如何存储浮点数，请研究定义它的 IEEE 754 标准。

## Constexpr if Expressions

C++17 将 constexpr if 表达式引入了语言，这大大简化了模板的编写。我们可以将前面使用 SFINAE 的三个模板重写为一个更简单的模板：

```
#include <iostream>

#include <type_traits>

template <typename T>
void print(T val)
{
    if constexpr(sizeof(T)==1) {
        printf("%c",val);
    }
    else if constexpr(std::is_integral_v<T>) {
        printf("%d",val);
    }
    else if constexpr(std::is_floating_point_v<T>) {
        printf("%f",val);
    }
    printf("\n");
}

int main(int argc, char** argv)
{
    print('c');
    print(55);
    print(32.1F);
    print(77.3);
}
```

对于对 **print(55)** 的调用，编译器生成的函数调用如下：

```
template<>
void print<int>(int val)
{
    printf("%d",val);
    printf("\n");
}
```

if/else if 语句去哪了？**constexpr if** 表达式发生的情况是，编译器在上下文中确定条件的值并将其转换为布尔值（true/false）。如果评估的值为 true，则 if 条件和 else 子句将被丢弃，只留下 true 子句来生成代码。同样，如果为 false，则 false 子句将被保留以生成代码。换句话说，只有第一个求值为 true 的 constexpr if 条件，其子句的代码才会被生成，而其余的则被丢弃。

## Non-Type Template Arguments

到目前为止，我们只看到模板参数是类型。也可以将整数值作为模板参数传递。这使我们能够防止函数中的数组衰减。例如，考虑一个计算 **sum** 的模板函数：

```
template <class T>
T sum(T data[], int number)
{
    T total = 0;
    for(auto i =0U ; i<number ; i++)
    {
        total += data[i];
    }
    return total;
}
```

在这种情况下，我们需要在调用中传递数组的长度：

```
float data[5] = {1.1, 2.2, 3.3, 4.4, 5.5};

auto total = sum(data, 5);
```

