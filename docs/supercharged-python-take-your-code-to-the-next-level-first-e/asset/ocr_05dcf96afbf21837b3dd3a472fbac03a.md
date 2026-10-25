```
# File listing for run_me.py ----
from module2 import *   # Import x, y, pr_nice
from module2 import z   # Import z
print('z is %i, for heaven's sake!' % z)
```

This version of the code prints the following:

```
z is 10, for heaven's sake!
```

This example works, because in this case, z is imported on its own, in a separate statement.

# 14.5 PUBLIC AND PRIVATE MODULE VARIABLES

The previous section raised an issue: Can you make a module-level symbol “private” to a module?

> The effect described in this section— of making names with a leading underscore (_) more difficult to import— is separate from the name mangling that takes place when you’re attempting to access a private attribute (signified by leading double underscores) from outside a class.
> 
> See Section 9.6, “Public and Private Variables,” for more information on name mangling.

Other computer languages tend to have ways of privatizing a symbolic name. In Python, that feature is deemphasized,