## Object-Oriented Programming

**Object-oriented programming (OOP)** 是一种将代码和数据组织到称为对象的可重用结构中的编程范式。在我们之前的代码片段中，我们可以将代码组织到过程或函数中以实现代码重用。对象允许我们重用既可执行又可存储信息的代码。在 OOP 中，对象是类的实例，类充当创建具有相似属性和行为的对象的蓝图或模板。

OOP 的基本概念如下：

- **Classes:** 类是一个蓝图，定义了该类对象的属性（属性）和行为（方法）。它描述了对象的结构和行为，但本身并不代表任何特定实例。
- **Objects:** 对象是类的实例。它由类蓝图创建并具有属性的值。对象可以通过调用其方法相互交互。
- **Encapsulation:** 封装是指将数据（属性）和操作该数据的方法捆绑到称为对象的单一单元中。封装隐藏对象的内部细节，并通过定义良好的接口提供对其功能的访问。
- **Inheritance:** 继承允许类从其他类继承属性和行为。它使类能够形成层次结构，其中子类（派生类）可以继承并扩展超类（基类）的属性和方法。
- **Polymorphism:** 多态意味着对象能够采取不同的形式或行为。在 OOP 的上下文中，多态允许其他类的对象被视为公共超类的对象。多态允许你编写更通用的代码，可与不同类型的对象一起使用。
- **Abstraction:** 抽象涉及创建复杂系统的简化表示的概念。在 OOP 中，抽象允许你专注于对象的本质特征，同时隐藏实现细节。抽象类和接口用于在许多编程语言中定义抽象。

OOP 提供了多个好处，包括代码可重用性、模块化和更易于维护。它允许更好的代码组织，促进代码可读性，并支持大规模软件系统的开发。

支持 OOP 的流行编程语言包括 **Visual Basic (VB)**、Java、C++、Python、C#、Ruby 等。每种语言可能都有自己的语法和用于实现 OOP 概念的特性，但基本原理保持不变。

在本章中，我们将涵盖以下主要主题：

- Classes and fields





- 实例化类
- 方法
- 一对一“拥有”（has-a）关系

## 类与字段

在 VB 中，类用于定义具有特定属性（字段）和行为（方法）的对象。以下是 VB 中类和字段如何工作的概述。

## 类声明

要在 VB 中定义类，请使用 **Class** 关键字，后跟类名。示例如下：

```
Class MyClass
    ' Class members will be defined here
End Class
```

## 字段

字段（也称为属性或变量）是类的数据成员。它们表示对象的属性或特征。字段保存与从类创建的对象关联的数据值。字段在类中声明，通常位于开头，并且可以具有不同的访问修饰符（**Public、Private、Protected** 等）来控制其可见性和可访问性：

```
Class MyClass
    Private myField As Integer
    Public anotherField As String
End Class
```

在前面的示例中，**myField** 是私有字段，**anotherField** 是公共字段。

接下来，让我们看看访问修饰符，我们可以使用它们来控制对类中字段的访问。

**访问修饰符**决定字段在类内部和外部的可访问性。VB 提供了几种访问修饰符，包括以下这些：

- **Private（私有）：** 该字段只能在类内部访问
- **Public（公共）：** 该字段可以从任何能够访问该对象的代码中访问
- **Protected（受保护）：** 该字段可以在类及其派生类中访问
- **Friend（友元）：** 该字段可以在同一程序集或项目中访问

```
Public Sub DisplayMessage(message As String)
    Console.WriteLine(message)
End Sub
```

在前面的示例中，**DisplayMessage** 是一个公共方法，它接受一个名为 **message** 的字符串参数。它会在控制台上显示该消息。

- **函数过程（Function 过程）：** 函数过程是返回值的方法。当需要执行计算或从方法中检索数据时，可以使用它们。示例如下：

```
Public Function AddNumbers(a As Integer, b As Integer)
    As Integer
    Return a + b
End Function
```

在前面的示例中，**AddNumbers** 是一个公共方法，它接受两个 **Integer** 参数 a 和 b，并返回它们的和作为整数值。

