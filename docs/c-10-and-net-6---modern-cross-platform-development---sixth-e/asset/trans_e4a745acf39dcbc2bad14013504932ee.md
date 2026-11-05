
在 Person 类内部，编写语句声明两个公共字段用于存储一个人的姓名和出生日期，如以下代码所示：

```
public class Person : object
{
    // fields
    public string Name;
    public DateTime DateOfBirth;
}
```

你可以使用任何类型作为字段，包括数组和集合，如列表和字典。如果需要在一个命名字段中存储多个值，就会使用它们。在此示例中，一个人只有一个姓名和一个出生日期。

## 理解访问修饰符

封装的一部分是选择成员的可见性。

请注意，正如我们对类所做的那样，我们明确地将 public 关键字应用于这些字段。如果没有这样做，它们将隐式地对类私有，这意味着它们只能在类内部访问。

有四个访问修饰符关键字，以及两个可以应用于类成员（如字段或方法）的访问修饰符关键字组合，如下表所示：

| 访问修饰符 | 说明 |
|---|---|
| private | 成员只能在类型内部访问。这是默认值。 |
| internal | 成员可在类型内部以及同一程序集中的任何类型中访问。 |
| protected | 成员可在类型内部以及继承自该类型的任何类型中访问。 |
| public | 成员可在任何地方访问。 |
| internal protected | 成员可在类型内部、同一程序集中的任何类型以及继承自该类型的任何类型中访问。等效于名为 internal_or_protected 的虚构访问修饰符。 |
| private protected | 成员可在类型内部以及继承自该类型且位于同一程序集中的任何类型中访问。等效于名为 internal_and_protected 的虚构访问修饰符。此组合仅在 C# 7.2 或更高版本中可用。 |

> 良好实践：显式地将其中一个访问修饰符应用于所有类型成员，即使你想使用成员的隐式访问修饰符，即 private。此外，字段通常应为 private 或 protected，然后你应创建公共属性来获取或设置字段值。这是因为它控制访问。你将在本章后面执行此操作。

## 设置和输出字段值

现在我们将在你的代码中使用这些字段：

- 1. 在 Program.cs 顶部，确保导入了 System 命名空间。我们需要这样做才能使用 DateTime 类型。
- 2. 在实例化 bob 之后，添加语句设置他的姓名和出生日期，然后以漂亮的格式输出这些字段，如以下代码所示：
- 3. 运行代码并查看结果，如以下输出所示：
- 4. 在现有代码下方添加语句以创建另一个名为 Alice 的新人员。注意在将她写入控制台时出生日期的格式代码不同，如以下代码所示：
- 5. 运行代码并查看结果，如以下输出所示：

```
bob.Name = "Bob Smith";
bob.DateOfBirth = new DateTime(1965, 12, 22); // C# 1.0 or later

WriteLine(format: "{0} was born on {1:dddd, d MMMM yyyy}",
    arg0: bob.Name,
    arg1: bob.DateOfBirth);
```

我们也可以使用字符串插值，但对于长字符串，它会在多行上换行，这在印刷书籍中可能更难阅读。在本书的代码示例中，请记住 {0} 是 arg0 的占位符，以此类推。

```
Bob Smith was born on Wednesday, 22 December 1965
```

你的输出可能因你的区域设置（即语言和文化）而有所不同。

arg1 的格式代码由几个部分组成。dddd 表示星期几的名称。d 表示月份中的日期数字。MMMM 表示月份的名称。小写 m 用于时间值中的分钟。yyyy 表示年份的完整数字。yy 表示两位数的年份。

你也可以使用花括号的简写对象初始值设定项语法来初始化字段。我们来看看如何操作。

```
Person alice = new()
{
    Name = "Alice Jones",
    DateOfBirth = new(1998, 3, 7) // C# 9.0 or later
};

WriteLine(format: "{0} was born on {1:dd MMM yy}",
    arg0: alice.Name,
    arg1: alice.DateOfBirth);
```

```
Alice Jones was born on 07 Mar 98
```

## 使用枚举类型存储值

