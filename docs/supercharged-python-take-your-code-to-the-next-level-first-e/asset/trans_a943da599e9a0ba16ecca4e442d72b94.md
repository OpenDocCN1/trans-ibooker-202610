

## 正则表达式扫描器与逆波兰表达式计算器

list = re.split(pattern, string, maxsplit=0, flags=0)

import re
lst = re.split(pat, '3, 5 7 8,10, 11')

['3', '5', '7', '8', '10', '11']

['3', '2', '*', '2', '15', '*', '+', '4', '+']

scanner = re.Scanner (
    (r'[a-zA-Z]\w*', sc_ident),
    (r'[*+/-]',
     sc_oper),
    (r'\d+\.\d*',
     sc_float),
    (r'\d+',
     sc_int),
    (r'\s+',
     None)
)

function_name(scanner, tok_str)

import re

def sc_oper(scanner, tok): return
def sc_int(scanner, tok): return
def sc_float(scanner, tok): return
scanner = re.Scanner (
    (r'[*+/-]', sc_oper),
    (r'\d+\.\d*', sc_float),
    (r'\d+', sc_int),
    (r'\s+', None)
])

print(scanner.scan('32 6.67+ 10 5- *'))
([32, 6.67, '+', 10, 5, '-', '*'], '')
scanner = re.Scanner ([  
    (r'[*/+-]',   lambda s, t: bin_op(t)),
    (r'\d+\.\d*', lambda s, t: the_stk.append(float(t))),
    (r'\d+',       lambda s, t: the_stk.append(int(t))),
    (r'\s+',       None)
])
def bin_op(tok):
    op2, op1 = the_stk.pop(), the_stk.pop()
    if tok == '+':
        the_stk.append(op1 + op2)
    elif tok == '*':
        the_stk.append(op1 * op2)
    elif tok == '/':
        the_stk.append(op1 / op2)
    elif tok == '-':
        the_stk.append(op1 - op2)

## File scanner_rpn.py

import re

the_stk = []

scanner = re.Scanner ([
    (r'[*/-]',  lambda s, t: bin_op(t)),
    (r'\d+\.\d*', lambda s, t: the_stk.append(float(t))),
    (r'\d+',     lambda s, t: the_stk.append(int(t))),
    (r'\s+',     None)
])

def bin_op(tok):
    op2, op1 = the_stk.pop(), the_stk.pop()
    if tok == '+':
        the_stk.append(op1 + op2)
    elif tok == '*':
        the_stk.append(op1 * op2)
    elif tok == '/':
        the_stk.append(op1 / op2)
    elif tok == '-':
        the_stk.append(op1 - op2)

def main():
    input_str = input('Enter RPN string: ')
    tokens, unknown = scanner.scan(input_str)
    if unknown:
        print('Unrecognized input:', unknown)
    else:
        print('Answer is', the_stk.pop())

main()

## File scanner_rpn2.py

import re
import operator
the_stk = []
scanner = re.Scanner([
    (r'[+]',    lambda s, t: bin_op(operator.add)),
    (r'[*]',    lambda s, t: bin_op(operator.mul)),
    (r'[-]',    lambda s, t: bin_op(operator.sub)),
    (r'[/]',    lambda s, t: bin_op(operator.truediv)),
    (r'\d+\.\d*', lambda s, t: the_stk.append(float(t))),
    (r'\d+',    lambda s, t: the_stk.append(int(t))),
    (r'\s+',    None)
])
def bin_op(oper):
    op2, op1 = the_stk.pop(), the_stk.pop()
    the_stk.append(oper(op1, op2))
def main():
    input_str = input('Enter RPN string: ')
    tokens, unknown = scanner.scan(input_str)
    if unknown:
        print('Unrecognized input:', unknown)
    else:
        print('Answer is', the_stk.pop())
main()

## 文件操作示例

>>> import os
>>> os.getcwd()
'/Users/brianoverland'
>>> os.chdir('Documents')
>>> os.path.isfile('pythag.py')
True
>>> os.remove('pythag.py')
>>> os.path.isfile('pythag.py')
False

try:
    fname = input('Enter file to read:')
    f = open(fname, 'r')
    print(f.read())
except FileNotFoundError:
    print('File', fname, 'not found. Terminating.')

while True:
    try:
        fname = input('Enter file name: ')
        if not fname:     # Quit on empty string.
            break
        f = open(fname)   # Attempt file open here.
        print(f.read())
        f.close()
        break
    except FileNotFoundError:
        print('File could not be found. Re-enter.')

