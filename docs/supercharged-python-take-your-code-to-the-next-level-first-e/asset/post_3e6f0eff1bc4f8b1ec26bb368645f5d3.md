
## 文件与模块导入示例

### 示例 1：基本模块导入

```python
# File run_me.py ------------------------------------
from foo_vars import z, print_z_val
import foo_vars

z = 100
print_z_val()

# File foo_vars.py ------------------------------------
z = 0
def print_z_val():
    print('Value of z is', z)
```

### 示例 2：导入自定义模块 poly

```python
# File do_areas.py ------------------------------------
import poly         # Import the file poly.py

def main():
    r = float(input('Enter radius:'))
    print('Area of circle is', poly.get_circle_area(r))
    x = float(input('Enter side:'))
    print('Area of square is', poly.get_square_area(x))

main()

# File poly.py ------------------------------------
def get_circle_area(radius):
    pi = 3.141593
    return 3.141592 * radius * radius

def get_square_area(side):
    return side * side

def get_triangle_area(height, width):
    return height * width * 0.5

import poly         # Import the file poly.py
```

### 示例 3：模块函数调用语法

```python
poly.get_circle_area(radius)
poly.get_square_area(side)
poly.get_triangle_area(height, width)

module_name.function_name(args)

from module_name import sym1, sym2, sym3...
```

### 示例 4：使用 from 导入特定函数

```python
# File do_areas.py ----
from poly import get_circle_area, get_square_area
def main():
    r = float(input('Enter radius:'))
    print('Area of circle is', get_circle_area(r))

    x = float(input('Enter side:'))
    print('Area of square is', get_square_area(x))

main()

# File poly.py ----
def get_circle_area(radius):
    pi = 3.141593
    return 3.141592 * radius * radius

def get_square_area(side):
    return side * side

def get_triangle_area(height, width):
    return height * width * 0.5

from poly import get_circle_area, get_square_area
```

### 示例 5：通配符导入与 __all__

```python
# File run_me.py ------------------------------------
from module2 import *

pr_nice('x', x)
pr_nice('y', y)

# File module2.py ------------------------------------
x = 1000
y = 500
z = 5

def pr_nice(s, n):
    print('The value of %s is %i.' % (s, n))
    print('And z is %i.' % z)

__all__ = [sym_str1, sym_str2, sym_str3...]
```

### 示例 6：__all__ 列表定义

```python
# File module2.py ----------------------------------------

__all__ = ['x', 'y', 'pr_nice']

x = 1000
y = 500
z = 10

def pr_nice(s, n):
    print('The value of %s is %i.' % (s, n))
    print('And z is %i.' % z)

__all__ = ['x', 'y', 'pr_nice']

# File listing for run_me.py ----------------------------------------

from module2 import *   # Import x, y, pr_nice
from module2 import z   # Import z

print('z is %i, for heaven's sake!' % z)
```

### 示例 7：私有变量导入

```python
# File run_me.py ------------------------------------
from mod_a import *          # This will fail.

print(_a)
print(__b)

# File mod_a.py ------------------------------------
_a, __b = 10, 100

# File run_me.py ------------------------------------
from mod_a import _a, __b   # This now works.

print(_a)
print(__b)

# File a_mod.py ------------------------------------
_a, __b = 10, 100
```

### 示例 8：__name__ 与 dir() 示例

```python
# File run_me.py ----
import mod_a

x = 1
y = 2

print('My name is %s.\n' % __name__)

print(dir())

My name is __main__.

['__annotations__', '__builtins__', '__doc__',
'__file__', '__loader__', '__name__', '__package__',
'__spec__', 'mod_a', 'x', 'y']

# File mod_a.py ----
print('My name is %s.\n' % __name__)

# File start_me_up.py ----
def call_me_first():
    print('Hi, there, Python!')

if __name__=='__main__':
    call_me_first()
```

### 示例 9：循环导入

```python
# File run_me.py ----
import mod_a
mod_a.funcA(5)
# File mod_a.py ----
import mod_b
def funcA(n):
    if n > 0:
        mod_b.funcB(n - 1)
# File mod_b.py ----
import mod_a
def funcB(n):
    print(n)
    mod_a.funcA(n)
# File mod_a.py ----
import mod_b
mod_b.funcB(3)
def funcA(n):
    if n > 0:
        mod_b.funcB(n - 1)
```

## RPN 计算器实现

### 文件 rpn.py

