
```
Console.ReadLine();

static IEnumerable<string> GetStringSubset()
{
    string[] colors = {"Light Red", "Green", "Yellow", "Dark Red", "Red", "Purple"};
    // Note subset is an IEnumerable<string>-compatible object.
    IEnumerable<string> theRedColors = from c in colors where c.Contains("Red") select c;
    return theRedColors;
}
```

结果符合预期。

```
Light Red
Dark Red
Red
```

## 通过立即执行返回 LINQ 结果

这个示例之所以能按预期工作，仅仅是因为 `GetStringSubset()` 的返回值以及该方法中的 LINQ 查询都是强类型的。如果你使用 `var` 关键字来定义 subset 变量，那么只有当方法仍然被声明为返回 `IEnumerable<string>`（并且隐式类型的局部变量实际上与指定的返回类型兼容）时，才允许返回该值。

因为操作 `IEnumerable<T>` 有点不方便，你可以使用立即执行。例如，你可以不返回 `IEnumerable<string>`，而是直接返回 `string[]`，只要将序列转换为强类型的数组。请看 `Program` 类的这个新方法，它正是这样做的：

```
static string[] GetStringSubsetAsArray()
{
    string[] colors = {"Light Red", "Green", "Yellow", "Dark Red", "Red", "Purple"};

    var theRedColors = from c in colors where c.Contains("Red") select c;

    // Map results into an array.
    return theRedColors.ToArray();
}
```

这样一来，调用者可以完全不知道结果来自 LINQ 查询，而只是像预期那样处理字符串数组。下面是一个示例：

```
foreach (string item in GetStringSubsetAsArray())
{
    Console.WriteLine(item);
}
```

当试图向调用者返回 LINQ 投影的结果时，立即执行也至关重要。本章稍后你将研究这个主题。接下来，让我们看看如何将 LINQ 查询应用于泛型和非泛型集合对象。

## 将 LINQ 查询应用到集合对象

除了从简单的数据数组中提取结果外，LINQ 查询表达式还可以操作 `System.Collections.Generic` 命名空间中的成员（例如 `List<T>` 类型）所包含的数据。创建一个名为 `LinqOverCollections` 的新控制台应用程序项目，并定义一个基本的 `Car` 类，该类维护当前速度、颜色、制造商和昵称，如以下代码所示：

```
namespace LinqOverCollections
{
    class Car
    {
        public string PetName {get; set;} = "";
        public string Color {get; set;} = "";
        public int Speed {get; set;}
        public string Make {get; set;} = "";
    }
}
```

现在，在顶层语句中，定义一个 `Car` 类型的局部变量 `List<T>`，并使用对象初始化语法向列表中填充几个新的 `Car` 对象。

```
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using LinqOverCollections;

Console.WriteLine("**** LINQ over Generic Collections ****\n");

// Make a List<> of Car objects.
List<Car> myCars = new List<Car>() {
    new Car{ PetName = "Henry", Color = "Silver", Speed = 100, Make = "BMW"},
    new Car{ PetName = "Daisy", Color = "Tan", Speed = 90, Make = "BMW"},
    new Car{ PetName = "Mary", Color = "Black", Speed = 55, Make = "VW"},
    new Car{ PetName = "Clunker", Color = "Rust", Speed = 5, Make = "Yugo"},
    new Car{ PetName = "Melvin", Color = "White", Speed = 43, Make = "Ford"}
};

Console.ReadLine();
```

### 访问包含的子对象

对泛型容器应用 LINQ 查询与对简单数组应用 LINQ 查询没有区别，因为 LINQ to Objects 可用于任何实现 `IEnumerable<T>` 的类型。这一次，你的目标是构建一个查询表达式，仅从 `myCars` 列表中选择速度大于 55 的 `Car` 对象。

获得子集后，你将通过调用 `PetName` 属性打印每个 `Car` 对象的名称。假设你有以下辅助方法（接受 `List<Car>` 参数），并从顶层语句调用它：

```
static void GetFastCars(List<Car> myCars)
{
    // Find all Car objects in the List<>, where the Speed is
    // greater than 55.
    var fastCars = from c in myCars where c.Speed > 55 select c;

    foreach (var car in fastCars)
    {
        Console.WriteLine("{0} is going too fast!", car.PetName);
    }
}
```

