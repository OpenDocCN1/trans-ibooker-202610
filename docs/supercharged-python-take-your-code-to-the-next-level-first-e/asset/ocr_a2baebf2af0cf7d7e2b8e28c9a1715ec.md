```
import module
import module as short_name
from module import sym1 [, sym2]...
from module import *
```

The first two versions make symbols accessible but only as qualified names, as in `math.pi` or `math.e`. The third version makes symbols accessible without qualification, but only those listed. The fourth version makes all symbols in the module available without qualification.

This last version is the most convenient, but it presents the danger of naming conflicts if the named module or package defines many symbols. For large modules and packages with large namespaces, the other versions of the `import` statement are recommended.

See Chapter 14, “Multiple Modules and the RPN Example,” for more information.

# ## nonlocal Statement

This statement has a syntax that is similar to that of the `global` statement.

```
nonlocal var1 [, var2]...
```

The purpose of the `nonlocal` statement is similar to that of the `global` statement, with one difference: `nonlocal` is used to deny local scope and to prefer an enclosing, yet nonglobal, scope. This only ever happens when a function definition is nested inside another function definition. For that reason, the