In VB, the Exit and Continue statements are used to modify the flow of execution in loops and conditional statements.

The Exit statement immediately exits a loop or a Select Case or Do Select statement. When the Exit statement is reached, control passes to the next statement after the loop or the Select Case or Do Select statement.

Here is a VB.NET example that searches for a specific value in a loop and will exit the loop if found:

```
Dim test_value As Integer = 7
Dim found As Boolean = False
For x As Integer = 1 To 20
    If x = test_value Then
        Found = True
        Exit For
    End If
Next
If found Then
    Console.WriteLine("The search value was found.")
End If
```

The Continue statement is used to skip to the next loop iteration immediately. When the Continue statement is executed, the loop jumps directly to the next iteration without executing any statements that follow it in the loop body.

Here is a VB.NET example that calculates the sum of all odd numbers in a loop:

```
Dim sum As Integer = 0
For x As Integer = 1 to 20
    If x Mod 2 = 0 Then
        Continue For
    End If
    sum += x
Next
Console.WriteLine("Sum of even numbers: " & sum)
```

In this example, the loop utilizes the Mod operator to test whether the number is evenly divisible by 2. If it is, the Continue statement will skip to the next iteration.

Next up, we think about some changes in VB.NET with loops.

# TYPE SAFETY DIFFERENCES IN LOOPS BETWEEN VB.NET AND OTHER FAMILY MEMBERS

VB.NET is a strongly typed language, meaning you need to declare the data type of variables. The type is not necessary for older VB family members. For example, in VB.NET, you would declare the counter variable in a For loop like this:

```
For counter As Integer = start To end Step increment
```

In other VB family members, you would use the following:

```
For counter = start To end Step increment
```

# Infinite loops