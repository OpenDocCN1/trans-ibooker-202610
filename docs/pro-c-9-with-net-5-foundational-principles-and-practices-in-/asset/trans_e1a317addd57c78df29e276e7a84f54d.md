

# 理解静态只读字段

与常量字段不同，只读字段并非隐式静态。因此，如果你想从类级别公开 PI，则必须显式使用 static 关键字。如果你能在编译时知道静态只读字段的值，初始赋值看起来与常量类似（然而，在这种情况下，最初就使用 const 关键字会更容易，因为你在声明时就为数据字段赋值）。

```
class MyMathClass
{
    public static readonly double PI = 3.14;
}
```

```
//Program.cs
Console.WriteLine("***** Fun with Const *****");
Console.WriteLine("The value of PI is: {0}", MyMathClass.PI);
Console.ReadLine();
```

然而，如果静态只读字段的值要到运行时才知道，你必须使用本章前面描述的静态构造函数。

```
class MyMathClass
{
    public static readonly double PI;

    static MyMathClass()
    { PI = 3.14; }
}
```

## 理解部分类

在处理类时，了解 C# partial 关键字的角色很重要。partial 关键字允许将单个类划分到多个代码文件中。当你从数据库脚手架 Entity Framework Core 类时，创建的类都作为部分类创建。这样，你为增强这些文件而编写的任何代码都不会被覆盖，前提是你的代码在标记为 partial 关键字的单独类文件中。另一个原因是也许你的类随着时间的推移变得难以管理，作为重构该类的中间步骤，你可以将其拆分为 partials。

在 C# 中，你可以将单个类划分到多个代码文件中，以将样板代码与更实用（且更复杂）的成员隔离开来。为了说明部分类可能有用的地方，打开本章前面创建的 EmployeeApp 项目，然后在 Visual Studio 中打开 Employee.cs 文件进行编辑。正如你所回忆的，此单个文件包含类的所有方面的代码。

```
class Employee
{
    // Field Data

    // Constructors

    // Methods

    // Properties
}
```

使用部分类，你可以选择将（例如）属性、构造函数和字段数据移动到名为 Employee.Core.cs 的新文件中（文件名无关紧要）。第一步是向当前类定义添加 partial 关键字并剪切要放入新文件中的代码。

```
// Employee.cs
partial class Employee
{
    // Methods
    // Properties
}
```

接下来，假设你已在项目中插入了一个新的类文件，你可以使用简单的剪切粘贴操作将数据字段和属性移动到新文件。此外，你必须向此部分类定义添加 partial 关键字。以下是一个示例：

```
// Employee.Core.cs
partial class Employee
{
    // Field data
    // Properties
}
```

注意 请记住，每个部分类都必须标记为 partial 关键字！

编译修改后的项目后，你应该看不到任何差异。部分类的整个思想仅在设计时实现。应用程序编译后，程序集中只有一个统一的类。定义部分类型时唯一的必要条件是类型的名称（本例中为 Employee）必须相同并且在同一个 .NET Core 命名空间中定义。

## 使用记录（新 9.0）

C# 9.0 新增的 record 类型是一种特殊的类类型。记录是引用类型，提供合成方法以提供相等性的值语义。默认情况下，记录类型默认是不可变的。虽然你可以使用 init-only setter 和只读属性的组合本质上创建一个不可变类，但记录类型消除了这项额外工作。

要开始尝试记录，创建一个名为 FunWithRecords 的新控制台应用程序。考虑以下 Car 类，它修改自本章早期的示例：

```
class Car
{
    public string Make { get; set; }
    public string Model { get; set; }
    public string Color { get; set; }

    public Car() {}

    public Car(string make, string model, string color)
{
    Make = make;
    Model = model;
    Color = color;
}
}
```

正如你现在所熟知的，一旦你创建了此类的实例，你可以在运行时更改任何属性。如果每个实例都需要不可变，你可以将属性定义更改为以下内容：

```
public string Make { get; init; }
public string Model { get; init; }
public string Color { get; init; }
```

为了练习这个新类，以下代码创建了 Car 类的两个实例，一个通过对象初始化，另一个通过自定义构造函数。将 Program.cs 文件更新为以下内容：