while True:
    fname = input('Enter file name: ')
    if not fname:
        break
    try:
        f = open(fname)   # Attempt file open here.
    except FileNotFoundError:
        print('File could not be found. Re-enter.')
    else:
        print(f.read())
        f.close()
        break

with open(filename, mode_str) as file_obj:
    statements

with open('stuff.txt', 'r') as f:
    lst = f.readlines()
    for thing in lst:
        print(thing, end='')

file.write(str)
file.writelines(str | list_of_str)

with open('file.txt', 'w') as f:
    f.write('To be or not to be\n')
    f.write('That is the question.\n')
    f.write('Whether tis nobler in the mind\n')
    f.write('To suffer the slings and arrows\n')

with open('file.txt', 'r') as f:
    print(f.read())

To be or not to be
That is the question.
Whether tis nobler in the mind
To suffer the slings and arrows

with open('file.txt', 'r') as f:
    s = ' ' # Set to a blank space initially
    while s:
        s = f.readline()
        print(s)

To be or not to be
That is the question.
Whether tis nobler in the mind
To suffer the slings and arrows

with open('file.txt', 'r') as f:
    s = ' '    # Set to a blank space, initially
    while s:
        s = f.readline()
        s = s.rstrip('\n')
        print(s)

with open('file.txt', 'r') as f:
    str_list = f.readlines()
    for s in str_list:
        print(s, end='')

file_obj.seek(0, 0)    # Go back to beginning of the file.

file_obj.seek(rec_size * rec_num, 0)

file_pointer = file_obj.tell()

## 完整的逆波兰表达式计算器

import re
import operator

stack = []   # Stack to hold the values.

# Scanner object. Isolate each token and take
# appropriate action: push a numeric value, but perform
# operation on top two elements on stack if an operator
# is found.
scanner = re.Scanner([
    (r"[ \t\n]", lambda s, t: None),
    (r"-?(\d*\.)?\d+", lambda s, t:
        stack.append(float(t))),
    (r"\d+", lambda s, t: stack.append(int(t))),
    (r"[+]", lambda s, t: bin_op(operator.add)),
    (r"[-]", lambda s, t: bin_op(operator.sub)),
    (r"[*]", lambda s, t: bin_op(operator.mul)),
    (r"[/]", lambda s, t: bin_op(operator.truediv)),
    (r"[^]", lambda s, t: bin_op(operator.pow)),
])

# Binary Operator function. Pop top two elements from
# stack and push the result back on the stack.

def bin_op(action):
    op2, op1 = stack.pop(), stack.pop()
    stack.append(action(op1, op2))

def main():
    while True:
        input_str = input('Enter RPN line: ')
        if not input_str:
            break

        try:
            tokens, unknown = scanner.scan(input_str)
            if unknown:
                print('Unrecognized input:', unknown)
            else:
                print(str(stack[-1]))
        except IndexError:
            print('Stack underflow.')

main()

Enter RPN line: **25 4 ***
100.0
Enter RPN line: **25 4 * 50.75-**
49.25
Enter RPN line: **3 3* 4 4* + .5^**
5.0
Enter RPN line:
square_root((3 * 3) + (4 * 4))

import re
import operator

stack = []   # Stack to hold the values.

# Scanner object. Isolate each token and take
# appropriate action: push a numeric value, but perform
# operation on top two elements on stack if an operator
# is found.

scanner = re.Scanner([
    (r"[ \t\n]", lambda s, t: None),
    (r"-?(\d*\.)?\d+", lambda s, t:
        stack.append(float(t))),
    (r"\d+", lambda s, t: stack.append(int(t))),
    (r"[+]", lambda s, t: bin_op(operator.add)),
    (r"[-]", lambda s, t: bin_op(operator.sub)),
    (r"[*]", lambda s, t: bin_op(operator.mul)),
    (r"[/]", lambda s, t: bin_op(operator.truediv)),
    (r"[^]", lambda s, t: bin_op(operator.pow)),
])

# Binary Operator function. Pop top two elements from
# stack and push the result back on the stack.

def bin_op(action):
    op2, op1 = stack.pop(), stack.pop()
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
            else:
                print(str(stack[-1]))

def open_rpn_file():
    """Open-source-file function. Open a named
    file and read lines into a list, which is
    returned.
    """
    while True:
        try:
            fname = input('Enter RPN source: ')
            f = open(fname, 'r')
            if not f:
                return None
            else:
                break
        except:
            print('File not found. Re-enter.')
    a_list = f.readlines()
    return a_list

main()

