

# 带模板的函数重载决议

当我们首次讨论函数重载时，仅考虑了函数名称来自我们手动编写的函数列表的场景。现在，我们需要更新这一认知。我们也可以编写具有相同名称的模板函数。与之前一样，当编译器遇到 `print(55)` 这一行时，它需要确定调用之前定义的哪个函数。因此，它执行以下过程（极度简化）：

![Figure 2B.22: Function overload resolution with templates (simplified)](image/C14583_02B_22.jpg)

*图 2B.22：带模板的函数重载决议（简化版）*

## 模板类型推导

当我们首次介绍模板时，曾涉及模板类型推导。现在，我们将进一步探讨这一点。我们将从考虑函数模板的通用声明开始：

```cpp
template<typename T>
void function(ParamType parameter);
```

其调用可能如下所示：

```cpp
function(expression); // 从 expression 推导 T 和 ParamType
```

当编译器到达此行时，它现在必须推导出与模板相关的两个类型——`T` 和 `ParamType`。由于附加在 `ParamType` 中的 `T` 上的限定符和其他属性（例如指针、引用、const 等），这两个类型通常是不同的。这些类型虽有关联，但推导过程会根据所用 `expression` 的形式而有所不同。

## 显示推导出的类型

在深入探讨不同形式之前，如果能让编译器告诉我们它推导出的类型，那将很有用。我们有几种选择，包括 IDE 编辑器显示类型、编译器生成错误以及运行时支持（由于 C++ 标准的原因，这不一定有效）。我们将利用编译器错误来帮助我们探索一些类型推导。

我们可以通过声明一个没有定义的模板来实现类型显示器。任何尝试实例化该模板的行为都会导致编译器生成错误消息（因为没有定义），并附带它试图实例化的类型信息：

```cpp
template<typename T>
struct TypeDisplay;
```

让我们尝试编译以下程序：

```cpp
template<typename T>
class TypeDisplay;

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

编译器吐出以下错误：

![Figure 2B.23: Compiler errors showing deduced types](image/C14583_02B_23.jpg)

*图 2B.23：显示推导类型的编译器错误*

请注意，在每种情况下，被命名的聚合体都包含正在被推导的类型——对于 `x`，它是 `int`；对于 `y`，是 `unsigned int`；对于 `x+y`，是 `unsigned int`。另请注意，`TypeDisplay` 模板要求其参数为类型，因此使用 `decltype()` 函数让编译器提供括号内表达式的类型。

也可以在运行时显示推导出的类型，既可以使用内置的 `typeid(T).name()` 运算符（返回 `std::string`），也可以使用名为 `type_index` 的 boost 库。

### 注意

有关更多信息，请访问以下链接：[https://www.boost.org/doc/libs/1_70_0/doc/html/boost_typeindex.html](https://www.boost.org/doc/libs/1_70_0/doc/html/boost_typeindex.html)。

由于类型推导规则，内置运算符会给你类型的指示，但会丢失引用（`&` 和 `&&`）以及任何 const 信息（const 或 volatile）。如果在运行时需要，请考虑 `boost::type_index`，它将为所有编译器产生相同的输出。

## 模板类型推导——详情

让我们回到通用模板：

```cpp
template<typename T>
void function(ParamType parameter);
```

假设调用如下所示：

```cpp
function(expression); // 从 expression 推导 T 和 ParamType
```

类型推导的进行方式取决于所用 `ParamType` 的形式：

*   `ParamType` 是值（`T`）：传值函数调用
*   `ParamType` 是引用或指针（`T&` 或 `T*`）：传引用函数调用
*   `ParamType` 是右值引用（`T&&`）：传引用函数调用或其他情况

### 情况 1：ParamType 是传值 (T)

```cpp
template<typename T>
void function(T parameter);
```

作为传值调用，这意味着参数将是传入内容的副本。因为这是对象的一个新实例，所以对表达式应用以下规则：

*   如果表达式的类型是引用，则忽略引用部分。
*   如果在步骤 1 之后，剩余类型是 const 和/或 volatile，则也忽略那些。

剩下的就是 `T`。让我们尝试编译以下文件代码：

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

![Figure 2B.24: Compiler error showing a deduced type for the pass by type](image/C14583_02B_24.jpg)

*图 2B.24：显示传值类型推导类型的编译器错误*

因此，类型被推导为 `int`。同样，如果我们声明以下内容，会得到完全相同的错误：

```cpp
const int x = 42;
function(x);
```

如果声明此版本，也会发生同样的情况：

```cpp
int x = 42;
const int& rx = x;
function(rx);
```

在所有三种情况下，根据前面所述规则，推导出的类型均为 `int`。

### 情况 2：ParamType 是传引用 (T&)

作为传引用调用，这意味着参数将能够访问对象的原始存储位置。因此，生成的函数必须遵守我们之前忽略的 constness 和 volatileness。对类型推导应用以下规则：

*   如果表达式的类型是引用，则忽略引用部分。
*   将剩余的表达式类型与 `ParamType` 进行模式匹配以确定 `T`。

让我们尝试编译以下文件：

```cpp
template<typename T>
class TypeDisplay;

template<typename T>
void function(T& parameter)
{
    TypeDisplay<T> type;
}

void types()
{
    int x = 42;
    function(x);
}
```

编译器将生成以下错误：

![Figure 2B.25: Compiler error showing the deduced type for pass by reference](image/C14583_02B_25.jpg)

*图 2B.25：显示传引用推导类型的编译器错误*

由此我们可以看到，编译器从 `ParamType` 为 `int&` 推导出 `T` 为 `int`。将 `x` 改为 `const int` 也不足为奇，因为 `T` 从 `ParamType` 为 `const int&` 推导为 `const int`：

![Figure 2B.26: Compiler error showing the deduced type for pass by const reference](image/C14583_02B_26.jpg)

*图 2B.26：显示传 const 引用推导类型的编译器错误*

同样，像以前那样引入 `rx` 作为对 `const int` 的引用，也不足为奇，因为 `T` 从 `ParamType` 为 `const int&` 推导为 `const int`：

```cpp
void types()
{
    const int x = 42;
    const int& rx = x;
    function(rx);
}
```

![Figure 2B.27: Compiler error showing the deduced type when passing a const reference](image/C14583_02B_27.jpg)

*图 2B.27：传递 const 引用时显示推导类型的编译器错误*

如果我们更改声明以包含 const，则编译器在从模板生成函数时将遵守 constness：

```cpp
template<typename T>
void function(const T& parameter)
{
    TypeDisplay<T> type;
}
```

这一次，编译器报告如下：

*   `int x`：`T` 为 `int`（将遵守 constness），而参数类型为 `const int&`。
*   `const int x`：`T` 为 `int`（const 在模式中，剩下 int），而参数类型为 `const int&`。