```
using System;
using FunWithRecords;

Console.WriteLine("Fun with Records!");

//Use object initialization
Car myCar = new Car
{
    Make = "Honda",
    Model = "Pilot",
    Color = "Blue"
};
Console.WriteLine("My car: ");
DisplayCarStats(myCar);
Console.WriteLine();
//Use the custom constructor
Car anotherMyCar = new Car("Honda", "Pilot", "Blue");
Console.WriteLine("Another variable for my car: ");
DisplayCarStats(anotherMyCar);
Console.WriteLine();

//Compile error if property is changed
//myCar.Color = "Red";

Console.ReadLine();

static void DisplayCarStats(Car c)
{
    Console.WriteLine("Car Make: {0}", c.Make);
    Console.WriteLine("Car Model: {0}", c.Model);
    Console.WriteLine("Car Color: {0}", c.Color);
}
```

如预期，两者对象创建方法都有效，属性得到显示，并且尝试在构造后更改属性会引发编译错误。

要创建 Car 记录类型，向项目添加一个名为 (CarRecord.cs) 的新文件并添加以下代码：

```
record CarRecord
{
    public string Make { get; init; }
    public string Model { get; init; }
    public string Color { get; init; }

    public CarRecord () {}
    public CarRecord (string make, string model, string color)
    {
        Make = make;
        Model = model;
        Color = color;
    }
}
```

你可以通过在 Program.cs 中运行以下代码来确认行为与具有 init-only 设置的 Car 类相同：

```
Console.WriteLine("/***************** RECORDS ********************/");
//Use object initialization
CarRecord myCarRecord = new CarRecord
{
    Make = "Honda",
    Model = "Pilot",
    Color = "Blue"
};
Console.WriteLine("My car: ");
DisplayCarRecordStats(myCarRecord);
Console.WriteLine();

//Use the custom constructor
CarRecord anotherMyCarRecord = new CarRecord("Honda", "Pilot", "Blue");
Console.WriteLine("Another variable for my car: ");
Console.WriteLine(anotherMyCarRecord.ToString());
Console.WriteLine();

//Compile error if property is changed
//myCarRecord.Color = "Red";

Console.ReadLine();
```

虽然我们尚未涵盖记录的相等性（下一节）或继承（下一章），但对记录的首次了解似乎没有多大好处。当前的 Car 示例包含我们所期望的所有管道代码。有一个显著的不同输出：ToString() 方法对记录类型进行了美化，如以下示例输出所示：

/************** RECORDS ****************/
My car:
CarRecord { Make = Honda, Model = Pilot, Color = Blue }
Another variable for my car:
CarRecord { Make = Honda, Model = Pilot, Color = Blue }

但考虑 Car 记录的此更新定义：

```
record CarRecord(string Make, string Model, string Color);
```

称为 *位置记录类型*，构造函数定义记录上的属性，并且所有其他管道代码已被删除。使用此语法时有三个考虑因素：第一是不能使用紧凑定义语法对记录类型进行对象初始化，第二是记录必须使用正确的顺序构造属性，第三是构造函数中属性的大小写直接转换为记录类型上属性的大小写。

## 理解记录类型的相等性

在 Car 类示例中，两个 Car 实例使用完全相同的数据创建。人们 *可能* 认为这两个类是相等的，如以下代码行测试的那样：

```
Console.WriteLine($"Cars are the same? {myCar.Equals(anotherMyCar)}");
```

然而，它们并不相等。回想一下，记录类型是类的一种专门类型，而类是 *引用类型*。对于两个引用类型相等，它们必须指向内存中的同一对象。作为进一步测试，检查两个 Car 对象是否指向同一对象：

```
Console.WriteLine($"Cars are the same reference? {ReferenceEquals(myCar, anotherMyCar)}");
```

再次运行程序会产生以下（简略）结果：

```
Cars are the same? False
CarRecords are the same? False
```

记录类型的行为不同。记录类型隐式重写 Equals、== 和 !=，并且产生的结果就像实例是值类型一样。考虑以下代码及其后续结果：

```
Console.WriteLine($"CarRecords are the same? {myCarRecord.Equals(anotherMyCarRecord)}");
Console.WriteLine($"CarRecords are the same reference? {ReferenceEquals(myCarRecord,anotherMyCarRecord)}");
Console.WriteLine($"CarRecords are the same? {myCarRecord == anotherMyCarRecord}");
Console.WriteLine($"CarRecords are not the same? {myCarRecord != anotherMyCarRecord}");
```

