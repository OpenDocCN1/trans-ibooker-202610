

# C++ 中的函数

函数允许我们将解决方案分解为更易于管理和理解的语句序列。当我们编写这些打包好的语句时，可以在合适的地方重复使用它们。如果需要根据上下文以不同方式操作，则传入一个参数。如果它返回结果，则函数需要一个返回类型。

由于 C++ 是一种强类型语言，我们需要指定与所实现函数相关的类型——函数返回的值的类型（包括无返回值）以及传递给它的参数的类型（如果有）。

## Hello World 示例

以下是一个典型的 hello world 程序：

```cpp
#include <iostream>

void hello_world()
{
    std::cout << "Hello world\n";
}

int main(int argc, char** argv)
{
    std::cout << "Starting program\n";
    hello_world();
    std::cout << "Exiting program\n";
    return 0;
}
```

前面的示例中声明了两个函数——`hello_world()` 和 `main()`。`main()` 函数是有史以来每个 C++ 程序的**入口点**，它返回一个传递给宿主系统的 `int` 值。这被称为**退出代码**。

从返回类型声明一直到左大括号（`{`）之间的所有内容称为**函数原型**。它定义了三件事：返回类型、函数名称以及参数的数量和类型。

对于第一个函数，返回类型是 `void`——即不返回任何值；它的名字是 `hello_world`，且不接受参数：

![Figure 2A.15: Declaration of a function that takes no arguments and returns nothing](image/C14583_02A_15.jpg)

**图 2A.15：不接受参数且无返回值的函数声明**

第二个函数返回一个 `int` 值，名为 `main`，并接受两个参数。这些参数分别是 `argc` 和 `argv`，类型分别为 `int` 和 `char**`（指向 `char` 的指针的指针）：

![Figure 2A.16: Declaration of a function that takes two arguments and returns an int](image/C14583_02A_16.jpg)

**图 2A.16：接受两个参数并返回 int 的函数声明**

函数原型之后的所有内容称为**函数体**。函数体包含变量声明和要执行的语句。

## 前向声明

函数必须在使用前声明——即编译器需要知道它的参数和返回类型。如果函数定义在调用它之后的同一个文件中，则可以通过在使用前提供函数的**前向声明**来解决此问题。

前向声明是通过将以分号结尾的函数原型放在调用它之前的文件中来完成的。对于 `hello_world()`，操作如下：

```cpp
void hello_world();
```

对于 `main` 函数，操作如下：

```cpp
int main(int, char**);
```

函数原型不需要参数名称，只需要类型。但是，为了帮助函数的使用者，保留它们是个好主意。

在 C++ 中，函数的定义可以在一个文件中，而需要从另一个文件中调用它。那么，第二个文件如何知道它想要调用的函数的原型呢？这是通过将前向声明放入一个称为**头文件**的单独文件中，并将其包含在第二个文件中来实现的。

## 练习 2：声明函数

在本练习中，我们将测试编译器在遇到函数调用时需要知道什么，并实现前向声明以解决未知函数的问题。

1. 在 Eclipse 中打开 **Lesson2A** 项目，然后在 **Project Explorer** 中展开 **Lesson2A**，再展开 **Exercise02**，双击 **Exercise2.cpp** 将该练习的文件在编辑器中打开。
2. 点击 **Launch Configuration** 下拉菜单并选择 **New Launch Configuration…**。
3. 配置 **Exercise2** 以名为 **Exercise2** 的方式运行。完成后，它将成为当前选定的 Launch Configuration。
4. 点击 **Run** 按钮。练习 2 将运行并产生以下输出：

![Figure 2A.17: Output from the exercise2 program](image/C14583_02A_17.jpg)

**图 2A.17：exercise2 程序的输出**

5. 进入编辑器并通过移动 `gcd` 函数使其位于 `main` 之后来更改代码。它应该如下所示：

```cpp
int main(int argc, char** argv)
{
    std::cout << "\n\n------ Exercise 2 ------\n";
    std::cout << "The greatest common divisor of 44 and 121 is " << gcd(44, 121) << "\n";
    std::cout << "Complete.\n";
    return 0;
}

int gcd(int x, int y)
{
    while (y != 0)
    {
        auto c { x % y };
        x = y;
        y = c;
    }
    return x;
}
```

6. 再次点击 **Run** 按钮。当出现 **Errors in Workspace** 对话框时，点击 **Cancel**。在 **CDT Build Console [Lesson2A]** 中，我们将看到失败的原因：

![Figure 2A.18: Build failure due to undefined function](image/C14583_02A_18.jpg)

**图 2A.18：因未定义函数导致的构建失败**

这一次，编译器不知道如何处理对 `gcd()` 函数的调用。在需要调用它时，它对该函数一无所知，即使它定义在同一个文件中但位于调用之后。

7. 在编辑器中，在 `main` 函数定义之前放置前向声明。并在末尾添加分号（`;`）：

```cpp
int gcd(int x, int y);
```

8. 再次点击 **Run** 按钮。这一次，程序编译通过，原始输出得以恢复。

在本练习中，我们学习了如何前向声明函数，以及如何排查因函数在使用前未声明而导致的编译器错误。

在早期的 C 编译器版本中，这是可以接受的。程序会假设该函数存在并返回 `int`。函数的参数可以从调用中推断出来。然而，在现代 C++ 中情况并非如此，因为您必须在使用函数、类、变量等之前声明它们。

## 指针类型

由于其源于 C 语言——编写最优高效的系统并直接访问硬件——C++ 允许您将变量声明为**指针类型**。其格式如下：

```cpp
type-specifier* pvar = &var;
```

这与之前相同，除了两点：

* 使用特殊的声明符星号（`*`）来指示名为 `pvar` 的变量指向内存中的一个位置或地址。
* 使用特殊的运算符 Ampersand（`&`）进行初始化，在此上下文中它告诉编译器返回 `var` 变量的地址。

由于 C 是一种高级语言但具有低级访问能力，指针允许用户直接访问内存，这在我们希望向硬件提供输入/输出并从而控制硬件时很有帮助。指针的另一个用途是允许函数访问公共数据项，并在调用函数时消除复制大量数据的需要，因为默认情况下是按值传递。

要访问指针所指向的值，使用特殊的运算符星号（`*`）来**解引用**该位置：

```cpp
int five = 5;           // 声明 five 并初始化它
int *pvalue = &five;    // 声明 pvalue 为指向 int 的指针并使其
                        // 指向 five 的位置
*pvalue = 6;            // 将 6 赋值给 five 所在的位置。
```

下图显示了编译器如何分配内存。`pvalue` 需要内存来存储指针，而 `five` 需要内存来存储整数值 5：

![Figure 2A.19: Memory layout for pointer variables](image/C14583_02A_19.jpg)

**图 2A.19：指针变量的内存布局**

