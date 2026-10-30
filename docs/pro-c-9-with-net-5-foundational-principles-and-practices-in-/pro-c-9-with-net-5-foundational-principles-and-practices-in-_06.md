## 第 4 章

## 核心 C# 编程构造，第 2 部分

本章在第 3 章结束的地方继续，并完成了你对 C# 编程语言核心方面的调查。你将首先调查使用 C# 语法操作数组的细节，并熟悉相关的 System.Array 类类型中包含的功能。

接下来，你将检查 C# 方法构造的各个细节，探索 out、ref 和 params 关键字。在此过程中，你还将检查可选参数和命名参数的角色。我以方法重载的介绍来结束对方法的讨论。

接下来，本章讨论枚举类型和结构类型的构造，包括对值类型和引用类型之间区别的详细检查。本章通过检查可空数据类型的角色和相关的运算符来收尾。

完成本章后，你将处于完美的位置开始学习 C# 的面向对象功能，从第 5 章开始。

## 了解 C# 数组

正如我猜你已经知道的那样，数组是一组使用数字索引访问的数据项。更具体地说，数组是同一类型的一组连续数据点（int 的数组、string 的数组、SportsCars 的数组等）。使用 C# 声明、填充和访问数组都非常简单。为了说明这一点，创建一个名为 FunWithArrays 的新控制台应用程序项目，其中包含一个名为 SimpleArrays() 的辅助方法；如下所示：

```
Console.WriteLine("***** Fun with Arrays *****");
SimpleArrays();
Console.ReadLine();
```

```
static void SimpleArrays()
{
    Console.WriteLine("=> Simple Array Creation.");
    // Create and fill an array of 3 integers
    int[] myInts = new int[3];
    // Create a 100 item string array, indexed 0 - 99
    string[] booksOnDotNet = new string[100];
    Console.WriteLine();
}
```

仔细查看之前的代码注释。当使用此语法声明 C# 数组时，数组声明中使用的数字表示项目总数，而不是上界。还请注意，数组的下界始终从 0 开始。因此，当你写 int[] myInts = new int[3] 时，你最终得到一个包含三个元素的数组，索引位置为 0、1 和 2。

在定义数组变量后，你就可以按索引逐个填充元素，如下所示更新的 SimpleArrays() 方法所示：

```
static void SimpleArrays()
{
    Console.WriteLine("=> Simple Array Creation.");
    // Create and fill an array of 3 Integers
    int[] myInts = new int[3];
    myInts[0] = 100;
    myInts[1] = 200;
    myInts[2] = 300;

    // Now print each value.
    foreach(int i in myInts)
    {
        Console.WriteLine(i);
    }
    Console.WriteLine();
}
```

注意 请注意，如果你声明了一个数组但没有显式填充每个索引，每个项目将被设置为数据类型的默认值（例如，bool 的数组将被设置为 false 或 int 的数组将被设置为 0）。

## 查看 C# 数组初始化语法

除了逐个填充数组元素外，你还可以使用 C# 数组初始化语法填充数组的项目。为此，请在花括号（{}）范围内指定每个数组项目。当你创建已知大小的数组并希望快速指定初始值时，此语法会很有帮助。例如，考虑以下替代数组声明：

```
static void ArrayInitialization()
{
    Console.WriteLine("=> Array Initialization.");

    // Array initialization syntax using the new keyword.
    string[] stringArray = new string[]
    { "one", "two", "three" };
    Console.WriteLine("stringArray has {0} elements", stringArray.Length);

    // Array initialization syntax without using the new keyword.
    bool[] boolArray = { false, false, true };
    Console.WriteLine("boolArray has {0} elements", boolArray.Length);

    // Array initialization with new keyword and size.
    int[] intArray = new int[4] { 20, 22, 23, 0 };
}
```


Console.WriteLine("intArray has {0} elements", intArray.Length);
Console.WriteLine();
}

请注意，当你使用这种“大括号”语法时，你不需要指定数组的大小（在构造 stringArray 变量时可以看到），因为它会根据大括号范围内项目的数量自动推断出来。另请注意，使用 new 关键字是可选的（在构造 boolArray 类型时已显示）。

对于 intArray 声明，再次回想一下，指定的数值表示数组中的元素数量，而不是上界的值。如果声明的大小与初始化器的数量不匹配（无论是初始化器太多还是太少），你会收到编译时错误。以下是一个示例：

// OOPS! Mismatch of size and elements!
int[] intArray = new int[2] { 20, 22, 23, 0 };

## 理解隐式类型局部数组

在第 3 章中，你学习了隐式类型局部变量的主题。请回想一下，var 关键字允许你定义一个变量，其底层类型由编译器确定。类似地，var 关键字可用于定义隐式类型局部数组。使用这种技术，你可以在不指定数组本身所包含的类型的情况下分配新的数组变量（请注意，使用此方法时必须使用 new 关键字）。

```
static void DeclareImplicitArrays()
{
    Console.WriteLine("=> Implicit Array Initialization.");

    // a is really int[].
    var a = new[] { 1, 10, 100, 1000 };
    Console.WriteLine("a is a: {0}", a.ToString());

    // b is really double[].
    var b = new[] { 1, 1.5, 2, 2.5 };
    Console.WriteLine("b is a: {0}", b.ToString());

    // c is really string[].
    var c = new[] { "hello", null, "world" };
    Console.WriteLine("c is a: {0}", c.ToString());
    Console.WriteLine();
}
```

当然，就像你使用显式 C# 语法分配数组一样，数组初始化列表中的项目必须具有相同的底层类型（例如，全部为 int、全部为 string 或全部为 SortsCars）。与你可能期望的不同，隐式类型局部数组不会默认为 System.Object；因此，以下代码会生成编译时错误：

// Error! Mixed types!
var d = new[] { 1, "one", 2, "two", false };

## 定义对象数组

在大多数情况下，当你定义一个数组时，你会通过指定可以存在于数组变量中的项目的显式类型来完成。虽然这看起来很直接，但有一个值得注意的转折。正如你在第 6 章中将要了解的那样，System.Object 是 .NET Core 类型系统中每个类型（包括基本数据类型）的最终基类。鉴于此事实，如果你定义了一个 System.Object 数据类型的数组，其子项可以是任何东西。请考虑以下 ArrayOfObjects() 方法：

```
static void ArrayOfObjects()
{
    Console.WriteLine("=> Array of Objects.");

    // An array of objects can be anything at all.
    object[] myObjects = new object[4];
    myObjects[0] = 10;
    myObjects[1] = false;
    myObjects[2] = new DateTime(1969, 3, 24);
    myObjects[3] = "Form & Void";
    foreach (object obj in myObjects)
    {
        // Print the type and value for each item in array.
        Console.WriteLine("Type: {0}, Value: {1}", obj.GetType(), obj);
    }
    Console.WriteLine();
}
```

在这里，当你遍历 myObjects 的内容时，你使用 System.Object 的 GetType() 方法打印每个项目的底层类型，以及当前项目的值。在本书的这一阶段，不必过多详细讨论 System.Object.GetType()，只需理解此方法可用于获取项目的完全限定名称（第 17 章将详细讨论类型信息和反射服务）。以下输出显示了调用 ArrayOfObjects() 的结果：

```
=> Array of Objects.

Type: System.Int32, Value: 10
Type: System.Boolean, Value: False
Type: System.DateTime, Value: 3/24/1969 12:00:00 AM
Type: System.String, Value: Form & Void
```

## 使用多维数组

除了迄今为止看到的单维数组外，C# 还支持两种多维数组。第一种称为矩形数组，它只是一个多维数组，其中每一行的长度相同。要声明并填充多维矩形数组，请按如下方式进行：

```
static void RectMultidimensionalArray()
{
    Console.WriteLine("=> Rectangular multidimensional array.");
    // A rectangular MD array.
```

调用 RectMultidimensionalArray() 和 JaggedMultidimensionalArray() 方法的输出如下所示：

```
=> Rectangular multidimensional array:

0       0       0       0
0       1       2       3
0       2       4       6

=> Jagged multidimensional array:

0 0 0 0 0 0
0 0 0 0 0 0 0
0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
```

## 将数组用作参数或返回值

在创建数组后，你可以将其作为参数传递或作为成员返回值接收。例如，以下 PrintArray() 方法接收一个 int 数组并将每个成员打印到控制台，而 GetStringArray() 方法填充一个字符串数组并将其返回给调用者：

```
static void PrintArray(int[] myInts)
{
    for(int i = 0; i < myInts.Length; i++)
    {
        Console.WriteLine("Item {0} is {1}", i, myInts[i]);
    }
}
```

```
static string[] GetStringArray()
{
    string[] theStrings = {"Hello", "from", "GetStringArray"};
    return theStrings;
}
```

这些方法可以按预期调用。

```
static void PassAndReceiveArrays()
{
    Console.WriteLine("=> Arrays as params and return values.");
    // Pass array as parameter.
    int[] ages = {20, 22, 23, 0};
    PrintArray(ages);

    // Get array as return value.
    string[] strs = GetStringArray();
    foreach(string s in strs)
{
    Console.WriteLine(s);
}

Console.WriteLine();
}
```

到目前为止，你应该对定义、填充和检查 C# 数组变量内容的过程感到熟悉。为了完善图景，让我们现在来检查 System.Array 类的作用。

## 使用 System.Array 基类

你创建的每个数组都从 System.Array 类获取了大部分功能。使用这些通用成员，你可以使用一致的对象模型对数组进行操作。表 4-1 列出了其中一些更有趣的成员（请务必查看文档以获取完整详细信息）。

Table 4-1. Select Members of System.Array

| Member of Array Class | Meaning in Life |
|---|---|
| Clear() | 此静态方法将数组中的元素范围设置为空值（数字为 0，对象引用为 null，布尔值为 false）。 |
| CopyTo() | 此方法用于将元素从源数组复制到目标数组。 |
| Length | 此属性返回数组中的项目数。 |
| Rank | 此属性返回当前数组的维度数。 |
| Reverse() | 此静态方法反转一维数组的内容。 |
| Sort() | 此静态方法对内在类型的一维数组进行排序。如果数组中的元素实现了 IComparer 接口，你也可以对自定义类型进行排序（参见第 8 章和第 10 章）。 |

让我们看看这些成员的实际应用。以下辅助方法使用静态 Reverse() 和 Clear() 方法将字符串类型数组的信息输出到控制台：

```
static void SystemArrayFunctionality()
{
    Console.WriteLine("=> Working with System.Array.");
    // Initialize items at startup.
    string[] gothicBands = {"Tones on Tail", "Bauhaus", "Sisters of Mercy"};

    // Print out names in declared order.
    Console.WriteLine("=> Here is the array:");
    for (int i = 0; i < gothicBands.Length; i++)
    {
        // Print a name.
        Console.Write(gothicBands[i] + ", ");
    }
    Console.WriteLine("\n");
```


// 反转它们...
Array.Reverse(gothicBands);
Console.WriteLine("- > 反转后的数组");

// ... 然后打印它们。
for (int i = 0; i < gothicBands.Length; i++)
{
    // 打印一个名称。
    Console.Write(gothicBands[i] + ", ");
}
Console.WriteLine("\n");

// 清除除第一个成员外的所有内容。
Console.WriteLine("- > 清除了除一个之外的所有内容...");
Array.Clear(gothicBands, 1, 2);

for (int i = 0; i < gothicBands.Length; i++)
{
    // 打印一个名称。
    Console.Write(gothicBands[i] + ", ");
}
Console.WriteLine();
}

如果调用此方法，您将得到此处所示的输出：

```
=> Working with System.Array.

-> Here is the array:
Tones on Tail, Bauhaus, Sisters of Mercy,

-> The reversed array
Sisters of Mercy, Bauhaus, Tones on Tail,

-> Cleared out all but one...
Sisters of Mercy, , ,
```

请注意，System.Array 的许多成员被定义为静态成员，因此在类级别调用它们（例如，Array.Sort() 和 Array.Reverse() 方法）。这些方法会传入您想要处理的数组。System.Array 的其他成员（例如 Length 属性）在对象级别绑定；因此，您可以直接在数组上调用该成员。

## 使用索引和范围（新 8.0）

为了简化序列（包括数组）的工作，C# 8 引入了两种新类型和两个新运算符，用于在处理数组时使用：

- System.Index 表示序列中的一个索引。
- System.Range 表示索引的子范围。
- 索引从末尾运算符（^）指定索引相对于序列末尾。
- 范围运算符（..）指定其操作数的开始和结束。

> 注意 索引和范围可与数组、字符串、Span<T> 和 ReadOnlySpan<T> 一起使用。

