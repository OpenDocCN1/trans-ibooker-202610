

RPN syntax assigns a value; as in Python itself, the symbol is created if it does not yet exist. Otherwise, its value is replaced by the new value.

![](img/6e45221a8147f2442c653a3d4a866570_495_0.png)

```
`symbol expression =`
```

Here are some examples:

```
`x 35.5 =
x 2 2 + =
my_val 4 2.5 * 2 + =
x my_val +`
```

The effect of these statements should be to place the value 35.5 in `x`, then place 4 (which is 2 + 2) into `x`, and then place the amount 12 into `my_val`. Finally, the effect would be to place the expression `x my_val +` on the top of the stack, which should cause the value 16 to be printed.

Thanks to Python’s dictionary capabilities, it’s easy to add a symbol to the table. For example, you can place the symbol `x` in the table with a value of 35.5.

```
`sym_tab['x'] = 35.5`
```

We can incorporate this action into the Scanner object, along with other operations.

```
scanner = re.Scanner([
    (r"[ \t\n]", lambda s, t: None),
    (r"[+-]*(/\d*.)?/\d+", lambda s, t:
        stack.append(float(t))),
    (r"\d+", lambda s, t: stack.append(int(t))),
    (r"[a-zA-Z_][a-zA-Z_0-9]*", lambda s, t:
        stack.append(t)),
    (r"[+]", lambda s, t: bin_op(operator.add)),
    (r"[-]", lambda s, t: bin_op(operator.sub)),
    (r"[*]", lambda s, t: bin_op(operator.mul)),
    (r"[/]", lambda s, t: bin_op(operator.truediv)),
    (r"[^]", lambda s, t: bin_op(operator.pow)),
    (r"[=]", lambda s, t: assign_op()),
])
```

In this new version of `scanner`, the following regular expression says, “Look for a pattern that starts with a lowercase letter, uppercase letter, or underscore (_) and then contains zero or more instances of one of those characters or a digit character.”

That item is added to the stack, as a string. Note that Python lists may freely intermix strings and numbers. When such a symbol is added, it will be stored as just that: a string. As such it may be the target of an assignment. The `assign_op` function is defined as follows:

```
def assign_op():
    op2, op1 = stack.pop(), stack.pop()
    if type(op2) == str:  # Source may be another
    var!
    op2 = sym_tab[op2]

sym_tab[op1] = op2
```

Although `op1` refers to a variable name (that is, a variable in the RPN language), `op2` may refer to either a variable name or a numeric value. So, as with the next block of code, `op2` must be looked up in the symbol table, `sym_tab`, if it’s a string.

> **Note**
>
> In the previous example, if `op1` does not refer to a variable name, then it represents a syntax error.

In the case of other binary actions—addition, multiplication, etc.—each operand may be either a symbolic name (stored in a string) or a numeric value. Therefore, with the `bin_op` function, it’s necessary to check the type of each operand and look up the value if it’s a string.

Click here to view code image

```
def bin_op(action):
    op2, op1 = stack.pop(), stack.pop()
    if type(op1) == str:
        op1 = sym_tab[op1]
    if type(op2) == str:
        op2 = sym_tab[op2]
    stack.append(action(op1, op2))
```

We can now create the fully revised application. However, this raises a design issue. Should the program evaluate and print the result of every single line of RPN?

Probably it should not, because some RPN lines will do nothing more than assign values, and such an action will not place any value on top of the stack. Therefore, this version of the program does not print any result except the final one. Other than input and error messages, this version of the application waits until the end of execution to print anything.

Click here to view code image

