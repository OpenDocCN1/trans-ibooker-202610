## 列表、栈和队列

### 学习目标

学完本章后，你将能够：

- 描述在任何应用程序中使用正确的数据结构的重要性
- 根据问题实现各种内置数据结构，使应用程序开发更简单
- 当 C++ 提供的数据结构不足以满足特定用例时，针对给定情况实现自定义线性数据结构
- 分析不同类型线性数据结构有助于解决的实际问题，并判断哪种结构最适合给定的用例

本章将描述在任何应用程序中使用正确数据结构的重要性。我们将学习如何在 C++ 中使用一些最常见的数据结构，以及如何使用这些结构的内置和自定义容器。

### 简介

在设计任何应用程序时，数据的管理是需要牢记的最重要的考虑因素之一。任何应用程序的目的都是获取一些数据作为输入，对其进行处理或操作，然后提供合适的数据作为输出。例如，让我们考虑一个医院管理系统。在这里，我们可以拥有不同医生、患者和档案记录等数据。医院管理系统应允许我们执行各种操作，例如接收患者，以及更新不同专科医生的入职和离职信息。虽然面向用户的界面会以与医院管理员相关的格式呈现信息，但在内部，系统会管理不同的记录和项目列表。

程序员可以拥有多种结构来在内存中保存任何数据。选择正确的数据保存结构（也称为数据结构）对于确保可靠性、性能以及在应用程序中实现所需功能至关重要。除了正确的数据结构外，选择正确的算法来访问和操作数据对于应用程序的最佳行为也是必要的。本书将使你具备为应用程序设计实现正确数据结构和算法的能力，从而帮助你开发高度优化且可扩展的应用程序。

本章介绍 C++ 中提供的基本且常用的线性数据结构。我们将研究它们各自的设计、优点和缺点。我们还将通过练习来实现这些结构。理解这些数据结构将帮助你以更高性能、更标准化、更可读、更可维护的方式管理任何应用程序中的数据。

线性数据结构大致可分为连续结构和链接结构。让我们了解两者之间的区别。

### 连续数据结构与链接数据结构

在任何应用程序中处理数据之前，我们必须决定如何存储数据。这个问题的答案取决于我们想要对数据执行何种操作以及操作的频率。我们应该选择在延迟、内存或其他任何参数方面提供最佳性能的实现，同时不影响应用程序的正确性。

确定数据结构类型的一个有用指标是算法复杂度，也称为时间复杂度。时间复杂度表示相对于数据大小执行某个操作所需的时间量。因此，时间复杂度显示了当数据集大小改变时时间将如何变化。任何数据类型上不同操作的时间复杂度都取决于数据在其中的存储方式。

静态数组是分配在栈上的，因此当流程退出函数时，它会被释放。另一方面，动态数组分配在堆上，并一直保留在那里，直到手动释放内存。

由于所有元素都彼此相邻，当访问其中一个元素时，它附近的几个元素也会被加载到缓存中。因此，如果你想访问这些元素，这是一个非常快速的操作，因为数据已经存在于缓存中。这个特性也称为缓存局部性。虽然它不会影响任何操作的渐近时间复杂度，但在遍历数组时，对于连续数据来说，它在实践中可以带来惊人的优势。由于遍历需要依次遍历所有元素，在获取第一个元素后，接下来的几个元素可以直接从缓存中获取。因此，可以说数组具有良好的缓存局部性。

### 链接数据结构

链接数据结构将数据保存在多个内存块中，这些内存块也称为节点，它们可能位于内存中的不同位置。下图展示了数据在链接数据结构中的存储方式：

![](img/b37c7e7c5fa2518dbf1afd10524f8d11_23_0.png)

**图 1.2：链接数据结构**

在链表的基本结构中，每个节点包含要存储在该节点中的数据以及指向下一个节点的指针。最后一个节点包含一个 NULL 指针，用于指示链表的末尾。要访问任何元素，我们必须从链表的开头（即头部）开始，然后沿着 next 指针前进，直到到达目标元素。因此，要访问位于索引 i 处的元素，我们需要遍历链表并迭代 i 次。因此，我们可以说访问元素的复杂度为 O(n)；也就是说，时间随节点数量成比例变化。

下一节总结了连续数据结构和链接数据结构的比较。

### 比较

下表简要总结了链接数据结构和连续数据结构之间的一般重要差异：

| 连续数据结构 | 链接数据结构 |
|---|---|
| 所有数据在内存中彼此相邻存储。 | 数据存储在节点中，节点可能分散在内存各处。 |
| 由于数据连续，访问随机元素是即时的。 | 访问随机元素是线性时间，因此较慢。 |
| 由于数据连续存储，遍历因缓存局部性而更快。 | 遍历稍慢，因为没有缓存局部性。 |
| 除了存储元素本身所需的内存外，没有额外内存开销。 | 需要额外内存来在每个节点中存储指针。 |

图 1.4：比较连续数据结构和链接数据结构的表格

下表汇总了数组和链表在各种参数下的性能：

| 参数 | 数组 | 链表 |
|---|---:|---:|
| 随机访问 | O(1) | O(n) |
| 末尾插入 | O(1) | O(1) |
| 中间插入 | O(n) | O(1) |
| 缓存局部性 | 是 | 否 |

图 1.5：展示数组和链表某些操作时间复杂度的表格

对于任何应用程序，我们可以根据需求和各种操作的频率来选择其中一种数据结构或两者的组合。

数组和链表非常常见，并且在任何应用程序中广泛用于存储数据。因此，这些数据结构的实现必须尽可能无 bug 且高效。为了避免重复造轮子，C++ 提供了各种结构，例如 std::array、std::vector 和 std::list。我们将在接下来的章节中更详细地介绍其中一些。





## C 风格数组的局限性

尽管 C 风格数组能够胜任工作，但它们并不常用。有许多局限性表明需要更好的解决方案。其中一些主要局限性如下：

- 内存的分配和释放必须手动处理。未能释放可能导致内存泄漏，即内存地址变得不可访问。
- `operator[]` 函数不检查参数是否大于数组大小。如果使用不当，这可能导致段错误或内存损坏。
- 嵌套数组的语法变得非常复杂，导致代码难以阅读。
- 深拷贝不能作为默认函数使用，必须手动实现。

为了避免这些问题，C++ 在 C 风格数组之上提供了一个非常轻量的包装器，称为 `std::array`。

## std::array

`std::array` 自动管理内存的分配和释放。`std::array` 是一个模板类，它接受两个参数——元素类型和数组大小。

在下面的示例中，我们将声明一个大小为 10 的 `int` 类型 `std::array`，设置其中某个元素的值，然后打印该值以确认其正常工作：

```
std::array<int, 10> arr;      // array of int of size 10

arr[0] = 1;                   // Sets the first element as 1
std::cout << "First element: " << arr[0] << std::endl;

std::array<int, 4> arr2 = {1, 2, 3, 4};
std::cout << "Elements in second array: ";
for(int i = 0; i < arr2.size(); i++)
    std::cout << arr2[i] << " ";
```

该示例将产生以下输出：

```
First element: 1
Elements in second array: 1 2 3 4
```

如我们所见，`std::array` 提供了与 C 风格数组相同的 `operator[]`，以避免检查索引是否小于数组大小的开销。此外，它还提供了一个名为 `at(index)` 的函数，如果参数无效，它会抛出异常。这样，我们就能以适当的方式处理异常。因此，如果我们的代码在访问元素时存在一定的不确定性，例如数组索引依赖于用户输入，那么我们总可以使用异常处理来捕获错误，如下例所示。

```
try
{
    std::cout << arr.at(4);    // No error
    std::cout << arr.at(5);    // Throws exception std::out_of_range
}
catch (const std::out_of_range& ex)
{
    std::cerr << ex.what();
}
```

除此之外，将 `std::array` 传递给另一个函数类似于传递任何内置数据类型。我们可以按值或按引用传递，可以带或不带 `const`。此外，语法不涉及任何指针相关操作，也不涉及取地址和解引用操作。因此，与 C 风格数组相比，可读性要好得多，即使对于多维数组也是如此。下面的示例演示了如何按值传递数组：

```
void print(std::array<int, 5> arr)
{
    for(auto ele: arr)
    {
        std::cout << ele << ", ";
    }
}
```

```
std::array<int, 5> arr = {1, 2, 3, 4, 5};
print(arr);
```

该示例将产生以下输出：

1, 2, 3, 4, 5

我们不能为这个函数传递其他任何大小的数组，因为数组大小是函数参数数据类型的一部分。例如，如果我们传递 `std::array<int, 10>`，编译器将返回错误，说明它无法匹配函数参数，也无法从一种类型转换为另一种类型。然而，如果我们想要一个能够处理任意大小 `std::array` 的泛型函数，我们可以让该函数的数组大小模板化，它将为所有需要的数组大小生成代码。因此，函数签名将如下所示：

