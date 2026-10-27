
# Polymorphism

Polymorphism is a fundamental concept in programming and is often associated with **object-oriented programming (OOP)**. It allows objects of different classes to be used as objects of a common superclass or interface, providing a consistent interface to interact with these objects regardless of their specific types.

In this chapter, we’re going to cover the following main topics:

- Polymorphism
- Abstract classes
- Sealed classes

## Polymorphism

Polymorphism allows a programmer to specialize functionality based on a context; the programming language will use the proper version. There are two main types of polymorphism:

- **Compile-time (static) polymorphism:** Also known as method overloading or compile-time method dispatch, this type of polymorphism occurs when different methods or functions have the same name but different parameter lists. The decision on which method to call is made at compile time based on the number and types of arguments provided. This allows you to have multiple methods with the same name but different behaviors based on the input parameters.
- **Runtime (dynamic) polymorphism:** Also known as method overriding or runtime method dispatch, this type of polymorphism occurs when a subclass provides a specific implementation of a method already defined in its superclass (or an interface). At runtime, the method call is resolved to the subclass implementation if the object referenced is an instance of the subclass. This allows you to use a common interface to invoke different behaviors based on the actual type of the object.

In **Visual Basic (VB)**, as in other OOP languages, polymorphism is achieved through inheritance and method overriding. VB supports runtime polymorphism, where the objects of derived classes can be treated as objects of their base class, and the appropriate method implementation is invoked based on the actual type of the object at runtime.

To demonstrate polymorphism in VB, we can create a simple example with a base class and its derived classes:

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

In this example, we defined an abstract class called Shape, with an abstract method called Draw, and a non-abstract method called GetArea. The Draw method is marked with the MustOverride keyword, indicating that any derived class must provide its implementation for the Draw method.

Next, let’s create a derived class that inherits from the abstract class and implements the Draw method:

```
Public Class Circle
    Inherits Shape
    Public Overrides Sub Draw()
        Console.WriteLine("Drawing a circle")
    End Sub
End Class
```

In this example, the Circle class inherits from the Shape abstract class and provides an implementation for the Draw method.

Since abstract classes cannot be directly instantiated, we can use them through their derived classes:

```
Module MainModule
    Sub Main()
        Dim circle As New Circle()
        circle.Draw()  ' Output: Drawing a circle
        circle.GetArea()  ' Output: Calculating area...
    End Sub
End Module
```

In this Main method, we created an instance of the Circle class and called its Draw method, which is defined in the Shape abstract class. We can also call the non-abstract GetArea method provided by the abstract class.

Abstract classes are useful when you want to define a common structure and behavior across multiple related classes while enforcing that certain methods must be implemented in the derived classes. They provide a level of abstraction and help organize code effectively in OOP.

Next, we’ll look at ways to ensure that a class cannot be extended through inheritance.

## Sealed classes

In VB, there is no direct equivalent keyword to sealed, which exists in some other programming languages, such as C#. In C#, the sealed keyword is used to prevent a class from being inherited (that is, it cannot serve as a base class). The sealed

concept is important if the programmer has code that should never be specialized. An example could be an authorization class that validates a user and you do not want changes to the authorization method. However, in VB, all classes are inheritable by default, meaning that any class can be used as a base class unless it is specifically designed as an abstract class (using the MustInherit keyword, as explained in the previous section). To prevent a class from being inherited in VB, you can use the NotInheritable keyword when defining the class. The NotInheritable keyword is the equivalent of the sealed keyword in C#.

Here’s an example of a NotInheritable class in VB:

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

In this example, the Singleton class is marked as NotInheritable, meaning no other class can inherit from it. The Singleton class is often used when you want to create a class with only one instance (the Singleton pattern) and ensure that no one can create additional instances by inheriting from it.

In VB, you can use the NotInheritable keyword to prevent a class from being inherited, similar to the sealed keyword in C#.

## Summary

This chapter introduced polymorphism in VB. We looked at defining classes that do not have a complete definition but can be extended down in the inheritance tree. We also looked at ways to implement functionality to enforce that a class cannot be extended with inheritance. The next chapter will look at utilizing interfaces in VB programming to allow for multiple inheritance.

# Interfaces

In **object-oriented programming (OOP)**, interfaces are a fundamental concept that allows you to define a contract or a set of method signatures that classes must implement. An interface serves as a blueprint for implementing classes, ensuring that specific methods are available in those classes. Interfaces enable polymorphism and provide a way to achieve abstraction and separation of concerns in OOP.

In this chapter, we’re going to cover the following main topics:

- Interfaces
- Developing interfaces
- Standard interfaces

## Interfaces

The interface contract has been a tremendous tool for team programming and software extensibility. The interface contract specifies the method name, number of parameters, and data type of the parameters. Here are the key characteristics and benefits of using interfaces in OOP:

- **Abstraction:** Interfaces help in achieving abstraction by defining a clear and standardized set of method signatures without specifying their implementation details. This allows you to hide the internal complexity of classes and focus on the behavior they expose.
- **Separation of concerns:** By using interfaces, you can separate the interface of a class from its implementation. This enables you to change or extend the behavior of a class without it affecting the client code that uses the interface.
- **Polymorphism:** Interfaces play a crucial role in achieving polymorphism. When a class implements an interface, it can be treated as an instance of that interface, allowing objects of different classes to be used interchangeably if they implement the same interface.
- **Multiple inheritance (interface inheritance):** In languages that support multiple inheritance, a class can implement multiple interfaces. This allows a class to inherit behavior from multiple sources, promoting code reuse and flexibility.
- **Contractual obligation:** When a class implements an interface, it must provide concrete implementations for all the methods declared in the interface. This contractual obligation ensures consistency and ensures that all implementing classes adhere to the specified interface.
- **Interface segregation principle (ISP):** Interfaces support the ISP, which states that a class should not be forced to implement interfaces that it does not use. By defining specific interfaces with well-defined responsibilities, you can adhere to the ISP and promote cleaner and more maintainable code.

Interfaces are powerful tools in OOP, helping build flexible and maintainable software by promoting a standard way of interacting with classes and enabling
