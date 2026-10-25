```
f2 f1 f2 + =
f1 temp =
PRINTVAR f2
n n 1 - =
n 0 > 4 ?
```

The last line now says the following: If `n` is greater than 0,
then jump to (zero-based) line 4. This improves the script,
because if the user enters a negative number, the RPN program
doesn’t go into an infinite loop.

Finally— although this is not necessary for most scripts— let’s
add an operation that gets a random integer in a specified
range.

![](bbox=[0.149, 0.433, 0.255, 0.514])

```
op1 op2 !
```

The action of this RPN expression is to call
**`random.randint`**, passing `op1` and `op2` as the _begin_ and _end_
arguments, respectively. The random integer produced in this
range is then pushed on the stack.

Adding support for this expression is also easy. However, it
involves importing another package. The code will be easy to
write if we can refer to it directly. Therefore, let’s import it this
way:

```
from random import randint
```