```
template <size_t N>

void print(const std::array<int, N>& arr)
```

除了可读性之外，在传递 `std::array` 时，默认会将所有元素复制到一个新数组中。因此，会自动执行深拷贝。如果我们不需要这个特性，总可以使用其他类型，例如引用和 `const` 引用。因此，它为程序员提供了更大的灵活性。

实际上，对于大多数操作，`std::array` 提供了与 C 风格数组相似的性能，因为它只是一个轻量包装器，旨在减少程序员的工作量并让代码更安全。`std::array` 提供了两个不同的函数来访问数组元素——`operator[]` 和 `at()`。`operator[]` 与 C 风格数组类似，不执行任何索引检查。而 `at()` 函数则对索引进行检查，如果索引越界则抛出异常。因此，它在实践中会稍慢一些。

如前所述，遍历数组是一个非常常见的操作。`std::array` 借助基于范围的 for 循环和迭代器提供了非常友好的接口。因此，打印数组中所有元素的代码如下所示：

```
std::array<int, 5> arr = {1, 2, 3, 4, 5};

for(auto element: arr)
{
    std::cout << element << ' ';
}
```

该示例将显示以下输出：

1 2 3 4 5

在前面的示例中，当我们演示打印所有元素时，使用了索引变量进行迭代，必须确保根据数组大小正确使用它。因此，与这个示例相比，它更容易出现人为错误。

我们之所以能够使用基于范围的循环遍历 `std::array`，是因为迭代器。`std::array` 有名为 `begin()` 和 `end()` 的成员函数，它们返回访问第一个和最后一个元素的方式。为了从一个元素移动到下一个元素，它还提供了算术运算符，例如递增运算符（`++`）和加法运算符（`+`）。因此，基于范围的 `for` 循环从 `begin()` 开始，到 `end()` 结束，使用递增运算符（`++`）逐步前进。迭代器在所有可动态迭代的 STL 容器（如 `std::array`、`std::vector`、`std::map`、`std::set` 和 `std::list`）上提供了统一的接口。

除了迭代之外，所有需要指定容器内位置的函数都基于迭代器；例如，在特定位置插入、删除某个范围内的元素或删除特定位置的元素，以及其他类似函数。这使得代码更可重用、更易维护、更具可读性。

> **注意**
>
> 在 C++ 中，所有借助迭代器指定范围的函数，`begin()` 迭代器通常是包含的，而 `end()` 迭代器通常是不包含的，除非另有说明。

因此，`array::begin()` 函数返回指向第一个元素的迭代器，而 `array::end()` 返回最后一个元素之后的迭代器。所以，基于范围的循环可以写成如下形式：

```
for(auto it = arr.begin(); it != arr.end(); it++)
{
    auto element = (*it);
    std::cout << element << ' ';
}
```

还有一些其他形式的迭代器，例如 `const_iterator` 和 `reverse_iterator`，它们也很有用。`const_iterator` 是普通迭代器的 const 版本。如果数组被声明为 `const`，那么与迭代器相关的函数（如 `begin()` 和 `end()`）将返回 `const_iterator`。

`reverse_iterator` 允许我们反向遍历数组。因此，它的功能（如递增运算符（`++`）和 `advance`）与普通迭代器的此类操作相反。

除了 `operator[]` 和 `at()` 函数之外，`std::array` 还提供了其他访问器，如下表所示：

| 函数 | 描述 |
|---|---|
| `front()` | 返回数组的第一个元素。 |
| `back()` | 返回数组的最后一个元素。 |
| `data()` | 返回指向对象内部存储的实际缓冲区的指针。这允许我们进行指针运算或任何类似操作。在处理仅接受原始指针作为函数参数的旧版/遗留代码时，此函数尤其有用。 |

图 1.6：展示 std::array 的一些访问器的表

以下代码片段演示了这些函数的用法：

```
std::array<int, 5> arr = {1, 2, 3, 4, 5};
std::cout << arr.front() << std::endl;      // Prints 1
std::cout << arr.back() << std::endl;       // Prints 5
std::cout << *(arr.data() + 1) << std::endl; // Prints 2
```

`std::array` 提供的另一个有用功能是用于深比较的关系运算符和用于深拷贝的拷贝赋值运算符。`std::array` 定义了全部六个运算符（`<`、`>`、`<=`、`>=`、`==`、`!=`）来比较两个数组，前提是 `std::array` 的底层类型也提供了相同的运算符。

C 风格数组也支持所有关系运算符，但这些运算符实际上并不比较数组内的元素；事实上，它们只比较指针。因此，只是将元素的地址作为整数进行比较，而不是对数组进行深比较。这也称为浅比较，实际用处不大。类似地，赋值操作也不会创建所赋值数据的副本。相反，它只是创建一个指向同一数据的新指针。

> **注意**
>
> 关系运算符仅适用于相同大小的 `std::array`。这是因为数组大小是数据类型本身的一部分，不允许比较两种不同数据类型的值。




在下面的示例中，我们将看到如何封装一个C风格数组，其大小由用户定义。

## 练习1：实现动态大小数组

让我们编写一个小应用程序来管理学校的学生记录。班级中的学生人数及其详细信息将作为输入。编写一个类似数组的容器来管理数据，并支持动态调整大小。我们还将实现一些实用函数来合并不同的班级。

执行以下步骤以完成练习：

- 1. 首先，包含所需的头文件：

  ```cpp
  #include <iostream>
  #include <sstream>
  #include <algorithm>
  ```

- 2. 现在，让我们编写一个名为**dynamic_array**的基本模板结构，以及主要数据成员：

  ```cpp
  template <typename T>
  class dynamic_array
  {
      T* data;
      size_t n;
  ```

- 3. 现在，让我们添加一个构造函数，它接收数组的大小并进行复制：

  ```cpp
  public:
  dynamic_array(int n)
  {
      this->n = n;
      data = new T[n];
  }
  
  dynamic_array(const dynamic_array<T>& other)
  {
      n = other.n;
      data = new T[n];
  
      for(int i = 0; i < n; i++)
          data[i] = other[i];
  }
  ```

4. 现在，让我们在公共访问区中添加`operator[]`和`function()`，以支持直接访问数据，方式类似于`std::array`：

```
T& operator[](int index)
{
    return data[index];
}

const T& operator[](int index) const
{
    return data[index];
}

T& at(int index)
{
    if(index < n)
        return data[index];
    throw "Index out of range";
}
```

5. 现在，让我们添加一个名为`size()`的函数来返回数组的大小，以及一个析构函数以避免内存泄漏：

```
size_t size() const
{
    return n;
}

~dynamic_array()
{
    delete[] data;    // A destructor to prevent memory leak
}
```

6. 现在，让我们添加迭代器函数，以支持基于范围的循环来遍历**dynamic_array**：

```
T* begin()
{
    return data;
}

const T* begin() const
{
    return data;
```

```
class1.at(nStudents) << student{"John", 8}; // Will throw exception
}
catch(...)
{
std::cout << "Exception caught" << std::endl;
}
```

```
auto class2 = class1; // Deep copy
```

```
std::cout << "Second class after initialized using first array: " << class2.to_string() << std::endl;
```

```
auto class3 = class1 + class2;
// Combines both classes and creates a bigger one
```

```
std::cout << "Combined class: ";
std::cout << class3.to_string() << std::endl;
```

```
return 0;
}
```

11. 使用三名学生——Raj（8）、Rahul（10）和Viraj（6）——作为输入来执行上述代码。控制台中的输出如下所示：

```
Enter number of students in class 1 : 3
Enter name and class of student 1: Raj 8
Enter name and class of student 2: Rahul 10
Enter name and class of student 3: Viraj 6
class1 student set out of range without exception
Exception caught
Second class after initialized using first array : [Name: Raj, Standard: 8], [Name: Rahul, Standard: 10], [Name: Viraj, Standard: 6]
Combined class : [Name: Raj, Standard: 8], [Name: Rahul, Standard: 10], [Name: Viraj, Standard: 6], [Name: Raj, Standard: 8], [Name: Rahul, Standard: 10], [Name: Viraj, Standard: 6]
```

这里提到的大多数函数都有与`std::array`类似的实现。

既然我们已经了解了各种容器，下面我们将学习如何实现一个可以接受任意类型数据并将其以通用形式存储的容器，这将在下一个练习中进行。

## 练习2：通用且快速的数据存储容器构建器

在本练习中，我们将编写一个函数，该函数接受任意数量的任意类型元素，这些元素可以转换为一个通用类型。该函数还应返回一个容器，其中所有元素都已转换为该通用类型，并且遍历速度也要快：

