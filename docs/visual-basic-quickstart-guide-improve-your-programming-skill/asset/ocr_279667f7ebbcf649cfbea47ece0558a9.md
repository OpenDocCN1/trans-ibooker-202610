# Iteration

In programming, iteration refers to the process of repeating a sequence of instructions multiple times. A programming language that supports iteration provides a way to execute a block of code repeatedly, either a fixed number of times or until a specific condition is met.

Most programming languages provide some form of iteration control structure, such as a for loop, while loop, do-while loop, or foreach loop, that allows programmers to perform iterations in a structured way.

VB is a programming language that supports various forms of iteration, including For loops, While loops, and Do loops. These loops allow you to repeat a block of code a specific number of times or until a particular condition is met. VB also supports statements to change the loop flow while inside the iteration.

In this chapter, we’re going to cover the following main topics:

+   - For loops
+   - While loops
+   - Do loops
+   - Exit and Continue statements
+   - Type safety differences in loops between VB.NET and other family members
+   - Infinite loops

# For loops

A For loop is a control structure that allows you to execute a block of code repeatedly for a fixed number of times. The basic syntax for a For loop in VB is as follows:

```
For cntr As Integer = start To end Step stepValue
    'code to execute
Next
```

In this syntax, cntr is the loop variable that is used to control the loop. The loop will start with the value of start and end with the value of end, incrementing the variable by stepValue each iteration the loop executes. The stepValue is optional and defaults to 1 if not specified.

Here’s an example of VB.NET code that will loop over a range of numbers and prints each number to the console: