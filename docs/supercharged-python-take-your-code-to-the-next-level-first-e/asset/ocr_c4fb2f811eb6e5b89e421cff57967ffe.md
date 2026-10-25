This translates into

```
2 * (4 + (2 + (3 + 7)))
```

which evaluates to 32. The beauty of an RPN expression is that parentheses are never needed. The best part is that the interpreter follows only a few simple rules:

+   - If the next item is a number, push it on the stack.
+   - If the next item is an operator, pop the top two items off the stack, apply the operation, and then push the result.

Here’s the pseudocode for the application:

![](bbox=[0.151, 0.4736, 0.2573, 0.55])

```
Get an input string.
Split it into tokens and store in a list.
For each item in the list,
    If item is an operator,
        Pop stack into op2
        Pop stack into op1
        Carry out operation and push the result onto
the stack.
    Else
        Push item onto the stack as a float value.
Pop stack and print the value.
```