正如您已经看到的，数组的索引从零（0）开始。序列的末尾是序列长度减 1。之前打印 gothicBands 数组的 for 循环可以更新为以下内容：

```
for (int i = 0; i < gothicBands.Length; i++)
{
    Index idx = i;
    // Print a name
    Console.Write(gothicBands[idx] + ", ");
}
```

索引从末尾运算符让您可以指定距离序列末尾多少个位置，从长度开始。请记住，序列中的最后一项比实际长度少一个，因此 ^0 会导致错误。以下代码以相反顺序打印数组：

```
for (int i = 1; i <= gothicBands.Length; i++)
{
    Index idx = ^i;
    // Print a name
    Console.Write(gothicBands[idx] + ", ");
}
```

范围运算符指定开始和结束索引，并允许访问列表中的子序列。范围的开始是包含的，范围的结束是排除的。例如，要取出数组的前两个成员，从 0（第一个成员）到 2（比所需索引位置多一个）创建范围。

```
foreach (var itm in gothicBands[0..2])
{
    // Print a name
    Console.Write(itm + ", ");
}
Console.WriteLine("\n");
```

范围也可以使用新的 Range 数据类型传递给序列，如下所示：

```
Range r = 0..2; //the end of the range is exclusive
foreach (var itm in gothicBands[r])
{
    // Print a name
    Console.Write(itm + ", ");
}
Console.WriteLine("\n");
```

范围可以使用整数或 Index 变量定义。以下代码将产生相同的结果：

```
Index idx1 = 0;
Index idx2 = 2;
Range r = idx1..idx2; //the end of the range is exclusive
foreach (var itm in gothicBands[r])
{
    // Print a name
    Console.Write(itm + ", ");
}
Console.WriteLine("\n");
```

如果省略范围的开头，则使用序列的开头。如果省略范围的结尾，则使用范围的长度。这不会导致错误，因为范围末尾的值是排除的。对于前面示例中的三个项目的数组，所有范围都表示相同的子集。

```
gothicBands[..]
gothicBands[0..^0]
gothicBands[0..3]
```

## 理解方法

让我们检查定义方法的细节。方法由访问修饰符和返回类型（或 void 表示无返回类型）定义，可能接受也可能不接受参数。返回值的调用者的方法通常称为函数，而不返回值的方法通常称为方法。

> 注意 方法（和类）的访问修饰符在第 5 章中介绍。方法参数在下一节中介绍。

在文本的这一点上，您的方法具有以下基本格式：

```
// Recall that static methods can be called directly
// without creating a class instance.
class Program
{
    // static returnType MethodName(parameter list) { /* Implementation */ }
    static int Add(int x, int y)
    {
        return x + y;
    }
}
```

正如您将在接下来的几章中看到的那样，方法可以在类、结构或（C# 8 中的新功能）接口的范围内实现。

## 理解表达式体成员

您已经了解了返回值的简单方法，例如 Add() 方法。C# 6 引入了表达式体成员，可缩短单行方法的语法。例如，Add() 可以使用以下语法重写：

```
static int Add(int x, int y) => x + y;
```

这通常被称为语法糖，意味着生成的 IL 没有任何不同。它只是另一种编写方法的方式。有些人认为它更容易阅读，有些人则不然，因此选择权在您（或您的团队）决定偏好哪种风格。

> 注意 不要对 => 运算符感到惊讶。这是 lambda 操作，将在第 12 章中详细介绍。该章节还解释了表达式体成员的工作原理。具体来说，现在只需将它们视为编写单行语句的快捷方式。

## 理解局部函数（新 7.0，更新 9.0）

C# 7.0 引入的一项功能是能够在方法内创建方法，官方称为局部函数。局部函数是在另一个函数内声明的函数，必须为私有，C# 8.0 起可以是静态的（见下一节），并且不支持重载。局部函数支持嵌套：局部函数内部可以声明局部函数。

要了解其工作原理，请创建一个名为 FunWithLocalFunctions 的新控制台应用程序项目。举例来说，假设您想扩展之前使用的 Add() 示例以包含输入验证。有很多方法可以实现这一点，一个简单的方法是直接在 Add() 方法中添加验证。让我们这样做并将前面的示例更新为以下内容（注释代表验证逻辑）：

```
static int Add(int x, int y)
{
    //Do some validation here
    return x + y;
}
```

正如您所见，没有大的变化。只有一条注释表示实际代码应该做一些事情。如果您想将方法实际原因（返回参数的总和）与参数验证分开呢？您可以创建其他方法并从 Add() 方法调用它们。但这需要为仅供另一个方法使用而创建另一个方法。也许这有点过头了。局部函数允许您先进行验证，然后将方法的真正目标封装在 AddWrapper() 方法内定义的 Add() 中，如下所示：

```
static int AddWrapper(int x, int y)
{
    //Do some validation here
    return Add();

    int Add()
    {
```

```
return x + y;
}
}
```

包含的 Add() 方法只能从包装的 AddWrapper() 方法调用。所以，我知道您在想的问题是：“这给我带来了什么好处？” 对于这个特定示例，答案很简单，几乎没有（如果有的话）。但如果 AddWrapper() 需要从多个位置执行 Add() 函数呢？现在您应该开始看到在不需要时不公开代码重用的局部函数的优势。当我们介绍自定义迭代器方法（第 8 章）和异步方法（第 15 章）时，您将看到局部函数带来的更多好处。

> 注意 AddWrapper() 局部函数是具有嵌套局部函数的局部函数的示例。请记住，在顶层语句中声明的函数被创建为局部函数。Add() 局部函数位于 AddWrapper() 局部函数中。这种功能通常不会在教学示例之外使用，但如果您需要嵌套局部函数，您知道 C# 支持它。

C# 9.0 更新了局部函数，允许为局部函数、其参数和类型参数添加属性，如以下示例所示（不必担心 NotNullWhen 属性，它将在本章后面介绍）：

```
#nullable enable
private static void Process(string?[] lines, string mark)
{
    foreach (var line in lines)
    {
        if (IsValid(line))
        {
            // Processing logic...
        }
    }

    bool IsValid([NotNullWhen(true)] string? line)
    {
        return !string.IsNullOrEmpty(line) && line.Length >= mark.Length;
    }
}
```

## 理解静态局部函数（新 8.0）

C# 8 引入的局部函数的改进是能够将局部函数声明为静态。在前面的示例中，局部 Add() 函数直接引用主函数中的变量。这可能导致意外的副作用，因为局部函数可以更改变量的值。

要亲眼看到这一点，请创建一个名为 AddWrapperWithSideEffect() 的新方法，如下所示：

