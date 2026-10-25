## 函数与类定义

```python
def set_list_vals(list_arg):
    assert isinstance(list_arg, list), 'arg must be a list'
    assert len(list_arg) >= 3, 'list argument too short'
    list_arg[0] = 100
    list_arg[1] = 200
    list_arg[2] = 150

total = 0.0
while True:
    s = input('Enter number: ')
    if not s:         # Break on empty-string input.
        break
    total += float(s) # Only executed on non-empty s.
print(total)

class class_name [(base_classes)]:
    statements

my_pt = Point(10, 20)   # my_pt.x = 10, my_pt.y = 20

for let in 'You moved Dover!':
    if let == 'D' or let == 'd':
        continue
    print(let, end='')

def hypotnuse(side1, side2):
    total = side1 * side1 + side2 * side2
    return total ** 0.5   # Return square root of total.

def floopy():
    return 100

print(floopy())     # Call floopy, no args! Print 100
print(hypotenuse(3, 5))

a_list = [1, 2, 3]
b_list = a_list     # Create alias for the list
del a_list          # Remove a_list from symbol table
print(b_list)       # List referred to still exists.

for loop_var in iterable:
    statements
[else:
    statements]     # Executed if first block of statements
                    # finished successfully, without break
```
