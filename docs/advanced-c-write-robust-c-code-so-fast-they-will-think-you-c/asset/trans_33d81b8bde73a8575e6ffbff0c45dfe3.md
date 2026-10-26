

通过检查代码并将其与输出进行比较，我们会发现 **rvalue** 变量允许我们操作（从中读取和写入）存储在 **value** 变量中的数据。我们有一个对 **value** 变量的引用，**rvalue**。我们还可以看到，**a** 和 **b** 变量中存储的值已通过 **swap()** 函数交换。

- 5. 在编辑器中，修改 swap 的函数定义：

```
void swap(const int& lhs, const int& rhs)
```

- 6. 单击 **Run** 按钮。当出现 Errors in Workspace 对话框时，单击 **Cancel**。编译器报告的第一个错误如下所示：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_146_0.png)

图 2A.39：赋值时只读错误

通过将参数从 **int& lhs** 改为 **const int& lhs**，我们告诉编译器这些参数不应在此函数内部被更改。而由于我们在函数中对 lhs 进行了赋值，编译器会生成关于 lhs 为只读的错误并终止。

## 实现标准运算符

要像内置类一样使用分数，我们需要让它们能够与标准数学运算符（+、-、*、/）及其赋值对应运算符（+=、-=、*=、/=）一起工作。如果你不熟悉赋值运算符，那么请考虑以下两个表达式——它们产生相同的输出：

```
a = a + b;
a += b;
```

为 Fraction 声明这两个运算符的语法如下：

```
// 成员函数声明
Fraction& operator+=(const Fraction& rhs);
Fraction operator+(const Fraction& rhs) const;
```

```
// operator+ 的普通函数声明
Fraction operator+(const Fraction& lhs, const Fraction& rhs);
```

由于 operator+= 方法会修改左值变量的内容（将 a 加到 b 上然后再存回 a），因此建议将其实现为成员函数。在这种情况下，由于我们没有创建新值，我们只需返回对现有 lhs 的引用即可。

另一方面，operator+ 方法不应修改 lhs 也不应修改 rhs，而应返回一个新对象。实现者可以自由地将其实现为成员函数或自由函数。上述代码中展示了这两种方式，但只能存在其中一种。有趣的是要注意成员函数实现末尾的 const 关键字。这告诉编译器在调用此成员函数时，它不会修改对象的内部状态。虽然两种方法都有效，但如果可能，operator+ 应该实现为普通函数，在类之外。

对于其他运算符 -（减法）、*（乘法）和 /（除法）也可以使用相同的方法。上述方法实现了标准数学运算符的语义，并使我们的类型像内置类型一样工作。

## 实现输出流运算符（<<）

C++ 将输入/输出（I/O）抽象为标准库中的流类层次结构（我们将在第 2B 章 No Ducks Allowed - Templates and Deduction 中讨论）。在练习 5，Declaring and Initializing Fractions 中，我们看到可以将分数插入输出流如下：

```
std::cout << "fraction = " << fraction.getNumerator() << "/" << fraction.getDenominator() << "\n";
```

到目前为止，对于我们的 Fraction 类，我们是通过使用 getNumerator() 和 getDenominator() 方法从外部访问数据值来写出分子和分母值，但有更好的方法。作为使我们的类成为 C++ 中的一等公民的一部分，在有意义的地方，我们应该重载 I/O 运算符。在本章中，我们只会查看输出运算符 <<，也称为插入运算符。这样，我们就可以用更简洁的版本替换之前的代码：

```
std::cout << "fraction = " << fraction << "\n";
```

我们可以将运算符重载为友元函数或普通函数（如果类提供了我们需要插入的数据的 getter 函数）。为我们的目的，我们将其定义为普通函数：

```
inline std::ostream& operator<< (std::ostream &out, const Fraction &rhs)
{
    out << rhs.getNumerator() << " / " << rhs.getDenominator();
    return out;
}
```

## 组织我们的代码

在深入到实现运算符并将我们的 Fraction 变成 C++ 世界中的完整类型的练习之前，我们需要简要讨论一下我们将类的各个部分——声明和定义——放在哪里。声明是我们类的蓝图，指示它需要什么数据存储以及它将实现的方法。定义是每个方法的实际实现细节。

在 Java 和 C# 等语言中，声明和定义是同一个东西，它们必须存在于一个文件中（Java）或跨多个文件（C# 局部类）。在 C++ 中，根据类的情况以及你希望向其他类公开多少内容，声明 必须 出现在头文件中（该头文件可以 #include 到其他文件中使用），而定义可以出现在三个地方之一——内联在定义中、内联在与定义相同的源文件中，或在单独的实现文件中。

头文件通常以 .hpp 扩展名命名，而实现文件通常是 *.cpp 或 *.cxx。实现文件也称为翻译单元。通过将函数定义为 inline，我们允许编译器以一种方式优化代码，使得该函数可能甚至不存在于最终程序中——它已将我们放入函数中的步骤替换为我们从中调用函数的位置。

## 练习 7：为 Fraction 类添加运算符

在本练习中，我们的目标是使用单元测试来实现 Fraction 类中的运算符来开发功能。这使我们的 Fraction 类成为真正的类型。让我们开始吧：

- 1. 在 Eclipse 中打开 **Lesson2A** 项目，然后在 **Project Explorer** 中展开 **Lesson2A**，然后展开 **Exercise07**，双击 **Exercise7.cpp** 以在编辑器中打开此练习的文件。
- 2. 单击 **Launch Configuration** 下拉菜单并选择 **New Launch Configuration....** 将 Exercise7 配置为以名称 Exercise7 运行。
- 3. 完成后，它将成为当前选定的 Launch Configuration。
- 4. 我们还需要配置一个单元测试。在 Eclipse 中，单击菜单项 **Run | Run Configurations...**，在左侧右键单击 **C/C++ Unit**，并选择 **New Configuration**。
- 5. 将名称从 **Lesson2A Debug** 改为 **Exercise7 Tests**。
- 6. 在 **C/C++ Application** 下，选择 **Search Project** 选项并在新对话框中选择 **tests**。
- 7. 接下来，转到 **C/C++ Testing** 选项卡并在下拉菜单中选择 **Google Test Runner**。单击对话框底部的 **Apply**，然后单击测试的 **Run** 选项，我们必须首次运行它：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_149_0.png)

图 2A.40：失败的测试 – 乘法

- 8. 在编辑器中打开 **Fraction.cpp** 文件并找到 **operator*=** 函数。用以下代码更新它：

```
Fraction& Fraction::operator*=(const Fraction& rhs)
{
    Fraction tmp(m_numerator*rhs.m_numerator, m_denominator*rhs.m_denominator);
    *this = tmp;
    return *this;
}
```

- 9. 单击 **Run** 按钮重新运行测试。这次，所有测试都通过了：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_150_0.png)

图 2A.41：通过的测试

- 10. 在你的 IDE 中，打开 **tests/FractionTests.cpp** 文件并找到那两个失败的测试。其中一个测试了 **operator*=()**，另一个测试了 **operator*()**。修复 **operator*=()** 如何修复了 **operator*()**？如果你在编辑器中打开 Fraction.hpp，你会发现 **operator*()** 函数是通过调用 **operator*=()** 为你实现的，即它被标记为 inline，并且是普通函数而不是成员函数。通常，重载这些运算符时应采用这种方法——修改调用它的对象的那个是成员函数，而必须生成新值的那个是调用成员函数的普通函数。

- 11. 在编辑器中打开 **Fraction.hpp** 并更改文件顶部附近的行，使其如下所示：

```
#define EXERCISE7_STEP 11
```
