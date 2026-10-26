

但如果我们能直接调用以下代码不是更好吗？

```cpp
auto total = sum(data);
```

我们可以通过修改模板来实现，就像下面的代码一样：

```cpp
template <class T, std::size_t size>
T sum(T (&data)[size])
{
    T total = 0;
    for(auto i=0U ; i< size; i++)
    {
        total += data[i];
    }
    return total;
}
```

在这里，我们将 data 改为对特定大小数组的引用——该大小作为模板参数传递——因此编译器可以推断出来。我们不再需要在函数调用中提供第二个参数。这个简单的例子展示了如何直接传递和使用非类型参数。我们将在模板类型推导部分进一步探讨这一点。

## 练习 3：实现 Stringify – 特化与 constexpr

在本练习中，我们将通过利用 constexpr 实现一个 stringify 模板，从而产生更易读且更简单的代码版本。按照以下步骤实现本练习：

> **注意**
> 
> stringify 特化模板可以在 https://isocpp.org/wiki/faq/templates#template-specialization-example 找到。

- 1. 在 Eclipse 中打开 **Lesson2B** 项目，然后在 **Project Explorer** 中展开 **Lesson2B**，再展开 **Exercise03**，双击 **Exercise3.cpp** 在编辑器中打开该练习文件。

- 2. 点击 **Launch Configuration** 下拉菜单并选择 **New Launch Configuration...** 将 **L2BExercise3** 配置为以名称 **Exercise3** 运行。

在本练习中，我们了解到使用 C++17 中的新 `constexpr if` 构造后，我们的模板可以变得多么简单和紧凑。

## 函数重载回顾

当我们首次讨论函数重载时，我们只考虑了函数名来自我们手工编写的函数列表的场景。现在，我们需要对此进行更新。我们也可以编写具有相同名称的模板化函数。就像我们之前所做的那样，当编译器遇到行 `print(55)` 时，它需要确定调用之前定义的哪个函数。因此，它执行以下过程（大幅简化）：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_196_0.png)

## 模板类型推导

当我们首次介绍模板时，我们涉及到了模板类型推导。现在，我们将进一步探讨这一点。我们将从函数模板的通用声明开始：

```cpp
template<typename T>

void function(ParamType parameter);
```

对此的调用可能如下所示：

```cpp
function(expression);              // 从 expression 推导 T 和 ParamType
```

当编译器到达这一行时，它必须现在推导出与模板相关的两种类型——T 和 **ParamType**。这些通常不同，因为 ParamType 中的 T 附带了限定符和其他属性（例如指针、引用、const 等）。类型相关，但推导过程取决于所使用的 **expression** 的形式而有所不同。

### 显示推导出的类型

在查看不同形式之前，如果我们能让编译器告诉我们它推导出的类型可能会很有用。我们在这里有几个选择，包括 IDE 编辑器显示类型、编译器生成错误，以及运行时支持（由于 C++ 标准，这不一定有效）。我们将使用编译器错误来帮助我们探索一些类型推导。

我们可以通过声明一个没有定义的模板来实现类型显示器。任何尝试实例化该模板的尝试都会导致编译器生成错误消息，因为没有定义，同时还会提供它试图实例化的类型信息：

```cpp
template<typename T>
struct TypeDisplay;
```

让我们尝试编译以下程序：

```cpp
template<typename T>
class TypeDisplay;
```

```cpp
int main()
{
    signed int x = 1;

    unsigned int y = 2;

    TypeDisplay<decltype(x)> x_type;

    TypeDisplay<decltype(y)> y_type;

    TypeDisplay<decltype(x+y)> x_y_type;

    return 0;
}
```

编译器输出以下错误：

