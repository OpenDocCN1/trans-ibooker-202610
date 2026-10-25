"a", "b" "c", or "d" in the next position.

```
[abcd]
```

Likewise, the following expression says that either a space or
a minus sign (-) can be matched—which is what we want in this
case:

```
[ -]
```

In this context, the square brackets are the only special
characters; the two characters inside are literal and at most one
of them will be matched. The minus sign often has a special
meaning within square brackets, but not when it appears in the
very front or end of the characters inside the brackets.
Here’s the full regular expression we need:

Click here to view code image

```
pattern = r'\d\d\d[ -]\d\d\d[ -]\d\d\d\d$'
```

Now, putting everything together with the refined pattern
we’ve come up with in this section, here’s the complete
example:

Click here to view code image

```
import re
pattern = r'\d\d\d[ -]\d\d\d[ -]\d\d\d\d$'
s = input('Enter tel. number: ')
if re.match(pattern, s):
```