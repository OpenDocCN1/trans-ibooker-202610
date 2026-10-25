## 变量赋值与条件语句

```python
var1 = var2 = var2 = ... varN = expression
a = 5.5
b = 5.5 * 100
x = y = z = 0        # Assign x, y, z the same val
var1 = var2 = 1000 / 3 # Assign var1, var2 the same val
a, b = 1, 100
if a < b:
    print('a is less than b')
    print('a < b')
else:
    print('a not less than b')
a, b = 1, 100
if a < b:
    print('a < b')
else:
    print('a not less than b')    # ERROR!
a = 1; b = 100; a += b; print(a)
assert expression, error_msg_str
```
