
在本练习中，`typedef` 和 `using` 别名之间似乎差别不大。无论哪种情况，使用一个命名良好的别名都能使代码更易于阅读和理解。当涉及更复杂的别名时，`using` 提供了一种更简便的编写别名的方法。`using` 于 C++11 引入，现已成为定义别名的首选方法。它相较于 `typedef` 还有其他优势，例如能够在模板内部使用。

## 模板——不仅仅是泛型编程

模板所能提供的远不止泛型编程（一种带有类型的切割机）。在泛型编程的情况下，模板作为一个无法更改的蓝图运作，并为指定的类型提供模板的编译版本。

模板可以编写为根据涉及的类型提供函数或算法的特化。这被称为 `模板特化`，并不符合我们之前使用的泛型编程的定义。只有当它使某些类型在给定上下文中表现出我们预期的行为时，才能称之为泛型编程。当用于所有类型的算法被修改时，就不能称之为泛型编程。

请检查以下特化代码示例：

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

它定义了一个模板，该模板根据使用 `std::enable_if_t<>` 和 `sizeof()` 的模板特化，调用带有不同格式字符串的 `printf()`。运行时，将生成以下输出：

![图 2B.14：错误的打印模板程序输出](image/C14583_02B_14.jpg)

图 2B.14：错误的打印模板程序输出

### 替换失败不是错误——SFINAE

为 `32.1F` 打印的值（`-1073741824`）与该数字毫无相似之处。如果我们检查编译器为以下程序生成的代码，会发现它生成的代码就如同我们编写了以下内容（甚至更多）：

```cpp
template<typename int, int=0>
void print<int,0>(int val)
{
    printf("%d\n",val);
}

template<typename float, int=0>
void print<float,0>(float val)
{
    printf("%d\n", val);
}
```

为什么会生成这段代码？前面的模板使用了 C++ 编译器的一项称为 `SFINAE`（替换失败不是错误）的特性。基本上，在模板的替换阶段，基于类型，如果编译器无法形成有效代码，它就会丢弃该定义并继续，而不是产生错误。让我们尝试修复前面的代码，使 `print` 得到正确的结果。为此，我们将引入 `std::enable_if_t<>` 的用法，并访问所谓的 `类型特征` 来帮助我们。首先，我们将用以下代码替换最后一个模板：

```cpp
#include <type_traits>

template <typename T, std::enable_if_t<std::is_floating_point_v<T>, int> = 0>
void print(T val)
{
    printf("%f\n", val);
}
```

这需要一些解释。首先，我们考虑 `std::enable_if_t` 的定义，它实际上是一个类型别名：

```cpp
template<bool B, class T = void>
struct enable_if {};

template<class T>
struct enable_if<true, T> { typedef T type; };

template< bool B, class T = void >
using enable_if_t = typename enable_if<B,T>::type;
```

第一个 `enable_if` 模板将导致一个空结构体（或类）的定义。第二个 `enable_if` 模板是针对第一个模板参数为 `true` 的特化，将导致一个包含 `typedef` 定义的类。`enable_if_t` 的定义是一个辅助模板，它消除了我们在使用时在模板末尾输入 `::type` 的需要。那么，这是如何工作的？请考虑以下代码：

```cpp
template <typename T, std::enable_if_t<condition, int> = 0>
void print(T val) { … }
```

如果在编译时计算的条件结果为 `true`，则 `enable_if_t` 模板将产生如下所示的模板：

```cpp
template <typename T, int = 0>
void print(T val) { … }
```

这是有效的语法，该函数作为候选函数被添加到符号表中。如果在编译时计算的条件结果为 `false`，则 `enable_if_t` 模板将产生如下所示的模板：

```cpp
template <typename T, = 0>
void print(T val) { … }
```

这是格式错误的代码，现在被丢弃了——SFINAE 在起作用。

`std::is_floating_point_v<T>` 是另一个辅助类，它访问 `std::is_floating_point<T>` 模板的 `::value` 成员。它的名字lo说明了一切——如果 `T` 是浮点类型（`float`、`double`、`long double`），它将为 `true`；否则为 `false`。如果我们做这个更改，编译器（GCC）将生成以下错误：

![图 2B.15：修改后的打印模板程序的编译器错误](image/C14583_02B_15.jpg)

图 2B.15：修改后的打印模板程序的编译器错误

现在的问题是，当类型为 `float` 时，有两个模板都能满足条件：

```cpp
template <typename T, std::enable_if_t<sizeof(T) == sizeof(int), int> = 0>
void print(T val)
{
    printf("%d\n", val);
}

template <typename T, std::enable_if_t<std::is_floating_point_v<T>, int> = 0>
void print(T val)
{
    printf("%f\n", val);
}
```

结果发现（通常）`sizeof(float) == sizeof(int)`，所以我们需要做另一个更改。我们将用另一个类型特征——`std::is_integral_v<>` 来替换第一个条件：

```cpp
template <typename T, std::enable_if_t<std::is_integral_v<T>, int> = 0>
void print(T val)
{
    printf("%d\n", val);
}
```

如果我们做这个更改，编译器（GCC）将生成以下错误：

![图 2B.16：修改后的打印模板程序的第二个编译器错误](image/C14583_02B_16.jpg)

图 2B.16：修改后的打印模板程序的第二个编译器错误

我们修复了浮点数的歧义，但这里的问题是 `std::is_integral_v<char>` 返回 `true`，对于 `char` 类型，模板又生成了两个具有相同原型的函数。结果发现，传递给 `std::enable_if_t<>` 的条件遵循标准的 C++ 逻辑表达式。因此，为了修复这个问题，我们将添加一个额外的条件来排除 `char`：

```cpp
template <typename T, std::enable_if_t<std::is_integral_v<T> && sizeof(T) != 1, int> = 0>
void print(T val)
{
    printf("%d\n", val);
}
```

如果我们现在编译程序，它将完成编译并链接程序。如果我们运行它，现在将产生以下（预期的）输出：

![图 2B.17：修正后的打印模板程序输出](image/C14583_02B_17.jpg)

图 2B.17：修正后的打印模板程序输出

### 浮点数表示
