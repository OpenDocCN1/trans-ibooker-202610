

虽然这是一个简单的例子，但它确实展示了默认接口的一个问题。在将此功能用于你的代码之前，请确保衡量调用代码必须知道实现存在的位置所带来的影响。

## 静态构造函数和成员（新增于 8.0）

C# 8.0 中对接口的另一个新增功能是能够拥有静态构造函数和成员，其功能与类定义上的静态成员相同，但定义在接口上。用一个示例静态属性和一个静态构造函数更新 IRegularPointy 接口。

```
interface IRegularPointy : IPointy
{
    int SideLength { get; set; }
    int NumberOfSides { get; set; }
    int Perimeter => SideLength * NumberOfSides;

    //Static members are also allowed in C# 8
    static string ExampleProperty { get; set; }

    static IRegularPointy() => ExampleProperty = "Foo";
}
```

静态构造函数必须无参数，并且只能访问静态属性和方法。要访问接口静态属性，将以下代码添加到顶层语句中：

```
Console.WriteLine($"Example property: {IRegularPointy.ExampleProperty}");
IRegularPointy.ExampleProperty = "Updated";
Console.WriteLine($"Example property: {IRegularPointy.ExampleProperty}");
```

注意静态属性必须从接口调用，而不是实例变量。

## 接口作为参数

由于接口是有效类型，你可以构造接受接口作为参数的方法，如本章前面 CloneMe() 方法所示。对于当前示例，假设你已定义了另一个名为 IDraw3D 的接口。

```
namespace CustomInterfaces
{
    // Models the ability to render a type in stunning 3D.
    public interface IDraw3D
    {
        void Draw3D();
    }
}
```

接下来，假设你的三个形状中有两个（ThreeDCircle 和 Hexagon）已配置为支持此新行为。

```
// Circle supports IDraw3D.
class ThreeDCircle : Circle, IDraw3D
{
...
public void Draw3D()
    => Console.WriteLine("Drawing Circle in 3D!"); }
}
```

```
// Hexagon supports IPointy and IDraw3D.
class Hexagon : Shape, IPointy, IDraw3D
{
...
public void Draw3D()
    => Console.WriteLine("Drawing Hexagon in 3D!");
}
```

图 8-2 展示了更新后的 Visual Studio 类图。

![](img/587021f4d5ba039be70a11107130377b_356_0.png)

图 8-2. 更新后的形状层次结构

如果现在定义一个以 IDraw3D 接口作为参数的方法，你可以有效地传入任何实现 IDraw3D 的对象。如果尝试传入不支持必要接口的类型，你会收到编译时错误。考虑在 Program 类中定义的以下方法：

```
// I'll draw anyone supporting IDraw3D.
static void DrawIn3D(IDraw3D itf3d)
{
    Console.WriteLine("- > Drawing IDraw3D compatible type");
    itf3d.Draw3D();
}
```

你现在可以测试 Shape 数组中的项目是否支持此新接口，如果支持，就将其传入 DrawIn3D() 方法进行处理。

```
Console.WriteLine("***** Fun with Interfaces *****
");
Shape[] myShapes = { new Hexagon(), new Circle(),
    new Triangle("Joe"), new Circle("JoJo") };
for(int i = 0; i < myShapes.Length; i++)
{
    // Can I draw you in 3D?
    if (myShapes[i] is IDraw3D s)
    {
        DrawIn3D(s);
    }
}
```

以下是更新后应用程序的输出。注意只有 Hexagon 对象以 3D 打印出来，因为 Shape 数组的其他成员未实现 IDraw3D 接口。

```
***** Fun with Interfaces *****
...
-> Drawing IDraw3D compatible type
Drawing Hexagon in 3D!
```

## 接口作为返回值

接口也可以用作方法返回值。例如，你可以编写一个方法，接受 Shape 对象数组并返回第一个支持 IPointy 的项目的引用。

