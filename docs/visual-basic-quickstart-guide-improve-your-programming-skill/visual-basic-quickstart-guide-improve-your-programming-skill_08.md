# 多态

多态是编程中的一个基本概念，通常与**面向对象编程（OOP）**相关联。它允许将不同类的对象用作公共超类或接口的对象，从而提供一致的接口来与这些对象交互，无论它们的具体类型是什么。

在本章中，我们将介绍以下主要主题：

- 多态
- 抽象类
- 密封类

## 多态

多态允许程序员根据上下文特化功能；编程语言会使用适当的版本。多态主要有两种类型：

- **编译时（静态）多态：** 也称为方法重载或编译时方法分派，这种类型的多态发生在不同方法或函数具有相同名称但参数列表不同时。调用哪个方法的决定是在编译时根据所提供参数的数量和类型做出的。这允许你拥有多个同名方法，但基于输入参数具有不同的行为。
- **运行时（动态）多态：** 也称为方法重写或运行时方法分派，这种类型的多态发生在子类为其超类（或接口）中已定义的方法提供特定实现时。在运行时，如果所引用的对象是子类的实例，则方法调用将解析为子类实现。这允许你使用公共接口根据对象的实际类型调用不同的行为。

在**Visual Basic (VB)**中，与其他OOP语言一样，多态是通过继承和方法重写实现的。VB支持运行时多态，派生类的对象可以被视为其基类的对象，并在运行时根据对象的实际类型调用适当的方法实现。

为了演示VB中的多态，我们可以创建一个包含基类及其派生类的简单示例：

```
' Base class
Public Class Shape
    Public Overridable Sub Draw()
        Console.WriteLine("Drawing a shape")
    End Sub
End Class
```

```
Public MustInherit Class Shape
    Public MustOverride Sub Draw()
    Public Sub GetArea()
        Console.WriteLine("Calculating area...")
    End Sub
End Class
```

在此示例中，我们定义了一个名为Shape的抽象类，其中包含一个名为Draw的抽象方法和一个名为GetArea的非抽象方法。Draw方法使用 MustOverride 关键字标记，表示任何派生类都必须为自己的 Draw 方法提供实现。

接下来，让我们创建一个继承自该抽象类并实现 Draw 方法的派生类：

```
Public Class Circle
    Inherits Shape
    Public Overrides Sub Draw()
        Console.WriteLine("Drawing a circle")
    End Sub
End Class
```

在此示例中，Circle 类继承自 Shape 抽象类，并为 Draw 方法提供了实现。

由于抽象类不能直接实例化，我们可以通过其派生类来使用它们：

```
Module MainModule
    Sub Main()
        Dim circle As New Circle()
        circle.Draw()  ' Output: Drawing a circle
        circle.GetArea()  ' Output: Calculating area...
    End Sub
End Module
```

在此 Main 方法中，我们创建了一个 Circle 类的实例，并调用了它的 Draw 方法，该方法在 Shape 抽象类中定义。我们还可以调用抽象类提供的非抽象 GetArea 方法。

当你希望跨多个相关类定义公共结构和行为，同时强制要求某些方法必须在派生类中实现时，抽象类非常有用。它们提供了一定程度的抽象，并有助于在 OOP 中有效地组织代码。

接下来，我们将介绍如何确保类不能通过继承进行扩展。

## 密封类

在VB中，没有与 sealed 直接等价的关键字，sealed 存在于其他一些编程语言（如C#）中。在C#中，sealed 关键字用于防止类被继承（即它不能作为基类）。如果程序员拥有不应被特化的代码，sealed 概念就很重要。例如，一个验证用户的授权类，你不希望授权方法被更改。然而，在VB中，默认情况下所有类都是可继承的，这意味着任何类都可以用作基类，除非它被特别设计为抽象类（使用 MustInherit 关键字，如前一节所述）。要防止类在VB中被继承，可以在定义类时使用 NotInheritable 关键字。NotInheritable 关键字相当于C#中的 sealed 关键字。

下面是一个VB中 NotInheritable 类的示例：

```
NotInheritable Public Class Singleton
    Private Shared instance As Singleton
    Private Sub New()
        ' Private constructor to prevent direct
        instantiation.
    End Sub
    Public Shared Function GetInstance() As Singleton
        If instance Is Nothing Then
            instance = New Singleton()
        End If
        Return instance
    End Function
    ' Other methods and properties...
End Class
```

在此示例中，Singleton 类被标记为 NotInheritable，这意味着没有其他类可以继承它。当你希望创建一个只有一个实例的类（单例模式）并确保没有人可以通过继承它来创建额外实例时，通常使用 Singleton 类。

在VB中，你可以使用 NotInheritable 关键字来防止类被继承，类似于C#中的 sealed 关键字。

## 小结

本章介绍了VB中的多态。我们了解了如何定义没有完整定义但可以在继承树中向下扩展的类。我们还了解了如何实现功能以强制类不能通过继承来扩展。下一章将介绍如何在VB编程中利用接口来实现多重继承。
