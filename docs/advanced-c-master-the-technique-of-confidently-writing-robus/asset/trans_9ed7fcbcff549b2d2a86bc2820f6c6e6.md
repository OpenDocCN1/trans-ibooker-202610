
# 指针与资源管理

当通过指针访问用户定义类型时，有一个第二个特殊运算符（`->`），也用于解引用成员变量和函数。在现代 C++ 中，这些指针被称为**原始指针**，其使用方式已发生显著变化。在 C 和 C++ 中使用指针一直对程序员构成挑战，其错误使用是许多问题的根源，最常见的是资源泄漏。资源泄漏是指程序获取了资源（内存、文件句柄或其他系统资源）供自己使用，但在使用完毕后未能释放。这些资源泄漏可能导致性能问题、程序故障，甚至系统崩溃。自 C++11 引入智能指针以来，在现代 C++ 中使用原始指针管理资源所有权已被弃用。智能指针（作为 STL 中的类实现）现在负责做好宿主系统中良好公民所需的内部管理工作。更多内容将在《第 3 章：Can 和 Should 的距离——对象、指针与继承》中介绍。

在前面的代码中，声明 **`pvalue`** 时，编译器仅分配内存来存储其将引用的内存地址。与其他变量一样，应始终确保指针在使用前已初始化，因为解引用未初始化的指针会导致未定义行为。为存储指针而分配的确切内存量取决于编译器目标系统及处理器支持的位数。但无论指向何种类型，所有指针的大小都相同。

指针也可作为参数传递给函数。这允许函数访问被指向的数据并可能修改它。请看下面的 `swap` 实现：

```
void swap(int* data1, int* data2)
{
    int temp{*data1};          // Initialize temp from value pointed to by data1
    *data1 = *data2;           // Copy data pointed to by data2 into location
                               // pointed to by data1
    *data2 = temp;             // Store the temporarily cached value from temp
                               // into the location pointed to by data2
}
```

这展示了如何声明指针作为函数参数，如何使用解引用运算符 **`*`** 从指针获取值，以及如何通过解引用运算符设置值。

以下示例使用 `new` 运算符从宿主系统分配内存，并使用 `delete` 运算符将其释放回宿主系统：

```
char* name = new char[20];   // Allocate 20 chars worth of memory and assign it
                             // to name.
  Do something with name
delete[] name;
```

在前面的代码中，第一行使用 `new` 运算符的数组分配形式创建了一个 20 字符的数组。它调用宿主系统为我们分配 `20 * sizeof(char)` 字节的内存。实际分配多少内存由宿主系统决定，但保证至少为 `20 * sizeof(char)` 字节。如果无法分配所需内存，则会发生以下两种情况之一：

*   抛出异常
*   返回 **`nullptr`**。这是 C++11 引入的特殊字面量。早期 C++ 使用 `0` 或 `NULL` 表示无效指针。C++11 将其设为强类型值。

在大多数系统上，第一种结果最常见，你需要处理该异常。第二种结果可能源于两种场景：调用 `new` 的 `nothrow` 变体，即 **`new(std::nothrow) int[250]`**；或在嵌入式系统上，异常处理的开销不够确定性。

最后，注意 `delete` 调用使用了 `delete` 运算符的数组形式，即带方括号 **`[]`**。务必确保 `new` 和 `delete` 运算符使用相同形式。当 `new` 用于用户定义类型（将在下一节讨论）时，它不仅分配内存：

```
MyClass* object = new MyClass;
```

在前面的代码中，`new` 调用分配足以存储 `MyClass` 的内存，若成功，则继续调用构造函数初始化数据：

```
MyClass* objects = new MyClass[12];
```

在前面的代码中，`new` 调用分配足以存储 12 个 `MyClass` 副本的内存，若成功，则继续调用 12 次构造函数以初始化每个对象的数据。

注意，前面代码片段中声明的 **`object`** 和 **`objects`** 具有**相同类型**。严格来说，`objects` 应为 `MyClass` 数组的指针，但它实际上是 `MyClass` 实例的指针。`objects` 指向 `MyClass` 数组中的第一个实例。

请看下面的代码片段：

```
void printMyClasses(MyClass* objects, size_t number)
{
    for( auto i{0U} ; i<number ; i++ ) {
        std::cout << objects[i] << "\n";
    }
}

void process()
{
    MyClass objects[12];
    // Do something with objects
    printMyClasses(objects, sizeof(objects)/sizeof(MyClass));
}
```

在 `process()` 函数中，**`objects`** 的类型是“12 个 `MyClass` 项的数组”，但当它传递给 **`printMyClasses()`** 时，会被（编译器）转换为“指向 `MyClass` 的指针”类型。这是设计使然（继承自 C），称为**数组退化**，可能导致新程序员出错。我们可以尝试按如下方式声明 **`printMyClasses()`**：

```
void printMyClasses(MyClass objects[12], size_t number)
```

这仍会遭遇数组退化，因为编译器将参数 `objects` 改为 `MyClass*`；在这种情况下它不保留维度信息。数组退化就是我们需要将 `number` 传递给 **`printMyClasses()`** 函数的原因：以便知道数组中有多少项。C++ 提供两种机制处理数组退化：

*   使用迭代器将范围传递给方法。STL 容器（见《第 2B 章：不允许鸭子——模板与推导》中的“C++ 预打包模板”节）提供 **`begin()`** 和 **`end()`** 方法，以便获取迭代器，允许算法遍历数组或其部分。

    #### Note

    对于 C++20，ISO 标准委员会正在考虑引入称为 Ranges 的概念，这将允许在一个对象中同时捕获 `begin` 和 `end` 迭代器。

*   使用模板（见《第 2B 章：不允许鸭子——模板与推导》中的“非类型模板参数”节）。

# 练习 3：声明和使用指针

在本练习中，我们将实现接受指针和数组作为参数的函数，并在考虑数组退化的情况下比较它们的行为。开始吧：

1.  在 Eclipse 中打开 **`Lesson2A`** 项目，然后在 Project Explorer 中展开 **`Lesson2A`**，再展开 **`Exercise03`**，双击 **`Exercise3.cpp`** 以在编辑器中打开该练习文件。

2.  点击 **Launch Configuration** 下拉菜单，选择 **New Launch Configuration…**。配置 **`Exercise3`** 以名为 **`Exercise3`** 的方式运行。完成后，它将成为当前选中的 Launch Configuration。

3.  点击 **Run** 按钮。练习 3 将运行并产生以下输出：

    ![Figure 2A.20: Exercise 3 output](image/C14583_02A_20.jpg)

    ###### Figure 2A.20: Exercise 3 output

4.  在编辑器中插入一个空行，然后点击 **Run** 按钮。（通过更改文件，将强制构建系统重新编译 **`Exercise3.cpp`**。）

5.  如果我们现在查看 **CDT Global Build Console**，将看到编译器发出的两个警告：

    ![Figure 2A.21: Exercise 3 compiler warnings](image/C14583_02A_21.jpg)

    ###### Figure 2A.21: Exercise 3 compiler warnings