有时，一个值需要是有限选项集中的一个。例如，世界上有七大奇迹，一个人可能有一个最喜欢的。有时，一个值需要是有限选项集的组合。例如，一个人可能有一个想去参观的古代世界奇迹清单。我们可以通过定义枚举类型来存储此数据。

枚举类型是存储一个或多个选择非常有效的方式，因为在内部，它使用整数值结合字符串描述的查找表：

- 1. 在 PacktLibrary 项目中添加一个名为 `WondersOfTheAncientWorld.cs` 的新文件。
- 2. 修改 `WondersOfTheAncientWorld.cs` 文件，如以下代码所示：
- 3. 在 Person 类中，将以下语句添加到你的字段列表中：
- 4. 在 Program.cs 中，添加以下语句：

```csharp
namespace Packt.Shared
{
    public enum WondersOfTheAncientWorld
    {
        GreatPyramidOfGiza,
        HangingGardensOfBabylon,
        StatueOfZeusAtOlympia,
        TempleOfArtemisAtEphesus,
        MausoleumAtHalicarnassus,
        ColossusOfRhodes,
        LighthouseOfAlexandria
    }
}
```

> **良好实践：** 如果你正在 .NET Interactive 笔记本中编写代码，那么包含枚举的代码单元必须位于定义 Person 类的代码单元之上。

```csharp
public WondersOfTheAncientWorld FavoriteAncientWonder;
```

```csharp
bob.FavoriteAncientWonder = WondersOfTheAncientWorld.StatueOfZeusAtOlympia;

WriteLine(
    format: "{0}'s favorite wonder is {1}. Its integer is {2}.",
    arg0: bob.Name,
    arg1: bob.FavoriteAncientWonder,
    arg2: (int)bob.FavoriteAncientWonder);
```

- 5. 运行代码并查看结果，如以下输出所示：

```
Bob Smith's favorite wonder is StatueOfZeusAtOlympia. Its integer is 2.
```

枚举值在内部以 int 存储以提高效率。int 值会自动从 0 开始分配，因此我们枚举中的第三个世界奇迹的值为 2。你可以分配枚举中未列出的 int 值。它们将作为 int 值输出而不是名称，因为找不到匹配项。

## 使用枚举类型存储多个值

对于清单，我们可以创建一个枚举实例的数组或集合，集合将在本章后面解释，但有更好的方法。我们可以使用枚举标志将多个选择组合成一个值：

- 1. 通过用 [System.Flags] 属性装饰枚举来修改它，并显式为每个奇迹设置一个表示不同位列的字节值，如以下代码中突出显示的那样：

```
namespace Packt.Shared
{
    [System.Flags]
    public enum WondersOfTheAncientWorld : byte
    {
        None = 0b_0000_0000, // i.e. 0
        GreatPyramidOfGiza = 0b_0000_0001, // i.e. 1
        HangingGardensOfBabylon = 0b_0000_0010, // i.e. 2
        StatueOfZeusAtOlympia = 0b_0000_0100, // i.e. 4
        TempleOfArtemisAtEphesus = 0b_0000_1000, // i.e. 8
        MausoleumAtHalicarnassus = 0b_0001_0000, // i.e. 16
        ColossusOfRhodes = 0b_0010_0000, // i.e. 32
        LighthouseOfAlexandria = 0b_0100_0000 // i.e. 64
    }
}
```

我们为每个选择分配显式值，以便在查看内存中存储的位时不会重叠。我们还应使用 System.Flags 属性装饰枚举类型，以便在返回该值时，它可以自动与多个值匹配为逗号分隔的字符串，而不是返回 int 值。

通常，枚举类型在内部使用 int 变量，但由于我们不需要那么大的值，我们可以通过告诉它使用字节变量，将内存需求减少 75%，即每个值 1 字节而不是 4 字节。

如果我们想表示我们的清单包括巴比伦空中花园和哈利卡纳苏斯陵墓古代世界奇迹，那么我们希望 16 和 2 位设置为 1。换句话说，我们将存储值 18：
