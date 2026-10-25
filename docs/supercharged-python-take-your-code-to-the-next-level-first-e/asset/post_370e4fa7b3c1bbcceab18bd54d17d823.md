

## 字符串方法

stoooge_list = 'Moe, Larry, Curly, Shemp'.split(',')

['Moe', ' Larry', ' Curly', ' Shemp']

str.strip(extra_chars=' ')  # Strip leading & trailing.
str.lstrip(extra_chars=' ')  # String leading chars.
str.rstrip(extra_chars=' ')  # String trailing chars.

str.ljust(width [, fillchar])    # Left justify
str.rjust(width [, fillchar])    # Right justify
str.center(width [, fillchar])   # Center the text.
digit_str.zfill(width)           # Pad with 0's.

new_str = 'Help!'.center(10, '#')
print(new_str)

new_str = '750'.rjust(6, '0')
print(new_str)

## 列表基础

my_int_list = [5, -20, 5, -69]
beat_list = [ 'John', 'Paul', 'George', 'Ringo' ]
mixed_list = [10, 'John', 5, 'Paul' ]
num_list = [3, 2, 17, 2.5]
num_list.sort()  # Sorts into [2, 2.5, 3, 17]
my_list = []    # Must do this before you append!
my_list.append(1)
my_list.append(2)
my_list.append(3)
my_list.remove(1)      # List is now [2, 3]
the_scores = [1.0, 1.0, 9.8, 1.0]
the_scores.remove(1.0) # List now equals [1.0, 9.8, 1.0]
b_list.append(100)
a_list.append(200)
b_list.append(1)
print(a_list)   # This prints [2, 5, 10, 100, 200, 1]
my_list = [1, 10, 5]
yr_list = my_list[:]   # Perform member-by-member copy.

my_list = [100, 500, 1000]
print(my_list[0])         # Print 100.

my_list[1] = 55           # Set second element to 55.

a_list = [100, 200, 300, 400, 500, 600]

print(a_list[0])          # Prints 100.
print(a_list[1])          # Prints 200.
print(a_list[2])          # Prints 300.

print(a_list[0], a_list[1], a_list[2], sep='\n')

a_list = [100, 200, 300, 400, 500, 600]
print(a_list[-1])         # Prints 600.
print(a_list[-3])         # Prints 400.

a_list = ['Tom', 'Dick', 'Jane']

for s in a_list:
    print(s)

[(1, 'Tom'), (2, 'Dick'), (3, 'Jane')]

for item_num, name_str in enumerate(a_list, 1):
    print(item_num, '. ', name_str, sep='')

## 列表切片

a_list = [1, 2, 5, 10, 20, 30]

b_list = a_list[1:3]      # Produces [2, 5]
c_list = a_list[4:]       # Produces [20, 30]

d_list = a_list[-4:-1]    # Produces [5, 10, 20]
e_list = a_list[-1:]      # Produces [30]

a_list = [100, 200, 300, 400, 500, 600]
b_list = a_list[2:5:2]    # Produces [300, 500]

a_list = [100, 200, 300]
rev_list = a_list[::-1]
print(rev_list)           # Prints [300, 200, 100]

my_list = [10, 20, 30, 40, 50, 60]
my_list[1:4] = [707, 777]

my_list = [1, 2, 3, 4]
my_list[0:0] = [-50, -40]
print(my_list)            # prints [-50, -40, 1, 2, 3, 4]

a_list = [1, 3, 5, 0, 2]
b_list = a_list           # Make an alias.
c_list = a_list[:]        # Member-by-member copy

## 成员检测与内置函数

a = [1, 2, 3]
None in a
[] in a

# This produces False
# So does this.

b = [1, 2, 3, [], None]
None in b
[] in b

# This produces True
# So does this.

a_list = [1, 2, [5, 10]]
b_list = a_list[:]   # Member-by-member copy.

import copy

a_list = [1, 2, [5, 10]]
b_list = copy.deepcopy(a_list)   # Create a DEEP COPY.

len(collection)   # Return length of the collection
max(collection)   # Return the elem with maximum
                  # value.
min(collection)   # Return the elem with minimum
                  # value.
reversed(collection)   # Produce iter in reversed order.
sorted(collection)   # Produce list in sorted order.
sum(collection)   # Adds up all the elements, which
                  # must be numeric.

a_list = [100, -3, -5, 120]
print('Length of the list is', len(a_list))
print('Max and min are', max(a_list), min(a_list))

a_tup = (30, 55, 15, 45)
print(sorted(a_tup))  # Print [15, 30, 45, 55]

>>> num_list = [2.45, 1, -10, 55.5, 100.03, 40, -3]
>>> print('The average is ', sum(num_list) / len(num_list))
The average is  26.56857142857143

list.append(value)      # Append a value
list.clear()            # Remove all contents
list.extend(iterable)   # Append a series of values
list.insert(index, value) # At index, insert value
list.remove(value)      # Remove first instance of
                        # value

a_list = [1, 2, 3]

a_list.append(4)
a_list.extend([4])      # This has the same effect.