```
import re
import operator

# Provide a symbol table; values of variables will
#   stored here.
sym_tab = { }
stack = []    # Stack to hold the values.

# Scanner: Add items to recognize variable names,
# which
#   are stored in the symbol table, and to perform
#   assignments, which enter values into the sym.
#   table.

scanner = re.Scanner([
    (r"[ \t\n]", lambda s, t: None),
    (r"[+-]*(?:\d\*.)?\d+", lambda s, t:
        stack.append(float(t))),
    (r"[a-zA-Z_][a-zA-Z_0-9]*", lambda s, t:
        stack.append(t)),
    (r"\d+", lambda s, t: stack.append(int(t))),
    (r"[+]", lambda s, t: bin_op(operator.add)),
    (r"[-]", lambda s, t: bin_op(operator.sub)),
    (r"[*]", lambda s, t: bin_op(operator.mul)),
    (r"[/]", lambda s, t: bin_op(operator.truediv)),
    (r"[^]", lambda s, t: bin_op(operator.pow)),
    (r"[=]", lambda s, t: assign_op()),
])
```

```
])def assign_op():
    '''Assignment Operator function: Pop off a name
    and a value, and make a symbol-table entry.
    Remember
    to look up op2 in the symbol table if it is a
    string.
    '''
    op2, op1 = stack.pop(), stack.pop()
    if type(op2) == str:   # Source may be another
    var!
        op2 = sym_tab[op2]
    sym_tab[op1] = op2
def bin_op(action):
    '''Binary Operation evaluator: If an operand is
    a variable name, look it up in the symbol table
    and replace with the corresponding value, before
    being evaluated.
    '''
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

print('Unrecognized input:',
unknown)
print(str(stack[-1]))
def open_rpn_file():
    """Open-source-file function. Open a named
    file and read lines into a list, which is
    returned.
    """
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
main()
```

Here’s a sample session. Assume that the file `rpn2.txt` contains the following file:

Click here to view code image

```
side1 30 =
side2 40 =
sum side1 side1 * side2 side2 *+ =
sum 0.5 ^
```

The effect of these RPN statements, if correctly executed, is to apply the Pythagorean Theorem to the inputs 30 and 40, which ought to output 50.0. If this is the content of `rpn2.txt`, then

the following session demonstrates how it is evaluated.

```
Enter RPN source: rpn2.txt
50.0
```

There are some limitations of this program. Not every kind of error is properly reported here. Also, if the last statement is an assignment, ideally the program should report the value assigned, but it does not.

We’ll solve that problem in Chapter 14, by adding INPUT and PRINT statements to the RPN grammar.

Before leaving this topic, let’s review how this Python program works. First, it sets up a Scanner object, the use of which was explained in Chapter 7, “Regular Expressions II.” This object looks for individual items, or tokens, and takes action depending on what kind of token it is.

+   • If a numerical expression is found, it’s converted to a true number
+   and placed on the stack.

+   • If it’s a symbolic name—that is, a variable—it’s put on the stack as a
+   string; later, as the result of an assignment, it can be added to the
+   symbol table.

+   • If it’s an operator, the two most recent operands are popped off the
+   stack and evaluated, and the result is placed back on the stack. An
+   exception is assignment (=), which doesn’t place anything on the
+   stack (although arguably, maybe it should).

And there’s a new twist: If a variable name is popped off the stack, it’s looked up in the symbol table, and the operand is replaced by the variable’s value before being used as part of an operation.

> **Note**
>
> If you look through the code in this application, you may notice that the symbol-look-up code is repetitive and could be replaced by a function call. The function would have to be written in a sufficiently general way that it would accommodate any operand, but that shouldn’t be hard. This approach would only save a line here and there, but it’s a reasonable use of _code refactoring_, which gathers similar operations and replaces them with a common function call. For example, right now the code uses
>
> ```
> if type(op1) == str:
>     op1 = sym_tab[op1]
> ```
>
> This could be replaced by a common function call, as follows:
>
> ```
> op1 = symbol_look_up(op1)
> ```
>
> Of course, you would need to define the function.

## 8.10 DIRECT BINARY READ/WRITE

For the rest of the chapter, we move onto the new horizon of reading and writing binary files.

When you open a file in binary mode, you can, if you choose, read and write bytes of data directly into the file. These operations deal with strings of type **bytes**.

Low-level binary read/write operations in Python use some of the same methods that are used with text-file operations, but with **bytes** data.

