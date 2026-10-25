```
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
```