- **访问修饰符：** 与方法类似，字段可以具有不同的访问修饰符来控制其可见性和可访问性。VB 中常见的访问修饰符包括 **Public**、**Private**、**Protected** 和 **Friend**。

- **方法重载：** VB 支持方法重载，这允许你定义多个名称相同但参数不同的方法。编译器根据方法调用期间提供的参数来确定调用哪个方法。以下是方法重载的示例：

```
Public Sub DoSomething()
    ' Method implementation without parameters
End Sub
Public Sub DoSomething(parameter As Integer)
    ' Method implementation with an integer parameter
End Sub
Public Sub DoSomething(parameter As String)
    ' Method implementation with a string parameter
End Sub
```

在前面的示例中，**DoSomething** 根据参数的存在和类型被重载为三个不同的版本。

VB 中的方法使你可以将功能封装在类中，并提供一种执行操作或计算的方式。它们允许代码重用、模块化以及在程序中更好地组织逻辑。

接下来，我们将研究如何在 VB 中实例化类。

## 实例化类

在 VB 中，可以通过从类创建对象来实例化类。该过程涉及使用 **New** 关键字，后跟类名以及任何必要的参数。以下是如何在 VB 中实例化类的示例：

```
Dim myObject As New ClassName()
```

在前面的示例中，**ClassName** 表示你要实例化的类的名称，**myObject** 是用于保存所创建对象的变量。

如果该类具有参数化构造函数，则可以在实例化类时传递参数：

```
Dim myObject As New ClassName(arg1, arg2)
```

在这种情况下，arg1 和 arg2 是 ClassName 构造函数所需的参数。

一旦实例化了类，就可以通过对象变量访问其属性、方法和其他成员。示例如下：

```
myObject.PropertyName = value ' Set the value of a property
myObject.MethodName() ' Call a method
```

需要注意的是，实例化类的过程会创建该类的一个新实例，这意味着该对象及其关联数据会与该类的其他实例分开存储在内存中。

此外，你可以创建类的多个实例，每个实例都有自己的数据集。这允许你基于相同的类蓝图处理不同的对象。

请记住为你的类和对象变量选择有意义的名称，以确保代码的清晰性和可维护性。

接下来，我们将研究如何在 VB 中表示一对一“拥有”（has-a）关系。

## 一对一“拥有”（has-a）关系

在 VB 中，类之间的一对一“拥有”关系是指一个类将另一个类的实例作为字段或属性包含的情况。这种关系表示两个类之间的关联，其中第一个类直接引用第二个类的实例。以下是在 VB 中一对一“拥有”关系的示例：

```
Public Class Car
    Public Property Engine As Engine
    ' Other car-related properties and methods
End Class
Public Class Engine
    ' Engine-related properties and methods
End Class
```

在前面的示例中，我们有两个类：Car 和 Engine。Car 类有一个名为 Engine 的属性，其类型为 Engine，表示汽车的引擎。这种关系表明一辆汽车只有一个引擎。

要使用这种关系，你通常会创建类的实例并设置适当的值：

```
Dim myCar As New Car()
```

```
myCar.Engine = New Engine()
```

在前面的代码片段中，我们创建了一个名为 `myCar` 的 `Car` 对象，并将一个新的 `Engine` 对象分配给它的 `Engine` 属性。

通过这种一对一关系，`Car` 类可以通过其 `Engine` 属性访问关联 `Engine` 类的属性和方法：

```
myCar.Engine.Start()
```

需要注意的是，在 VB 中，一对一“拥有”关系也可以使用字段而不是属性来实现，具体取决于你的特定需求和设计偏好。

当不同的类需要协作并共享信息或功能时，一对一关系会很有帮助。通过将关注点分离到其他类中，同时保持它们之间的直接关联，它们可以实现模块化和可扩展的代码。

## 总结

本章介绍了 VB 中的面向对象编程。我们研究了在 VB 中定义类和字段。本章将是接下来三章的基础，我们将在这些章节中深入探讨特定的面向对象软件开发技术。下一章将研究在 VB 编程中利用继承的同时构建类层次结构。
