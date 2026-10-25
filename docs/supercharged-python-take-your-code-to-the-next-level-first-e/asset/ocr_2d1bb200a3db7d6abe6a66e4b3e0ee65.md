fifth line (`temp f2 =`). This forms a loop that continues until n is zero.

As we promised, the jump-if-not-zero operator, `?`, is easy to implement. Just add one line to the Scanner code and one short function. Here is the revised Scanner code, with the new line to be entered in bold.

Click here to view code image

```
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
    (r"[?]", **lambda s, t: jnz_op()**)
])

```

The new function, `jnz_op`, pops two items off the stack, looks them up in the symbol table if necessary, and carries out the operation itself, which is simple.

Click here to view code image

```
def jnz_op():
    global pc
    op2, op1 = stack.pop(), stack.pop()
    if type(op1) == str:

```