```python
#File rpn.py ----------------------------------------
import re
import operator
from rpn_io import *
# Provide a symbol table; values of variables will be
#  stored here.
sym_tab = {}
stack = []   # Stack to hold the values.
# Scanner: Add items to recognize variable names, which
#  are stored in the symbol table, and to perform
#  assignments, which enter values into the sym. table.
scanner = re.Scanner([
    (r"[ \t\n]", lambda s, t: None),
    (r"-?(\d*)?\.\d+", lambda s, t:
        stack.append(float(t))),
    (r"-?\d+", lambda s, t: stack.append(int(t))),
    (r"[a-zA-Z_][a-zA-Z_0-9]*", lambda s, t:
        stack.append(t)),
    (r"[+]", lambda s, t: bin_op(operator.add)),
    (r"[-]", lambda s, t: bin_op(operator.sub)),
    (r"[*]", lambda s, t: bin_op(operator.mul)),
    (r"[/]", lambda s, t: bin_op(operator.truediv)),
    (r"[\^]", lambda s, t: bin_op(operator.pow)),
    (r"[=]", lambda s, t: assign_op()),
])

def assign_op():
    ''''Assignment Operator function: Pop off a name
    and a value, and make a symbol-table entry.
    ...
    op2, op1 = stack.pop(), stack.pop()
    if type(op2) == str:  # Source may be another var!
        op2 = sym_tab[op2]
    sym_tab[op1] = op2

def bin_op(action):
    ''''Binary Operation evaluator: If an operand is
    a variable name, look it up in the symbol table
    and replace with the corresponding value, before
    being evaluated.
    ...
    op2, op1 = stack.pop(), stack.pop()
    if type(op1) == str:
        op1 = sym_tab[op1]
    if type(op2) == str:
        op2 = sym_tab[op2]
    stack.append(action(op1, op2))

def main():
    a_list = open_rpn_file()
    if not a_list:
        print('Bye!')
        return

    for a_line in a_list:
        a_line = a_line.strip()

if a_line:
    tokens, unknown = scanner.scan(a_line)
    if unknown:
        print('Unrecognized input:', unknown)
print(str(stack[-1]))

main()
```

### 文件 rpn_io.py

```python
#File rpn_io.py
#-----------------------------------------------

def open_rpn_file():
    ''"Open-source-file function. Open a named
    file and read lines into a list, which is
    returned.
    ...
    while True:
        try:
            fname = input('Enter RPN source: ')
            if not fname:
                return None
            f = open(fname, 'r')
            break
        except:
            print('File not found. Re-enter.')
    a_list = f.readlines()
    return a_list
```

### 主程序逻辑

```python
from rpn_io import open_rpn_file
def main():
    a_list = open_rpn_file()
    if not a_list:
        print('Bye!')
        return

for a_line in a_list:
    a_line = a_line.strip()
    if a_line.startswith('PRINTS'):
        do_prints(a_line[7:])
    elif a_line.startswith('PRINTLN'):
        do_println(a_line[8:])
    elif a_line.startswith('PRINTVAR'):
        do_printvar(a_line[9:], sym_tab)
    elif a_line.startswith('INPUT'):
        do_input(a_line[6:], sym_tab)
    elif a_line:
        tokens, unknown = scanner.scan(a_line)
        if unknown:
            print('Unrecognized input:', unknown)
```

### 指令处理函数

```python
def do_prints(s):
    """ Carry out PRINTS directive by printing a
    string.
    """
    a_str = get_str(s)
    print(a_str, end='')

def do_println(s):
    """ Carry out PRINTLN directive: print the
    optional string argument, if specified, and then
    print a newline, unconditionally.
    """
    if s:
        do_prints(s)
    print()

def get_str(s):
    """ Helper function for do_prints.
    """
    a = s.find('"')
    b = s.rfind('"')
    if a == -1 or b == -1:
        return ''
    return s[a+1:b]

def do_printvar(s, sym_tab):
    """ Carry out PRINTVAR directive; look up the
    variable name contained in the string s, and
    then look this name up in the symbol table.
    """
    wrd = s.split()[0]
    print(sym_tab[wrd], end=' ')

def do_input(s, sym_tab):
    """ Carry out INPUT directive; get value input
    from the end user, then enter this value in the
    symbol table, for name contained in string s.
    """
    wrd = input()
    sym_tab[s] = float(wrd)
```

### 运行示例

```
PRINTS 'Enter side 1: '
INPUT side1
PRINTS 'Enter side 2: '
INPUT side2
total side1 side1 * side2 side2 * + =
hyp total 0.5 ^ =
PRINTS 'Hypotenuse equals '
PRINTVAR hyp

Enter RPN source: rpn_hyp.txt
Enter side 1: 30
Enter side 2: 40
Hypotenuse equals 50.0

sym_tab = { }     # Symbol table (for variables)
stack = []        # Stack to hold the values.
pc = -1           # Program Counter

def main():
    global pc
    a_list = open_rpn_file()
    if not a_list:
        print('Bye!')
```
