

以下是输出：

```
***** Fun with Extension Methods *****
Int32 lives here: => System.Private.CoreLib

DataSet lives here: => System.Data.Common

Value of myInt: 12345678
Reversed digits of myInt: 87654321
```

## 导入扩展方法

当你定义一个包含扩展方法的类时，它无疑会定义在某个命名空间内。如果该命名空间不同于使用扩展方法的命名空间，则需要使用预期的 C# using 关键字。这样做之后，你的代码文件就可以访问被扩展类型的所有扩展方法。这一点很重要，因为如果你没有显式导入正确的命名空间，那么这些扩展方法对于该 C# 代码文件是不可用的。

实际上，虽然表面上看扩展方法似乎是全局性的，但它们实际上仅限于定义它们的命名空间或导入它们的命名空间。回想一下，你将 MyExtensions 类包装到了名为 MyExtensionMethods 的命名空间中，如下所示：

```
namespace MyExtensionMethods
{
    static class MyExtensions
    {
        ...
    }
}
```

要使用该类中的扩展方法，你需要显式导入 MyExtensionMethods 命名空间，就像我们在用于演练示例的顶级语句中所做的那样。

## 扩展实现特定接口的类型

到目前为止，你已经了解了如何通过扩展方法使用新功能来扩展类（以及间接扩展遵循相同语法的结构）。也可以定义只能扩展实现了相应接口的类或结构的扩展方法。例如，你可以说这样的话：“如果一个类或结构实现了 IEnumerable<T>，那么该类型将获得以下新成员。”当然，你也可以要求某个类型支持任何接口，包括你自己的自定义接口。

为了说明这一点，创建一个名为 InterfaceExtensions 的新控制台应用程序项目。这里的目标是向任何实现 IEnumerable 的类型添加一个新方法，这将包括任何数组和许多非泛型集合类（回想第 10 章，泛型 IEnumerable<T> 接口扩展了非泛型 IEnumerable 接口）。将以下扩展类添加到你的新项目中：

```
using System;

namespace InterfaceExtensions
{
    static class AnnoyingExtensions
    {
        public static void PrintDataAndBeep(
            this System.Collections.IEnumerable iterator)
        {
            foreach (var item in iterator)
            {
                Console.WriteLine(item);
                Console.Beep();
            }
        }
    }
}
```

鉴于 PrintDataAndBeep() 方法可以被任何实现 IEnumerable 的类或结构使用，你可以通过以下代码进行测试：

```
using System;
using System.Collections.Generic;
using InterfaceExtensions;

Console.WriteLine("***** Extending Interface Compatible Types *****
");

// System.Array implements IEnumerable!
string[] data =
    { "Wow", "this", "is", "sort", "of", "annoying",
      "but", "in", "a", "weird", "way", "fun!"};
data.PrintDataAndBeep();

Console.WriteLine();

// List<T> implements IEnumerable!
List<int> myInts = new List<int>() {10, 15, 20};
myInts.PrintDataAndBeep();

Console.ReadLine();
```

这结束了你对 C# 扩展方法的探讨。请记住，无论何时你想要扩展某个类型的功能，但出于多态性的目的不想子类化（或者如果类型是密封的而无法子类化），这个语言特性都会很有用。稍后你将在本书中看到，扩展方法在 LINQ API 中扮演着关键角色。事实上，你将会看到，在 LINQ API 中，最常见的被扩展对象之一就是实现（惊讶吧！）泛型版本 IEnumerable 的类或结构。

## 扩展方法 GetEnumerator 支持（9.0 新增）

在 C# 9.0 之前，要对类使用 foreach，必须在类上直接定义 GetEnumerator() 方法。从 C# 9.0 开始，foreach 语句会检查类上的扩展方法，如果找到 GetEnumerator() 方法，就会使用该方法获取该类的 IEnumerator。要查看实际效果，请添加一个名为 ForEachWithExtensionMethods 的新控制台应用程序，并添加第 8 章中 Car 和 Garage 类的简化版本。

```
//Car.cs
using System;

namespace ForEachWithExtensionMethods
{
    class Car
    {
        // Car properties.
        public int CurrentSpeed {get; set;} = 0;
        public string PetName {get; set;} = "";

        // Constructors.
        public Car() {}
        public Car(string name, int speed)
        {
            CurrentSpeed = speed;
            PetName = name;
        }

        // See if Car has overheated.
    }
}
```

