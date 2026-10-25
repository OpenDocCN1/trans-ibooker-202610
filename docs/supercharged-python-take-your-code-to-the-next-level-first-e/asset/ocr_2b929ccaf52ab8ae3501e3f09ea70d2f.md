because the philosophy of Python is to make it easy to create something and just run it. However, names beginning with an underscore have special properties in Python.

![](bbox=[0.149, 0.187, 0.257, 0.266])

```
`_name`
```

When a name beginning with an underscore is created in a Python module, it’s not accessible to another module that imports it using the `import *` syntax.

```
`from mod_a import *`
```

For example, the following program causes errors if `run_me.py` is run, because it assumes that `_a` and `_ _b` are accessible, but they’re not.

Click here to view code image

```
```python
# File run_me.py ------------------------------------------------
from mod_a import *        # This will fail.

print(_a)
print(__b)

# File mod_a.py ------------------------------------------------
_a, __b = 10, 100
```
```