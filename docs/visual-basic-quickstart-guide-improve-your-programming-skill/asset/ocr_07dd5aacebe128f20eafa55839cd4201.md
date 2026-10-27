```
```vb
Public Sub DisplayMessage(message As String)
    Console.WriteLine(message)
End Sub
```
```

In the preceding example, **DisplayMessage** is a public method that accepts a string parameter named **message**. It displays the message on the console.

+   - **Function procedures**: Function procedures are methods that return a value. They are used when you need to perform calculations or retrieve data from a method. Here’s an example:

```
```vb
Public Function AddNumbers(a As Integer, b As Integer)
    As Integer
    Return a + b
End Function
```
```

In the preceding example, **AddNumbers** is a public method that accepts two **Integer** parameters, a and b, and returns their sum as an integer value.

+   - **Access modifiers**: Like fields, methods can have different access modifiers to control their visibility and accessibility. Common access modifiers in VB include **Public**, **Private**, **Protected**, and **Friend**.

+   - **Method overloading**: VB supports method overloading, which allows you to define multiple methods with the same name but different parameters. The compiler determines which method to call based on the arguments provided during the method invocation. Here’s an example of method overloading:

```
```vb
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
```

In the preceding example, **DoSomething** is overloaded with three different versions based on the presence and type of parameters.

Methods in VB enable you to encapsulate functionality within classes and provide a way to perform actions or calculations. They allow for code reuse, modularity, and better logic organization within your programs.

Next, we will look at instantiating a class in VB.

# Instantiating a class

In VB, you can instantiate classes by creating objects from those classes. The process involves using the **New** keyword, followed by the class name and any necessary arguments. Here’s an example of how to instantiate a class in VB:

```
```vb
Dim myObject As New ClassName()
```
```

In the preceding example, **ClassName** represents the name of the class you want to instantiate, and **myObject** is the variable that will hold the created object.