a_list.extend([4, 5, 6]) # Adds 3 elements to the list.

a_list = [10, 20, 40]   # Missing 30.
a_list.insert(2, 30 )   # At index 2 (third), insert 30.
print(a_list)           # Prints [10, 20, 30, 40]
a_list.insert(100, 33)
print(a_list)           # Prints [10, 20, 30, 40, 33]
a_list.insert(-100, 44)
print(a_list)           # Prints [44, 10, 20, 30, 40, 33]

## 列表方法

my_list = [15, 25, 15, 25]
my_list.remove(25)
print(my_list)  # Prints [15, 15, 25]

def eval_scores(a_list):
    a_list.remove(max(a_list))
    a_list.remove(min(a_list))
    return sum(a_list) / len(a_list)

the_scores = [8.5, 6.0, 8.5, 8.7, 9.9, 9.0]
print(eval_scores(the_scores))

list.count(value)    # Get no. of
                     # instances.
list.index(value[, beg [, end]])  # Get index of value.
list.pop([index])    # Return and remove
                     # indexed item: use
                     # last by default.

yr_list = [1, 2, 1, 1,[3, 4]]
print(yr_list.count(1))   # Prints 3
print(yr_list.count(2))   # Prints 1
print(yr_list.count(3))   # Prints 0
print(yr_list.count([3, 4])) # Prints 1

beat_list = ['John', 'Paul', 'George', 'Ringo']
print(beat_list.index('Ringo'))  # Print 3.

beat_list = ['John', 'Paul', 'George', 'Ringo', 'Ringo']
list.sort([key=None] [, reverse=False])
list.reverse() # Reverse existing order.
def main():
    my_list = [] # Start with empty list
    while True:
        s = input('Enter next name: ')
        if len(s) == 0:
            break
        my_list.append(s)
        my_list.sort() # Place all elems in order.
        print('Here is the sorted list:')
        for a_word in my_list:
            print(a_word, end=' ')
main()

def ignore_case(s):
    return s.casefold()

a_list = [ 'john', 'paul', 'George', 'brian', 'Ringo' ]
b_list = a_list[:]
a_list.sort()
b_list.sort(key=ignore_case)

>>> a_list
['George', 'Ringo', 'brian', 'john', 'paul']
>>> b_list
['brian', 'George', 'john', 'paul', 'Ringo']

my_list = ['Brian', 'John', 'Paul', 'George', 'Ringo']
my_list.reverse()   # Reverse elems in place.
for a_word in my_list:
    print(a_word, end=' ')

## 栈与 RPN 计算器

the_stack = []

def push(v):
    the_stack.append(v)

def pop():
    return the_stack.pop()

def main():
    s = input('Enter RPN string: ')
    a_list = s.split()
    for item in a_list:
        if item in '+-*/':
            op2 = pop()
            op1 = pop()
            if item == '+':
                push(op1 + op2)
            elif item == '-':
                push(op1 - op2)
            elif item == '*':
                push(op1 * op2)
            else:
                push(op1 / op2)
        else:
            push(float(item))
    print(pop())

main()

op1 = the_stack.pop()
...
the_stack.append(op1 + op2) # Push op1 + op2.
push(eval(str(op1) + item + str(op2)))
**functools.reduce(function, list)**
import functools
def add_func(a, b):
    return a + b
def mul_func(a, b):
    return a * b
n = 5
a_list = list(range(1, n + 1))
triangle_num = functools.reduce(add_func, a_list)
fact_num   = functools.reduce(mul_func, a_list)
triangle_num = 1 + 2 + 3 + 4 + 5
fact_num   = 1 * 2 * 3 * 4 * 5
**lambda arguments: return_value**

sum1 = my_f(3, 7)
print(sum1)     # Print 10.
sum2 = my_f(10, 15)
print(sum2)     # Print 25.

t5 = functools.reduce(lambda x, y: x + y, [1,2,3,4,5])
f5 = functools.reduce(lambda x, y: x * y, [1,2,3,4,5])

b_list = [i  for i in a_list]
b_list = [i for i in a_list]
b_list = [i * i   for i in a_list]

mult_list = []
for i in range(3):
    for j in range(3):
        mult_list.append(i * j)

mult_list = [i * j for i in range(3) for j in range(3)]

[ value for_statement_header if_expression ]

my_list = [10, -10, -1, 12, -500, 13, 15, -3]
new_list = []
for i in my_list:
    if i > 0:
        new_list.append(i)
new_list = [i for i in my_list if i > 0]
new_list = [i for i in my_list if i > 0]
my_list = [1, 2, -10, -500, 33, 21, -1]
neg_list = [i for i in my_list if i < 0]
a_list = [5, 5, 5, -20, 2, -1, 2]
my_set = set()
for i in a_list:
    if i > 0:
        my_set.add(i)
my_set = {i for i in a_list if i > 0}
my_set = {i * i for i in a_list if i > 0}
vals_list = [('pi', 3.14), ('phi', 1.618)]
my_dict = {i[0]: i[1] for i in vals_list}
my_dict['pi'] # Produces 3.14.