- 1. 首先，让我们包含所需的库：

```
#include <iostream>
#include <array>
#include <type_traits>
```

- 2. 首先，我们将尝试构建函数的签名。由于返回类型是一个便于快速遍历的容器，我们将使用`std::array`。为了允许任意数量的参数，我们将使用可变参数模板：

```
template<typename ... Args>
std::array<?, ?> build_array(Args&&... args)
```

- 3. 现在，我们必须为`std::array`提供元素类型和元素数量。我们可以使用`std::common_type`模板来确定`std::array`内部元素的类型。由于这取决于参数，我们将函数的返回类型作为尾置返回类型提供：

```
template<typename ... Args>
auto build_array(Args&&... args) -> std::array<typename std::common_type<Args...>::type, ?>
{
    using commonType = typename std::common_type<Args...>::type;
    // Create array
}
```

4. 如前面的代码所示，我们现在需要解决两个问题——元素的数量，以及如何使用**commonType**创建数组：

```
template< typename ... Args>
auto build_array(Args&&... args) -> std::array<typename std::common_type<Args...>::type, sizeof...(args)>
{
    using commonType = typename std::common_type<Args...>::type;
    return {std::forward<commonType>(args)...};
}
```

5. 现在，让我们编写**main**函数来看看我们的函数是如何工作的：

```
int main()
{
    auto data = build_array(1, 0u, 'a', 3.2f, false);
    for(auto i: data)
        std::cout << i << " ";
    std::cout << std::endl;
}
```

6. 运行代码应得到以下输出：

```
1 0 97 3.2 0
```

正如我们所见，所有最终输出都是float形式，因为所有内容都可以转换为float。

7. 为了进一步测试，我们可以在**main**函数中添加以下内容并测试输出：

```
auto data2 = build_array(1, "Packt", 2.0);
```

进行此修改后，我们应该会看到一个错误，提示所有类型无法转换为通用类型。确切的错误消息应提到模板推导失败。这是因为不存在一个可以同时转换字符串和数字的单一类型。

诸如此次练习中创建的构建器函数，可以在你不确定数据类型但又需要优化效率时使用。

有很多有用的特性和实用函数是**std::array**没有提供的。其中一个主要原因是为了保持与C风格数组相似或更好的性能和内存需求。

为了获得更高级的功能和灵活性，C++提供了另一种称为**std::vector**的结构。我们将在下一节中研究它是如何工作的。

## std::vector

正如我们之前看到的，`std::array`是对C风格数组的一个很好的改进。但`std::array`也有一些局限性，在编写应用程序时，它缺少一些频繁使用场景所需的函数。以下是`std::array`的一些主要缺点：

- - `std::array`的大小必须是常量，在编译时提供，并且是固定的。因此，我们无法在运行时更改它。
- - 由于大小限制，我们无法从数组中插入或移除元素。
- - `std::array`无法进行自定义分配。它始终使用栈内存。

在大多数实际应用中，数据是相当动态的，而不是固定大小的。例如，在我们之前的医院管理系统示例中，可能有更多医生加入医院，紧急情况下可能有更多患者，等等。因此，预先知道数据的大小并不总是可能的。所以，`std::array`并不总是最佳选择，我们需要具有动态大小的东西。

现在，我们来看看`std::vector`如何为这些问题提供解决方案。

## std::vector – 可变长度数组

正如标题所示，`std::vector`解决了数组最突出的问题之一——固定大小。`std::vector`不要求我们在初始化时提供其长度。

以下是我们初始化向量的一些方式：

```
std::vector<int> vec;

// Declares vector of size 0
```

```
std::vector<int> vec = {1, 2, 3, 4, 5};

// Declares vector of size 5 with provided elements
```

```
std::vector<int> vec(10);

// Declares vector of size 10
```

```
std::vector<int> vec(10, 5);

// Declares vector of size 10 with each element's value = 5
```





从第一次初始化可以看出，提供大小并不是强制性的。如果我们没有显式指定大小，并且也没有通过指定其元素来推断大小，vector 将根据编译器实现初始化为取决于元素的容量。术语“size”指的是 vector 中实际存在的元素数量，这可能与其容量不同。因此，对于第一次初始化，size 将为零，但 capacity 可能是某个较小数字或零。

我们可以使用 `push_back` 或 `insert` 函数在 vector 中插入元素。`push_back` 会在末尾插入元素。`insert` 将迭代器作为第一个参数用于位置，并且可用于在任何位置插入元素。`push_back` 是 vector 中使用频率非常高的函数，因为其性能。`push_back` 算法的伪代码如下：

```
push_back(val):
    if size < capacity
        // If vector has enough space to accommodate this element
        - Set element after the current last element = val
        - Increment size
        - return;

    if vector is already full
        - Allocate memory of size 2*size
        - Copy/Move elements to newly allocated memory
        - Make original data point to new memory
        - Insert the element at the end
```

实际实现可能会略有不同，但逻辑保持不变。如我们所见，如果有足够的空间，在末尾插入东西只需要 O(1) 时间。然而，如果空间不足，它将不得不复制/移动所有元素，这将花费 O(n) 时间。大多数实现在耗尽容量时都会将 vector 的大小加倍。因此，O(n) 时间操作是在 n 个元素之后执行的。所以，平均而言，它只需要多一步，使其平均时间复杂度更接近 O(1)。这在实践中提供了相当好的性能，因此它是一个使用频率很高的容器。

对于 `insert` 函数，你别无选择，只能将给定迭代器之后的元素向右移动。`insert` 函数为我们做了这件事。它还在需要时处理重新分配。由于需要移动元素，它花费 O(n) 时间。以下示例演示如何实现 vector 插入函数。

考虑一个包含前五个自然数的 vector：

```
std::vector<int> vec = {1, 2, 3, 4, 5};
```

> **注意：** Vector 没有 `push_front` 函数。它有一个通用的 `insert` 函数，该函数将迭代器作为位置参数。

通用的 `insert` 函数可用于在前面插入元素，如下所示：

```
vec.insert(vec.begin(), 0);
```

让我们再看几个 `push_back` 和 `insert` 函数的示例：

```
std::vector<int> vec;

// Empty vector {}

vec.push_back(1);

// Vector has one element {1}

vec.push_back(2);

// Vector has 2 elements {1, 2}

vec.insert(vec.begin(), 0);

// Vector has 3 elements {0, 1, 2}

vec.insert(find(vec.begin(), vec.end(), 1), 4);

// Vector has 4 elements {0, 4, 1, 2}
```

如前面的代码所示，`push_back` 在末尾插入元素。此外，`insert` 函数将插入位置作为参数。它以迭代器的形式接收。因此，`begin()` 函数允许我们在开头插入元素。

现在我们已经了解了正常的插入函数，让我们来看一下与 `push_back` 和 `insert` 函数相比，vector 可用的一些更好的替代方案。`push_back` 和 `insert` 的一个缺点是它们首先构造元素，然后将元素复制或移动到 vector 缓冲区内的新位置。此操作可以通过调用新位置本身的新元素的构造函数来优化，这可以通过 `emplace_back` 和 `emplace` 函数完成。建议你为获得更好的性能而使用这些函数而不是正常的插入函数。由于我们是在原地构造元素，我们只需要传递构造函数参数，而不是构造好的值本身。然后，函数将负责将参数转发到适当位置的构造函数。

`std::vector` 还提供了 `pop_back` 和 `erase` 函数来从中删除元素。`pop_back` 从 vector 中删除最后一个元素，有效地将大小减少一个。`erase` 有两个重载——通过指向它的迭代器删除单个元素，以及通过迭代器删除一系列元素，其中范围由要删除的第一个元素（包含）和要删除的最后一个元素（不包含）定义。C++ 标准不要求这些函数减少 vector 的容量。它完全取决于编译器实现。`pop_back` 不需要重新排列元素，因此可以非常快速地完成。它的复杂度为 O(1)。然而，`erase` 需要移动元素，因此花费 O(n) 时间。在下面的练习中，我们将看到这些函数是如何实现的。

现在，让我们来看一下关于以不同方式从 vector 中删除元素的示例：

考虑一个包含 10 个元素的 vector - {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}：

```
vec.pop_back();
// Vector has now 9 elements {0, 1, 2, 3, 4, 5, 6, 7, 8}
```

```
vec.erase(vec.begin());
// Vector has now 8 elements {1, 2, 3, 4, 5, 6, 7, 8}
```

```
vec.erase(vec.begin() + 1, vec.begin() + 4);
// Now, vector has 5 elements {1, 5, 6, 7, 8}
```

## std::forward_list

