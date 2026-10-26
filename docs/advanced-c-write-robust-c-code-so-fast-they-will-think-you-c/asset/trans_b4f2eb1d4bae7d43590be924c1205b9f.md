

10. 在编辑器中打开 **Exercise4.cpp** 并将文件顶部附近的那一行更改为如下所示：

```cpp
#define EXERCISE4_STEP 12
```

11. 单击 **Run** 按钮编译并运行程序。生成以下输出：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_270_0.png)

图 3.32：练习 4 的第 12 步注释输出

12. 将输出与 **testSensors()** 方法中的代码进行比较。我们会发现我们可以轻松地将一个空的 **unique_ptr**（**light**）赋值，并且可以将一个 **shared_ptr** 赋值给另一个（**light3 = light2**），在任何一种情况下都不需要使用 **std::move()**。

13. 在编辑器中打开 **Exercise4.cpp** 并将文件顶部附近的那一行更改为如下所示：

```cpp
#define EXERCISE4_STEP 15
```

14. 单击 **Run** 按钮编译并运行程序。输出切换为以下内容：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_271_0.png)

图 3.33：unique_ptr 中数组的管理

15. 打开编辑器并找到 `testArrays()` 方法：

```cpp
void testArrays()
{
    std::unique_ptr<int[]> board = std::make_unique<int[]>(8 * 8);

    for (int i = 0; i < 8; i++)
        for (int j = 0; j < 8; j++)
            board[i * 8 + j] = 10 * (i + 1) + j + 1;

    for (int i = 0; i < 8; i++)
    {
        char sep{' '};
        for (int j = 0; j < 8; j++)
            std::cout << board[i * 8 + j] << sep;
        std::cout << "\n";
    }
}
```

这段代码中有几件事需要注意。首先，类型声明为 `int[]`。我们为本次练习选择了 `int`，但它可以是几乎任何类型。其次，当 **unique_ptr**（以及从 C++17 开始的 **shared_ptr**）用于管理数组时，**operator[]** 被定义。因此，我们通过从二维索引计算一维索引来模拟二维数组：**board[i * 8 + j]**。

16. 编辑该方法的第一行并声明 **auto** 类型：

```cpp
auto board = std::make_unique<int[]>(8 * 8);
```

17. 单击 **Run** 按钮编译并运行程序——输出将与之前的运行结果相同。这是一个 `auto` 非常有帮助的案例，因为你不再需要在类型声明中输入所有细节，也不需要在 `make_unique()` 调用中重复。

在本练习中，我们实现了一个工厂函数，使用 `unique_ptr` 来管理传感器的生命周期。我们随后实现了代码，将其从 `unique_ptr` 更改为共享给多个对象。最后，我们开发了一种 `unique_ptr` 技术，使用一维数组来管理多维数组。

## Rule of Zero/Five – A Different Perspective

当我们引入 **BufferedWriter** 时，它管理着两种资源：内存和文件。我们随后讨论了默认编译器生成的拷贝操作是所谓的浅拷贝。我们谈到了如何以不同的方式管理资源——停止拷贝、执行深拷贝或转移所有权。我们在这些情况下所做的决定被称为资源管理策略。你选择的策略会影响你执行 **Rule of Zero/Five** 的方式。

就资源管理而言，一个类可以不管理任何资源、管理可以拷贝但不能移动的资源、管理可以移动但不能拷贝的资源，或管理不应被拷贝也不应被移动的资源。以下类展示了这些可能的表现形式：

```cpp
struct NoResourceToManage
{
    // use compiler-generated copy & move constructors and operators
};
```

```cpp
struct CopyOnlyResource
{
    ~CopyOnlyResource()                                   {/* defined */ }
    CopyOnlyResource(const CopyOnlyResource& rhs)          {/* defined */ }
    CopyOnlyResource& operator=(const CopyOnlyResource& rhs) {/* defined */ }
    CopyOnlyResource(CopyOnlyResource&& rhs) = delete;
    CopyOnlyResource& operator=(CopyOnlyResource&& rhs) = delete;
```

## Activity 1: Implementing Graphics Processing with RAII and Move

