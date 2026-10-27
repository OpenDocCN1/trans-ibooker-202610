## 继承

继承是**面向对象编程（OOP）**中的一个基本概念，它允许子类继承另一个类（父类或基类）的属性和行为。继承实现了代码重用，促进了类的层次结构组织，并有助于创建更有组织、更易维护的代码库。

继承遵循*is-a*关系，意味着子类是父类的特化版本。它可以访问父类的所有公共和受保护成员（变量和方法），但不能访问私有成员。子类还可以重写父类的方法以提供其特定实现，同时保持其余继承行为的完整性。

在本章中，我们将涵盖以下主要主题：

- **VB.NET** 中的继承
- 家族成员之间的继承差异
- 异常处理
- Is-a 关系

## VB.NET 中的继承

在 **VB.NET** 继承中，类可以继承另一个类（称为基类或父类）的属性、方法和事件。从基类继承的类称为子类。继承允许你创建类的层次结构，促进代码重用和更有组织的代码结构。

为了演示 VB.NET 中的继承，让我们创建一个带有基类 **Animal** 和两个派生类 **Dog** 和 **Cat** 的示例：

```
Public Class Animal
    Public Property Name As String
    Public Overridable Function Talk() As String
        Return "animal noise"
    End Function
End Class
```

**Dog** 类将继承自 **Animal** 类：

```
Public Class Dog Inherits Animal
    Public Property Breed As String
    Public Overrides Function Talk() As String
        Return "Bark!"
    End Function
End Class
```





The Cat class will also inherit from the **Animal** class:

```
```vb
Public Class Cat Inherits Animal
    Public Property Color As String
    Public Overrides Function Talk() As String
        Return "Meow!"
    End Function
End Class
```
```

We can use these classes in code:

```
```vb
Module Module1
    Sub Main()
        Dim myDog As New Dog()
        myDog.Name = "Maisie"
        myDog.Breed = "Dalmatian"
        Console.WriteLine(myDog.Name + " - " + myDog.Breed
            + " - " + myDog.Talk())
        Dim myCat As New Cat()
        myCat.Name = "Woody"
        myCat.Color = "Black"
        Console.WriteLine(myCat.Name + " - " + myCat.Color
            + " - " + myCat.Talk())
    End Sub
End Module
```
```

In the preceding code block, we created a base class called **Animal** with a **Name** property and a **Talk** method. The **Talk** method is marked as **Overridable**, which means it can be overridden in the derived classes.

Then, we created two derived classes, **Dog** and **Cat**, each inheriting from **Animal**. In the derived classes, we added specific properties (**Breed** for Dog and **Color** for Cat) and overrode the **Talk** method to provide their implementations of the sound they make.

Using inheritance, we can define typical behavior in the base class and extend it in the derived classes, promoting code reuse and maintaining a clear hierarchy of classes.

Next, we will explore the difference in inheritance in **VB** family members.

## Inheritance differences in family members

Inheritance in VB.NET and VB6/VBA share the same core concept of allowing classes to inherit properties and behaviors from other classes. However, there are significant differences in how inheritance is implemented and supported between the two versions of VB. Let’s look at these differences.

## VB6/VBA

- **Limited support:** VB6 supports basic OOP, including inheritance. However, it lacks some essential features that are found in modern object-oriented languages.
- **Class modules:** In VB6, you create classes using class modules. Each class can have properties, methods, and events.
- **Inheritance keyword:** VB6 uses the `Inherits` keyword to establish an inheritance relationship between derived and base classes. However, VB6 does not support the inheritance of implementation details as it lacks the concept of access modifiers such as `public`, `protected`, or `private`.
- **Interface-based inheritance:** VB6 supports interface-based inheritance, allowing a class to implement one or more interfaces to provide a set of specific functionalities. However, VB6 does not support multiple inheritance of classes, meaning a class can only inherit from a single base class.

## VB.NET