到目前为止，我们只看到类数组结构，但正如我们所见，在连续数据结构中间进行插入和删除是非常低效的操作。而这正是链表类结构发挥作用的地方。许多应用程序需要在数据结构中间频繁进行插入和删除。例如，任何具有多个标签页的浏览器都可以在任何时间点和任何位置添加额外的标签页。同样，任何音乐播放器都会有一系列循环播放的歌曲，你也可以在中间插入任何歌曲。在这种情况下，我们可以使用链表结构以获得良好的性能。我们将在活动 1 *实现歌曲播放列表* 中看到音乐播放器的用例。现在，让我们探索 C++ 为我们提供了哪些容器。

链表的基本结构要求我们拥有一个指针，并使用 **new** 和 **delete** 运算符手动管理内存分配和释放。虽然并不困难，但它可能导致难以追踪的错误。因此，就像 `std::array` 为 C 风格数组提供了一个薄包装一样，`std::forward_list` 为基本的链表提供了一个薄包装。

`std::forward_list` 的目的是在不影响与基本链表相比的性能的情况下提供一些附加功能。为了保持性能，它不提供获取列表大小或直接获取除第一个元素之外的任何元素的函数。因此，它有一个名为 `front()` 的函数来获取对第一个元素的引用，但没有像 `back()` 这样访问最后一个元素的函数。它确实提供了常见操作的函数，例如插入、删除、反转和拼接。这些函数不会影响基本链表的内存需求或性能。

此外，就像 `std::vector` 一样，`std::forward_list` 如果需要也可以接受第二个模板参数作为自定义分配器。因此，我们可以轻松地将其用于从自定义内存管理中受益的高级应用程序。

### 在 forward_list 中插入和删除元素

`std::forward_list` 提供了 `push_front` 和 `insert_after` 函数，可用于在链表中插入元素。这两者与 vector 的插入函数略有不同。`push_front` 用于在前面插入元素。由于 `forward_list` 没有直接访问最后一个元素的权限，因此它不提供 `push_back` 函数。对于在特定位置插入，我们使用 `insert_after` 而不是 `insert`。这是因为在链表中插入元素需要更新我们想要在之后插入新元素的元素的 next 指针。如果我们只提供想要插入新元素的迭代器，我们就无法快速访问前一个元素，因为在 `forward_list` 中向后遍历是不允许的。




由于这是基于指针的机制，在插入时我们实际上不需要移动元素。因此，与任何基于数组的结构相比，这两个插入函数都要快得多。这两个函数只需修改指针，即可在预期位置插入新元素。此操作不依赖于列表的大小，因此时间复杂度为 O(1)。我们将在接下来的练习中查看这些函数的实现。

现在，让我们看看如何向链表中插入元素：

```
std::forward_list<int> fwd_list = {1, 2, 3};
```

```
fwd_list.push_front(0);
// list becomes {0, 1, 2, 3}
```

```
auto it = fwd_list.begin();
```

```
fwd_list.insert_after(it, 5);
// list becomes {0, 5, 1, 2, 3}
```

```
fwd_list.insert_after(it, 6);
// list becomes {0, 6, 5, 1, 2, 3}
```

**forward_list** 还提供了 **emplace_front** 和 **emplace_after**，它们类似于 vector 的 **emplace**。这两个函数执行与插入函数相同的操作，但通过避免额外的复制和移动而更加高效。

**forward_list** 还有用于删除元素的 **pop_front** 和 **erase_after** 函数。**pop_front**，顾名思义，会移除第一个元素。由于它不需要任何移动，该操作在实践中非常快，时间复杂度为 O(1)。**erase_after** 有两个重载——一个用于移除单个元素（通过接受指向其前一个元素的迭代器），另一个用于移除范围内的多个元素（通过接受指向范围内第一个元素之前元素的迭代器，以及指向最后一个元素的迭代器）。

**erase_after** 函数的时间复杂度与被删除元素的数量成线性关系，因为删除元素不能通过只释放单个内存块来实现。由于所有节点都分散在内存中的随机位置，该函数需要分别释放每个节点。

现在，让我们看看如何从列表中移除元素：

```
std::forward_list<int> fwd_list = {1, 2, 3, 4, 5};
```

```
fwd_list.pop_front();
// list becomes {2, 3, 4, 5}
```

```
auto it = fwd_list.begin();
```

```
fwd_list.erase_after(it);
// list becomes {2, 4, 5}
```

```
fwd_list.erase_after(it, fwd_list.end());
// list becomes {2}
```

让我们在下一节中探讨我们可以使用 forward_list 进行哪些其他操作。

## forward_list 上的其他操作

除了根据迭代器确定的位置删除元素的 erase 函数之外，forward_list 还提供了 remove 和 remove_if 函数，用于根据元素值删除元素。remove 函数接受单个参数——要删除的元素的值。它基于为该值类型定义的相等运算符，删除所有与给定元素匹配的元素。如果没有相等运算符，编译器将不允许我们调用该函数，并抛出编译错误。由于 remove 仅基于相等运算符删除元素，因此不能用于基于其他条件的删除，因为一旦定义了相等运算符，我们就无法更改它。对于条件删除，forward_list 提供了 remove_if 函数。它接受一个谓词作为参数，该谓词是一个函数，接受值类型的一个元素作为参数，并返回一个布尔值。因此，所有谓词返回 true 的元素都会从列表中移除。使用最新的 C++ 版本，我们还可以轻松地用 lambda 指定谓词。下面的练习应该能帮助你理解如何实现这些函数。

## 练习 3：使用 remove_if 从链表中条件移除元素

在本练习中，我们将使用选举期间一些印度公民的示例信息，并根据年龄从选民名册中移除不符合资格的公民。为简单起见，我们只存储公民的姓名和年龄。

我们将数据存储在链表中，并使用 remove_if 移除所需元素，它提供了一种移除满足特定条件的元素的方法，而不是定义要移除元素的位置：

+   1. 让我们首先包含所需的头文件，并添加 citizen 结构体：

```
#include <iostream>
#include <forward_list>

struct citizen
{
    std::string name;
    int age;
};

std::ostream& operator<<(std::ostream& os, const citizen& c)
{
    return (os << "[Name: " << c.name << ", Age: " << c.age << "]");
}
```

+   2. 现在，让我们编写一个 main 函数，并在 std::forward_list 中初始化几个公民。我们还将复制一份，以避免再次初始化：

```
int main()
{
    std::forward_list<citizen> citizens = {{"Raj", 22}, {"Rohit", 25}, {"Rohan", 17}, {"Sachin", 16}};

    auto citizens_copy = citizens;

    std::cout << "All the citizens: ";
    for (const auto &c : citizens)
        std::cout << c << " ";
    std::cout << std::endl;
```

**remove_if** 函数的时间复杂度为 O(n)，因为它只需遍历列表一次，同时按需移除所有元素。如果我们要移除具有特定值的元素，可以使用另一个版本的 **remove**，它只接受一个参数，即对象，并从列表中移除所有匹配给定值的对象。它还要求我们为给定类型实现 == 运算符。

**forward_list** 还提供了一个 **sort** 函数来对数据进行排序。所有与数组相关的结构都可以通过通用函数 **std::sort(first iterator, last iterator)** 进行排序。但是，基于链表的结构无法使用它，因为我们无法随机访问任何数据。这也使得 **forward_list** 提供的迭代器不同于数组或 vector 的迭代器。我们将在下一节中更详细地了解这一点。作为 **forward_list** 的一部分提供的 sort 函数有两个重载——基于小于运算符 (<) 的 **sort**，以及基于作为参数提供的比较器的排序。默认的 **sort** 函数使用 **std::less<value_type>** 进行比较。如果第一个参数小于第二个参数，它就返回 true，因此要求我们为自定义类型定义小于运算符 (<)。

除此之外，如果我们想基于其他参数进行比较，可以使用接受二元谓词的参数化重载。这两个重载的时间复杂度均为线性对数复杂度——O(n × log n)。以下示例演示了 **sort** 的两个重载：

```
std::forward_list<int> list1 = {23, 0, 1, -3, 34, 32};

list1.sort();

// list becomes {-3, 0, 1, 23, 32, 34}

list1.sort(std::greater<int>());

// list becomes {34, 32, 23, 1, 0, -3}
```

这里，**greater<int>** 是标准库本身提供的谓词，它是对大于运算符 (>) 的包装，用于将元素按降序排序，正如我们从列表的值中看到的那样。