```
//Garage.cs
namespace ForEachWithExtensionMethods
{
    class Garage
    {
        public Car[] CarsInGarage { get; set; }

        // Fill with some Car objects upon startup.
        public Garage()
        {
            CarsInGarage = new Car[4];
            CarsInGarage[0] = new Car("Rusty", 30);
            CarsInGarage[1] = new Car("Clunker", 55);
            CarsInGarage[2] = new Car("Zippy", 30);
            CarsInGarage[3] = new Car("Fred", 30);
        }
    }
}
```

注意，Garage 类并没有实现 IEnumerable，也没有 GetEnumerator() 方法。GetEnumerator() 方法是通过 GarageExtensions 类添加的，如下所示：using System.Collections;

```
namespace ForEachWithExtensionMethods
{
    static class GarageExtensions
    {
        public static IEnumerator GetEnumerator(this Garage g)
            => g.CarsInGarage.GetEnumerator();
    }
}
```

测试这一新功能的代码与第 8 章中用于测试 GetEnumerator() 方法的代码相同。将 Program.cs 更新为以下内容：

```
using System;
using ForEachWithExtensionMethods;

Console.WriteLine("***** Support for Extension Method GetEnumerator *****
");
Garage carLot = new Garage();

// Hand over each car in the collection?
foreach (Car c in carLot)
{
    Console.WriteLine("{0} is going {1} MPH",
        c.PetName, c.CurrentSpeed);
}
```

你会看到代码正常工作，向控制台打印汽车列表及其速度。

```
***** Support for Extension Method GetEnumerator *****

Rusty is going 30 MPH
Clunker is going 55 MPH
Zippy is going 30 MPH
Fred is going 30 MPH
```

注意，这一新功能有一个潜在缺点，即原本不应被 foreach 迭代的类现在也可以被 foreach 迭代了。

## 理解匿名类型

作为一名面向对象的程序员，你知道定义类来表示你试图建模的给定项的状态和功能的好处。当然，每当你需要定义一个旨在跨项目重用，并通过一组方法、事件、属性和自定义构造函数提供许多功能的类时，创建一个新的 C# 类是常见的做法。

然而，在其他时候，你只是想定义一个类来建模一组封装的（并且以某种方式相关的）数据点，而不需要任何关联的方法、事件或其他专门功能。此外，如果这种类型只被程序中的少量方法使用怎么办？当你非常清楚这个类只会在少数地方使用时，还要像下面那样定义一个完整的类定义，那就相当麻烦了。为了强调这一点，下面是你需要创建一个遵循典型基于值的语义的“简单”数据类型时可能需要做的工作的粗略概要：

```
class SomeClass
{
    // Define a set of private member variables...

    // Make a property for each member variable...

    // Override ToString() to account for key member variables...

    // Override GetHashCode() and Equals() to work with value-based equality...
}
```

如你所见，这并不一定那么简单。你不仅需要编写相当多的代码，而且系统中还要多维护一个类。对于这样的临时数据，即时创建一个自定义数据类型会很有用。例如，假设你需要构建一个接收一组传入参数的自定义方法。你希望使用这些参数来创建一个新的数据类型，以供该方法作用域使用。此外，你希望使用典型的 ToString() 方法快速打印这些数据，也许还可以使用 System.Object 的其他成员。你可以使用匿名类型语法来实现这一点。

## 定义匿名类型

定义匿名类型时，需要使用 var 关键字（参见第 3 章）并结合对象初始化语法（参见第 5 章）。你必须使用 var 关键字，因为编译器会在编译时自动生成一个新的类定义（并且你永远不会在 C# 代码中看到这个类的名称）。初始化语法用于告诉编译器为新创建的类型创建私有后备字段和（只读）属性。

为了说明这一点，创建一个名为 AnonymousTypes 的新控制台应用程序项目。现在，向你的 Program 类中添加以下方法，该方法使用传入的参数数据即时构造一个新类型：

```
static void BuildAnonymousType( string make, string color, int currSp )
{
    // Build anonymous type using incoming args.
    var car = new { Make = make, Color = color, Speed = currSp };

    // Note you can now use this type to get the property data!
    Console.WriteLine("You have a {0} {1} going {2} MPH", car.Color, car.Make, car.Speed);
}
```

