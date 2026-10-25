```
global pc
op2, op1 = stack.pop(), stack.pop()
if type(op1) == str:
    op1 = sym_tab[op1]
if type(op2) == str:
    op2 = sym_tab[op2]
if op1:
    pc = int(op2) - 1  # Convert op to int format.

def assign_op():
    """Assignment Operator function.
    Pop off a name and a value, and make a symbol-table
    entry.
    """
    op2, op1 = stack.pop(), stack.pop()
    if type(op2) == str:  # Source may be another var!
        op2 = sym_tab[op2]
    sym_tab[op1] = op2

def bin_op(action):
    """Binary Operation function.
    If an operand is a variable name, look it up in
    the symbol table and replace with the corresponding
    value, before being evaluated.
    """
    op2, op1 = stack.pop(), stack.pop()
    if type(op1) == str:
        op1 = sym_tab[op1]
    if type(op2) == str:
        op2 = sym_tab[op2]
    stack.append(action(op1, op2))

def main():
    """Main function.
    This is the function that drives the
    program. After opening the file and getting operations
    into a_list, process strings in a_list one at a time.
    """
    global pc
    dir('__main__')
    a_list = open_rpn_file()
```