- **Full object-oriented support:** VB.NET is a fully OOP language, and it is part of the .NET framework, which provides extensive support for OOP principles, including inheritance.
- **Classes and access modifiers:** In VB.NET, classes are created using the `Class` keyword. You can define properties, methods, events, and access modifiers (`public`, `private`, `protected`, and so on) to control the visibility of members within the class hierarchy.
- **Inheritance keywords:** VB.NET uses the `Inherits` keyword to denote inheritance relationships between classes, similar to VB6. However, it also uses the `Overrides` keyword to indicate that a method in the derived class overrides one in the base class.
- **Multiple inheritance through interfaces:** VB.NET supports multiple inheritance through interfaces, allowing a class to implement multiple interfaces to provide various functionalities. However, multiple inheritance of classes is not directly supported to avoid issues related to the “diamond problem.”
- **Abstract and sealed classes:** VB.NET allows you to define abstract classes (using the `MustInherit` keyword) that cannot be instantiated but can serve as base classes for other classes. Additionally, you can create sealed classes (using the `NotInheritable` keyword) to prevent further inheritance from that class.

VB.NET offers more robust support for OOP and inheritance than VB6. It introduces modern OOP concepts, access modifiers, interface-based inheritance, and more advanced language features.

Next, we will look at handling errors, which requires an understanding of inheritance.

## Exception handling

In VB, exception handling is primarily accomplished using structured error-handling constructs, such as Try…Catch…Finally blocks, rather than inheritance. However, it’s worth noting that exception handling in VB does involve classes and inheritance but in a different manner.

VB uses the **Exception** class, which is a base class for all exceptions in the .NET Framework, to handle errors and exceptions. The **Exception** class has several derived classes representing specific types of exceptions, such as `SystemException` and `ApplicationException`. When an exception occurs, VB throws an object of one of these derived exception classes, which you can then catch and handle using Try…Catch…Finally blocks.

Here’s an example demonstrating exception handling in VB using Try…Catch…Finally:

```
Sub Main()
    Try
        Dim n1 As Integer = 20
        Dim n2 As Integer = 0
        Dim result As Integer = n1 \ n2
        Console.WriteLine("Result: " & result)
    Catch ex As DivideByZeroException
        Console.WriteLine("Error: " & ex.Message)
    Catch ex As Exception
        Console.WriteLine("Error: " & ex.Message)
    Finally
        Console.WriteLine("Finally block executed.")
    End Try
    ' Rest of the code continues after exception handling.
End Sub
```

In this example, we attempted to divide an integer by zero, which resulted in DivideByZeroException. The Try…Catch…Finally block allows us to handle this exception gracefully, providing custom error messages or performing cleanup operations in the Finally block.

While inheritance isn’t directly involved in the exception-handling process, the Exception class and its derived classes represent a hierarchy of exceptions that can be used to catch specific types of errors and provide appropriate error handling based on the particular exception type. In this sense, inheritance plays a role in the design of the exception hierarchy.

Next, we will look at is-a relationships and taxonomies.

## Is-a relationships

The is-a relationship is a fundamental concept in OOP that describes the inheritance relationship between classes. It is also known as the “inheritance relationship” or “subclass-superclass relationship.” The is-a relationship signifies that one class is a specialized version of another, emphasizing a hierarchical classification of objects.

When we say Class A is-a Class B, this means that Class A is a specific type of Class B. Class A inherits the properties, methods, and behavior of Class B, and it can also have additional properties and methods specific to its nature.

In the example with cats and dogs we built earlier in this chapter, we can say the following:

- Dog is-a Animal
- Cat is-a Animal

These is-a statements imply that `Dog` and `Cat` are specialized types of `Animal`, inheriting the common properties and methods defined in the `Animal` class.

The is-a relationship enables code reuse and promotes a hierarchical organization of classes, leading to more organized and maintainable code. It also allows you to model real-world relationships and hierarchies in your software, making it more intuitive and easier to understand.

In OOP, the is-a relationship is often used as the basis to build class hierarchies, where more specific classes inherit from more general classes, leading to a more efficient and structured design.

## Summary

This chapter introduced inheritance in VB. First, we looked at defining taxonomies in VB that allow for enhanced code reuse. Then, we drilled into the concept of is-a relationships, where children classes are instances of the parent class. The next chapter will utilize these taxonomies to implement polymorphism in VB programming.