```
// This method returns the first object in the
// array that implements IPointy.
static IPointy FindFirstPointyShape(Shape[] shapes)
{
    foreach (Shape s in shapes)
    {
        if (s is IPointy ip)
        {
            return ip;
        }
    }
    return null;
}
```

你可以按如下方式与此方法交互：

```
Console.WriteLine("***** Fun with Interfaces *****
");
// Make an array of Shapes.
Shape[] myShapes = { new Hexagon(), new Circle(),
    new Triangle("Joe"), new Circle("JoJo")};

// Get first pointy item.
IPointy firstPointyItem = FindFirstPointyShape(myShapes);
// To be safe, use the null conditional operator.
Console.WriteLine("The item has {0} points",
    firstPointyItem?.Points);
```

## 接口类型数组

请记住，同一个接口可以由众多类型实现，即使它们不在同一个类层次结构中，并且除了 System.Object 之外没有共同的父类。这可以产生一些强大的编程结构。例如，假设你在当前项目中开发了三种新的类类型，用于建模厨房用具（通过 Knife 和 Fork 类）和另一种建模园艺设备（à la PitchFork）。这些类的相关代码如下所示，更新后的类图如图 8-3 所示：

```
//Fork.cs
namespace CustomInterfaces
{
    class Fork : IPointy
    {
        public byte Points => 4;
    }
}
//PitchFork.cs
namespace CustomInterfaces
{
    class PitchFork : IPointy
    {
        public byte Points => 3;
    }
}
//Knife.cs.cs
namespace CustomInterfaces
{
    class Knife : IPointy
    {
        public byte Points => 1;
    }
}
```

![](img/587021f4d5ba039be70a11107130377b_359_0.png)

图 8-3. 请记住，接口可以“插入”到类层次结构任何部分的任何类型中

如果定义了 PitchFork、Fork 和 Knife 类型，你现在可以定义一个 IPointy 兼容对象的数组。由于这些成员都支持相同的接口，你可以遍历数组并将每个项目视为 IPointy 兼容对象，而不管类层次结构的整体多样性如何。

```
// This array can only contain types that
// implement the IPointy interface.
IPointy[] myPointyObjects = {new Hexagon(), new Knife(),
    new Triangle(), new Fork(), new PitchFork()};

foreach(IPointy i in myPointyObjects)
{
    Console.WriteLine("Object has {0} points.", i.Points);
}
Console.ReadLine();
```

为了突出此示例的重要性，请记住这一点：当你有一个给定接口的数组时，数组可以包含实现该接口的任何类或结构。

## 使用自动方式实现接口

尽管基于接口的编程是一种强大的技术，但实现接口可能需要大量的输入。鉴于接口是一组命名的抽象成员，你需要在支持该行为的每个类型上输入每个接口方法的定义和实现。因此，如果你想支持定义了总共五个方法和三个属性的接口，你需要处理全部八个成员（否则你会收到编译器错误）。

正如你所希望的，Visual Studio 和 Visual Studio Code 都支持各种工具，使实现接口的任务负担更小。通过一个简单的测试，在当前项目中插入一个最终类，命名为 PointyTestClass。当你向类类型添加接口（如 IPointy 或任何接口）时，你可能注意到当你完成输入接口名称时（或当你在代码窗口中将鼠标光标定位在接口名称上时），Visual Studio 和 Visual Studio Code 都会添加一个灯泡，该灯泡也可以通过 Ctrl+句点 (.) 键组合调用。当你单击灯泡时，你将看到一个下拉列表，允许你实现接口（参见图 8-4 和 8-5）。

![](img/587021f4d5ba039be70a11107130377b_360_0.png)

图 8-4. 使用 Visual Studio Code 自动实现接口

![](img/587021f4d5ba039be70a11107130377b_360_1.png)

图 8-5. 使用 Visual Studio 自动实现接口

注意你会看到两个选项，其中第二个（显式接口实现）将在下一节中讨论。目前，选择第一个选项，你会看到 Visual Studio/Visual Studio Code 已为你生成了存根代码以供更新。（注意默认实现会抛出 System.NotImplementedException，这显然可以删除。）