在第 2A 章 No Ducks Allowed – Types and Deduction 中，你的团队努力工作并完成了 Point3d 和 Matrix3d 的实现。现在，你的公司希望推广该库，在推广之前需要进行两项重大改进：

- 类必须位于我们公司的命名空间中，即 Advanced C Plus Plus Inc。因此，图形的命名空间为 `accp::gfx`。
- Point3d 和 Matrix3d 中矩阵的存储是类的内在组成部分，因此它是从栈分配而不是堆分配。随着库矩阵支持的演进，我们需要从堆分配内存。随着我们为未来版本实现更大矩阵的目标，我们还希望在我们的类中引入移动语义。

按照以下步骤实现此目标：

1. 以我们当前版本的库为起点（可在 Lesson3/Activity01 文件夹中找到），将我们所有的类都放入 `accp::gfx` 命名空间中。
2. 修复因更改而导致的所有失败的测试。（失败可能意味着编译失败，而不仅仅是测试运行失败。）
3. 在 Matrix3d 中，将矩阵直接在类中声明的方式改为堆分配内存用于存储。
4. 通过实现拷贝构造函数的深拷贝实现和拷贝赋值运算符来修复失败的测试。进行必要的其他更改以适应新的内部表示。注意，你不需要修改任何测试即可使它们通过；它们只访问公共接口，这意味着我们可以更改内部结构而不影响客户端。
5. 通过在返回语句中使用 `std::move` 强制在 `CreateTranslationMatrix()` 中调用移动构造函数来触发另一个失败。在 Matrix3d 类中引入所需的移动操作以使测试编译并通过。
6. 对 Point3d 重复步骤 3 到 4。

实现上述步骤后，预期的输出应与开始时保持不变：

```text
------ Activity 1 ------
The point [ 1, 1, 1, 1 ]
...rotated around X by 90 degrees
...rotated around Y by 90 degrees
...rotated around Z by 90 degrees
moves to [ 1, 1, -1, 1 ]
...scaled by x=2, y = 3, z = 4
moves to [ 2, 3, -4, 1 ]
...translated by dx=-2, dy = 1, dz = -3
moves to [ 0, 4, -7, 1 ]

Complete.
```

图 3.34：成功转换为使用 RAII 后的活动 1 输出

> **Note**
>
> 本活动的解决方案可在第 657 页找到。

## When Is a Function Called?

C++ 程序执行的所有操作本质上都是函数调用（尽管编译器可能会将它们优化为内联的操作序列）。然而，由于 **syntactic sugar** 的存在，可能并不明显你正在进行函数调用。Syntactic sugar 是编程语言中的语法，使其更易于阅读或表达。例如，当你写 **a = 2 + 5** 时，你本质上是在调用 **operator=(&a, operator+(2, 5))**。只是语言允许我们写第一种形式，但第二种形式允许我们重载运算符并将这些功能扩展到用户定义的类型。

以下机制会导致函数调用：

- 显式调用函数。
- 所有运算符，如 +, -, *, /, % 等，以及 `new`/`delete`。
- 变量声明——如果存在初始化值，将导致使用参数调用构造函数。
- 用户定义字面量——我们尚未处理这些，但本质上，我们为类型 `operator "" name(argument)` 定义重载。然后我们可以写诸如 `10_km`，这使我们的代码更易于理解，因为它携带了语义信息。
- 从一种值到另一种值的转换（`static_cast<>`、`const_cast<>`、`reinterpret_cast<>` 和 `dynamic_cast<>`）。同样，我们有另一个运算符重载，允许我们将一种类型转换为另一种类型。
- 在函数重载期间，可能需要将一种类型转换为另一种类型以使其匹配函数原型。它可以通过使用正确参数类型的构造函数创建临时对象来实现，也可以通过隐式调用的转换运算符来实现。

每一个都会导致编译器确定必须调用一个函数。确定需要调用函数后，它必须找到与名称和参数匹配的函数。这就是我们将在下一节中讨论的内容。

## Which Function to Call

在第 2A 章 No Ducks Allowed – Types and Deduction 中，我们看到函数重载解析按如下方式执行：

![](img/6507d435e2edfa3c00c220d0ccfcaf94_276_0.png)

