

// This struct implicitly derives from System.ValueType and
// implements two interfaces.
public struct PitchFork : ICloneable, IPointy
{...}

理解这一点很重要：对于不包含默认实现的接口成员，实现接口是一个全有或全无的命题。实现类型不能选择性决定它将实现哪些成员。鉴于 IPointy 接口只定义了一个只读属性，这还算不上沉重的负担。但是，如果你实现的是一个定义了十个成员的接口（例如前面展示的 IDbConnection 接口），该类型现在就有责任充实全部十个抽象成员的细节。

例如，插入一个名为 Triangle 的新类类型，它“is-a”（是一种）Shape 并支持 IPointy。请注意，只读 Points 属性的实现（使用表达式体成员语法实现）只是返回正确的点数（三个）。

```
using System;
namespace CustomInterfaces
{
    // New Shape derived class named Triangle.
    class Triangle : Shape, IPointy
    {
        public Triangle() { }
        public Triangle(string name) : base(name) { }
        public override void Draw()
        {
            Console.WriteLine("Drawing {0} the Triangle", PetName);
        }

        // IPointy implementation.
        //public byte Points
        //{
        //    get { return 3; }
        //}
        public byte Points => 3;
    }
}
```

现在，更新你现有的 Hexagon 类型，使其也支持 IPointy 接口类型。

```
using System;
namespace CustomInterfaces
{
    // Hexagon now implements IPointy.
    class Hexagon : Shape, IPointy
    {
        public Hexagon(){ }
        public Hexagon(string name) : base(name){ }
        public override void Draw()
        {
            Console.WriteLine("Drawing {0} the Hexagon", PetName);
        }
```
```
// IPointy implementation.
public byte Points => 6;
}
```

到目前为止，概括一下：图 8-1 所示的 Visual Studio 类图使用流行的“棒棒糖”表示法展示了与 IPointy 兼容的类。请再次注意，Circle 和 ThreeDCircle 没有实现 IPointy，因为这种表现对这两个类来说没有意义。

![](img/587021f4d5ba039be70a11107130377b_351_0.png)

图 8-1 形状层次结构，现在包含接口

注意：要在类设计器中显示或隐藏接口名称，请右键单击接口图标，然后选择“折叠”或“展开”选项。

## 在对象级别调用接口成员

现在你已经有了一些支持 IPointy 接口的类，接下来的问题是如何与新功能进行交互。与给定接口提供的功能进行交互的最直接方式，是在对象级别直接调用这些成员（前提是接口成员没有被显式实现；你可以在后面的“实现显式接口”一节中了解更多细节）。例如，考虑以下代码：

```
Console.WriteLine("***** Fun with Interfaces *****
");
// Call Points property defined by IPointy.
Hexagon hex = new Hexagon();
Console.WriteLine("Points: {0}", hex.Points);
Console.ReadLine();
```

在这种情况下，这种方法很有效，因为你理解 Hexagon 类型已经实现了所讨论的接口，因此拥有 Points 属性。然而，在其他情况下，你可能无法确定给定类型支持哪些接口。例如，假设你有一个包含 50 个 Shape 兼容类型的数组，其中只有部分支持 IPointy。显然，如果你尝试在未实现 IPointy 的类型上调用 Points 属性，就会收到错误。那么，你如何才能动态判断某个类或结构是否支持正确的接口呢？

在运行时确定类型是否支持特定接口的一种方法是使用显式强制转换。如果该类型不支持所请求的接口，你会收到 InvalidCastException。为了妥善处理这种可能性，可以使用结构化异常处理，如下例所示：

```
...
// Catch a possible InvalidCastException.
Circle c = new Circle("Lisa");
IPointy itfPt = null;
try
{
    itfPt = (IPointy)c;
    Console.WriteLine(itfPt.Points);
}
catch (InvalidCastException e)
{
    Console.WriteLine(e.Message);
}
Console.ReadLine();
```

虽然你也可以使用 try/catch 逻辑并寄希望于一切顺利，但最好还是先确定支持哪些接口，然后再调用接口成员。让我们看看两种实现方法。

