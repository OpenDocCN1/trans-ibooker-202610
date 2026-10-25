

keys = ['Bob', 'Carol', 'Ted', 'Alice']
vals = [4.0, 4.0, 3.75, 3.9]
grade_dict = { keys[i]: vals[i] for i in range(len(keys)) }

grade_dict = { 'Bob':4.0, 'Carol':4.0, 'Ted':3.75,
'Alice':3.9 }

grade_dict = { key: val for key, val in zip(keys, vals)}

{ value for_statement_header optional_if_cond }

{ key : value for_statement_header optional_if_cond }

idict = {v : k for k, v in phone_dict.items() }

def set_list_vals(list_arg):
    list_arg[0] = 100
    list_arg[1] = 200
    list_arg[2] = 150

a_list = [0, 0, 0]
set_list_vals(a_list)
print(a_list) # Prints [100, 200, 150]

def set_list_vals(list_arg):
    list_arg = [100, 200, 150]

a_list = [0, 0, 0]
set_list_vals(a_list)
print(a_list) # Prints [0, 0, 0]

my_list[0] = new_data  # This really changes list data.

weird_list = [ [1, 2, 3], 'John', 'George' ]

mat = [[10, 11, 21], [20, 21, 22], [25, 15, 15]]

list_name[row_index][column_index]

weird_mat = [[1, 2, 3, 4], [0, 5], [9, 8, 3]]

len(weird_mat)      # Equal to 3.

len(weird_mat[0])   # Equal to 4.
len(weird_mat[1])   # Equal to 2.
len(weird_mat[2])   # Equal to 3.

big_list = [0] * 100  # Create a list of 100 elements
                    # each initialized to 0.

mat = [ [0] * 100 for i in range(200) ]

matrix_name = [[init] * ncols for var in range(nrows)]

mat2 = [ [0] * 25 for _ in range(30) ]

mat2 = [[ [0] * 25 for _ in range(20) ]
        for _ in range(30) ]

mat2 = [[[ [0] * 10 for _ in range(10) ]
for _ in range(10) ]
for _ in range(10) ]

>>> my_str = 'I am Hen-er-y the Eighth,' ' I am!'
>>> print(my_str)
I am Hen-er-y the Eighth, I am!

my_str = 'I am Hen-er-y the Eighth,' \
' I am!'

my_str = ('I am Hen-er-y the Eighth, '
'I am! I am not just any Henry VIII, '
'I really am!')

length_of_hypotenuse = ( (side1 * side1 + side2 * side2)
** 0.5 )

beat_list = ['John', 'Paul', 'George', 'Ringo']
for i in range(len(beat_list)):
    print(beat_list[i])

beat_list = ['John', 'Paul', 'George', 'Ringo']
for guy in beat_list:
    print(guy)

beat_list = ['John', 'Paul', 'George', 'Ringo']
for i, name in enumerate(beat_list, 1):
    print(i, '. ', name, sep='')

s1: A string...with more stuff!
s2: A string.

str_list = []
n = ord('a')
for i in range(n, n + 26):
    str_list += chr(i)
alphabet_str = ''.join(str_list)

a = 4, 8, 12  # a is now a tuple containing three values.

temp = a     # Preserve old value of a
a = a + b    # Set new value of a
b = temp     # Set b to old value of a

tup = 10, 20, 30
a, b, c = tup
print(a, b, c)  # Produces 10, 20, 30

tup = 10, 20, 30
a, b = tup  # Error: too many values to unpack

my_tup = (3,)   # Assign tuple with one member, 3.

big, bigger, *many = 100, 200, 300, 400, 500, 600

>>> print(big, bigger, many, sep='\n')
100
200
[300, 400, 500, 600]

my_list = [0, 0, 0, 0, 0, 0, 0, 0...]

def double_me(n):
    n *= 2

a = 10
double_me(a)
print(a)    # Value of a did not get doubled!!

def quad(a, b, c):
    determin = (b * b - 4 * a * c) ** .5
    x1 = (-b + determin) / (2 * a)
    x2 = (-b - determin) / (2 * a)
    return x1, x2

>>> x = quad(1, -1, -1)
>>> x
(1.618033988749895, -0.6180339887498949)

def find_divisor(n, max):
    for i in range(2, max + 1):
        if n % i == 0:
            print(i, 'divides evenly into', n)
            break
    else:
        print('No divisor found')

test_str = input('Enter test string: ')
a_list = [c.upper() for c in test_str if c.isalnum()]
print(a_list == a_list[::-1])

Enter test string: A man, a plan, a canal, Panama!
True
a_list = [c for c in s if c not in 'aeiou']
s = ''.join(a_list)
def get_avg(a_list):
    return sum(a_list) / len(a_list)
if 0 < x < 100:    # Use 'chained' comparisons.
    print('x is in range.')
