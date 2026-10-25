If the `conditional_expr` is any value other than zero, the program counter, `pc`, is set to the value of `line_num`. Otherwise, do nothing.

Could it possibly be that simple? Yes, it is! The only complication is that we need to permit `conditional_expr` to be a variable name, and also (although the use of this will likely be uncommon) permit `line_num` to be a variable.

So far, there are no line labels, so this isn’t a perfect solution. (The implementation of line labels is left as an exercise at the end of the chapter.) The RPN writer will have to count lines, using zero-based indexing, to decide where to jump to.

Here’s an example of what the user will be able to write.

Click here to view code image

```
`PRINTS 'Enter number of fibos to print: '
INPUT n
f1 0 =
f2 1 =
temp f2 =
f2 f1 f2 + =
f1 temp =
PRINTVAR f2
n n 1 - =
n 4 ?`
```

Do you see what this does? We’ll return to that question later.

But look at the last line (`n 4 ?`). To understand what this does, remember that our program counter is designed to be zero-based. It didn’t have to be, but that simplified some of the programming. Because the program counter is zero based, the last line— assuming `n` is not zero— causes a jump back to the