```
static int AddWrapperWithSideEffect(int x, int y)
{
    //Do some validation here
    return Add();




```
int Add()
{
    x += 1;
    return x + y;
}
```

当然，这个例子非常简单，在实际代码中可能不会出现。为了防止这种类型的错误，请为局部函数添加 static 修饰符。这会阻止局部函数直接访问父方法变量，并导致编译器异常 CS8421，“A static local function cannot contain a reference to ‘<variable name>’。”

下面展示了前面方法的改进版本：

```
static int AddWrapperWithStatic(int x, int y)
{
    //Do some validation here
    return Add(x,y);

    static int Add(int x, int y)
    {
        return x + y;
    }
}
```

## 理解方法参数

方法参数用于将数据传递到方法调用中。在接下来的几个部分中，你将学习方法（及其调用方）如何处理参数的细节。

## 理解方法参数修饰符

参数传递到函数中的默认方式是按值传递。简单来说，如果你没有用参数修饰符标记一个参数，就会将数据的副本传递到函数中。如本章后面所述，具体复制什么将取决于参数是值类型还是引用类型。

虽然 C# 中方法的定义相当直接，但你可以使用一些方法来控制参数如何传递给方法，如表 4-2 所列。

表 4-2. C# 参数修饰符

| 参数修饰符 | 实际含义 |
|--------------------|-----------------|
| (无) | 如果值类型参数未标记修饰符，则假定按值传递，即被调用方法接收原始数据的副本。没有修饰符的引用类型按引用传递。 |
| out | 输出参数必须由被调用方法赋值，因此按引用传递。如果被调用方法未能为输出参数赋值，你会收到编译器错误。 |

(续)

## 表 4-2.（续）

| 参数修饰符 | 实际含义 |
|---|---|
| ref | 值最初由调用方赋值，并且可以由被调用方法选择性修改（因为数据也是按引用传递的）。如果被调用方法未能为 ref 参数赋值，不会生成编译器错误。 |
| in | C# 7.2 中新增，in 修饰符表示 ref 参数对被调用方法只读。 |
| params | 此参数修饰符允许你将可变数量的参数作为单个逻辑参数发送。方法只能有一个 params 修饰符，并且它必须是方法的最后一个参数。你可能并不需要经常使用 params 修饰符；然而，请注意，基类库中的许多方法确实使用了此 C# 语言特性。 |

为了说明这些关键字的用法，创建一个名为 FunWithMethods 的新控制台应用程序项目。现在，让我们逐步了解每个关键字的角色。

## 理解默认参数传递行为

当参数没有修饰符时，值类型的行为是按值传递参数，而引用类型的行为是按引用传递参数。

> 注 值类型和引用类型将在本章后面介绍。

### 值类型的默认行为

值类型参数传递到函数中的默认方式是按值传递。简单来说，如果你没有用修饰符标记参数，就会将数据的副本传递到函数中。将以下方法添加到 Program 类中，该方法按值操作两个数值数据类型：

```
// Value type arguments are passed by value by default.
static int Add(int x, int y)
{
    int ans = x + y;
    // Caller will not see these changes
    // as you are modifying a copy of the
    // original data.
    x = 10000;
    y = 88888;
    return ans;
}
```

数值数据属于值类型类别。因此，如果你在成员的作用域内更改参数的值，调用方对此一无所知，因为你是在调用方原始数据的副本上更改值。

```
Console.WriteLine("***** Fun with Methods *****
");
// Pass two variables in by value.
```

```
int x = 9, y = 10;
Console.WriteLine("Before call: X: {0}, Y: {1}", x, y);
Console.WriteLine("Answer is: {0}", Add(x, y));
Console.WriteLine("After call: X: {0}, Y: {1}", x, y);
Console.ReadLine();
```

正如你所期望的，x 和 y 的值在调用 Add() 之前和之后保持相同，如以下输出所示，因为数据点是按值传递的。因此，在 Add() 方法中对这些参数所做的任何更改都不会被调用方看到，因为 Add() 方法操作的是数据的副本。

```
**** Fun with Methods ****

Before call: X: 9, Y: 10
Answer is: 19
After call: X: 9, Y: 10
```

### 引用类型的默认行为

引用类型参数传递到函数中的默认方式是对其属性按引用传递，但对自身按值传递。这将在本章后面详细讨论，在讨论值类型和引用类型之后。

> 注 尽管 string 数据类型在技术上是引用类型，如第 3 章所述，但它是一个特例。当字符串参数没有修饰符时，它是按值传递的。

## 使用 out 修饰符（更新 7.0）

接下来，你可以使用输出参数。已定义为接受输出参数（通过 out 关键字）的方法有义务在退出方法作用域之前将它们赋值为适当的值（如果你未能这样做，你将收到编译器错误）。

为了说明，这里是 Add() 方法的替代版本，它使用 C# out 修饰符返回两个整数的和（注意此方法的物理返回值现在是 void）：

```
// Output parameters must be assigned by the called method.
static void AddUsingOutParam(int x, int y, out int ans)
{
    ans = x + y;
}
```

调用具有输出参数的方法也需要使用 out 修饰符。然而，作为输出变量传递的局部变量在作为输出参数传递之前不需要被赋值（如果你这样做，原始值会在调用后丢失）。编译器允许你发送看似未赋值的數據的原因是被调用的方法必须进行赋值。要调用更新后的 Add 方法，创建一个 int 类型的变量，并在调用中使用 out 修饰符，像这样：

```
int ans;
AddUsingOutParam(90, 90, out ans);
```

从 C# 7.0 开始，out 参数不需要在使用前声明。换句话说，它们可以在方法调用中声明，像这样：

```
AddUsingOutParam(90, 90, out int ans);
```

以下是调用方法时内联声明 out 参数的示例：

```
Console.WriteLine("***** Fun with Methods *****");

// No need to assign initial value to local variables
// used as output parameters, provided the first time
// you use them is as output arguments.
// C# 7 allows for out parameters to be declared in the method call
AddUsingOutParam(90, 90, out int ans);
Console.WriteLine("90 + 90 = {0}", ans);
Console.ReadLine();
```

前面的示例旨在具有说明性；你真的没有理由使用输出参数返回求和值。然而，C# out 修饰符确实有其用途：它允许调用方从单次方法调用中获得多个输出。

```
// Returning multiple output parameters.
static void FillTheseValues(out int , out string b, out bool c)
{
    a = 9;
    b = "Enjoy your string.";
    c = true;
}
```

调用方将能够调用 FillTheseValues() 方法。请记住，在调用方法时以及在实现方法时都必须使用 out 修饰符。

```
Console.WriteLine("***** Fun with Methods *****");
FillTheseValues(out int i, out string str, out bool b);

Console.WriteLine("Int is: {0}", i);
Console.WriteLine("String is: {0}", str);
Console.WriteLine("Boolean is: {0}", b);
Console.ReadLine();
```

注 C# 7 还引入了元组，这是在方法调用中返回多个值的另一种方式。你将在本章后面了解更多关于它的信息。

请始终记住，定义了输出参数的方法必须在退出方法作用域之前将参数赋值为有效值。因此，以下代码将导致编译器错误，因为输出参数在方法作用域内未被赋值：

```
static void ThisWontCompile(out int a)
{
    Console.WriteLine("Error! Forgot to assign output arg!");
}
```




## 丢弃输出参数（新增 7.0）

如果你不关心输出参数的值，可以使用 discard 作为占位符。Discard 是有意未使用的临时、虚拟变量。它们未被赋值，没有值，甚至可能根本不分配任何内存。这可以提供性能优势，同时使代码更易读。Discard 可用于输出参数、元组（本章后面介绍）、模式匹配（第 6 章和第 8 章），或甚至作为独立变量。

例如，如果你想获取前一个示例中 int 的值，但不关心后面两个参数，可以编写以下代码：

```
//This only gets the value for a, and ignores the other two parameters
FillTheseValues(out int a, out _, out _);
```

请注意，被调用的方法仍在执行为所有三个参数设置值的工作；只是最后两个参数在方法调用返回时被丢弃。

## 构造函数和初始化器中的 out 修饰符（新增 7.3）

C# 7.3 扩展了使用 out 参数的允许位置。除了方法之外，构造函数、字段和属性初始化器以及查询子句的参数也可以用 out 修饰符进行修饰。这些示例将在本书后面进行检查。

## 使用 ref 修饰符

现在考虑 C# ref 参数修饰符的使用。引用参数在你希望允许方法对调用方作用域中声明的各种数据点进行操作（并且通常更改其值）时是必要的（例如排序或交换例程）。请注意输出参数和引用参数之间的区别：

- 输出参数在传递给方法之前不需要初始化。
  原因在于方法必须在退出前为输出参数赋值。
- 引用参数在传递给方法之前必须初始化。
  原因在于你传递的是对现有变量的引用。如果你不将其赋值为初始值，那就相当于对未赋值的局部变量进行操作。

让我们通过一个交换两个字符串变量的方法来查看 ref 关键字的用法（当然，这里可以使用任何两种数据类型，包括 int、bool、float 等。）。

```
// Reference parameters.
public static void SwapStrings(ref string s1, ref string s2)
{
    string tempStr = s1;
    s1 = s2;
    s2 = tempStr;
}
```

此方法可以按如下方式调用：

```
Console.WriteLine("***** Fun with Methods *****");

string str1 = "Flip";
string str2 = "Flop";
```

```
Console.WriteLine("Before: {0}, {1} ", str1, str2);
SwapStrings(ref str1, ref str2);
Console.WriteLine("After: {0}, {1} ", str1, str2);
Console.ReadLine();
```

这里，调用方已为局部字符串数据（str1 和 str2）分配了初始值。调用 SwapStrings() 返回后，str1 现在包含值 "Flop"，而 str2 报告值为 "Flip"。

```
Before: Flip, Flop

After: Flop, Flip
```

## 使用 in 修饰符（新增 7.2）

in 修饰符按引用传递值（对于值类型和引用类型），并防止被调用方法修改值。这在代码中明确表达了设计意图，同时可能减少内存压力。当值类型按值传递时，被调用方法会（内部）复制它们。如果对象很大（例如大型结构体），为本地使用制作副本的额外开销可能很显著。此外，即使引用类型在没有修饰符的情况下传递，也可能被被调用方法修改。这两个问题都可以使用 in 修饰符解决。

重新审视之前的 Add() 方法，有两行代码修改了参数，但不影响调用方法的值。值不受影响，因为 Add() 方法对变量 x 和 y 制作了副本以供本地使用。虽然调用方法没有不良副作用，但如果将 Add() 方法更改为以下代码会怎样？

```
static int Add2(int x,int y)
{
    x = 10000;
    y = 88888;
    int ans = x + y;
    return ans;
}
```

运行此代码后，无论传入的方法数字如何，都会返回 98888。这显然是个问题。为纠正此问题，将方法更新为以下内容：

```
static int AddReadOnly(in int x,in int y)
{
    //Error CS8331 Cannot assign to variable 'in int' because it is a readonly variable
    //x = 10000;
    //y = 88888;
    int ans = x + y;
    return ans;
}
```

当代码尝试更改参数的值时，编译器会引发 CS8331 错误，指出由于 in 修饰符，值无法被修改。

## 使用 params 修饰符

C# 支持使用 params 关键字使用参数数组。params 关键字允许你将任意数量的相同类型参数（或由继承相关的类）作为单个逻辑参数传递给方法。此外，用 params 关键字标记的参数如果调用方发送强类型数组或逗号分隔的项目列表，也可以被处理。是的，这可能令人困惑！为了澄清，假设你想创建一个函数，允许调用方传入任意数量的参数并返回计算出的平均值。

如果你将此方法原型化为接受 double 数组，这将迫使调用方首先定义数组，然后填充数组，最后将其传递给方法。然而，如果你将 CalculateAverage() 定义为接受 params double[] 数据类型，调用方只需传递逗号分隔的 double 列表即可。double 列表将在幕后打包成 double 数组。

```
// Return average of "some number" of doubles.
static double CalculateAverage(params double[] values)
{
    Console.WriteLine("You sent me {0} doubles.", values.Length);

    double sum = 0;
    if(values.Length == 0)
    {
        return sum;
    }
    for (int i = 0; i < values.Length; i++)
    {
        sum += values[i];
    }
    return (sum / values.Length);
}
```

此方法已定义为接受 double 的参数数组。这实际上意味着该方法说的是“给我任意数量的 double（包括零），我会计算平均值。”在此基础上，你可以以以下任何方式调用 CalculateAverage()：

```
Console.WriteLine("***** Fun with Methods *****");
```

```
// Pass in a comma-delimited list of doubles...
double average;
average = CalculateAverage(4.0, 3.2, 5.7, 64.22, 87.2);
Console.WriteLine("Average of data is: {0}", average);
```

```
// ...or pass an array of doubles.
double[] data = { 4.0, 3.2, 5.7 };
average = CalculateAverage(data);
Console.WriteLine("Average of data is: {0}", average);
```

```
// Average of 0 is 0!
Console.WriteLine("Average of data is: {0}", CalculateAverage());
Console.ReadLine();
```

如果你在 CalculateAverage() 的定义中未使用 params 修饰符，此方法的第一次调用将导致编译器错误，因为编译器将寻找一个接受五个 double 参数的 CalculateAverage() 版本。

注意 为了避免任何歧义，C# 要求方法仅支持单个 params 参数，该参数必须是参数列表中的最后一个参数。

正如你可能猜到的，这种技术不过是对调用方的便利，因为数组由 .NET Core 运行时根据需要创建。到了数组进入被调用方法的作用域时，你可以将其视为包含 System.Array 基类库类型所有功能的完整 .NET Core 数组。考虑以下输出：

```
You sent me 5 doubles.

Average of data is: 32.864
You sent me 3 doubles.
Average of data is: 4.3
You sent me 0 doubles.
Average of data is: 0
```

## 定义可选参数

C# 允许你创建可以接受可选参数的方法。此技术允许调用方在省略被认为不必要的参数的同时调用单个方法，前提是调用方对指定的默认值感到满意。

为了说明使用可选参数的工作方式，假设你有一个名为 EnterLogData() 的方法，它定义了一个可选参数。

```
static void EnterLogData(string message, string owner = "Programmer")
{
    Console.Beep();
    Console.WriteLine("Error: {0}", message);
    Console.WriteLine("Owner of Error: {0}", owner);
}
```

这里，最后一个字符串参数已通过参数定义中的赋值被赋予默认值 "Programmer"。在此基础上，你可以以两种方式调用 EnterLogData()。

```
Console.WriteLine("***** Fun with Methods *****");
...
EnterLogData("Oh no! Grid can't find data");
EnterLogData("Oh no! I can't find the payroll data", "CFO");

Console.ReadLine();
```

由于第一次调用 EnterLogData() 未指定第二个字符串参数，你会发现程序员是导致网格数据丢失的责任人，而 CFO 则弄丢了工资数据（由第二次方法调用中的第二个参数指定）。

需要注意的一件重要事情是，分配给可选参数的值必须在编译时已知，而不能在运行时解析（如果你尝试这样做，你将收到编译时错误！）。为了说明，假设你想用以下额外可选参数更新 EnterLogData()：


// Error! The default value for an optional arg must be known
// at compile time!
static void EnterLogData(string message, string owner = "Programmer", DateTime timeStamp = DateTime.Now)
{
    Console.Beep();
    Console.WriteLine("Error: {0}", message);
    Console.WriteLine("Owner of Error: {0}", owner);
    Console.WriteLine("Time of Error: {0}", timeStamp);
}

这段代码无法编译，因为 DateTime 类的 Now 属性的值是在运行时解析的，而不是编译时。

> 注意 为避免歧义，可选参数必须始终打包到方法签名的末尾。将可选参数列在非可选参数之前是编译错误。

## 使用命名参数（更新于 7.2）

C# 中另一个语言特性是对命名参数的支持。命名参数允许你通过按任意顺序指定参数值来调用方法。因此，与仅按位置传递参数（大多数情况下你会这样做）不同，你可以选择使用冒号运算符按名称指定每个参数。为了说明命名参数的使用，假设你已向 Program 类添加了以下方法：

```
static void DisplayFancyMessage(ConsoleColor textColor,
    ConsoleColor backgroundColor, string message)
{
    // Store old colors to restore after message is printed.
    ConsoleColor oldTextColor = Console.ForegroundColor;
    ConsoleColor oldbackgroundColor = Console.BackgroundColor;
    // Set new colors and print message.
    Console.ForegroundColor = textColor;
    Console.BackgroundColor = backgroundColor;
    Console.WriteLine(message);
    // Restore previous colors.
    Console.ForegroundColor = oldTextColor;
    Console.BackgroundColor = oldbackgroundColor;
}
```

现在，按 DisplayFancyMessage() 的编写方式，你会期望调用者通过传递两个 ConsoleColor 变量后跟一个字符串类型来调用此方法。然而，使用命名参数，以下调用完全没问题：

```
Console.WriteLine("***** Fun with Methods *****");
```

```
DisplayFancyMessage(message: "Wow! Very Fancy indeed!",
    textColor: ConsoleColor.DarkRed,
    backgroundColor: ConsoleColor.White);
```

```
DisplayFancyMessage(backgroundColor: ConsoleColor.Green,
    message: "Testing...",
    textColor: ConsoleColor.DarkBlue);
Console.ReadLine();
```

使用命名参数的规则在 C# 7.2 中略有更新。在 7.2 之前，如果你开始使用位置参数调用方法，你必须在任何命名参数之前列出它们。从 7.2 及更高版本开始，如果参数位置正确，则可以混合使用命名和未命名参数。

注意 尽管在 C# 7.2 及更高版本中你可以混合匹配命名参数和位置参数，但这并不被认为是一个好主意。仅仅因为你可以这样做并不意味着你应该这样做！

以下代码是一个示例：

```
// This is OK, as positional args are listed before named args.
DisplayFancyMessage(ConsoleColor.Blue,
    message: "Testing...",
    backgroundColor: ConsoleColor.White);

// This is OK, all arguments are in the correct order
DisplayFancyMessage(textColor: ConsoleColor.White, backgroundColor:ConsoleColor.Blue,
"Testing...");

// This is an ERROR, as positional args are listed after named args.
DisplayFancyMessage(message: "Testing...",
    backgroundColor: ConsoleColor.White,
    ConsoleColor.Blue);
```

除此限制外，你可能仍在想什么时候会想要使用此语言特性。毕竟，如果你需要向方法指定三个参数，为什么还要费心调换它们的位置呢？事实证明，如果你有一个定义了可选参数的方法，此功能会很有帮助。假设 DisplayFancyMessage() 已被重写为现在支持可选参数，因为你已分配了合适的默认值。

```
static void DisplayFancyMessage(ConsoleColor textColor = ConsoleColor.Blue,
    ConsoleColor backgroundColor = ConsoleColor.White,
    string message = "Test Message")
{
    ...
}
```

由于每个参数都有默认值，命名参数允许调用者仅指定他们不希望使用默认值的参数。因此，如果调用者希望值 "Hello!" 以蓝色文本出现在白色背景中，他们只需指定以下内容：

```
DisplayFancyMessage(message: "Hello!");
```

或者，如果调用者希望看到“Test Message”以绿色背景包含蓝色文本打印出来，他们可以调用以下内容：

```
DisplayFancyMessage(backgroundColor: ConsoleColor.Green);
```

正如你所见，可选参数和命名参数往往是相辅相成的。为了完成你对构建 C# 方法的考察，我需要讨论方法重载的主题。

## 理解方法重载

像其他现代面向对象语言一样，C# 允许方法重载。简单来说，当你定义一组同名但参数数量（或类型）不同的方法时，该方法就被认为被重载了。

为了理解重载为何如此有用，请考虑作为老式 Visual Basic 6.0（VB6）开发者的生活。假设你正在使用 VB6 构建一组返回各种传入数据类型（整数、双精度等）之和的方法。由于 VB6 不支持方法重载，你将被要求定义一组本质上做相同事情（返回参数之和）但名称唯一的方法。

```
' VB6 code examples.
Public Function AddInts(ByVal x As Integer, ByVal y As Integer) As Integer
    AddInts = x + y
End Function

Public Function AddDoubles(ByVal x As Double, ByVal y As Double) As Double
    AddDoubles = x + y
End Function

Public Function AddLongs(ByVal x As Long, ByVal y As Long) As Long
    AddLongs = x + y
End Function
```

不仅此类代码难以维护，而且调用者现在必须痛苦地记住每个方法的名称。使用重载，你可以允许调用者调用单个名为 Add() 的方法。关键是确保每个方法版本都有不同的参数集（仅返回类型不同的方法是不足够唯一的）。

> 注意 如第 10 章将解释的那样，可以构建通用方法，将重载的概念提升到下一个层次。使用泛型，你可以为方法实现定义类型占位符，这些占位符在调用相关成员时指定。

为了亲身体验这一点，创建一个名为 FunWithMethodOverloading 的新控制台应用程序项目。添加一个名为 AddOperations.cs 的新类，并将代码更新为以下内容：

```
namespace FunWithMethodOverloading {
    // C# code.
    // Overloaded Add() method.
```

```
```csharp
public static class AddOperations
{
    // Overloaded Add() method.
    public static int Add(int x, int y)
    {
        return x + y;
    }
    public static double Add(double x, double y)
    {
        return x + y;
    }
    public static long Add(long x, long y)
    {
        return x + y;
    }
}
```
```

用以下内容替换 Program.cs 中的代码：

```
```csharp
using System;
using FunWithMethodOverloading;
using static FunWithMethodOverloading.AddOperations;

Console.WriteLine("***** Fun with Method Overloading *****\n");

// Calls int version of Add()
Console.WriteLine(Add(10, 10));

// Calls long version of Add() (using the new digit separator)
Console.WriteLine(Add(900_000_000_000, 900_000_000_000));

// Calls double version of Add()
Console.WriteLine(Add(4.3, 4.4));

Console.ReadLine();
```
```

> **注意** `using static` 语句将在第 5 章中介绍。目前，将其视为使用 `FunWithMethodOverloading` 命名空间中名为 `AddOperations` 的静态类中的方法的键盘快捷方式。

顶层语句调用了三个不同版本的 Add 方法，每个方法使用不同的数据类型。Visual Studio 和 Visual Studio Code 在调用重载方法时也会提供帮助。当你输入重载方法名称（例如你的好朋友 `Console.WriteLine()`）时，IntelliSense 会列出该方法的每个版本。请注意，你可以使用上、下箭头键在重载方法的每个版本之间循环，如图 4-1 所示。





![](img/587021f4d5ba039be70a11107130377b_185_0.png)

## 图 4-1. Visual Studio 对重载方法的 IntelliSense

如果你的重载方法具有可选参数，那么编译器会根据命名和/或位置参数，为调用代码选择最佳匹配的方法。添加以下方法：

```
static int Add(int x, int y, int z = 0)
{
    return x + (y*z);
}
```

如果调用方未传入可选参数，编译器将匹配第一个签名（即不带可选参数的那个）。虽然存在方法解析的规则集，但通常不建议创建仅在可选参数上不同的重载方法。

最后，in、ref 和 out 在方法重载的签名中不被视为一部分，当使用多个修饰符时。换句话说，以下重载会抛出编译器错误：

```
static int Add(ref int x) { /* * */ }
static int Add(out int x) { /* * */ }
```

然而，如果只有一种方法使用 in、ref 或 out，编译器可以区分签名。因此，以下写法是允许的：

```
static int Add(ref int x) { /* * */ }
static int Add(int x) { /* * */ }
```

至此，使用 C# 语法构建方法的初步检查就结束了。接下来，让我们看看如何构建和操作枚举和结构。

## 理解 enum 类型

回顾第 1 章，.NET Core 类型系统由类、结构、枚举、接口和委托组成。为了开始探索这些类型，让我们先看看枚举（或简称为 enum）在名为 FunWithEnums 的新控制台应用程序项目中的作用。

> 注意 不要将术语 enum 与 enumerator 混淆；它们是完全不同的概念。enum 是一种名称-值对的自定义数据类型。enumerator 是一个实现名为 IEnumerable 的 .NET Core 接口的类或结构。通常，该接口在集合类以及 System.Array 类上实现。正如你在第 8 章中将看到的那样，支持 IEnumerable 的对象可以在 foreach 循环中使用。

在构建系统时，创建一组映射到已知数值的符号名称通常很方便。例如，如果你正在创建薪资系统，你可能希望使用副总裁、经理、承包商和普通员工等常量来引用员工类型。C# 正是为了这个目的而支持自定义枚举。例如，这里有一个名为 EmpTypeEnum 的枚举（如果你将其放在文件末尾，可以在与顶层语句相同的文件中定义它）：

```
using System;

Console.WriteLine("**** Fun with Enums ****\n");
Console.ReadLine();

//local functions go here:

// A custom enumeration.
enum EmpTypeEnum
{
    Manager,     // = 0
    Grunt,       // = 1
    Contractor,  // = 2
    VicePresident // = 3
}
```

> 注意 按惯例，枚举类型通常以 Enum 作为后缀。这不是必须的，但能使代码更易读。

EmpTypeEnum 枚举定义了四个命名常量，对应离散的数值。默认情况下，第一个元素设为零 (0)，随后按 n+1 递增。你可以自行更改初始值。例如，如果将 EmpTypeEnum 的成员编号为 102 到 105 很有意义，你可以这样做：

```
// Begin with 102.
enum EmpTypeEnum
{
    Manager = 102,
    Grunt,        // = 103
    Contractor,   // = 104
    VicePresident // = 105
}
```

枚举不一定需要遵循顺序排列，也不一定需要具有唯一值。如果出于某种原因，你希望将 EmpTypeEnum 建立如下，编译器仍然会接受：

```
// Elements of an enumeration need not be sequential!
enum EmpType
{
    Manager = 10,
    Grunt = 1,
    Contractor = 100,
    VicePresident = 9
}
```

## 控制 enum 的底层存储

默认情况下，用于保存枚举值的存储类型是 System.Int32（C# 中的 int）；不过你可以随意更改。C# 枚举可以为任何核心系统类型（byte、short、int 或 long）定义。例如，如果你想将 EmpTypeEnum 的底层存储值设为 byte 而不是 int，可以写如下：
// 这次，EmpTypeEnum 映射到底层 byte。

```
enum EmpTypeEnum : byte
{
    Manager = 10,
    Grunt = 1,
    Contractor = 100,
    VicePresident = 9
}
```

更改枚举的底层类型在你构建将部署到低内存设备的 .NET Core 应用程序并尽可能节省内存时可能很有帮助。当然，如果你将枚举设为使用 byte 作为存储，每个值必须在其范围内！例如，以下 EmpTypeEnum 版本会导致编译器错误，因为值 999 不能放入 byte 的范围内：

```
// Compile-time error! 999 is too big for a byte!
enum EmpTypeEnum : byte
{
    Manager = 10,
    Grunt = 1,
    Contractor = 100,
    VicePresident = 999
}
```

## 声明 enum 变量

一旦确定了枚举的范围和存储类型，你就可以用它来代替所谓的魔数。因为枚举只不过是一种用户定义的数据类型，你可以将其用作函数返回值、方法参数、局部变量等。假设你有一个名为 AskForBonus() 的方法，唯一参数为 EmpTypeEnum 变量。根据传入参数的值，你将打印出对奖金请求的合适回应。

```
Console.WriteLine("**** Fun with Enums ****");
// Make an EmpTypeEnum variable.
EmpTypeEnum emp = EmpTypeEnum.Contractor;
AskForBonus(emp);
Console.ReadLine();

// Enums as parameters.
static void AskForBonus(EmpTypeEnum e)
{
    switch (e)
    {
        case EmpType.Manager:
            Console.WriteLine("How about stock options instead?");
            break;
        case EmpType.Grunt:
            Console.WriteLine("You have got to be kidding...");
            break;
        case EmpType.Contractor:
            Console.WriteLine("You already get enough cash...");
            break;
        case EmpType.VicePresident:
            Console.WriteLine("VERY GOOD, Sir!");
            break;
    }
}
```

注意，当你为枚举变量赋值时，必须将枚举名（EmpTypeEnum）作用域到值（Grunt）。因为枚举是一组固定的名称-值对，所以不允许将枚举变量设为枚举类型未直接定义的值。

```
static void ThisMethodWillNotCompile()
{
    // Error! SalesManager is not in the EmpTypeEnum enum!
    EmpTypeEnum emp = EmpType.SalesManager;

    // Error! Forgot to scope Grunt value to EmpTypeEnum enum!
    emp = Grunt;
}
```

## 使用 System.Enum 类型

.NET Core 枚举有趣的地方在于它们从 System.Enum 类类型获得了功能。该类定义了若干方法，允许你查询和转换给定的枚举。一个有用的方法是静态的 Enum.GetUnderlyingType()，顾名思义，它返回用于存储枚举类型值的的数据类型（在当前 EmpTypeEnum 声明的情况下为 System.Byte）。

```
Console.WriteLine("**** Fun with Enums ****");
...

// Print storage for the enum.
Console.WriteLine("EmpTypeEnum uses a {0} for storage",
    Enum.GetUnderlyingType(emp.GetType()));
Console.ReadLine();
```

Enum.GetUnderlyingType() 方法要求你将 System.Type 作为第一个参数传入。正如第 17 章中全面考察的那样，Type 表示给定 .NET Core 实体的元数据描述。

一种可能的获取元数据的方法（如前所示）是使用 GetType() 方法，该方法在 .NET Core 基础类库中的所有类型中都很常见。另一种方法是使用 C# 的 typeof 运算符。这样做的一个好处是，你不需要拥有要获取元数据描述的实体的变量。




// 这次使用 typeof 来提取一个 Type。
Console.WriteLine("EmpTypeEnum uses a {0} for storage", Enum.GetUnderlyingType(typeof(EmpTypeEnum)));

## 动态发现枚举的名称-值对

除了 Enum.GetUnderlyingType() 方法之外，所有 C# 枚举都支持一个名为 ToString() 的方法，该方法返回当前枚举值的字符串名称。以下代码是一个示例：

```
EmpTypeEnum emp = EmpTypeEnum.Contractor;
...
// 打印出 "emp is a Contractor"。
Console.WriteLine("emp is a {0}.", emp.ToString());
Console.ReadLine();
```

如果您有兴趣发现给定枚举变量的值，而不是其名称，则可以简单地将枚举变量强制转换为底层存储类型。以下是一个示例：

```
Console.WriteLine("**** Fun with Enums ****");
EmpTypeEnum emp = EmpTypeEnum.Contractor;
...
// 打印出 "Contractor = 100"。
Console.WriteLine("{0} = {1}", emp.ToString(), (byte)emp);
Console.ReadLine();
```

注意 静态 Enum.Format() 方法通过指定期望的格式标志提供了更精细的格式化选项。请查阅文档以获取完整的格式标志列表。

System.Enum 还定义了另一个名为 GetValues() 的静态方法。此方法返回 System.Array 的实例。数组中的每个项对应于指定枚举的一个成员。考虑以下方法，该方法将打印出您作为参数传入的任何枚举中的每个名称-值对：

// 此方法将打印出任何枚举的详细信息。
static void EvaluateEnum(System.Enum e)
{
    Console.WriteLine("=> Information about {0}", e.GetType().Name);

    Console.WriteLine("Underlying storage type: {0}", Enum.GetUnderlyingType(e.GetType()));

    // 获取传入参数的所有名称-值对。
    Array enumData = Enum.GetValues(e.GetType());
    Console.WriteLine("This enum has {0} members.", enumData.Length);

    // 现在显示字符串名称和相关联的值，使用 D 格式标志（参见第 3 章）。
    for(int i = 0; i < enumData.Length; i++)
    {
Console.WriteLine("Name: {0}, Value: {0:D}",
    enumData.GetValue(i));
}
```

要测试此新方法，请更新您的代码以创建 System 命名空间中声明的几种枚举类型的变量（以及为了好玩而添加一个 EmpTypeEnum 枚举）。以下代码是一个示例：

```
Console.WriteLine("**** Fun with Enums *****");
...
EmpTypeEnum e2 = EmpTypeEnum.Contractor;

// 这些类型是 System 命名空间中的枚举。
DayOfWeek day = DayOfWeek.Monday;
ConsoleColor cc = ConsoleColor.Gray;

EvaluateEnum(e2);
EvaluateEnum(day);
EvaluateEnum(cc);
Console.ReadLine();
```

这里显示了部分输出：

```
=> Information about DayOfWeek

Underlying storage type: System.Int32
This enum has 7 members.
Name: Sunday, Value: 0
Name: Monday, Value: 1
Name: Tuesday, Value: 2
Name: Wednesday, Value: 3
Name: Thursday, Value: 4
Name: Friday, Value: 5
Name: Saturday, Value: 6
```

正如您将在本书后续内容中看到的那样，枚举在 .NET Core 基类库中被广泛使用。当您使用任何枚举时，请始终记住您可以使用 System.Enum 的成员与名称-值对进行交互。

## 使用枚举、标志和按位运算

按位运算提供了一种快速机制，可在位级别上对二进制数进行操作。表 4-3 包含 C# 按位运算符、它们的作用以及每个运算符的示例。

### 表 4-3. 按位运算

| 运算符 | 操作 | 示例 |
|---|---|---|
| & (AND) | 如果位同时存在于两个操作数中则复制该位 | 0110 & 0100 = 0100 (4) |
| | (OR) | 如果位存在于任一操作数中则复制该位 | 0110 | 0100 = 0110 (6) |
| ^ (XOR) | 如果位仅存在于一个操作数中则复制该位 | 0110 ^ 0100 = 0010 (2) |
| ~ (按位取反) | 翻转位 | ~0110 = -7 (由于溢出) |
| << (左移) | 将位左移 | 0110 << 1 = 1100 (12) |
| >> (右移) | 将位右移 | 0110 >> 1 = 0011 (3) |

为了展示这些操作，创建一个名为 FunWithBitwiseOperations 的新控制台应用程序项目。将 Program.cs 文件更新为以下代码：

```
using System;
using FunWithBitwiseOperations;
Console.WriteLine("==== Fun with Bitwise Operations");
Console.WriteLine("6 & 4 = {0} | {1}", 6 & 4, Convert.ToString((6 & 4),2));
Console.WriteLine("6 | 4 = {0} | {1}", 6 | 4, Convert.ToString((6 | 4),2));
Console.WriteLine("6 ^ 4 = {0} | {1}", 6 ^ 4, Convert.ToString((6 ^ 4),2));
Console.WriteLine("6 << 1 = {0} | {1}", 6 << 1, Convert.ToString((6 << 1),2));
Console.WriteLine("6 >> 1 = {0} | {1}", 6 >> 1, Convert.ToString((6 >> 1),2));
Console.WriteLine("~6 = {0} | {1}", ~6, Convert.ToString(~(short)6,2));
Console.WriteLine("Int.MaxValue {0}", Convert.ToString((int.MaxValue),2));
Console.ReadLine();
```

执行代码后，您将看到以下结果：

```
==== Fun with Bitwise Operations

6 & 4 = 4 | 100
6 | 4 = 6 | 110
6 ^ 4 = 2 | 10
6 << 1 = 12 | 1100
6 >> 1 = 3 | 11
~6 = -7 | 1111111111111111111111111111001
Int.MaxValue 1111111111111111111111111111111
```

现在您已经了解了按位运算的基础知识，是时候将它们应用于枚举了。添加一个名为 ContactPreferenceEnum.cs 的新文件并将代码更新为以下内容：

```
using System;
namespace FunWithBitwiseOperations
{
    [Flags]
    public enum ContactPreferenceEnum
    {
        None = 1,
Email = 2,
Phone = 4,
Ponyexpress = 6
}
}
```

注意 Flags 属性。这允许将枚举的多个值组合到单个变量中。例如，Email 和 Phone 可以这样组合：

```
ContactPreferenceEnum emailAndPhone = ContactPreferenceEnum.Email | ContactPreferenceEnum.Phone;
```

这允许您检查组合值中是否存在其中一个值。例如，如果您想检查 emailAndPhone 变量中是否存在 ContactPreference 值，可以使用以下代码：

```
Console.WriteLine("None? {0}", (emailAndPhone | ContactPreferenceEnum.None) == emailAndPhone);
Console.WriteLine("Email? {0}", (emailAndPhone | ContactPreferenceEnum.Email) == emailAndPhone);
Console.WriteLine("Phone? {0}", (emailAndPhone | ContactPreferenceEnum.Phone) == emailAndPhone);
Console.WriteLine("Text? {0}", (emailAndPhone | ContactPreferenceEnum.Text) == emailAndPhone);
```

执行后，控制台窗口会显示以下内容：

```
None? False
Email? True
Phone? True
Text? False
```

## 理解结构（又名值类型）

现在您已经了解了枚举类型的角色，让我们来研究 .NET Core 结构（或简称为结构体）的使用。结构类型非常适合在应用程序中建模数学、几何和其他“原子”实体。结构（例如枚举）是一种用户定义的类型；然而，结构不仅仅是一组名称-值对。相反，结构是可包含任意数量数据字段和操作这些字段的成员的类型。

> 注意 如果您有面向对象编程的背景，可以将结构视为“轻量级类类型”，因为结构提供了一种定义支持封装的类型的方式，但不能用于构建相关类型的族。当您需要通过继承构建相关类型的族时，您需要使用类类型。

表面上，定义和使用结构的过程很简单，但正如他们所说，魔鬼在细节中。为了开始了解结构类型的基础知识，创建一个名为 FunWithStructures 的新项目。在 C# 中，结构使用 struct 关键字定义。定义一个名为 Point 的新结构，该结构定义了两个 int 类型的成员变量以及一组用于交互数据的操作方法。

```
struct Point
{
    // 结构的字段。
    public int X;
    public int Y;

    // 将 (X, Y) 位置加 1。
    public void Increment()
    {
        X++; Y++;
    }

    // 将 (X, Y) 位置减 1。
    public void Decrement()
    {
        X--; Y--;
    }

    // 显示当前位置。
    public void Display()
    {
        Console.WriteLine("X = {0}, Y = {1}", X, Y);
    }
}
```

这里，您使用 public 关键字定义了两个整数字段（X 和 Y），public 关键字是访问控制修饰符（第 5 章将继续讨论此内容）。使用 public 关键字声明数据可确保调用者可以通过点运算符直接从给定的 Point 变量访问数据。

注意 通常认为在类或结构中定义公共数据是不好的风格。相反，您希望定义私有数据，可以通过公共属性访问和更改。这些细节将在第 5 章中讨论。

以下是对 Point 类型进行测试的代码：

```
Console.WriteLine("***** A First Look at Structures ****\n");

// 创建一个初始 Point。
Point myPoint;
myPoint.X = 349;
myPoint.Y = 76;
myPoint.Display();

// 调整 X 和 Y 值。
myPoint.Increment();
myPoint.Display();
Console.ReadLine();
```


The output is as you would expect.

**** A First Look at Structures ****

X = 349, Y = 76
X = 350, Y = 77

## Creating Structure Variables

When you want to create a structure variable, you have a variety of options. Here, you simply create a Point variable and assign each piece of public field data before invoking its members. If you do not assign each piece of public field data (X and Y in this case) before using the structure, you will receive a compiler error.

```
// Error! Did not assign Y value.
Point p1;
p1.X = 10;
p1.Display();
```

```
// OK! Both fields assigned before use.
Point p2;
p2.X = 10;
p2.Y = 10;
p2.Display();
```

As an alternative, you can create structure variables using the C# new keyword, which will invoke the structure's default constructor. By definition, a default constructor does not take any arguments. The benefit of invoking the default constructor of a structure is that each piece of field data is automatically set to its default value.

```
// Set all fields to default values
// using the default constructor.
Point p1 = new Point();

// Prints X=0,Y=0.
p1.Display();
```

It is also possible to design a structure with a custom constructor. This allows you to specify the values of field data upon variable creation, rather than having to set each data member field by field. Chapter 5 will provide a detailed examination of constructors; however, to illustrate, update the Point structure with the following code:

```
struct Point
{
    // Fields of the structure.
    public int X;
    public int Y;

    // A custom constructor.
    public Point(int xPos, int yPos)
    {
```

```
X = xPos;
    Y = yPos;
}
...
}
```

With this, you could now create Point variables, as follows:

```
// Call custom constructor.
Point p2 = new Point(50, 60);

// Prints X=50,Y=60.
p2.Display();
```

## Using Read-Only Structs (New 7.2)

Structs can also be marked as read-only if there is a need for them to be immutable. Immutable objects must be set up at construction and because they cannot be changed, can be more performant. When declaring a struct as read-only, all the properties must also be read-only. But you might ask, how can a property be set (as all properties must be on a struct) if it is read-only? The answer is that the value must be set during the construction of the struct.

Update the point class to the following example:

```
readonly struct ReadOnlyPoint
{
    // Fields of the structure.
    public int X { get; }
    public int Y { get; }

    // Display the current position and name.
    public void Display()
    {
        Console.WriteLine($"X = {X}, Y = {Y}");
    }

    public ReadOnlyPoint(int xPos, int yPos)
    {
        X = xPos;
        Y = yPos;
    }
}
```

The Increment and Decrement methods have been removed since the variables are read-only. Notice also the two properties, X and Y. Instead of setting them up as fields, they are created as read-only automatic properties. Automatic properties are covered in Chapter 5.

## Using Read-Only Members (New 8.0)

New in C# 8.0, you can declare individual fields of a struct as readonly. This is more granular than making the entire struct read-only. The readonly modifier can be applied to methods, properties, and property accessors. Add the following struct code to your file, outside of the Program class:

```
struct PointWithReadOnly
{
    // Fields of the structure.
    public int X;
    public readonly int Y;
    public readonly string Name;

    // Display the current position and name.
    public readonly void Display()
    {
        Console.WriteLine($"X = {X}, Y = {Y}, Name = {Name}");
    }

    // A custom constructor.
    public PointWithReadOnly(int xPos, int yPos, string name)
    {
        X = xPos;
        Y = yPos;
        Name = name;
    }
}
```

To use this new struct, add the following to the top-level statements:

```
PointWithReadOnly p3 =
    new PointWithReadOnly(50,60,"Point w/RO");
p3.Display();
```

## Using ref Structs (New 7.2)

Also added in C# 7.2, the ref modifier can be used when defining a struct. This requires all instances of the struct to be stack allocated and cannot be assigned as a property of another class. The technical reason for this is that ref structs cannot referenced from the heap. The difference between the stack and the heap is covered in the next section.

These are some additional limitations of ref structs:

- They cannot be assigned to a variable of type object or dynamic, and cannot be an interface type.
- They cannot implement interfaces.
- They cannot be used as a property of a non-ref struct.
- They cannot be used in async methods, iterators, lambda expressions, or local functions.

The following code, which creates a simple struct and then attempts to create a property in that struct typed to a ref struct, will not compile:

```
struct NormalPoint
{
    //This does not compile
    public PointWithRef PropPointer { get; set; }
}
```

readonly and ref modifiers can be combined to gain the benefits and restrictions of both.

## Using Disposable ref Structs (New 8.0)

As covered in the previous section, ref structs (and read-only ref structs) cannot implement an interface and therefore cannot implement IDisposable. New in C# 8.0, ref structs and read-only ref structs can be made disposable by adding a public void Dispose() method.

Add the following struct definition to the main file:

```
ref struct DisposableRefStruct
{
    public int X;
    public readonly int Y;
    public readonly void Display()
    {
        Console.WriteLine($"X = {X}, Y = {Y}");
    }
    // A custom constructor.
    public DisposableRefStruct(int xPos, int yPos)
    {
        X = xPos;
        Y = yPos;
        Console.WriteLine("Created!");
    }
    public void Dispose()
    {
        //clean up any resources here
        Console.WriteLine("Disposed!");
    }
}
```

Next, add the following to the end of the top-level statements to create and dispose of the new struct:

```
var s = new DisposableRefStruct(50, 60);
s.Display();
s.Dispose();
```

> Note Object lifetime and disposing of objects are covered in depth in Chapter 9.

To deepen your understanding of stack and heap allocation, you need to explore the distinction between a .NET Core value type and a .NET Core reference type.

## Understanding Value Types and Reference Types

> Note The following discussion of value types and reference types assumes that you have a background in object-oriented programming. If this is not the case, you might want to skip to the “Understanding C# Nullable Types” section of this chapter and return to this section after you have read Chapters 5 and 6.

Unlike arrays, strings, or enumerations, C# structures do not have an identically named representation in the .NET Core library (i.e., there is no System.Structure class) but are implicitly derived from System.ValueType. The role of System.ValueType is to ensure that the derived type (e.g., any structure) is allocated on the stack, rather than the garbage-collected heap. Simply put, data allocated on the stack can be created and destroyed quickly, as its lifetime is determined by the defining scope. Heap-allocated data, on the other hand, is monitored by the .NET Core garbage collector and has a lifetime that is determined by many factors, which will be examined in Chapter 9.

Functionally, the only purpose of System.ValueType is to override the virtual methods defined by System.Object to use value-based versus reference-based semantics. As you might know, overriding is the process of changing the implementation of a virtual (or possibly abstract) method defined within a base class. The base class of ValueType is System.Object. In fact, the instance methods defined by System.ValueType are identical to those of System.Object.

```
// Structures and enumerations implicitly extend System.ValueType.
public abstract class ValueType : object
{
    public virtual bool Equals(object obj);
    public virtual int GetHashCode();
    public Type GetType();
    public virtual string ToString();
}
```

Given that value types are using value-based semantics, the lifetime of a structure (which includes all numerical data types [int, float], as well as any enum or structure) is predictable. When a structure variable falls out of the defining scope, it is removed from memory immediately.

```
// Local structures are popped off
// the stack when a method returns.
static void LocalValueTypes()
{
    // Recall! "int" is really a System.Int32 structure.
    int i = 0;

    // Recall! Point is a structure type.
    Point p = new Point();
} // "i" and "p" popped off the stack here!
```

## Using Value Types, Reference Types, and the Assignment Operator

When you assign one value type to another, a member-by-member copy of the field data is achieved. In the case of a simple data type such as System.Int32, the only member to copy is the numerical value. However, in the case of your Point, the X and Y values are copied into the new structure variable. To illustrate, create a new Console Application project named FunWithValueAndReferenceTypes and then copy your previous Point definition into your new namespace. Next, add the following local function to your top-level statements:

```
// Assigning two intrinsic value types results in
// two independent variables on the stack.
static void ValueTypeAssignment()
{
    Console.WriteLine("Assigning value types\n");



```
Point p1 = new Point(10, 10);
Point p2 = p1;

// Print both points.
p1.Display();
p2.Display();

// Change p1.X and print again. p2.X is not changed.
p1.X = 100;
Console.WriteLine("\n=> Changed p1.X\n");
p1.Display();
p2.Display();
}
```

这里，你创建了一个类型为 Point 的变量（名为 p1），然后将其赋值给另一个 Point（p2）。由于 Point 是一个值类型，栈上存在两个 Point 类型的副本，每个副本都可以独立操作。因此，当你更改 p1.X 的值时，p2.X 的值不受影响。

## Assigning value types

```
X = 10, Y = 10
X = 10, Y = 10
=> Changed p1.X
X = 100, Y = 10
X = 10, Y = 10
```

与值类型截然不同，当你对引用类型（即所有类实例）应用赋值运算符时，你是在重定向引用变量在内存中指向的内容。为说明这一点，创建一个名为 PointRef 的新类类型，它具有与 Point 结构相同的成员，只是将构造函数重命名为与类名匹配。

```
// Classes are always reference types.
class PointRef
{
    // Same members as the Point structure...
    // Be sure to change your constructor name to PointRef!
    public PointRef(int xPos, int yPos)
    {
        X = xPos;
        Y = yPos;
    }
}
```

现在，在以下新方法中使用你的 PointRef 类型。除了使用 PointRef 类而不是 Point 结构外，代码与 ValueTypeAssignment() 方法完全相同。

```
static void ReferenceTypeAssignment()
{
    Console.WriteLine("Assigning reference types\n");
    PointRef p1 = new PointRef(10, 10);
```

```
PointRef p2 = p1;

// Print both point refs.
p1.Display();
p2.Display();

// Change p1.X and print again.
p1.X = 100;
Console.WriteLine("\n=> Changed p1.X\n");
p1.Display();
p2.Display();
}
```

在这种情况下，你有两个引用指向托管堆上的同一个对象。因此，当你使用 p1 引用更改 X 的值时，p2.X 报告相同的值。假设你已调用此新方法，你的输出应如下所示：

```
Assigning reference types

X = 10, Y = 10
X = 10, Y = 10
=> Changed p1.X
X = 100, Y = 10
X = 100, Y = 10
```

## Using Value Types Containing Reference Types

现在你对值类型和引用类型的基本区别有了更好的了解，让我们检查一个更复杂的示例。假设你有以下引用（类）类型，它维护一个信息字符串，可以通过自定义构造函数设置：

```
class ShapeInfo
{
    public string InfoString;
    public ShapeInfo(string info)
    {
        InfoString = info;
    }
}
```

现在假设你想在名为 Rectangle 的值类型中包含此类的一个变量。为了允许调用者设置内部 ShapeInfo 成员变量的值，你还提供了一个自定义构造函数。以下是 Rectangle 类型的完整定义：

```
struct Rectangle
{
    // The Rectangle structure contains a reference type member.
    public ShapeInfo RectInfo;

    public int RectTop, RectLeft, RectBottom, RectRight;
```

```
public Rectangle(string info, int top, int left, int bottom, int right)
{
    RectInfo = new ShapeInfo(info);
    RectTop = top; RectBottom = bottom;
    RectLeft = left; RectRight = right;
}

public void Display()
{
    Console.WriteLine("String = {0}, Top = {1}, Bottom = {2}, " +
        "Left = {3}, Right = {4}",
        RectInfo.InfoString, RectTop, RectBottom, RectLeft, RectRight);
}
```

此时，你已在值类型中包含了一个引用类型。现在的百万美元问题是“如果你将一个 Rectangle 变量赋给另一个，会发生什么？”根据你对值类型已有的了解，你会正确地假设整数数据（确实是结构 System.Int32）对于每个 Rectangle 变量应该是独立的实体。但内部引用类型呢？对象的状态会被完全复制，还是会复制对该对象的引用？为回答这个问题，定义以下方法并调用它：

```
static void ValueTypeContainingRefType()
{
    // Create the first Rectangle.
    Console.WriteLine("=> Creating r1");
    Rectangle r1 = new Rectangle("First Rect", 10, 10, 50, 50);

    // Now assign a new Rectangle to r1.
    Console.WriteLine("=> Assigning r2 to r1");
    Rectangle r2 = r1;

    // Change some values of r2.
    Console.WriteLine("=> Changing values of r2");
    r2.RectInfo.InfoString = "This is new info!";
    r2.RectBottom = 4444;

    // Print values of both rectangles.
    r1.Display();
    r2.Display();
}
```

输出如下所示：

```
-> Creating r1

-> Assigning r2 to r1
-> Changing values of r2
String = This is new info!, Top = 10, Bottom = 50, Left = 10, Right = 50
String = This is new info!, Top = 10, Bottom = 4444, Left = 10, Right = 50
```

正如你所见，当你使用 r2 引用更改信息字符串的值时，r1 引用显示相同的值。默认情况下，当值类型包含其他引用类型时，赋值会复制引用。这样你就有两个独立的结构，每个结构都包含指向内存中同一对象的引用（即浅拷贝）。当你想要执行深度拷贝，即将内部引用的状态完全复制到新对象中时，一种方法是实现 ICloneable 接口（你将在第 8 章中这样做）。

## Passing Reference Types by Value

如本章前面所述，引用类型或值类型可以作为参数传递给方法。然而，按值传递引用类型（例如类）与按引用传递它完全不同。为了理解这种区别，假设你有一个简单的 Person 类，定义在名为 FunWithRefTypeValTypeParams 的新控制台应用程序项目中，定义如下：

```
class Person
{
    public string personName;
    public int personAge;

    // Constructors.
    public Person(string name, int age)
    {
        personName = name;
        personAge = age;
    }
    public Person(){}

    public void Display()
    {
        Console.WriteLine("Name: {0}, Age: {1}", personName, personAge);
    }
}
```

现在，假设你创建一个方法，允许调用者按值发送 Person 对象（注意参数修饰符的缺失，例如 out 或 ref）？

```
static void SendAPersonByValue(Person p)
{
    // Change the age of "p"?
    p.personAge = 99;

    // Will the caller see this reassignment?
    p = new Person("Nikki", 99);
}
```

注意 SendAPersonByValue() 方法如何尝试将传入的 Person 引用重新分配给新的 Person 对象，以及更改一些状态数据。现在让我们使用以下代码测试此方法：

```
// Passing ref-types by value.
Console.WriteLine("***** Passing Person object by value *****");
Person fred = new Person("Fred", 12);
```

```
Console.WriteLine("\nBefore by value call, Person is:");
fred.Display();

SendAPersonByValue(fred);
Console.WriteLine("\nAfter by value call, Person is:");
fred.Display();
Console.ReadLine();
```

此调用的输出如下：

```
**** Passing Person object by value ****

Before by value call, Person is:
Name: Fred, Age: 12

After by value call, Person is:
Name: Fred, Age: 99
```

正如你所见，personAge 的值已被修改。这种行为前面已讨论过，现在你理解了引用类型的工作方式后应该更有意义。鉴于你能够更改传入 Person 的状态，复制了什么？答案：对调用者对象的引用的副本。因此，由于 SendAPersonByValue() 方法指向与调用者相同的对象，因此可以更改对象的状态数据。无法做的是重新分配引用指向的内容。

## Passing Reference Types by Reference

现在假设你有一个 SendAPersonByReference() 方法，它按引用传递引用类型（注意 ref 参数修饰符）。

```
static void SendAPersonByReference(ref Person p)
{
    // Change some data of "p".
    p.personAge = 555;

    // "p" is now pointing to a new object on the heap!
    p = new Person("Nikki", 999);
}
```

正如你可能预期的那样，这允许被调用者完全灵活地操作传入参数。被调用者不仅可以更改对象的状态，如果它选择，还可以重新分配引用到新的 Person 对象。现在思考以下更新后的代码：

```
// Passing ref-types by ref.
Console.WriteLine("***** Passing Person object by reference *****");
...
```

```
Person mel = new Person("Mel", 23);
Console.WriteLine("Before by ref call, Person is:");
mel.Display();
```


```
SendAPersonByReference(ref mel);
Console.WriteLine("After by ref call, Person is:");
mel.Display();
Console.ReadLine();
```

注意以下输出：

```
**** Passing Person object by reference ****

Before by ref call, Person is:
Name: Mel, Age: 23
After by ref call, Person is:
Name: Nikki, Age: 999
```

正如你所见，一个名为 Mel 的对象在调用后变成了名为 Nikki 的对象，因为该方法能够改变传入引用在内存中指向的内容。传递引用类型的黄金法则是：

- 如果引用类型按引用传递，被调用方可以更改对象状态数据的值，以及它所引用的对象。
- 如果引用类型按值传递，被调用方可以更改对象状态数据的值，但不能更改它所引用的对象。

## 关于值类型和引用类型的最终细节

为了总结本主题，请考虑表 4-4 中的信息，该表概括了值类型和引用类型之间的核心区别。

表 4-4. 值类型和引用类型比较

| 发人深省的问题 | 值类型 | 引用类型 |
|---|---|---|
| 对象在哪里分配？ | 分配在栈上。 | 分配在托管堆上。 |
| 变量如何表示？ | 值类型变量是本地副本。 | 引用类型变量指向已分配实例占用的内存。 |
| 基本类型是什么？ | 隐式扩展 System.ValueType。 | 可以派生自任何其他类型（除 System.ValueType 外），如果该类型未被“密封”（有关更多详细信息，请参见第 6 章）。 |
| 此类型能否作为其他类型的基？ | 不可以。值类型始终被密封，不能被继承。 | 可以。如果类型未被密封，它可以作为其他类型的基。 |
| 默认参数传递行为是什么？ | 变量按值传递（即，变量的副本传递给被调用函数）。 | 对于引用类型，引用按值复制。 |

### 表 4-4.（续）

| 发人深省的问题 | 值类型 | 引用类型 |
|---|---|---|
| 此类型能否重写 System.Object.Finalize()？ | 不可以。 | 可以，间接地（有关更多详细信息，请参见第 9 章）。 |
| 我可以为此类型定义构造函数吗？ | 可以，但默认构造函数被保留（即，你的自定义构造函数必须都有参数）。 | 当然可以！ |
| 此类型的变量何时死亡？ | 当它们超出定义的作用域时。 | 当对象被垃圾回收时（参见第 9 章）。 |

尽管存在差异，值类型和引用类型都可以实现接口，并且可能支持任意数量的字段、方法、重载运算符、常量、属性和事件。

## 理解 C# 可空类型

让我们使用名为 FunWithNullableValueTypes 的控制台应用程序项目来研究可空数据类型的角色。正如你所知，C# 数据类型具有固定的范围，并表示为 System 命名空间中的一种类型。例如，System.Boolean 数据类型可以分配集合 {true, false} 中的值。现在，回忆一下，所有数值数据类型（以及 Boolean 数据类型）都是值类型。值类型永远不能被赋值为 null，因为 null 用于建立空对象引用。

```
// 编译错误！
// 值类型不能设为 null！
bool myBool = null;
int myInt = null;
```

C# 支持可空数据类型的概念。简单来说，可空类型可以表示其底层类型的所有值，外加 null 值。因此，如果你声明一个可空的 bool，它可以从集合 {true, false, null} 中分配值。当处理关系数据库时，这非常有帮助，因为在数据库表中遇到未定义列的情况很常见。如果没有可空数据类型的概念，在 C# 中就没有方便的方法来表示没有值的数值数据点。

要定义可空变量，请在底层数据类型后缀加上问号符号（?）。在 C# 8.0 之前，此语法仅在应用于值类型时合法（有关更多信息，请参见下一节“可空引用类型”）。与非可空变量一样，本地可空变量在使用前必须先分配初始值。

```
static void LocalNullableVariables()
{
    // 定义一些本地可空变量。
    int? nullableInt = 10;
    double? nullableDouble = 3.14;
    bool? nullableBool = null;
    char? nullableChar = 'a';
    int?[] arrayOfNullableInts = new int?[10];
}
```

## 使用可空值类型

在 C# 中，? 后缀表示法是创建泛型 System.Nullable<T> 结构类型的简写。它也用于创建可空引用类型（在下一节中介绍），尽管行为略有不同。虽然你直到第 10 章才会研究泛型，但重要的是要理解 System.Nullable<T> 类型提供了一组所有可空类型都可以使用 的成员。

例如，你可以使用 HasValue 属性或 != 运算符以编程方式发现可空变量是否确实被赋值为 null。可空类型的值可以直接获取或通过 Value 属性获取。事实上，由于 ? 后缀只是使用 Nullable<T> 的简写，你可以按如下方式实现 LocalNullableVariables() 方法：

```
static void LocalNullableVariablesUsingNullable()
{
    // 使用 Nullable<T> 定义一些本地可空类型。
    Nullable<int> nullableInt = 10;
    Nullable<double> nullableDouble = 3.14;
    Nullable<bool> nullableBool = null;
    Nullable<char> nullableChar = 'a';
    Nullable<int>[] arrayOfNullableInts = new Nullable<int>[10];
}
```

如上所述，当你与数据库交互时，可空数据类型特别有用，因为数据表中的列可能有意为空（例如，未定义）。为了说明，假设以下类，它模拟访问包含两列可能为 null 的表的数据库的过程。请注意，GetIntFromDatabase() 方法没有为可空整数成员变量赋值，而 GetBoolFromDatabase() 为 bool? 成员分配了有效值。

```
class DatabaseReader
{
    // 可空数据字段。
    public int? numericValue = null;
    public bool? boolValue = true;

    // 注意可空返回类型。
    public int? GetIntFromDatabase()
    { return numericValue; }

    // 注意可空返回类型。
    public bool? GetBoolFromDatabase()
    { return boolValue; }
}
```

现在，检查以下代码，它调用 DatabaseReader 类的每个成员，并使用 HasValue 和 Value 成员以及 C# 等价运算符（确切地说是不等）来发现分配的值：

```
Console.WriteLine("***** Fun with Nullable Value Types *****
");
DatabaseReader dr = new DatabaseReader();

// 从“数据库”获取 int。
int? i = dr.GetIntFromDatabase();
```

```
if (i.HasValue)
{
    Console.WriteLine("Value of 'i' is: {0}", i.Value);
}
else
{
    Console.WriteLine("Value of 'i' is undefined.");
}
// 从“数据库”获取 bool。
bool? b = dr.GetBoolFromDatabase();
if (b != null)
{
    Console.WriteLine("Value of 'b' is: {0}", b.Value);
}
else
{
    Console.WriteLine("Value of 'b' is undefined.");
}
Console.ReadLine();
```

## 使用可空引用类型（8.0 新增）

C# 8 中添加的一个重要功能是对可空引用类型的支持。事实上，这一变化如此重大，以至于 .NET Framework 无法更新以支持此新功能。因此，决定仅在 .NET Core 3.0 及更高版本中支持 C# 8，并决定默认禁用可空引用类型支持。当你在 .NET Core 3.0/3.1 或 .NET 5 中创建新项目时，引用类型的工作方式与 C# 7 相同。这是为了防止破坏 C# 8 生态系统之前存在的数十亿行代码。开发人员必须选择加入以在应用程序中启用可空引用类型。

可空引用类型遵循与可空值类型相同的许多规则。非可空引用类型必须在初始化时分配非空值，并且以后不能更改为 null 值。可空引用类型可以为 null，但仍必须在首次使用前分配某些内容（实际的某个实例或 null 值）。

可空引用类型使用相同的符号（?）来指示它们是可空的。然而，这不是使用 System.Nullable<T> 的简写，因为只有值类型可以用作 T 的替代。作为提醒，泛型和约束将在第 10 章中介绍。

## 选择加入可空引用类型

对可空引用类型的支持通过设置可空上下文来控制。这可以大到整个项目（通过更新项目文件）或小到几行代码（通过使用编译器指令）。还有两种可以设置的上下文：

- *可空注释上下文：* 这启用/禁用可空引用类型的可空注释（?）。
- *可空警告上下文：* 这启用/禁用可空引用类型的编译器警告。

要亲眼看看这些效果，创建一个名为 FunWithNullableReferenceTypes 的新控制台应用程序。打开项目文件（如果你使用 Visual Studio，双击解决方案资源管理器中的项目名称或右键单击项目名称并选择编辑项目文件）。通过添加 <Nullable> 节点来更新项目文件以支持可空引用类型（所有可用选项均显示在表 4-5 中）。





```
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>net5.0</TargetFramework>
        <Nullable>enable</Nullable>
    </PropertyGroup>
</Project>
```

## 表 4-5. 项目文件中 Nullable 的取值

| Value | Meaning in Life |
|-------|-----------------|
| Enable | 启用可空注释，且启用可空警告。 |
| Warnings | 禁用可空注释，且启用可空警告。 |
| Annotations | 启用可空注释，且禁用可空警告。 |
| Disable | 禁用可空注释，且禁用可空警告。 |

<Nullable> 元素影响整个项目。要控制项目中的更小部分，请使用表 4-6 中所示的编译器指令。

## 表 4-6. #nullable 编译器指令的取值

| Value | Meaning in Life |
|-------|-----------------|
| Enable | 启用注释，且启用警告。 |
| Disable | 禁用注释，且禁用警告。 |
| Restore | 将所有设置恢复为项目设置。 |
| disable warnings | 禁用警告，注释不受影响。 |
| enable warnings | 启用警告，注释不受影响。 |
| restore warnings | 警告重置为项目设置；注释不受影响。 |
| disable annotations | 禁用注释，警告不受影响。 |
| enable annotations | 启用注释，警告不受影响。 |
| restore annotations | 注释重置为项目设置；警告不受影响。 |

## 可空引用类型实战

很大程度上由于更改的重要性，可空类型只有在使用不当才会抛出错误。将以下类添加到 Program.cs 文件中：

```
public class TestClass
{
    public string Name { get; set; }
    public int Age { get; set; }
}
```

正如你所见，这只是一个普通的类。可空性在你在使用该类于代码中时才体现出来。看以下声明：

```
string? nullableString = null;
TestClass? myNullableClass = null;
```

项目文件设置使整个项目成为可空上下文。可空上下文允许字符串和 TestClass 类型的声明使用可空注释（?）。以下代码行会因在可空上下文中将 null 分配给非可空类型而生成警告（CS8600）：

```
//Warning CS8600 Converting null literal or possible null value to non-nullable type
TestClass myNonNullableClass = myNullableClass;
```

为了更精细地控制项目中可空上下文的位置，你可以使用编译器指令（前面已讨论过）来启用或禁用上下文。以下代码关闭可空上下文（在项目级别设置），然后通过恢复项目设置重新启用它：

```
#nullable disable
TestClass anotherNullableClass = null;
//Warning CS8632 The annotation for nullable reference types
//should only be used in code within a '#nullable' annotations
TestClass? badDefinition = null;
//Warning CS8632 The annotation for nullable reference types
//should only be used in code within a '#nullable' annotations
string? anotherNullableString = null;
#nullable restore
```

最后说明一点，可空引用类型不具有 HasValue 和 Value 属性，因为这些是由 System.Nullable<T> 提供的。

## 迁移注意事项

当你将代码从 C# 7 迁移到 C# 8 或 C# 9 并希望使用可空引用类型时，可以使用项目设置和编译器指令的组合来逐步处理代码。常见做法是首先为整个项目启用警告并禁用可空注释。然后，随着你清理代码的各个区域，使用编译器指令逐步启用注释。

## 操作可空类型

C# 为操作可空类型提供了多个运算符。下一节将介绍空合并运算符、空合并赋值运算符和空条件运算符。针对这些示例，请返回到 FunWithNullableValueTypes 项目。

## 空合并运算符

需要了解的下一个方面是，任何可能具有 null 值的变量都可以使用 C# ?? 运算符，该运算符正式称为空合并运算符。此运算符允许你在检索到的值确实为 null 时为可空类型分配一个值。对于此示例，假设你希望将本地可空整数赋值为 100，如果 GetIntFromDatabase() 返回的值为 null（当然，此方法被编程为始终返回 null，但我相信你明白大致意思）。返回到 NullableValueTypes 项目（并将其设为启动项目），并输入以下代码：

```
//omitted for brevity
Console.WriteLine("***** Fun with Nullable Data *****
");
DatabaseReader dr = new DatabaseReader();

// If the value from GetIntFromDatabase() is null,
// assign local variable to 100.
int myData = dr.GetIntFromDatabase() ?? 100;
Console.WriteLine("Value of myData: {0}", myData);
Console.ReadLine();
```

使用 ?? 运算符的好处是它提供了传统 if/else 条件更紧凑的版本。然而，如果你愿意，你也可以编写以下功能等价的代码，以确保如果返回的值为 null，它确实会被设置为值 100：

```
// Longhand notation not using ?? syntax.
int? moreData = dr.GetIntFromDatabase();
if (!moreData.HasValue)
{
    moreData = 100;
}
Console.WriteLine("Value of moreData: {0}", moreData);
```

## 空合并赋值运算符（新增 8.0）

在空合并运算符的基础上，C# 8 引入了空合并赋值运算符（??=）。此运算符仅在左侧为 null 时将左侧赋值为右侧。例如，输入以下代码：

```
//Null-coalescing assignment operator
int? nullableInt = null;
nullableInt ??= 12;
nullableInt ??= 14;
Console.WriteLine(nullableInt);
```

nullableInt 变量初始化为 null。下一行将值 12 分配给变量，因为左侧确实为 null。下一行不会将 14 分配给变量，因为它不为 null。

## 空条件运算符

在编写软件时，检查传入参数（即从类型成员（方法、属性、索引器）返回的值）是否等于 null 是很常见的。例如，假设你有一个方法以字符串数组作为单个参数。为了安全起见，你可能希望在继续之前先测试是否为 null。这样，如果数组为空，你就不会得到运行时错误。以下是一种传统的执行此类检查的方式：

```
static void TesterMethod(string[] args)
{
    // We should check for null before accessing the array data!
    if (args != null)
    {
        Console.WriteLine($"You sent me {args.Length} arguments.");
    }
}
```

在这里，你使用条件作用域来确保只有在数组不为 null 时才会访问字符串数组的 Length 属性。如果调用者在未创建数据数组的情况下调用你的方法，如下所示，你仍然是安全的，不会触发运行时错误：

```
TesterMethod(null);
```

C# 包含空条件运算符标记（在变量类型后、访问运算符前放置一个问号），以简化之前的错误检查。无需显式构建条件语句来检查 null，你现在可以编写以下内容：

```
static void TesterMethod(string[] args)
{
    // We should check for null before accessing the array data!
    Console.WriteLine($"You sent me {args?.Length} arguments.");
}
```

在这种情况下，你没有使用条件语句。相反，你将 ? 运算符直接后缀到字符串数组变量之后。如果变量为 null，对 Length 属性的调用不会抛出运行时错误。如果你想打印实际值，可以利用空合并运算符来分配默认值，如下所示：

```
Console.WriteLine($"You sent me {args?.Length ?? 0} arguments.");
```

在处理委托和事件时，C# 6.0 的空条件运算符在一些额外的编码区域中也非常有用。这些主题将在本书后面讨论（参见第 12 章），你将看到更多示例。

## 理解元组（新增/更新 7.0）

为了结束本章，让我们使用名为 FunWithTuples 的控制台应用程序项目来检查元组的作用。如本章前面所述，使用 out 参数的一种方法是让方法调用返回多个值。另一种方法是使用一种称为元组的轻量级构造。

元组是包含多个字段的轻量级数据结构。它们在 C# 6 中被添加到语言中，但方式极其有限。C# 6 实现中还有一个潜在的重大问题：每个字段都实现为引用类型，可能导致内存和/或性能问题（来自装箱/拆箱）。

在 C# 7 中，元组使用新的 ValueTuple 数据类型而不是引用类型，可能节省大量内存。ValueTuple 数据类型根据元组的属性数量创建不同的结构体。C# 7 中添加的另一个功能是元组中的每个属性都可以分配一个特定名称（就像变量一样），大大增强了可用性。





这些是元组的两个重要考虑因素：

- 字段不会被验证。
- 你无法定义自己的方法。

它们被设计为仅作为轻量级的数据传输机制。

## 开始使用元组

理论够了。让我们写一些代码！要创建元组，只需将要分配给元组的值用括号括起来，如下所示：

```
("a", 5, "c")
```

请注意，它们不一定都是相同的数据类型。圆括号结构也用于将元组分配给变量（或者你可以使用 var 关键字，编译器会为你分配数据类型）。要将前面的示例分配给变量，以下两行实现了相同的效果。values 变量将是一个元组，其中包含两个字符串属性和一个夹在中间的 int 属性。

```
(string, int, string) values = ("a", 5, "c");
var values = ("a", 5, "c");
```

默认情况下，编译器为每个属性分配名称 ItemX，其中 X 表示元组中基于一的位置。对于前面的示例，属性名称为 Item1、Item2 和 Item3。访问它们的方式如下：

```
Console.WriteLine($"First item: {values.Item1}");
Console.WriteLine($"Second item: {values.Item2}");
Console.WriteLine($"Third item: {values.Item3}");
```

也可以在语句的右侧或左侧为元组中的每个属性添加特定名称。虽然在语句的两侧同时指定名称不会导致编译错误，但如果这样做，右侧的名称将被忽略，只使用左侧的名称。以下两行代码展示了在左侧和右侧设置名称以达到相同目的：

```
(string FirstLetter, int TheNumber, string SecondLetter) valuesWithNames = ("a", 5, "c");
var valuesWithNames2 = (FirstLetter: "a", TheNumber: 5, SecondLetter: "c");
```

现在可以使用字段名称以及 ItemX 记法来访问元组上的属性，如以下代码所示：

```
Console.WriteLine($"First item: {valuesWithNames.FirstLetter}");
Console.WriteLine($"Second item: {valuesWithNames.TheNumber}");
Console.WriteLine($"Third item: {valuesWithNames.SecondLetter}");
//Using the item notation still works!
Console.WriteLine($"First item: {valuesWithNames.Item1}");
Console.WriteLine($"Second item: {valuesWithNames.Item2}");
Console.WriteLine($"Third item: {valuesWithNames.Item3}");
```

请注意，当在右侧设置名称时，你必须使用关键字 var 来声明变量。显式设置数据类型（即使没有自定义名称）会触发编译器使用左侧，分配属性使用 ItemX 记法，并忽略右侧设置的任何自定义名称。以下两个示例忽略了 Custom1 和 Custom2 名称：

```
(int, int) example = (Custom1:5, Custom2:7);
(int Field1, int Field2) example = (Custom1:5, Custom2:7);
```

还需要注意的是，自定义字段名称仅在编译时存在，在使用反射检查元组时在运行时不可用（反射在第 17 章中介绍）。

元组也可以嵌套为元组中的元组。由于元组中的每个属性都是数据类型，而元组本身也是数据类型，因此以下代码完全合法：

```
Console.WriteLine("=> Nested Tuples");
var nt = (5, 4, ("a", "b"));
```

## 使用推断变量名（更新 7.1）

C# 7.1 对元组的更新是 C# 能够推断元组的变量名，如下所示：

```
Console.WriteLine("=> Inferred Tuple Names");
var foo = new {Prop1 = "first", Prop2 = "second"};
var bar = (foo.Prop1, foo.Prop2);
Console.WriteLine($"{bar.Prop1};{bar.Prop2}");
```

## 理解元组相等/不相等（新增 7.3）

C# 7.1 中新增的功能是元组相等（==）和不相等（!=）。在测试不相等时，比较运算符会对元组中的数据类型执行隐式转换，包括比较可空和不可空的元组和/或属性。这意味着以下测试完全有效，尽管 int/long 之间存在差异：

```
Console.WriteLine("=> Tuples Equality/Inequality");
// lifted conversions
var left = (a: 5, b: 10);
(int? a, int? b) nullableMembers = (5, 10);
Console.WriteLine(left == nullableMembers); // Also true
// converted type of left is (long, long)
(long a, long b) longTuple = (5, 10);
Console.WriteLine(left == longTuple); // Also true
// comparisons performed on (long, long) tuples
(long a, int b) longFirst = (5, 10);
(int a, long b) longSecond = (5, 10);
Console.WriteLine(longFirst == longSecond); // Also true
```

包含元组的元组也可以进行比较，但前提是它们具有相同的形状。你不能将三个 int 属性的元组与两个 int 和一个元组的元组进行比较。

## 理解元组作为方法返回值

本章前面使用 out 参数从方法调用返回多个值。还有其他方法可以做到这一点，例如专门创建类或结构来返回这些值。但如果这个类或结构仅用于作为一个方法的数据传输，那就是额外的工作和不需要开发的额外代码。元组非常适合此任务，轻量级，易于声明和使用。

这是来自 out 参数部分的示例之一。它返回三个值，但需要将三个参数作为传输机制传递给调用代码。

```
static void FillTheseValues(out int a, out string b, out bool c)
{
    a = 9;
    b = "Enjoy your string.";
    c = true;
}
```

使用元组，你可以去掉参数并仍然获得这三个值。

```
static (int a,string b,bool c) FillTheseValues()
{
    return (9,"Enjoy your string.",true);
}
```

调用此方法与调用任何其他方法一样简单。

```
var samples = FillTheseValues();
Console.WriteLine($"Int is: {samples.a}");
Console.WriteLine($"String is: {samples.b}");
Console.WriteLine($"Boolean is: {samples.c}");
```

也许更好的例子是将全名分解为其各个部分（名、中间名、姓）。以下代码接收全名并返回包含不同部分的元组：

```
static (string first, string middle, string last) SplitNames(string fullName)
{
    //do what is needed to split the name apart
    return ("Philip", "F", "Japikse");
}
```

## 理解元组中的丢弃

延续 SplitNames() 示例，假设你只知道需要名字和姓氏，而不关心中间名。通过为你想要返回的值提供变量名，并用下划线（_）占位符填充不需要的值，可以像这样细化返回值：

```
var (first, _, last) = SplitNames("Philip F Japikse");
Console.WriteLine($"{first}:{last}");
```

元组的中间名值被丢弃。

## 理解元组模式匹配 switch 表达式（新增 8.0）

现在你对元组有了透彻的理解，是时候重新审视第 3 章中的元组 switch 表达式了。以下是示例再次：

```
//Switch expression with Tuples
static string RockPaperScissors(string first, string second)
{
    return (first, second) switch
    {
        ("rock", "paper") => "Paper wins.",
        ("rock", "scissors") => "Rock wins.",
        ("paper", "rock") => "Paper wins.",
        ("paper", "scissors") => "Scissors wins.",
        ("scissors", "rock") => "Rock wins.",
        ("scissors", "paper") => "Scissors wins.",
        (_, _) => "Tie.",
    };
}
```

在此示例中，两个参数在传递到 switch 表达式时被转换为元组。相关值在 switch 表达式中表示，任何其他情况都由最终元组处理，该元组由两个丢弃组成。

RockPaperScissors() 方法签名也可以写成接收元组，如下所示：

```
static string RockPaperScissors(
    (string first, string second) value)
{
    return value switch
    {
        //omitted for brevity
    };
}
```

## 解构元组

解构是指将元组的属性分离出来单独使用这一术语。FillTheseValues() 就是这样做的。但这种模式还有另一种用途可能有所帮助，那就是解构自定义类型。

取本章前面使用过的 Point 结构的简短版本。添加了一个名为 Deconstruct() 的新方法，用于将 Point 实例的各个属性作为具有 XPos 和 YPos 属性的元组返回。

```
struct Point
{
    // Fields of the structure.
    public int X;
    public int Y;
```





// A custom constructor.
public Point(int XPos, int YPos)
{
    X = XPos;
    Y = YPos;
}

public (int XPos, int YPos) Deconstruct() => (X, Y);
}

注意上一个代码清单中以粗体显示的新 `Deconstruct()` 方法。这个方法可以命名为任意名称，但按照约定，通常命名为 `Deconstruct()`。这允许通过一次方法调用，并返回元组来获取结构的各个值。

```
Point p = new Point(7,5);
var pointValues = p.Deconstruct();
Console.WriteLine($"X is: {pointValues.XPos}");
Console.WriteLine($"Y is: {pointValues.YPos}");
```

## 使用位置模式匹配解构元组（新增于 8.0）

当元组具有可访问的 `Deconstruct()` 方法时，可以在基于元组的 switch 表达式中使用该解构。以下代码使用 Point 示例，利用生成的元组，并将这些值用于每个表达式的 `when` 子句：

```
static string GetQuadrant1(Point p)
{
    return p.Deconstruct() switch
    {
        (0, 0) => "Origin",
        var (x, y) when x > 0 && y > 0 => "One",
        var (x, y) when x < 0 && y > 0 => "Two",
        var (x, y) when x < 0 && y < 0 => "Three",
        var (x, y) when x > 0 && y < 0 => "Four",
        var (_, _) => "Border",
    };
}
```

如果 `Deconstruct()` 方法定义了两个 `out` 参数，那么 switch 表达式将自动解构该点。向 Point 添加另一个 Deconstruct 方法，如下所示：

```
public void Deconstruct(out int XPos, out int YPos)
=> (XPos,YPos)=(X, Y);
```

现在，您可以将 `GetQuadrant()` 方法更新（或添加一个新方法）为：

```
static string GetQuadrant2(Point p)
{
    return p switch
    {
        (0, 0) => "Origin",
```

```
var (x, y) when x > 0 && y > 0 => "One",
var (x, y) when x < 0 && y > 0 => "Two",
var (x, y) when x < 0 && y < 0 => "Three",
var (x, y) when x > 0 && y < 0 => "Four",
var (_, _) => "Border",
};
}
```

这个变化非常微妙（并以粗体突出显示）。不是调用 `p.Deconstruct()`，而是在 switch 表达式中仅使用 Point 变量。

## 小结

本章首先研究了数组。然后，我们讨论了允许您构建自定义方法的 C# 关键字。回想一下，默认情况下参数按值传递；但是，如果您使用 `ref` 或 `out` 标记参数，则可以通过引用传递参数。您还了解了可选参数和命名参数的作用，以及如何定义和调用接受参数数组的方法。

在研究了方法重载这一主题之后，本章的大部分内容研究了在 C# 中如何定义枚举和结构体，以及它们如何在 .NET Core 基础类库中表示。在此过程中，您还研究了值类型和引用类型的几个细节，包括它们作为参数传递给方法时的响应方式，以及如何使用 `?`、`??` 和 `??=` 运算符与可空数据类型和可能为 null 的变量（例如引用类型变量和可空值类型变量）进行交互。

本章最后一节研究了 C# 中期待已久的功能——元组。在了解了它们是什么以及它们如何工作之后，您使用它们从方法返回多个值，以及解构自定义类型。

在第 5 章中，您将开始深入研究面向对象开发的细节。
