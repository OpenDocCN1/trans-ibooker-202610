The references to both \_a and \_\_b fail. This single-
underscore rule applies to double underscores, too, of course.
Changing the way you import the symbols makes the program
run successfully, as in the following example.

Click here to view code image

```
# File run_me.py ----------------------------------------
from mod_a import _a, __b  # This now works.

print(_a)
print(__b)

# File a_mod.py ----------------------------------------
_a, __b = 10, 100
```

The difference between the two examples is that the second
one uses the **import** keyword to recognize \_a and \_\_b
specifically, rather than relying on the asterisk (*) syntax.

# 14.6 THE MAIN MODULE AND “\_\_MAIN__”

Earlier, we claimed that modules are objects and that they have
attributes just as other objects do. Here’s a simple program that
creates some attributes and displays them. Note in particular
what’s printed as the module name.

Click here to view code image