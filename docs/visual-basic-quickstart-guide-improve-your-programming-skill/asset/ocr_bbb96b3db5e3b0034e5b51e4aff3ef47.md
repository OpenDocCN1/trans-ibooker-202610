```
x += 1
End While
```

Next, we will look at loops executing one or more times based on a conditional test.

# Do loops

A post-condition loop is a control flow structure in computer programming that repeatedly executes a code block until a specific condition is met after completing the loop body. In contrast to a pre-condition loop, a post-condition loop executes the loop body first and then evaluates the condition to determine whether it should continue iterating. Because of the first iteration, a post-condition loop will always run one or more times.

In VB, a Do loop is a control structure that implements a post-condition loop. The basic syntax for a Do loop in VB is as follows:

```
Do
    'code to run
Loop While test_condition
```

In this example, the loop will run the code block if test_condition is true. After executing the loop body, the Do loop evaluates test_condition at the end of each iteration. If test_condition is false, the loop terminates, and execution continues with the statement following the loop.

Here’s a VB.NET example of a Do loop in VB that reads user input until a valid integer is entered:

```
Dim sentinel As Boolean = False
Console.WriteLine("Enter test scores. Enter -1 to stop.")
Do
    Dim input As String = Console.ReadLine()
    If input = "-1" Then
        sentinel = True
    End If
Loop While Not sentinel
```

A sentinel in programming is a particular value or character that signals the end of a sequence or input data. A sentinel is used to mark the end of a list when the length of the list is not known at the start of the loop. The sentinel value is typically chosen not to be mistaken for a valid data value.

In the preceding example, the program reads a sequence of integers from the user, terminated by the value -1. The program uses -1 as the sentinel value to indicate the end of the input.

Next, we will look at ways to modify how the loop executes.

# Exit and Continue statements