**forward_list** 中提供的其他函数是 **reverse** 和 **unique**。**reverse** 函数简单地反转元素的顺序，其消耗时间与列表中元素的数量成线性关系，即时间复杂度为 O(n)。**unique** 函数仅保留列表中的唯一元素，并移除除第一个之外的所有重复值元素。由于它依赖于元素的相等性，因此有两个重载——第一个不带参数，使用值类型的相等运算符；第二个接受带有两个值类型参数的二元谓词。**unique** 函数在设计上是线性时间复杂度的。因此，它不会将每个元素与其他所有元素进行比较。相反，它只比较相邻元素是否相等，并在默认或自定义二元谓词判断后者与前者相同时移除后者。因此，要使用 **unique** 函数从列表中移除所有重复元素，我们需要在调用该函数之前对元素进行排序。在给定谓词的帮助下，**unique** 会将所有元素与其相邻元素进行比较，如果谓词返回 **true**，则移除后者。

现在让我们看看如何对列表使用 **reverse**、**sort** 和 **unique** 函数：

```
std::forward_list<int> list1 = {2, 53, 1, 0, 4, 10};

list1.reverse();
// list becomes {2, 53, 1, 0, 4, 10}

list1 = {0, 1, 0, 1, -1, 10, 5, 10, 5, 0};

list1.sort();
// list becomes {-1, 0, 0, 0, 1, 1, 5, 5, 10, 10}

list1.unique();
// list becomes {-1, 0, 1, 5, 10}

list1 = {0, 1, 0, 1, -1, 10, 5, 10, 5, 0};

list1.sort();
// list becomes {-1, 0, 0, 0, 1, 1, 5, 5, 10, 10}
```




下面的示例将删除与前一个有效元素相比差值小于2的元素：

```
list1.unique([](int a, int b) { return (b - a) < 2; });
```

```
// list becomes {-1, 1, 5, 10}
```

## 注意

在调用 unique 函数之前，程序员必须确保数据已经排序。因此，我们在它之前调用 sort 函数。unique 函数会将当前元素与前一个已经满足条件的元素进行比较。此外，它始终保留原始列表的第一个元素。因此，总有一个元素可以进行比较。

在下一节中，我们将了解 forward_list 迭代器与 vector/array 迭代器的不同之处。

## 迭代器

你可能已经注意到，在数组和 vector 的某些示例中，我们会向迭代器加上数字。迭代器类似于指针，但它们还为 STL 容器提供了通用接口。对这些迭代器的操作严格基于迭代器的类型，而类型又取决于容器。vector 和数组的迭代器在功能上最为灵活。由于数据的连续性，我们可以使用 operator[] 根据位置直接从容器中访问任意元素。这种迭代器也称为随机访问迭代器。然而，对于 forward_list，没有直接的方法可以向后遍历，甚至无法在不从头开始的情况下从一个节点移动到其前一个节点。因此，这种迭代器唯一允许的算术运算符是递增。这种迭代器也称为前向迭代器。

我们还可以使用其他实用函数，例如 advance、next 和 prev，具体取决于迭代器的类型。next 和 prev 接收一个迭代器和一个距离值，然后返回指向与给定迭代器相距指定距离的元素的迭代器。只要给定的迭代器支持相应操作，它们就会按预期工作。例如，如果我们尝试将 prev 函数用于前向迭代器，则会触发编译错误，因为这种迭代器是前向迭代器，只能向前移动。这些函数所需的时间取决于所使用的迭代器类型。对于随机访问迭代器，它们都是常数时间函数，因为加法和减法是常数时间操作。对于其余迭代器，它们的时间与需要向前或向后遍历的距离呈线性关系。我们将在下面的练习中使用这些迭代器。

## 练习 4：探索不同类型的迭代器

假设我们有一个过去几年新加坡F1大奖赛冠军的列表。借助 vector 迭代器，我们将了解如何从这些数据中获取有用信息。之后，我们将尝试用 forward_list 做同样的事情，并观察它与 vector 迭代器的不同之处。

- 1. 首先包含所需的头文件：

```
```cpp
#include <iostream>
#include <forward_list>
#include <vector>

