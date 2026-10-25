```
stack.append(float(t)),
(r"-?\\d+", lambda s, t: stack.append(int(t))),
(r"[a-zA-Z_][a-zA-Z_0-9]*", lambda s, t:
stack.append(t)),
(r"[+]", lambda s, t: bin_op(operator.add)),
(r"[-]", lambda s, t: bin_op(operator.sub)),
(r"[*]", lambda s, t: bin_op(operator.mul)),
(r"[/]", lambda s, t: bin_op(operator.truediv)),
(r"[>]", lambda s, t: bin_op(operator.gt)),
(r"[\\^]", lambda s, t: bin_op(operator.pow)),
(r"[=]", lambda s, t: assign_op()),
(r"[?]", lambda s, t: jnz_op())
])
```

That’s it! It may occur to you that adding new operators, as
long as they are standard arithmetic or comparison operators, is
so trivial, we should add them all.

That would be correct, except that if you’re depending on
single punctuation marks to represent different operations,
you’ll soon run out of symbols on the keyboard. The problem is
potentially solvable by making “LE”, for example, stand for “less
than or equal to,” but if you use that approach, you need to
rethink how the scanner analyzes tokens.

Armed with this one additional operator, it’s now possible to
make the Fibonacci script more reliable. Just look at the revised
script.

Click here to view code image

```
PRINTS 'Enter number of fibos to print: '
INPUT n
f1 0 =
f2 1 =
temp f2 =
```