请注意，你的查询表达式只从 `List<T>` 中获取 `Speed` 属性大于 55 的那些项。如果运行应用程序，你会发现只有 Henry 和 Daisy 这两项符合搜索条件。

如果要构建更复杂的查询，你可能只想查找速度值大于 90 的 BMW 汽车。要做到这一点，只需使用 C# 的 `&&` 运算符构建一个复合布尔表达式。

```
static void GetFastBMWs(List<Car> myCars)
{
    // Find the fast BMWs!
    var fastCars = from c in myCars where c.Speed > 90 && c.Make == "BMW" select c;
    foreach (var car in fastCars)
    {
        Console.WriteLine("{0} is going too fast!", car.PetName);
    }
}
```

在这种情况下，唯一打印出来的名字是 Henry。

### 将 LINQ 查询应用于非泛型集合

回顾一下，LINQ 的查询运算符被设计为可与任何实现 `IEnumerable<T>` 的类型（直接实现或通过扩展方法实现）一起工作。鉴于 `System.Array` 已经提供了这种必要的基础设施，你可能会惊讶地发现 `System.Collections` 中的传统（非泛型）容器却没有。幸运的是，仍然可以使用泛型 `Enumerable.OfType<T>()` 扩展方法来迭代非泛型集合中包含的数据。

当从非泛型集合对象（例如 `ArrayList`）调用 `OfType<T>()` 时，只需指定容器中项的类型，即可提取一个兼容的 `IEnumerable<T>` 对象。在代码中，你可以使用隐式类型变量来存储此数据点。

请考虑以下新方法，它向 `ArrayList` 填充一组 `Car` 对象（确保将 `System.Collections` 命名空间导入到你的 `Program.cs` 文件中）：

```
static void LINQOverArrayList()
{
    Console.WriteLine("***** LINQ over ArrayList *****");
    // Here is a nongeneric collection of cars.
    ArrayList myCars = new ArrayList() {
        new Car{ PetName = "Henry", Color = "Silver", Speed = 100, Make = "BMW"},
        new Car{ PetName = "Daisy", Color = "Tan", Speed = 90, Make = "BMW"},
        new Car{ PetName = "Mary", Color = "Black", Speed = 55, Make = "VW"},
        new Car{ PetName = "Clunker", Color = "Rust", Speed = 5, Make = "Yugo"},
        new Car{ PetName = "Melvin", Color = "White", Speed = 43, Make = "Ford"}
    };

    // Transform ArrayList into an IEnumerable<T>-compatible type.
    var myCarsEnum = myCars.OfType<Car>();

    // Create a query expression targeting the compatible type.
    var fastCars = from c in myCarsEnum where c.Speed > 55 select c;
    foreach (var car in fastCars)
    {
        Console.WriteLine("{0} is going too fast!", car.PetName);
    }
}
```

与前面的示例一样，当从顶层语句调用此方法时，基于 LINQ 查询的格式，它将仅显示 Henry 和 Daisy 这两个名字。

### 使用 OfType<T>() 过滤数据

如你所知，非泛型类型可以包含任意组合的项，因为这些容器（再次以 `ArrayList` 为例）的成员被设计为接收 `System.Object` 类型。例如，假设一个 `ArrayList` 包含多种项，其中只有一部分是数值。如果你想获得只包含数值数据的子集，可以使用 `OfType<T>()`，因为它会在迭代期间过滤掉类型与给定类型不同的每个元素。

```
static void OfTypeAsFilter()
{
    // Extract the ints from the ArrayList.
    ArrayList myStuff = new ArrayList();
    myStuff.AddRange(new object[] { 10, 400, 8, false, new Car(), "string data" });
    var myInts = myStuff.OfType<int>();

    // Prints out 10, 400, and 8.
    foreach (int i in myInts)
    {
        Console.WriteLine("Int value: {0}", i);
    }
}
```

到目前为止，你已经有机会将 LINQ 查询应用于数组、泛型集合和非泛型集合。这些容器既包含 C# 原始类型（整数、字符串数据），也包含自定义类。下一个任务是了解许多额外的 LINQ 运算符，这些运算符可用于构建更复杂、更有用的查询。
