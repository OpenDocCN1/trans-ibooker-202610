```
Else
    res = n1 / n2
End If
Case Else
    MsgBox ("Invalid operator!")
End Select
MsgBox ("The result is: " & res)
End If
Loop While Not sentinel = True
End Sub
```

# Coding a VBA calculator in Excel

The following steps allow you to code and execute a VBA macro displaying Hello World:

+   1. Start **Microsoft Excel**.
+   2. Display the **View** menu.
+   3. Click the **Macros** button and choose **View Macros** from the drop-down menu.
+   4. Type the name **test** as the macro name.
+   5. Click the **Create** button.
+   6. When the Designer loads, double-click inside the rectangle representing your form.
+   7. Enter the code example from the preceding section.
+   8. Click the green triangle on the toolbar above your code to run your macro.

We will now move on to input and output in VBScript, which is both similar to and different from what you have seen. This will be similar in terms of the flow of the program but different in the way the user input and output are embedded into the VBA host. In this example, the VBA host is Microsoft Excel.

In our next section, we will tackle the calculator example in VBScript.

# Building the project in VBScript

The following VBScript program prompts the user to enter two numbers and an operator, performs the specified arithmetic operation, and displays the result.

The **Option Explicit** statement enforces explicit variable declaration, which helps to prevent errors caused by misspelling or mistyping variable names.

The program uses a loop with a **sentinel** variable to allow the user to enter multiple sets of numbers and operators until they choose to quit. The **sentinel** variable is initially set to **False**.

The program prompts the user to enter the first number or **x** to quit. If the user enters **x**, the **sentinel** variable is set to **True**, and the program exits the loop. If the user enters a number, the program prompts them to enter the second number and the operator.