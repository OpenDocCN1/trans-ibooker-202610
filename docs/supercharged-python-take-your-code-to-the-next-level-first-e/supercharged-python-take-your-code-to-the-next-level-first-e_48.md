## 异常处理与文件操作

```python
class Dog:
    pass     # This class has no methods yet.

raise [exception_class [(args)]]

return a, b, c     # Exit and return three values.

>>> def div_me(x, y):
    try:
        quot = x / y
    except ZeroDivisionError as e:
        print("Bad division! Text:", e)

else:
    print("Quotient is %s." % quot)
finally:
    print("Execution complete!")

>>> div_me(2, 1)
Quotient is 2.0.
Execution complete!
>>> div_me(2, 0)
Bad division! Text: division by zero
Execution complete!

>> div_me("2", "3")
Execution complete!
Traceback (most recent call last):
  File "<pyshell#21>", line 1, in <module>
    div_me("2", "3")
  File "<pyshell#19>", line 3, in div_me
    quot = x / y
TypeError: unsupported operand type(s) for /: 'str' and 'str'

try:
    f = open('silly.txt', 'r')
    text = f.read()
    print(text)
except IOError as e:
    print('Problem opening file:', e)
except Exception as e:
    print('That was a Bozo no-no.')
    print('Error text:', e)

while condition:
    statements
[else:
    statements] # Executed if first block of statements
                # finished successfully, without break

with open('stuff.txt', 'r') as f:
    print(f.read())  # Print contents of a file.
```


