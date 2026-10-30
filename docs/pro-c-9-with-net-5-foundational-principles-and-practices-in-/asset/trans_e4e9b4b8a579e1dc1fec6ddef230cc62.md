

/************** RECORDS ****************/
我的汽车：
CarRecord { Make = Honda, Model = Pilot, Color = Blue }
我的汽车的另一个变量：
CarRecord { Make = Honda, Model = Pilot, Color = Blue }

CarRecords 相同吗？ True
CarRecords 是同一引用吗？ false
CarRecords 相同吗？ True
CarRecords 不相同吗？ False

请注意，即使变量指向内存中的两个不同变量，它们也被视为相等。

## 使用 with 表达式复制记录类型

对于记录类型，将记录类型实例分配给新变量会创建一个指向同一引用的指针，其行为与类相同。以下代码演示了这一点：

```
CarRecord carRecordCopy = anotherMyCarRecord;
Console.WriteLine("Car Record copy results");
Console.WriteLine($"CarRecords are the same? {carRecordCopy.Equals(anotherMyCarRecord)}");
Console.WriteLine($"CarRecords are the same? {ReferenceEquals(carRecordCopy,
anotherMyCarRecord)}");
```

执行时，两个测试都返回 Yes，证明它们在值和引用上是相同的。

要创建具有一个或多个属性被修改的记录的真正副本，C# 9.0 引入了 with 表达式。在 with 构造中，任何需要更新的属性都与其新值一起指定，而未列出的属性则完全复制。请查看以下示例：

```
CarRecord ourOtherCar = myCarRecord with {Model = "Odyssey"};
Console.WriteLine("My copied car:");
Console.WriteLine(ourOtherCar.ToString());

Console.WriteLine("Car Record copy using with expression results");
Console.WriteLine($"CarRecords are the same? {ourOtherCar.Equals(myCarRecord)}");
Console.WriteLine($"CarRecords are the same? {ReferenceEquals(ourOtherCar, myCarRecord)}");
```

该代码创建了 CarRecord 类型的新的实例，复制了 myCarRecord 实例的 Make 和 Color 值，并将 Model 设置为字符串 Odyssey。此代码的结果如下所示：

/************** RECORDS ****************/
我的复制汽车：
CarRecord { Make = Honda, Model = Odyssey, Color = Blue }

使用 with 表达式的 Car 记录复制结果
CarRecords 相同吗？ False
CarRecords 相同吗？ False

使用 with 表达式，您可以将复杂记录类型转换为具有更新属性值的新记录类型实例。

这结束了我们对新的 C# 9.0 记录类型的首次介绍。下一章将研究记录类型和继承。

## 摘要

本章的重点是向您介绍 C# 类类型的作用以及新的 C# 9.0 记录类型。正如您所见，类可以采用任意数量的构造函数，使对象用户能够在创建时建立对象的状态。本章还说明了几种类设计技术（以及相关的关键字）。this 关键字可用于获得对当前对象的访问。static 关键字允许您定义绑定在类（而非对象）级别的字段和成员。const 关键字、readonly 修饰符和 init-only 设置器允许您定义在初始赋值或对象构造后永远不会更改的数据点。记录类型是一种特殊的类类型，它是不可变的，并且在比较记录类型与其同一记录类型的另一个实例时表现得像值类型。

本章的大部分内容深入探讨了 OOP 的第一个支柱：封装。您学习了 C# 的访问修饰符以及类型属性、对象初始化语法和部分类的作用。有了这些知识，您现在可以转向下一章，在下一章中，您将学习如何使用继承和多态性构建相关类的族。

## 理解继承和多态性

第 5 章研究了 OOP 的第一个支柱：封装。当时，您学习了如何构建具有构造函数和各种成员（字段、属性、方法、常量和只读字段）的单个良好定义的类类型。本章将重点关注 OOP 的其余两个支柱：继承和多态性。

首先，您将学习如何使用 *继承* 构建相关类的族。正如您将看到的，这种代码重用形式允许您在父类中定义通用功能，由子类利用并可能更改。在此过程中，您将学习如何使用虚拟和抽象成员以及显式转换的角色来建立进入类层次结构的 *多态接口*。

本章将通过研究 .NET 基类库中的终极父类 System.Object 的作用结束。

## 理解继承的基本机制

