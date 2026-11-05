
1. 在 Records.cs 中，添加一个名为 ImmutableVehicle 的记录，如以下代码所示：

```
public record ImmutableVehicle
{
    public int Wheels { get; init; }
    public string? Color { get; init; }
    public string? Brand { get; init; }
}
```

2. 在 Program.cs 中，添加语句以创建一个汽车，然后创建其突变副本，如以下代码所示：

```
ImmutableVehicle car = new()
{
    Brand = "Mazda MX-5 RF",
    Color = "Soul Red Crystal Metallic",
    Wheels = 4
};

ImmutableVehicle repaintedCar = car
    with { Color = "Polymetal Grey Metallic" };

WriteLine($"Original car color was {car.Color}.");
WriteLine($"New car color is {repaintedCar.Color}.");
```

3. 运行代码查看结果，并注意突变副本中汽车颜色的变化，如以下输出所示：

```
Original car color was Soul Red Crystal Metallic.
New car color is Polymetal Grey Metallic.
```

## 记录中的位置数据成员

定义记录的语法可以使用位置数据成员大幅简化。

## 简化记录中的数据成员

而不是使用带大括号的对象初始化语法，有时你可能更喜欢提供一个带位置参数的构造函数，就像本章前面看到的那样。你也可以将它与解构器结合起来，以便将对象拆分为各个部分，如以下代码所示：

```
public record ImmutableAnimal
{
    public string Name { get; init; }
    public string Species { get; init; }
}
```

```
public ImmutableAnimal(string name, string species)
{
    Name = name;
    Species = species;
}

public void Deconstruct(out string name, out string species)
{
    name = Name;
    species = Species;
}
```

属性、构造函数和解构器可以为你生成：

- 1. 在 Records.cs 中，添加语句以使用称为位置记录的简化语法定义另一个记录，如以下代码所示：
- 2. 在 Program.cs 中，添加语句以构造和解构不可变动物，如以下代码所示：
- 3. 运行应用程序并查看结果，如以下输出所示：

