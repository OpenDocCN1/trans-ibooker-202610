
# C++ 对象初始化机制

我们有四种不同的机制来控制对象的初始状态。C++ 对于在特定情况下使用哪种初始化有许多规则。我们不会深入探讨 C++ 标准中的默认初始化、零初始化、值初始化、常量初始化等细节。只需知道，最好的方法是显式地初始化你的变量。

## 第一种机制：在构造函数体中赋值

**第一种**且最不推荐的初始化机制是在构造函数体中给成员变量赋值，如下所示：

```cpp
Fraction::Fraction()
{
    this->m_numerator = 0;
    this->m_denominator = 1;
}

Fraction::Fraction(int numerator, int denominator)
{
    m_numerator = numerator;
    m_denominator = denominator;
}
```

清楚地知道用什么值初始化变量。严格来说，这不是类的初始化——根据标准，当调用构造函数体时初始化就完成了。这对于维护很简单，特别是在这个类中。对于拥有多个构造函数和许多成员变量的大型类，这可能成为维护问题。如果你修改了一个构造函数，你需要修改所有构造函数。如果成员变量是引用类型（稍后讨论），则不能在构造函数体中完成。

默认构造函数使用 `this` 指针。每个成员函数（包括构造函数和析构函数）都带有一个隐式参数（尽管从未声明）——`this` 指针。`this` 指向当前对象实例。`->` 运算符是另一个解引用运算符，在此处是 `*(this).m_numerator` 的简写。使用 `this->` 是可选的，可以省略。其他语言（如 Python）要求声明并使用隐式指针/引用（Python 的约定称为 `self`）。

## 第二种机制：成员初始化列表

**第二种**机制是使用成员初始化列表，其使用有一个注意事项。对于 `Fraction` 类，我们有以下代码：

```cpp
Fraction::Fraction() : m_numerator(0), m_denominator(1)
{
}

Fraction::Fraction(int numerator, int denominator) :
    m_numerator(numerator), m_denominator(denominator)
{
}
```

冒号 `:` 之后、左大括号 `{` 之前的代码段（即 `m_numerator(0), m_denominator(1)` 和 `m_numerator(numerator), m_denominator(denominator)`）是成员初始化列表。我们可以在成员初始化列表中初始化引用类型。

### 成员初始化列表顺序

无论你在成员初始化列表中以何种顺序放置成员，编译器都将按照它们在类中声明的顺序初始化成员。

## 第三种机制：默认成员初始化（C++11）

**第三种**且**推荐**的初始化是 C++11 引入的默认成员初始化。我们在变量声明时使用赋值或大括号初始化器定义默认初始值：

```cpp
class Fraction
{
public:
    int m_numerator = 0;    // 赋值初始化器
    int m_denominator{1};   // 大括号初始化器
};
```

如果构造函数未定义成员变量的初始值，则使用此默认值初始化变量。这的优势在于确保所有构造函数产生相同的初始化，除非在构造函数定义中显式修改。

## 第四种机制：构造函数委托（C++11）

C++11 还引入了第四种初始化风格，称为构造函数委托。它是成员初始化列表的一种修改，不是列出成员变量及其初始值，而是调用另一个构造函数。以下示例是人为构造的，你不会这样编写类，但它展示了构造函数委托的语法：

```cpp
Fraction::Fraction(int numerator) : m_numerator(numerator), m_denominator(1)
{
}

Fraction::Fraction(int numerator, int denominator) : Fraction(numerator)
{
    auto factor = std::gcd(numerator, denominator);
    m_numerator /= factor;
    m_denominator = denominator / factor;
}
```

你从双参数构造函数中调用单参数构造函数。

# 练习 5：声明并初始化 Fraction

在本练习中，我们将使用可用的不同技术实现类成员初始化，包括构造函数委托。开始吧：

1. 在 Eclipse 中打开 **Lesson2A** 项目，然后在 **Project Explorer** 中展开 **Lesson2A**，再展开 **Exercise05**，双击 **Exercise5.cpp** 在编辑器中打开该练习文件。
2. 点击 **Launch Configuration** 下拉菜单并选择 **New Launch Configuration…**。配置 **Exercise5** 以名为 Exercise5 运行。
3. 完成后，它将成为当前选中的 Launch Configuration。
4. 点击 **Run** 按钮。**Exercise 5** 将运行并产生类似以下输出：

    ![](image/C14583_02A_35.jpg)

    **Figure 2A.35: Exercise 5 typical output**

    报告的分数值来自于未以任何方式初始化成员变量。如果再次运行，你很可能会得到不同的分数。

5. 多次点击 **Run** 按钮。你将看到分数发生变化。
6. 在编辑器中，将构造函数更改为如下所示：

    ```cpp
    Fraction() : m_numerator{0}, m_denominator{1}
    {
    }
    ```

7. 点击 **Run** 按钮并观察输出：

    ![](image/C14583_02A_36.jpg)

    **Figure 2A.36: Modified Exercise 5 output**

    这次，分数值由我们在成员初始化列表中指定的值定义。

8. 在编辑器中，添加以下两个**构造函数**：

    ```cpp
    Fraction(int numerator) : m_numerator(numerator), m_denominator(1)
    {
    }

    Fraction(int numerator, int denominator) : Fraction(numerator)
    {
        auto factor = std::gcd(numerator, denominator);
        m_numerator /= factor;
        m_denominator = denominator / factor;
    }
    ```

9. 在 `main` 函数中，更改 `fraction` 的声明以包含初始化：

    ```cpp
    Fraction fraction{3,2};
    ```

10. 点击 **Run** 按钮并观察输出：

    ![](image/C14583_02A_37.jpg)

    **Figure 2A.37: Example of constructor delegation**

在本练习中，我们使用成员初始化列表和构造函数委托实现了成员变量初始化。*我们将在练习 7《为 Fraction 类添加运算符》中回到分数。*

# 值与引用和 Const

到目前为止，我们只处理了值类型，即变量持有对象的值。指针持有一个值（即对象的地址），这是我们感兴趣的（或 `nullptr`）。但这可能导致效率低下和资源管理问题。我们将在这里解决效率问题，但在*第 3 章*《Can 和 Should 之间的距离——对象、指针和继承》中处理资源管理问题。

考虑以下问题。我们有一个 `double` 类型的 10×10 矩阵，希望为其编写一个求逆函数。类声明如下：

```cpp
class Matrix10x10
{
private:
    double m_data[10][10];
};
```

如果我们获取 `sizeof(Matrix10x10)`，将得到 `sizeof(double)` × 10 × 10 = 800 字节。现在，如果我们为其实现一个求逆矩阵函数，其签名可能如下所示：

```cpp
Matrix10x10 invert(Matrix10x10 lhs);

Matrix10x10 mat;
// set up mat
Matrix10x10 inv = invert(mat);
```
