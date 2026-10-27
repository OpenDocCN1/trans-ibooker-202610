execution at that point compared to the Return statement, which will immediately return to the calling code.

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

# Summary

This chapter introduced functions and procedures to modularize code for reuse. Once we have modularized our code, we can reuse it without having multiple copies of the code. This allows our code to have less bugs and also allows for more reusable code. In the next chapter, we will utilize the skills we have acquired so far in a project.