// simpler way to define a record
// auto-generates the properties, constructor, and deconstructor
public record ImmutableAnimal(string Name, string Species);
```

```
ImmutableAnimal oscar = new("Oscar", "Labrador");
var (who, what) = oscar; // calls Deconstruct method
WriteLine($"{who} is a {what}.");
```

```
Oscar is a Labrador.
```

![](img/c63b00dca7192cc58cfb9e1f1e64f483_246_0.png)

当我们查看第 6 章《实现接口和继承类》中关于 C# 10 对创建结构体记录的支持时，你将再次看到记录。

## 练习与探索

通过回答一些问题来测试你的知识和理解，进行一些实践操作，并通过更深入的研究探索本章主题。

## 练习 5.1 – 测试你的知识

回答以下问题：

- 1. 六种访问修饰符关键字的组合是什么，它们的作用是什么？
- 2. 将 static、const 和 readonly 关键字应用于类型成员时有什么区别？
- 3. 构造函数做什么？
- 4. 当你想存储组合值时，为什么应该将 [Flags] 属性应用于枚举类型？
- 5. partial 关键字有什么用处？
- 6. 元组是什么？
- 7. record 关键字做什么？
- 8. 重载是什么意思？
- 9. 字段和属性有什么区别？
- 10. 如何使方法参数可选？

## 练习 5.2 – 探索主题

使用以下页面上的链接了解本章涵盖主题的更多详细信息：

```
https://github.com/markjprice/cs10dotnet6/blob/main/book-links.md#chapter-5---building-your-own-types-with-object-oriented-programming
```

## 摘要

在本章中，你学习了如何使用 OOP 创建自己的类型。你了解了类型可以拥有的不同类别的成员，包括用于存储数据的字段和用于执行操作的方法，并且你使用了 OOP 概念，例如聚合和封装。你看到了如何使用现代 C# 功能的示例，如关系和属性模式匹配增强、仅初始化属性和记录。

在下一章中，你将通过定义委托和事件、实现接口以及从现有类继承来进一步扩展这些概念。

## 06

## 实现接口和继承类

本章是关于使用 **面向对象编程 (OOP)** 从现有类型派生新类型。你将学习关于定义运算符和局部函数以执行简单操作以及委托和事件以在类型之间交换消息。你将实现接口以实现通用功能。你将学习关于泛型以及引用类型和值类型之间的区别。你将创建一个派生类以从基类继承以重用功能，重写继承的类型成员，并使用多态。最后，你将学习如何创建扩展方法以及如何在继承层次结构中进行类型转换。

本章涵盖以下主题：

- 设置类库和控制台应用程序
- 关于方法的更多内容
- 引发和处理事件
- 使用泛型使类型安全可重用
- 实现接口
- 使用引用类型和值类型管理内存
- 处理空值
- 从类继承
- 在继承层次结构中进行类型转换
- 继承和扩展 .NET 类型
- 使用分析器编写更好的代码

## 设置类库和控制台应用程序

我们将从定义一个工作区/解决方案开始，其中包含两个项目，就像在第 5 章《使用面向对象编程构建自己的类型》中创建的那样。即使你完成了该章的所有练习，也请遵循以下说明，因为我们将在类库中使用 C# 10 功能，因此它需要以 .NET 6.0 为目标，而不是 .NET Standard 2.0：

- 1. 使用你喜欢的编码工具创建一个名为 Chapter06 的新工作区/解决方案。
- 2. 添加一个类库项目，如以下列表中定义：
  - 1. 项目模板：类库 / classlib
  - 2. 工作区/解决方案文件和文件夹：Chapter06
  - 3. 项目文件和文件夹：PacktLibrary
- 3. 添加一个控制台应用程序项目，如以下列表中定义：
  - 1. 项目模板：控制台应用程序 / console
  - 2. 工作区/解决方案文件和文件夹：Chapter06
  - 3. 项目文件和文件夹：PeopleApp
- 4. 在 PacktLibrary 项目中，将名为 Class1.cs 的文件重命名为 Person.cs。
- 5. 修改 Person.cs 文件内容，如以下代码所示：

```
using static System.Console;

namespace Packt.Shared;

public class Person : object
{
    // fields
    public string? Name; // ? allows null
    public DateTime DateOfBirth;
    public List<Person> Children = new(); // C# 9 or later

    // methods
    public void WriteToConsole()
    {
        WriteLine($"{Name} was born on a {DateOfBirth:dddd}.");
    }
}
```

- 6. 在 PeopleApp 项目中，添加对 PacktLibrary 的项目引用，如以下标记中突出显示的那样：

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net6.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>

  <ItemGroup>
    <ProjectReference Include="..\PacktLibrary\PacktLibrary.csproj" />
  </ItemGroup>

</Project>
```

- 7. 构建 PeopleApp 项目并注意输出，指示两个项目都已成功构建。

## 关于方法的更多内容

我们可能希望两个 Person 实例能够繁殖。我们可以通过编写方法来实现这一点。实例方法是对象对自身执行的操作；静态方法是类型执行的操作。

选择哪一个取决于对该操作最有意义的方式。

> **良好实践：** 为执行类似操作同时拥有静态方法和实例方法通常是有意义的。例如，`string` 既有 `Compare` 静态方法也有 `CompareTo` 实例方法。这使得使用你的类型的程序员可以自行选择如何使用该功能，从而提供更大的灵活性。

## 使用方法实现功能

让我们开始通过同时使用静态方法和实例方法来实现一些功能：

- 1. 向 Person 类添加一个实例方法和一个静态方法，以便两个 Person 对象能够繁殖，如以下代码所示：

```
// static method to "multiply"
public static Person Procreate(Person p1, Person p2)
{
```
