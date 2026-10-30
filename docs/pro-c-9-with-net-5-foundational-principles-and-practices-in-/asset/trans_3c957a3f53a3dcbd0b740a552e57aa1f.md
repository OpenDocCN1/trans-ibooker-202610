

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

