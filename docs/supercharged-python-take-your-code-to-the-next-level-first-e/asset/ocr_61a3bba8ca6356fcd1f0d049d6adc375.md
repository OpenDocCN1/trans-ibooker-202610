```
the_stk.append(op1 * op2)
elif tok == '/':
    the_stk.append(op1 / op2)
elif tok == '-':
    the_stk.append(op1 - op2)
```

The `bin_op` function is called by the top line of the scanner object whenever the scanner finds an operator: `*`, `+`, `/`, or `-`. That operator is then passed as an argument (`tok`), which in turn is used to decide which of the four operations to carry out.

These lambda functions, it should be clear, do relatively little except call other functions. The top line (recognizing operator tokens) just calls the `bin_op` function, passing along the operator token itself. The second and third lines append an integer or floating point as appropriate.

There’s a subtlety here. Each of the lambda functions gets called with two arguments, `s` and `t` (standing for the scanner and token, respectively), but each lambda function calls some other function while passing along one argument.

Now, armed with the appropriate Scanner object and a `bin_op` function to do much of the work, we just need a `main` function that gets a line of input, scans it, and finishes.

Here, then, is the completed application:

> Click here to view code image

```
# File scanner_rpn.py -------------------------------------------
-
import re
the_stk = [ ]
```