
Write code that is easier to read, understand, and maintain by defining custom functions.

Next, we will look at how to modularize code blocks utilizing **procedures**.

## Modularizing code in procedures

In addition to custom functions, VB allows developers to create custom procedures to perform specific tasks within their programs. Procedures are defined using the **Sub** keyword, followed by the name of the procedure, any arguments it takes (if any), and the code that should be executed when the procedure is called. Here’s an example:

```
Sub dspMsg(msg As String)
    MsgBox msg
End Sub
```

This procedure takes a string argument, msg, and displays it in a message box using the **MsgBox** function. The **Sub** keyword indicates that the procedure does not return a value.

To call the procedure, you can use its name and pass in the required arguments:

```
dspMsg("Hello and welcome!")
```

In this example, the **dspMsg** procedure is called with the argument, **“Hello and welcome!”**, displayed in a message box.

Custom procedures can be used to encapsulate complex tasks or sequences of operations and called from any part of the program when they are needed. By defining custom procedures, developers can write code that is easier to read, understand, and maintain. Additionally, procedures can be used to group related tasks, making managing and modifying code easier.

Next, we will examine how parameters are passed and used in functions and procedures.

## Understanding parameters in functions and procedures

Parameters are used in VB to pass values or objects to a function or procedure when it is called. Parameters are specified in the function or procedure definition and are used to define the inputs required by the function or procedure. Here’s an example of a function with two parameters:

```
Function myMult(n1 As Integer, n2 As Integer) As Integer
    Dim m As Integer
    m = n1 * n2
    myMult = m
End Function
```

In this example, the **myMult** function takes two integer parameters, n1 and n2. The function uses these parameters to multiply them, which is returned as an integer value.

You must pass in the required values or objects when calling a function or procedure that takes parameters. Here’s an example of calling the **myMult** function:

```
Dim res As Integer
res = myMult(7, 8)
Console.WriteLine("Result: " & res)
```

In this example, the **myMult** function is called with the arguments 7 and 8. These values are passed as the n1 and n2 parameters, respectively. The result of the calculation is stored in the resulting variable.

VB supports several types of parameters, including the following:

- **ByRef**: Passes a reference to a variable, allowing the called function or procedure to modify its value.
- **ByVal**: Passes a copy of a variable, allowing the called function or procedure to use its value but not modify it.
- **Optional**: Allows a parameter to be omitted when calling the function or procedure. Optional parameters are supported in all family members except VBScript.
- **ParamArray**: Allows various arguments to be passed to a function or procedure. **ParamArray** parameters are supported by all family members except VBScript.

Using parameters in VB allows for more flexible and reusable code, as functions and procedures can be designed to work with different inputs, depending on the program’s needs.

## Return statement

In VB, the **Return** statement is used to exit a function or a Sub procedure and return control to the calling code. It is used to provide a result or value back to the caller if necessary. The **Return** statement can be used in different contexts, depending on whether you work with a function or a procedure.

Here is an example of using the **Return** statement in the VB function:

```
Function myMult(n1 As Integer, n2 As Integer) As Integer
    Dim m As Integer
    m = n1 * n2
    Return m
End Function
```

In this example, the **Return** statement returns the result of the two numbers, n1 and n2, multiplied together to the caller. We saw earlier that a return value can also be set by assigning the function name a value. The second method does not stop execution at that point, compared to the Return statement, which will immediately return to the calling code.

Here is an example of using the Return statement in the VB procedure:

```
Sub DisplayMessage()
    Console.WriteLine("Welcome")
    Return
End Sub
```

In this example, the Return statement is used to exit the Sub procedure and return control to the caller without providing a specific value.

It’s important to note that the Return statement is not required in either a procedure or a function. If a Return statement is omitted, the control automatically returns to the caller when the procedure or function ends.

In addition to the Return statement, VB also provides the Exit Sub and Exit Function statements, which can be used to exit a Sub procedure or a function early without executing the remaining code.

## Summary

This chapter introduced functions and procedures to modularize code for reuse. Once we have modularized our code, we can reuse it without having multiple copies of the code. This allows our code to have less bugs and also allows for more reusable code. In the next chapter, we will utilize the skills we have acquired so far in a project.

## Project Part I

In our first project, we want to use the skills we have learned to build a simple calculator for each VB family member. The calculator should allow the user to enter a simple mathematical operation such as addition, subtraction, multiplication, or division. The calculator should also enable the user to enter the two operands used in the calculation. The calculator will then show the result and allow for another calculation.

In this chapter, we’re going to cover the following main topics:

- Developing a calculator in the VB.NET console
- Developing a calculator in VB6
- Developing a calculator in Visual Basic for Applications (VBA)
- Developing a calculator in VBScript
- Developing a calculator in Classic ASP
- Developing a calculator in ASP.NET

### Technical requirements

Please complete the steps to install and code with the Visual Basic (VB) family member from Chapter 1. The code for this chapter is available in the GitHub repository for this book: https://github.com/PacktPublishing/Learn-Visual-Basics-Quick-Start-Guide-/tree/main/ProjectPartI.

### Building the project in the VB.NET console

This simple console application written in VB.NET programming language performs basic arithmetic operations on two input numbers.

The program first imports the System namespace. Then, it defines a module called program, which contains a single Sub called Main that takes an array of String arguments.

Within the Main method, the program declares five variables: n1, n2, oper of type String, a res of type String initialized to an empty string, and a sentinel of type String initialized to False.

The program enters a Do-While loop until the sentinel is set to True. Within the loop, the program prompts the user to enter the first number or x to quit. If the user enters x, the sentinel variable is set to True, and the loop ends. Otherwise, the program prompts the user to enter the second number and the arithmetic operator.

The program then uses a Select Case statement to perform the appropriate arithmetic operation based on the user’s input. For example, if the operator is +, the program adds the two numbers; if it is -, the program subtracts the second number from the first; if it is *, the program multiplies the two numbers; and if it is /, the program divides the first number by the second, taking care to check for division by zero. Finally, the program displays an error message if the operator is not one of these.

The program then displays the result of the arithmetic operation. The loop continues until the user enters x to quit:

```
Imports System
Module Program
    Sub Main(args As String())
        Dim n1, n2, oper As String
        Dim res As String = ""
        Dim sentinel As String = False
        Do
            Console.WriteLine("Enter the first number or X to quit:")
            n1 = Console.ReadLine()
            If n1 = "X" Then
                sentinel = True
            Else
                Console.WriteLine("Enter the second number:")
                n2 = Console.ReadLine()
                Console.WriteLine("Enter the operator (+, -, *, /):")
                oper = Console.ReadLine()
                Select Case oper
                    Case "+"
                        res = CInt(n1) + CInt(n2)
                    Case "-"
                        res = n1 - n2
                    Case "*"
                        res = n1 * n2
                    Case "/"
                        If n2 = 0 Then
                            Console.WriteLine("Cannot divide by zero!")
                        Else
                            res = n1 / n2
                        End If
                    Case Else
                        Console.WriteLine("Invalid operator!")
                End Select
                Console.WriteLine("The result is: " & res)
            End If
        Loop While Not sentinel = True
    End Sub
End Module
```

Many small changes can be made to make this program nicer. Please try to experiment with ideas to see how you can improve the program.

### Coding a Visual Basic.NET console calculator

The following steps allow you to code and execute a Visual Basic.NET Windows Forms version of Hello World:

1. Start Microsoft Visual Studio.