int main()
{
```
```

- 2. 编写一个包含冠军列表的 vector：

```
```cpp
std::vector<std::string> vec = {"Lewis Hamilton", "Lewis Hamilton", "Nico Rosberg", "Sebastian Vettel", "Lewis Hamilton", "Sebastian Vettel", "Sebastian Vettel", "Sebastian Vettel", "Fernando Alonso"};

auto it = vec.begin();        // Constant time
std::cout << "Latest winner is: " << *it << std::endl;

it += 8;                      // Constant time
std::cout << "Winner before 8 years was: " << *it << std::endl;

advance(it, -3);              // Constant time
std::cout << "Winner before 3 years of that was: " << *it << std::endl;
```
```

- 3. 尝试对 **forward_list** 迭代器做同样的操作，看看它们与 vector 迭代器有何不同：

```
```cpp
std::forward_list<std::string> fwd(vec.begin(), vec.end());

auto it1 = fwd.begin();
std::cout << "Latest winner is: " << *it << std::endl;

advance(it1, 5);   // Time taken is proportional to the number of elements

std::cout << "Winner before 5 years was: " << *it << std::endl;
```
```

```
// Going back will result in compile time error as forward_list only allows us to move towards the end.

// advance(it1, -2);    // Compiler error
}
```

4. 运行本练习应产生以下输出：

```
Latest winner is : Lewis Hamilton
Winner before 8 years was : Fernando Alonso
Winner before 3 years of that was : Sebastian Vettel
Latest winner is : Sebastian Vettel
Winner before 5 years was : Sebastian Vettel
```

5. 现在，让我们看看如果将下面这一行添加到 main 函数末尾，给这个迭代器加上一个数字会发生什么：

```
it1 += 2;
```

我们会得到类似这样的错误消息：

```
no match for 'operator+=' (operand types are std::_Fwd_list_iterator<int>' and 'int')
```

我们在本练习中探索的各种迭代器对于轻松地从数据集中获取任何数据非常有用。

正如我们所见，std::array 是 C 风格数组的轻量封装，而 std::forward_list 只是单链表的轻量封装。它提供了一个简单且不易出错的接口，同时不会影响性能和内存。

此外，由于我们可以在 vector 中立即访问任何元素，因此 vector 迭代器上的加法和减法操作的时间复杂度为 O(1)。另一方面，forward_list 只能通过遍历来访问元素。因此，其迭代器的加法操作的时间复杂度为 O(n)，其中 n 是前进的步数。

在下面的练习中，我们将创建一个自定义容器，其工作方式与 std::forward_list 类似，但进行了一些改进。我们将定义许多与 forward_list 函数等效的函数。这也有助于你理解这些函数在底层是如何工作的。

## 练习 5：构建一个基本的自定义容器

在本练习中，我们将实现一个与 std::forward_list 等效的容器，并做出一些改进。我们将从一个名为 singly_ll 的基本实现开始，并逐步进行改进：

- 1. 添加所需的头文件，然后从包含单个节点的 singly_ll 基本实现开始：

```
```cpp
#include <iostream>
#include <algorithm>

struct singly_ll_node
{
    int data;
    singly_ll_node* next;
};
```
```

- 2. 现在，我们将实现真正的 singly_ll 类，它将节点封装起来以便于接口交互：

```
```cpp
class singly_ll
{
public:
    using node = singly_ll_node;
    using node_ptr = node*;

private:
    node_ptr head;
```
```

- 3. 现在，让我们像 forward_list 中那样添加 push_front 和 pop_front：

```
```cpp
public:
void push_front(int val)
{
    auto new_node = new node{val, NULL};
    if(head != NULL)
        new_node->next = head;
    head = new_node;
}

void pop_front()
{
    auto first = head;
    if(head)
```

6. 让我们添加作为友元函数的相等性操作：

```
friend bool operator==(const singly_ll_iterator& left, const singly_ll_iterator& right)
{
    return left.ptr == right.ptr;
}

friend bool operator!=(const singly_ll_iterator& left, const singly_ll_iterator& right)
{
    return left.ptr != right.ptr;
};
```

7. 让我们回到链表类。既然我们已经有了迭代器类，接下来实现 begin 和 end 函数以简化遍历。我们还将为两者添加 const 版本：

```
singly_ll_iterator begin()
{
    return singly_ll_iterator(head);
}

singly_ll_iterator end()
{
    return singly_ll_iterator(NULL);
}

singly_ll_iterator begin() const
{
    return singly_ll_iterator(head);
}

singly_ll_iterator end() const
{
    return singly_ll_iterator(NULL);
}
```

8. 让我们实现一个默认构造函数、一个用于深拷贝的拷贝构造函数，以及一个带有 initializer_list 的构造函数：

```
singly_ll() = default;

singly_ll(const singly_ll& other) : head(NULL)
{
    if(other.head)
    {
        head = new node;
        auto cur = head;
        auto it = other.begin();
        while(true)
        {
            cur->data = *it;

            auto tmp = it;
            ++tmp;
            if(tmp == other.end())
                break;

            cur->next = new node;
            cur = cur->next;
            it = tmp;
        }
    }
}

singly_ll(const std::initializer_list<int>& ilist) : head(NULL)
{
    for(auto it = std::rbegin(ilist); it != std::rend(ilist); it++)
        push_front(*it);
};
```




9. 让我们编写一个主函数来使用上述函数：

```
int main()
{
    singly_ll sll = {1, 2, 3};
    sll.push_front(0);

    std::cout << "First list: ";
    for(auto i: sll)
        std::cout << i << " ";
    std::cout << std::endl;

    auto sll2 = sll;
    sll2.push_front(-1);
    std::cout << "Second list after copying from first list and inserting -1 in front: ";
    for(auto i: sll2)
        std::cout << i << ' ';  // Prints -1 0 1 2 3
    std::cout << std::endl;

    std::cout << "First list after copying - deep copy: ";
    for(auto i: sll)
        std::cout << i << ' ';  // Prints 0 1 2 3
    std::cout << std::endl;
}
```

10. 运行此练习应产生以下输出：

```
First list: 0 1 2 3
Second list after copying from first list and inserting -1 in front: -1 0 1 2 3
First list after copying - deep copy: 0 1 2 3
```

正如我们在前面的示例中所看到的，我们能够使用 std::initializer_list 初始化我们的列表。我们可以调用 push、pop_front 和 back 函数。正如我们所看到的，sll2.pop_back 只从 sll2 中删除了元素，而没有从 sll 中删除。sll 仍然完好，包含全部五个元素。因此，我们也可以执行深复制。

## 活动1：实现歌曲播放列表

在本活动中，我们将探讨一些双向链表不够用或不方便的应用程序。我们将构建一个适合该应用程序的调整版本。我们经常遇到必须自定义默认实现的情况，例如在音乐播放器中循环播放歌曲，或在游戏中多个玩家围成一圈轮流进行时。

这些应用程序有一个共同特性——我们以循环方式遍历序列中的元素。因此，在遍历列表时，最后一个节点之后的节点将是第一个节点。这被称为循环链表。

我们将以音乐播放器作为用例。它应支持以下功能：

+ 1. 使用多首歌曲创建播放列表。
+ 2. 向播放列表添加歌曲。
+ 3. 从播放列表中移除歌曲。
+ 4. 循环播放歌曲（在本活动中，我们将所有歌曲打印一次）。

> 注意
> 你可以参考练习5：构建一个基本自定义容器，我们在其中从头构建了一个支持类似功能的容器。

以下是解决该问题的步骤：

+ 1. 首先，设计一个支持循环数据表示的基本结构。
+ 2. 然后，在结构中实现插入和删除函数，以支持各种操作。
+ 3. 我们必须编写一个自定义迭代器。这有点棘手。重要的是确保我们能够使用基于范围的循环方法遍历容器。因此，begin() 和 end() 应返回不同的地址，尽管结构是循环的。
+ 4. 构建容器后，在其上构建一个包装器，用于存储播放列表中的不同歌曲并执行相关操作，如下一首、上一首、全部打印、插入和删除。

> 注意
> 此活动的解决方案可以在第476页找到。

std::forward_list 有一些限制。std::list 提供了更灵活的列表实现，并有助于克服 forward_list 的一些缺点。

## std::list

如前一节所述，std::forward_list 只是对基本链表的简洁而轻量的包装。它不提供在末尾插入元素、向后遍历或获取列表大小等有用操作。其功能受限是为了节省内存和保持快速性能。除此之外，forward_list 的迭代器只能支持很少的操作。在任何应用的大多数实际情况下，诸如在末尾插入内容和获取容器大小等功能非常有用且经常使用。因此，在需要快速插入的情况下，std::forward_list 并不总是理想的容器。为了克服 std::forward_list 的这些限制，C++ 提供了 std::list，由于它是双向链表，因此具有一些额外的特性。然而，请注意，这是以额外内存需求为代价的。

双向链表的简单版本如下所示：

```
struct doubly_linked_list
{
    int data;
    doubly_linked_list *next, *prev;
};
```

如你所见，它有一个额外的指针来指向前一个元素。因此，它为我们提供了一种向后遍历的方式，并且我们还可以存储大小和最后一个元素，以支持快速的 push_back 和 size 操作。此外，与 forward_list 一样，它也可以支持自定义分配器作为模板参数。

## 用于 std::list 的常见函数

std::list 的大多数函数与 std::forward_list 的函数相同或相似，但有一些调整。其中一个调整是，以 _after 结尾的函数名有对应的不带 _after 的版本。因此，insert_after 和 emplace_after 就变成了 insert 和 emplace。这是因为，使用 std::list 迭代器时，我们也可以向后遍历，因此无需提供前一个元素的迭代器。相反，我们可以直接提供要执行操作的确切元素的迭代器。除此之外，std::list 还为 push_back、emplace_back 和 pop_back 提供了快速操作。以下练习演示了 std::list 的插入和删除函数的使用。

## 练习6：`std::list` 的插入和删除函数

在本练习中，我们将使用 `std::list` 创建一个简单的整数列表，并探索可以从中插入和删除元素的各种方法：

+ 1. 首先，让我们包含所需的头文件：

```
```cpp
#include <iostream>
#include <list>

int main()
{
```
```

+ 2. 然后，用一个包含少量元素的列表进行初始化，并使用各种插入函数对其进行实验：

```
```cpp
std::list<int> list1 = {1, 2, 3, 4, 5};

list1.push_back(6);
// list becomes {1, 2, 3, 4, 5, 6}

list1.insert(next(list1.begin()), 0);
// list becomes {1, 0, 2, 3, 4, 5, 6}

list1.insert(list1.end(), 7);
// list becomes {1, 0, 2, 3, 4, 5, 6, 7}
```
```

如你所见，**push_back** 函数在末尾插入一个元素。**insert** 函数在第一个元素之后插入 0，该位置由 **next(list1.begin())** 指定。之后，我们在最后一个元素之后插入 7，该位置由 **list1.end()** 指定。

+ 3. 现在，让我们看看删除函数 **pop_back**，它在 **forward_list** 中不存在：

```
```cpp
list1.pop_back();
// list becomes {1, 0, 2, 3, 4, 5, 6}

std::cout << "List after insertion & deletion functions: ";
for(auto i: list1)
    std::cout << i << " ";
}
```
```

## 双向迭代器

在“迭代器”一节中，我们看到了基于数组的随机访问迭代器与基于 forward_list 的前向迭代器在灵活性上的差异。std::list::iterator 的灵活性介于两者之间。与前向迭代器相比，它更灵活，因为它允许我们向后遍历。因此，std::list 还通过暴露反向迭代器来支持反向遍历函数，其操作是反转的。话虽如此，它并不像随机访问迭代器那样灵活。虽然我们可以向任一方向前进任意步数，但由于这些移动必须通过逐个遍历元素来完成，而不是直接跳到目标元素，时间复杂度仍然是线性的，而不是像随机访问迭代器那样的常数。由于这些迭代器可以沿任一方向移动，因此它们被称为双向迭代器。

## 不同容器的迭代器失效

到目前为止，我们已经看到，迭代器为我们提供了一种统一的方式来访问、遍历、插入和删除任何容器中的元素。但在某些情况下，修改容器后迭代器会变得无效，因为迭代器是基于指针实现的，而指针与内存地址绑定。因此，如果由于容器中的修改而导致任何节点或元素的内存地址发生变化，就会使迭代器失效，如果仍然使用它，则可能导致未定义的行为。

例如，一个非常基本的例子是 vector::push_back，它只是在末尾添加一个新元素。然而，正如我们之前看到的，在某些情况下，它还需要将所有元素移动到一个新的缓冲区。因此，所有迭代器、指针、甚至对任何现有元素的引用都将失效。类似地，如果 vector::insert 函数导致重新分配，则所有元素都需要被移动。因此，所有迭代器、指针和引用都会失效。如果没有导致重新分配，则该函数将使所有指向插入位置右侧元素的迭代器失效，因为这些元素在此过程中会被移动。

与 vector 不同，基于链表的迭代器在插入和删除操作中更安全，因为元素不会被移动或搬移。因此，std::list 或 forward_list 的任何插入函数都不会影响迭代器的有效性。一个例外是，删除相关操作会使被删除元素的迭代器失效，这既明显又合理。它不会影响其余元素迭代器的有效性。以下示例展示了不同迭代器的失效情况：

```
std::vector<int> vec = {1, 2, 3, 4, 5};
```

```
auto it4 = vec.begin() + 4;
```



// 现在 it4 指向 vec[4]

```
vec.insert(vec.begin() + 2, 0);
```

```
// vec becomes {1, 2, 0, 3, 4, 5}
```

此时 `it4` 已失效，因为它位于插入位置之后。访问它将导致未定义行为：

```
std::list<int> lst = {1, 2, 3, 4, 5};
```

```
auto l_it4 = next(lst.begin(), 4);
```

```
lst.insert(next(lst.begin(), 2), 0);
```

```
// l_it4 remains valid
```

正如我们所见，std::list 比 std::forward_list 灵活得多。它提供了许多操作，例如 size、push_back 和 pop_back，这些操作的时间复杂度均为 O(1)。因此，std::list 比 std::forward_list 使用得更频繁。如果我们的内存和性能约束非常严格，并且确定不需要向后遍历，那么 forward_list 是更好的选择。因此，在大多数情况下，std::list 是更安全的选择。

## 活动 2：模拟纸牌游戏

在本活动中，我们将分析给定情境，并尝试找出最适合的数据结构以达到最佳性能。

我们将尝试模拟一个纸牌游戏。游戏中有 4 名玩家，每人一开始有 13 张随机牌。然后，我们会从每名玩家的手牌中随机抽取一张牌。这样，我们就有了 4 张牌进行比较。之后，我们将从这 4 张牌中移除相匹配的牌。如果有剩余牌，则由打出这些牌的玩家收回。如果有多组匹配对，但只能移除其中一组，我们可以选择任意一组。如果没有匹配对，玩家可以重新洗自己的牌。

现在，我们需要不断重复这个过程，直到至少有一人手中没有牌。第一个出完所有牌的玩家赢得游戏。最后，我们将输出获胜者。

执行以下步骤来完成本活动：

- 1. 首先，确定哪种容器最适合存储每位玩家的牌。我们应该有四个容器，每个容器包含一组牌——每位玩家一个。
- 2. 编写一个函数来初始化并洗牌。
- 3. 编写一个函数，将所有的牌随机发给四位玩家。
- 4. 编写一个匹配函数。该函数将从每位玩家手中抽取一张牌，并按照游戏规则进行比较。然后，它会移除必要的牌。我们必须明智地选择牌，以便更快地移除。在决定使用哪种容器时，也应考虑这一因素。
- 5. 现在，让我们编写一个函数来判断是否产生了获胜者。
- 6. 最后，我们将编写游戏的核心逻辑。该逻辑只需调用匹配函数，直到根据上一步编写的函数确定获胜者为止。

> **注意**
> 
> 本活动的解决方案可在第 482 页找到。

## std::deque——std::vector 的特殊版本

到目前为止，我们已经了解了基于数组和基于链表的容器。**std::deque** 将两者混合起来，在一定程度上结合了各自的优点。正如我们所看到的，虽然 vector 是一个可变长数组，但它的某些函数，如 **push_front** 和 **pop_front**，是代价非常高昂的操作。**std::deque** 可以帮助我们克服这一点。Deque 是双端队列（double-ended queue）的缩写。

### deque 的结构

C++ 标准只定义了容器的行为，而没有定义其实现。我们目前所见的容器都足够简单，可以让我们预测其实现。然而，deque 比它们略微复杂一些。因此，我们首先来看一下它的要求，然后再尝试深入了解一点实现细节。

C++ 标准保证了 deque 不同操作的时间复杂度如下：

- - 对于 push_front、pop_front、push_back 和 pop_back，复杂度为 O(1)
- - 对于所有元素的随机访问，复杂度为 O(1)
- - 在中间插入或删除的情况下，最多需要 N/2 步，其中 N = deque 的大小

从这些要求来看，我们可以说，该容器应该能够在任意一端非常快速地增长，同时仍然能够对所有元素进行随机访问。因此，其结构应该有点像 vector，但也要能从头部和尾部扩展。插入和删除的要求暗示了我们将会移动元素，因为只允许最多 N/2 步的操作。这也验证了我们之前关于其行为类似于 vector 的假设。由于容器可以快速地向任一方向增长，我们不必每次都把元素向右移动。相反，我们可以将元素移向最近的末端。这样，时间复杂度最多为 N/2 步，因为容器内任意插入点距离最近的末端不会超过 N/2 个节点。

现在，让我们关注随机访问和在前端插入。该结构不能存储在单一内存块中。相反，我们可以拥有多个大小相同的内存块。这样，根据索引和块的大小（或每个块中的元素数量），我们就能确定需要哪个块中对应索引的元素。只有当我们将所有内存块的指针存储在一片连续的位置时，这才能帮助我们在 O(1) 时间内实现随机访问。因此，可以认为该结构类似于一个由数组组成的 vector。

当我们想在前端插入某个元素，但第一个内存块中没有足够空间时，就必须分配另一个块，并将其地址插入到指针 vector 的前端。这可能需要重新分配指针 vector，但实际数据不会被移动。为了优化这种重新分配，我们可以不从第一个块开始，而是从 vector 的中间块开始插入。这样，在一定数量的前端插入操作内，我们是安全的。在重新分配指针 vector 时，也可以采用同样的做法。

### 注意

由于 deque 并不像本章讨论的其他容器那样简单，实际实现可能会有所不同，或者可能比我们讨论的拥有更多的优化，但基本思想是不变的。也就是说，我们需要多个连续内存块来实现这样一个容器。

deque 支持的函数和操作更像是 vector 和 list 所支持函数的组合；因此，我们有 push_front、push_back、insert、emplace_front、emplace_back、emplace、pop_front、pop_back 和 erase 等等。我们也有 vector 的诸如 shrink_to_fit 之类的函数来优化容量，但我们没有名为 capacity 的函数，因为它高度依赖于实现，因此预计不会暴露出来。而且，正如你可能预料的那样，它像 vector 一样提供随机访问迭代器。

让我们看看如何在 deque 上使用不同的插入和删除操作：

```
std::deque<int> deq = {1, 2, 3, 4, 5};
```

```
deq.push_front(0);
// deque becomes {0, 1, 2, 3, 4, 5}
```

```
deq.push_back(6);
// deque becomes {0, 1, 2, 3, 4, 5, 6}
```

```
deq.insert(deq.begin() + 2, 10);
// deque becomes {0, 1, 10, 2, 3, 4, 5, 6}
```

```
deq.pop_back();
// deque becomes {0, 1, 10, 2, 3, 4, 5}
```

```
deq.pop_front();
// deque becomes {1, 10, 2, 3, 4, 5}
```

```
deq.erase(deq.begin() + 1);
// deque becomes {1, 2, 3, 4, 5}
```

```
deq.erase(deq.begin() + 3, deq.end());
// deque becomes {1, 2, 3}
```

这种结构可以用于例如航班登机队列等场景。

这些容器之间唯一的不同在于性能和内存需求。deque 在头部和尾部进行插入和删除时都能提供非常好的性能。在中间进行插入和删除的平均速度也比 vector 稍快，尽管渐进复杂度与 vector 相同。

除此之外，deque 也像 vector 一样允许我们使用自定义分配器（custom allocator）。我们可以在初始化时将其作为第二个模板参数指定。这里需要注意的一点是，分配器是类型的一部分，而不是对象的一部分。这意味着我们无法比较两个使用不同类型分配器的 deque 对象或 vector 对象。同样，我们也不能对具有不同类型分配器的对象执行其他操作，例如赋值或复制构造。

正如我们所见，`std::deque` 的结构比之前研究的其他容器略微复杂。事实上，它是唯一一个在提供高效的随机访问的同时，还具备快速的 **push_front** 和 **push_back** 函数的容器。正如我们将在下一节中看到的，deque 常被用作其他容器的底层容器。

## 容器适配器

到目前为止，我们所看到的容器都是从零开始构建的。在本节中，我们将了解构建在其他容器之上的容器。在现有容器之上提供包装器有多种原因，例如为代码提供更多语义意义、防止某人因为某些函数可用就偶然使用了并非预期的函数，以及提供特定的接口。

一个这样特定的用例是 **栈（stack）** 数据结构。栈遵循 **LIFO（后进先出）** 结构来访问和处理数据。在函数方面，它只能在容器的一端进行插入和删除，并且不能更新甚至访问除变更端以外的任何元素。这一端被称为栈顶。我们也可以轻松使用任何其他容器，例如 vector 或 deque，因为它们默认就能满足这些要求。然而，这样做存在一些根本性问题。

以下示例展示了栈的两种实现：

```
std::deque<int> stk;
```

```
stk.push_back(1);  // Pushes 1 on the stack = {1}
```

```
stk.push_back(2);  // Pushes 2 on the stack = {1, 2}
```



## 列表、栈和队列

```cpp
stk.pop_back();    // Pops the top element off the stack = {1}
stk.push_front(0); // This operation should not be allowed for a stack
```

```cpp
std::stack<int> stk;
stk.push(1);        // Pushes 1 on the stack = {1}
stk.push(2);        // Pushes 2 on the stack = {1, 2}
stk.pop();          // Pops the top element off the stack = {1}
stk.push_front(0); // Compilation error
```

正如这个例子中所看到的，第一种使用 deque 的栈版本仅通过变量名提供了语义含义。操作数据的函数仍然无法阻止程序员添加不应该允许的代码，例如 `push_front`。此外，`push_back` 和 `pop_back` 函数暴露了不必要的细节，而这些细节对于栈来说本应是默认已知的。

与此相比，如果我们来看第二种版本，它在表明其用途方面看起来更加准确。最重要的是，它不允许任何人做任何非预期操作，即使是无意中。

栈的第二种版本不过是对之前容器 deque 的包装，通过提供一个良好且受限的接口给用户。这被称为容器适配器。C++ 提供了三种容器适配器：`std::stack`、`std::queue` 和 `std::priority_queue`。让我们现在简要地看一下它们各自的情况。

### std::stack

如前所述，适配器只是重用其他容器，例如 deque、vector 或任何其他容器。`std::stack` 默认使用 `std::deque` 作为其底层容器。它提供了一个仅与栈相关的接口 – `empty`、`size`、`top`、`push`、`pop` 和 `emplace`。这里，`push` 只是为底层容器调用 `push_back` 函数，而 `pop` 只是调用 `pop_back` 函数。`top` 从底层容器调用 `back` 函数来获取最后一个元素，即栈顶。因此，它将用户操作限制为后进先出，因为它只允许我们在底层容器的某一端更新值。

这里，我们使用 deque 作为底层容器，而不是 vector。原因在于 deque 在重新分配时不需要移动所有元素，而 vector 需要。因此，与 vector 相比，使用 deque 更高效。然而，如果在某些情况下其他容器更有可能带来更好的性能，栈提供了将容器作为模板参数提供的功能。因此，我们也可以使用 vector 或 list 构建栈，如下所示：

```cpp
std::stack<int, std::vector<int>> stk;
```

```cpp
std::stack<int, std::list<int>> stk;
```

栈的所有操作的时间复杂度均为 O(1)。通常转发到底层容器的调用没有开销，因为编译器在优化时可以将所有内容内联。

### std::queue

就像 `std::stack` 一样，我们还有另一个容器适配器来处理许多应用程序中常见的 FIFO（先进先出）场景，这个结构由一个名为 `std::queue` 的适配器提供。它几乎有一组与栈相同的函数，但含义和行为不同，以便遵循 FIFO 而不是 LIFO。对于 `std::queue`，`push` 意味着 `push_back`，就像栈一样，但 `pop` 是 `pop_front`。由于队列应该在读取时暴露两端，因此它具有 `front` 和 `back` 函数。

以下是 `std::queue` 使用的一个小例子：

```cpp
std::queue<int> q;
q.push(1);  // queue becomes {1}
q.push(2);  // queue becomes {1, 2}
q.push(3);  // queue becomes {1, 2, 3}
q.pop();    // queue becomes {2, 3}
q.push(4);  // queue becomes {2, 3, 4}
```

如本例所示，首先，我们按顺序插入 1、2 和 3。之后，我们从队列中弹出一个元素。由于 1 最先被推入，因此它最先从队列中移除。然后，下一次推入将 4 插入到队列的末尾。

`std::queue` 也出于与栈相同的原因使用 `std::deque` 作为底层容器，并且这里显示的所有方法的时间复杂度也为 O(1)。

### std::priority_queue

优先队列通过其接口提供了一种非常有用的结构，称为堆。堆数据结构以快速访问容器中的最小（或最大）元素而闻名。获取最小/最大元素的操作时间复杂度为 O(1)。插入的时间复杂度为 O(log n)，而删除只能对最小/最大元素执行，该元素始终位于顶部。

这里需要注意的一点是，我们只能快速获得最小或最大函数中的一个，而不能同时获得两者。这由提供给容器的比较器决定。与栈和队列不同，优先队列默认基于 vector，但如果需要我们可以更改它。此外，默认情况下比较器为 `std::less`。由于这是一个堆，因此生成的容器是最大堆。这意味着默认情况下最大元素位于顶部。

这里，由于插入需要确保我们可以立即访问顶部元素（最小或最大取决于比较器），它并不是简单地将调用转发给底层容器。相反，它通过使用比较器按需将元素冒泡到顶部来实现堆化算法。此操作所需的时间与容器大小成对数比例，因此时间复杂度为 O(log n)。在用多个元素初始化时也需要维护不变量。这里，`priority_queue` 构造函数不会为每个元素简单地调用插入函数；相反，它应用不同的堆化算法以 O(n) 的速度更快地完成。

### 适配器的迭代器

到目前为止我们看到的所有适配器都仅按需要公开功能以满足其语义含义。从逻辑上讲，遍历栈、队列和优先队列是没有意义的。在任何时候，我们都只能看到前端元素。因此，STL 不为其提供迭代器。

### 基准测试

正如我们所见，不同的容器各有利弊，没有一种容器是适合所有情况的完美选择。有时，多个容器在给定场景下的平均性能可能相似。在这种情况下，基准测试就是我们的朋友。这是一种基于统计数据确定更好方法的过程。

考虑一个场景，我们希望将数据存储在连续内存中，访问它并使用各种函数对其进行操作。我们可以说我们应该使用 `std::vector` 或 `std::deque`。但我们不确定哪一个会更好。乍一看，它们似乎都对这种情况有良好的性能。在不同的操作中，例如访问、插入、`push_back` 和修改特定元素，有些有利于 `std::vector`，有些有利于 `std::deque`。那么，我们应该如何进行？

思路是创建一个实际模型的原型，并使用 `std::vector` 和 `std::deque` 两者实现它。然后，测量两者在原型上的性能。基于性能测试的结果，我们可以选择总体表现更好的那个。

最简单的方法是测量对两者执行不同操作所需的时间并进行比较。然而，由于存在其他因素，例如操作系统调度、缓存和中断等，相同的操作在不同运行中可能需要不同的时间。这些参数可能导致我们的结果出现很大偏差，因为执行任何操作一次只是几百纳秒的事。为了克服这一点，我们可以多次执行操作（我们指的是数百万次），直到两次测量之间出现可观的时间差异。

我们可以使用一些基准测试工具，例如 [quick-bench.com](https://quick-bench.com)，它为我们提供了一种轻松运行基准测试的方法。你可以尝试在 vector 和 deque 上运行前面提到的操作，以快速比较性能差异。

### 活动 3：模拟办公室共享打印机队列

在此活动中，我们将模拟办公室共享打印机的队列。在任何企业办公室，通常打印机在整层楼的打印机房内共享。房间里的所有电脑都连接到同一台打印机。但打印机在任何时候只能执行一个打印任务，并且完成任何任务也需要一些时间。在此期间，其他用户可能会发送另一个打印请求。在这种情况下，打印机需要将所有待处理作业存储在某个地方，以便在当前任务完成后处理它们。

执行以下步骤来解决此活动：

1. 创建一个名为 `Job` 的类（包含作业的 ID、提交作业的用户姓名以及页数）。
2. 创建一个名为 `Printer` 的类。它将提供添加新作业和处理迄今为止添加的所有作业的接口。




3. 要实现**打印机**类，需要存储所有待处理的任务。我们将实现一个非常基本的策略——先到先服务。谁先提交任务，谁就最先完成。

4. 最后，模拟一个场景：多人向打印机添加任务，打印机逐一处理这些任务。

> **注意**
> 本活动的解决方案见第487页。

### 总结

在本章中，我们学习了如何通过选择存储数据的方式来根据需求设计应用程序。我们解释了可以对数据执行的不同类型的操作，这些操作可以根据操作频率作为多种数据结构之间比较的参数。我们了解到容器适配器提供了一种非常有用的方式，可以在代码中表明我们的意图。我们看到，使用以适配器形式提供的更具限制性的容器，而不是使用提供更多功能的主容器，在可维护性方面更有效，并且还能减少人为错误。我们详细解释了各种数据结构——**std::array**、**std::vector**、**std::list** 和 **std::forward_list**——它们在任何应用程序开发过程中都非常常见，并介绍了C++默认提供的接口。这有助于我们编写高效的代码，而无需重新发明整个流程，并大大加快开发过程。

在本章中，我们看到的所有结构在逻辑上都是线性的，也就是说，我们可以从任何元素向前或向后移动。在下一章中，我们将探讨使用这些结构难以解决的问题，并实现新型结构来解决这些问题。
