+   2. Choose **Create New Project**.
+   3. Choose **Console (.NET Framework)** and click **Next**.
+   4. Accept the default project name and solution name.
+   5. Change the location if you want your code in a different folder.
+   6. Accept the default framework.
+   7. Click **Create Project**.
+   8. Enter the code example from the preceding section.
+   9. To run your program, click the green triangle on the toolbar above your code.

Our next section will tackle the calculator example in Visual Basic 6.

# Building the project in VB6

We will now build Visual Basic 6 code for a simple calculator program. The program has a form that loads when the application starts. The form contains a loop that prompts the user to input two numbers and an operator and then operates on the two numbers. The loop prompts the user until they enter x to quit. Finally, the program displays the result of the operation in a message box. For example, if the user tries to divide by zero, a message box says, “**Cannot divide by zero!**”. The program uses the **Cint** function to convert the inputted numbers into integers and handles errors using message boxes:

```
```vb
Private Sub Form_Load()
    Dim n1, n2, res, operator, sentinel As String
    sentinel = False
    Do
        n1 = InputBox("Enter the first number or X to quit:")
        If n1 = "X" Then
            sentinel = True
        Else
            n2 = InputBox("Enter the second number:")
            operator = InputBox("Enter the operator (+, -, *, /):")
            Select Case operator
                Case "+"
                    res = Cint(n1) + Cint(n2)
                Case "-"
                    res = n1 - n2
                Case "*"
                    res = n1 * n2
                Case "/"
                    If n2 = 0 Then
                        MsgBox ("Cannot divide by zero!")
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
```

Again, think of small modifications you can make to this program and give it a try.