```
$ g++ types.cpp -std=c++17 -o types.exe
types.cpp: In function ‘int main()’:
types.cpp:10:30: error: aggregate ‘TypeDisplay<int> x_type’ has incomplete type and cannot be defined
  10 |     TypeDisplay<decltype(x)> x_type;
      |                          ^~~~~~
types.cpp:11:30: error: aggregate ‘TypeDisplay<unsigned int> y_type’ has incomplete type and cannot be defined
  11 |     TypeDisplay<decltype(y)> y_type;
      |                          ^~~~~~
types.cpp:12:32: error: aggregate ‘TypeDisplay<unsigned int> x_y_type’ has incomplete type and cannot be defined
  12 |     TypeDisplay<decltype(x+y)> x_y_type;
      |                                ^~~~~~~~
```

图 2B.23：显示推导类型的编译器错误

请注意，在每种情况下，被命名的聚合都包含正在推导的类型——对于 x，它是 int，对于 y，是 unsigned int，对于 x+y，是 unsigned int。此外，请注意 TypeDisplay 模板需要其参数的类型，因此使用 decltype() 函数让编译器为括号中的表达式提供类型。

也可以在运行时显示推导出的类型，要么使用内置的 typeid(T).name() 运算符，它返回一个 std::string，要么使用名为 type_index 的 boost 库。

> **注意**
> 
> 有关更多信息，请访问以下链接：https://www.boost.org/doc/libs/1_70_0/doc/html/boost_typeindex.html。

由于类型推导规则，内置运算符会给出类型的指示，但会丢失引用（&, 和 &&）以及任何 constness 信息（const 或 volatile）。如果在运行时需要，则考虑 boost::type_index，它将为所有编译器产生相同的输出。

### 模板类型推导 – 细节

让我们回到通用模板：

```cpp
template<typename T>

void function(ParamType parameter);
```

假设调用如下所示：

```cpp
function(expression);    // 从 expression 推导 T 和 ParamType
```

类型推导过程取决于所使用的 ParamType 的形式而有所不同：

- **ParamType 是值（T）**：按值传递函数调用
- **ParamType 是引用或指针（T& 或 T*）**：按引用传递函数调用
- **ParamType 是右值引用（T&&）**：按引用传递函数调用或其他

**情况 1：ParamType 是按值传递（T）**

```cpp
template<typename T>
void function(T parameter);
```

作为按值调用，这意味着参数将是传入内容的副本。因为这是对象的全新实例，所以对表达式应用以下规则：

- 如果表达式的类型是引用，则忽略引用部分。
- 如果在步骤 1 之后，剩余类型为 const 和/或 volatile，则同样忽略它们。

剩下的就是 T。让我们尝试编译以下文件代码：

```cpp
template<typename T>
class TypeDisplay;

template<typename T>
void function(T parameter)
{
    TypeDisplay<T> type;
}

void types()
{
    int x = 42;
    function(x);
}
```

编译器产生以下错误：

```
types.cpp: In instantiation of ‘void function(T) [with T = int]’:
types.cpp:13:15:   required from here
types.cpp:7:20: error: ‘TypeDisplay<int> type’ has incomplete type
    7 |     TypeDisplay<T> type;
```

![](img/6507d435e2edfa3c00c220d0ccfcaf94_200_0.png)

因此，推导出的类型为 **int**。同样，如果我们声明以下内容，我们会得到完全相同的错误：

```cpp
const int x = 42;

function(x);
```

如果我们声明此版本也会发生同样的情况：

```cpp
int x = 42;

const int& rx = x;

function(rx);
```

在这三种情况下，推导出的类型都是 **int**，符合之前所述的规则。

**情况 2：ParamType 是按引用传递（T&）**

作为按引用调用，这意味着参数将能够访问对象的原始存储位置。由于这个原因，生成的函数必须遵守我们之前忽略的 constness 和 volatileness。对类型推导应用以下规则：

- 如果表达式的类型是引用，则忽略引用部分。
- 将表达式类型的剩余部分与 ParamType 进行模式匹配以确定 T。

让我们尝试编译以下文件：

```cpp
template<typename T>
class TypeDisplay;


template<typename T>
void function(T& parameter)
{
    TypeDisplay<T> type;
```