## 获取接口引用：as 关键字

你可以使用第 6 章中介绍的 as 关键字来确定给定类型是否支持某个接口。如果该对象可以被视为指定的接口，则会返回所讨论接口的引用；否则，你将收到 null 引用。因此，在继续操作之前，一定要检查是否为 null 值。

```
...
// Can we treat hex2 as IPointy?
Hexagon hex2 = new Hexagon("Peter");
IPointy itfPt2 = hex2 as IPointy;
if(itfPt2 != null)
{
    Console.WriteLine("Points: {0}", itfPt2.Points);
}
```
```
else
{
    Console.WriteLine("OOPS! Not pointy...");
}
Console.ReadLine();
```

请注意，使用 as 关键字时，你不需要使用 try/catch 逻辑；如果引用不为 null，你就知道所调用的是有效的接口引用。

## 获取接口引用：is 关键字（7.0 更新）

你还可以使用 is 关键字来检查已实现的接口（第 6 章中也首次讨论过）。如果所讨论的对象与指定接口不兼容，你将得到 false。如果在语句中提供了变量名，则该类型会被赋值到该变量中，从而无需进行类型检查和强制转换。前面的示例在这里更新为：

```
Console.WriteLine("***** Fun with Interfaces *****
");
...
if(hex2 is IPointy itfPt3)
{
    Console.WriteLine("Points: {0}", itfPt3.Points);
}
else
{
    Console.WriteLine("OOPS! Not pointy...");
}
Console.ReadLine();
```

## 默认实现（8.0 新增）

如前所述，C# 8.0 增加了让接口方法和属性拥有默认实现的能力。添加一个名为 IRegularPointy 的新接口，用于表示规则形状的多边形。代码如下所示：

```
namespace CustomInterfaces
{
    interface IRegularPointy : IPointy
    {
        int SideLength { get; set; }
        int NumberOfSides { get; set; }
        int Perimeter => SideLength * NumberOfSides;
    }
}
```

向项目中添加一个名为 Square.cs 的新类，继承 Shape 基类，并实现 IRegularPointy 接口，如下所示：

```
namespace CustomInterfaces
{
    class Square: Shape,IRegularPointy
    {
        public Square() { }
        public Square(string name) : base(name) { }
        //Draw comes from the Shape base class
        public override void Draw()
        {
            Console.WriteLine("Drawing a square");
        }

        //This comes from the IPointy interface
        public byte Points => 4;
        //These come from the IRegularPointy interface
        public int SideLength { get; set; }
        public int NumberOfSides { get; set; }
        //Note that the Perimeter property is not implemented
    }
}
```

在这里，我们无意中引入了在接口中使用默认实现的第一个“棘手之处”。Perimeter 属性定义在 IRegularPointy 接口上，但并未在 Square 类中定义，因此无法通过 Square 的实例访问它。为了直观地看到这一点，创建一个新的 Square 类实例，并将相关值输出到控制台，如下所示：

```
Console.WriteLine("\n**** Fun with Interfaces ****\n");
...
var sq = new Square("Boxy")
    {NumberOfSides = 4, SideLength = 4};
sq.Draw();
//This won’t compile
//Console.WriteLine($"{sq.PetName} has {sq.NumberOfSides} of length {sq.SideLength} and a perimeter of {sq.Perimeter}");
```

相反，必须将 Square 实例显式强制转换为 IRegularPointy 接口（因为实现就在该接口中），然后才能访问 Perimeter 属性。将代码更新为以下内容：

```
Console.WriteLine($"{sq.PetName} has {sq.NumberOfSides} of length {sq.SideLength} and a perimeter of {((IRegularPointy)sq).Perimeter}");
```

解决此问题的一种方法是始终面向类型的接口进行编码。将 Square 实例的定义改为 IRegularPointy，而不是 Square，如下所示：

```
IRegularPointy sq = new Square("Boxy") {NumberOfSides = 4, SideLength = 4};
```

这种方法的问题（就我们的情况而言）是，Draw() 方法和 PetName 属性并未在接口上定义，从而导致编译错误。