回顾第 5 章，继承是促进代码重用的 OOP 方面。具体来说，代码重用有两种形式：继承（“is-a” 关系）和包含/委托模型（“has-a” 关系）。让我们从研究“is-a”关系的经典继承模型开始。

当您在类之间建立“is-a”关系时，您是在两个或多个类类型之间建立依赖关系。经典继承背后的基本思想是，可以使用现有类作为起点来创建新类。从一个简单的例子开始，创建一个名为 BasicInheritance 的新控制台应用程序项目。现在假设您设计了一个名为 Car 的类，用于模拟汽车的一些基本细节。

```
namespace BasicInheritance
{
    // A simple base class.
    class Car
    {
        public readonly int MaxSpeed;
        private int _currSpeed;

        public Car(int max)
        {
            MaxSpeed = max;
        }
```

```
public Car()
{
    MaxSpeed = 55;
}
public int Speed
{
    get { return _currSpeed; }
    set
    {
        _currSpeed = value;
        if (_currSpeed > MaxSpeed)
        {
            _currSpeed = MaxSpeed;
        }
    }
}
}
```

请注意，Car 类正在使用封装服务，通过名为 Speed 的公共属性控制对私有 currSpeed 字段的访问。此时，您可以按如下方式使用您的 Car 类型：

```
using System;
using BasicInheritance;

Console.WriteLine("***** Basic Inheritance *****
");
// Make a Car object, set max speed and current speed.
Car myCar = new Car(80) {Speed = 50};

// Print current speed.
Console.WriteLine("My car is going {0} MPH", myCar.Speed);
Console.ReadLine();
```

## 指定现有类的父类

现在假设您想构建一个名为 MiniVan 的新类。与基本的 Car 一样，您希望将 MiniVan 类定义为支持最大速度、当前速度的数据以及名为 Speed 的属性，以允许对象用户修改对象的状态。显然，Car 和 MiniVan 类是相关的；事实上，可以说 MiniVan “is-a” 类型的 Car。“is-a”关系（正式称为经典继承）允许您构建扩展现有类功能的新类定义。

将作为新类基础的现有类称为基类、超类或父类。基类的作用是为扩展它的类定义所有通用数据和成员。扩展类正式称为派生类或子类。在 C# 中，您在类定义上使用冒号运算符在类之间建立“is-a”关系。假设您编写了以下新的 MiniVan 类：

```
namespace BasicInheritance
{
    // MiniVan "is-a" Car.
    sealed class MiniVan : Car
    {
```

目前，此新类尚未定义任何成员。于是，通过从 Car 基类扩展 MiniVan，您获得了什么？简单地说，MiniVan 对象现在可以访问父类中定义的每个公共成员。

> 注意 尽管构造函数通常定义为 public，但派生类永远不会继承父类的构造函数。构造函数仅用于构造它们所定义的类，尽管派生类可以通过构造函数链调用它们。这将在后面介绍。

鉴于这两种类类型之间的关系，您现在可以按如下方式使用 MiniVan 类：

```
Console.WriteLine("***** Basic Inheritance *****
");

// Now make a MiniVan object.
MiniVan myVan = new MiniVan {Speed = 10};
Console.WriteLine("My van is going {0} MPH", myVan.Speed);
Console.ReadLine();
```

再次注意，虽然您没有向 MiniVan 类添加任何成员，但您可以直接访问父类的公共 Speed 属性，从而重用了代码。这比创建一个具有与 Car 相同成员的 MiniVan 类（如 Speed 属性）要好得多。如果您在这些类之间重复代码，您现在需要维护两段代码，这显然是对您时间的浪费。

请始终记住继承保留封装；因此，以下代码会导致编译器错误，因为私有成员永远无法从对象引用访问：

```
Console.WriteLine("***** Basic Inheritance *****
");
...
// Make a MiniVan object.
MiniVan myVan = new MiniVan();
myVan.Speed = 10;
Console.WriteLine("My van is going {0} MPH",
    myVan.Speed);

// Error! Can't access private members!
myVan._currSpeed = 55;
Console.ReadLine();
```

相关地，如果 MiniVan 定义了自己的成员集，它仍然无法访问 Car 基类的任何私有成员。请记住，私有成员只能由定义它的类访问。例如，MiniVan 中的以下方法会导致编译器错误：

// MiniVan derives from Car.
class MiniVan : Car
{
    public void TestMethod()
    {