a, b, c = 5, 10, 15
if 0 < a <= c > b > 1:
    print('All these comparisons are true!')
    print('c is equal or greater than all the rest!')
a = b = c = d = e = 100
if a == b == c == d == e:
    print('All the variables are equal to each other.')
if min(a_list) == max(a_list):
    print('All the elements are equal to each other.')
fn = [do_plot, do_highlow_plot, do_volume_subplot,
    do_movingavg_plot][n-1]
fn(stockdf)    # Call the function

menu_dict = {'load':load_fn, 'save':save_fn,
 'exit':exit_fn, 'update':update_fn}
(menu_dict[selector])()   # Call the function

>>> s1 = 'I am what I am and that is all that I am.'
>>> s2 = 'I am what I am' + ' and that is all that I am.'
>>> s1 == s2
True
>>> s1 is s2
False

a_value = my_function()
if a_value is None:
    # Take special action if None is returned.

for var in sequence: statement
>>> for i in range(10): print(i, end=' ')
0 1 2 3 4 5 6 7 8 9
>>> for i in range(5): n=i*2; m = 5; print(n+m, end=' ')
5 7 9 11 13
>>> a = 1; b = 2; c = a + b; print(c)
3

cell = 'X' if turn % 2 else '0'
true_expr if conditional else false_expr

red, blue, green, black, white = range(5)

red, blue, green, black, white = range(1, 6)

row_of_asterisks = '*' * 40
s = ''
for i in range(20):
    s += row_of_asterisks + '\n'
print(s)

row_of_asterisks = '*' * 40
list_of_str = []
for i in range(20):
    list_of_str.append(row_of_asterisks)
print('\n'.join(list_of_str))

print('\n'.join(['*' * 40] * 20))

def quad(a, b, c):
    """Quadratic Formula function.

    This function applies the Quadratic Formula
    to determine the roots of x in a quadratic
    equation of the form ax^2 + bx + c = 0.
    """
    determin = (b * b - 4 * a * c) ** .5
    x1 = (-b + determin) / (2 * a)
    x2 = (-b - determin) / (2 * a)
    return x1, x2

>>> help(quad)
Help on function quad in module __main__:

quad(a, b, c)
    Quadratic Formula function.

    This function applies the Quadratic Formula
    to determine the roots of x in a quadratic
    equation of the form ax^2 + bx + c = 0.

def quad(a, b, c):
    ''"Quadratic Formula function.

    This function applies the Quadratic Formula
    to determine the roots of x in a quadratic
    equation of the form ax^2 + bx + c = 0.
    ''"
    determin = (b * b - 4 * a * c) ** .5
    x1 = (-b + determin) / (2 * a)
    x2 = (-b - determin) / (2 * a)
    return x1, x2

def quad(a, b, c):
    ''"Quadratic Formula function.

    This function applies the Quadratic Formula
    to determine the roots of x in a quadratic
    equation of the form ax^2 + bx + c = 0.
    ''"

import package_name [as new_name]

import matplotlib.pyplot as plt
from package_name import symbol_name
from package_name import *
def avg(a_list):
    """This function finds the average val in a list."""
    x = (sum(a_list) / len(a_list))
    print('The average is:', x)
    return x
def new_func(a_list):
    return (sum(a_list) / len(a_list))
old_avg = avg
avg = new_func
def func_info(func):
    print('Function name:', func.__name__)
    print('Function documentation:')
    help(func)
Function name: avg
Function documentation:
Help on function avg in module __main__:
avg(a_list)
    This function finds the average val in a list.
def func_name([ordinary_args,] *args):
    statements

def my_var_func(*args):
    print('The number of args is', len(args))
    for item in args:
        print(items)
>>> my_var_func(10, 20, 30, 40)
The number of args is 4
10
20
30
40

def avg(units, *args):
    print (sum(args)/len(args), units)
>>> avg('inches', 11, 22, 33)
22.0 inches

>>> ls = [1, 2, 3] # Unpacked list.
>>> print(*ls) # Print unpacked version
1 2 3
>>> print(ls) # Print packed (ordinary list).
[1, 2, 3]

print(10, 20, 30, end='.', sep=',')

def func_name([ordinary_args,] *args, **kwargs):
    statements

def pr_named_vals(**kwargs):
    for k in kwargs:
        print(k, ':', kwargs[k])
>>> pr_named_vals(a=10, b=20, c=30)
a : 10
b : 20
c : 30

def pr_vals_2(*args, **kwargs):
    for i in args:
        print(i)
    for k in kwargs:
        print(k, ':', kwargs[k])
pr_vals_2(1, 2, 3, -4, a=100, b=200)

import time
def make_timer(func):
    def wrapper():
        t1 = time.time()
        ret_val = func()
        t2 = time.time()
        print('Time elapsed was', t2 - t1)
        return ret_val
    return wrapper

