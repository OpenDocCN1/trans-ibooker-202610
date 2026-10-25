example of the Pythagorean Theorem, as it is calculating the value of

Click here to view code image

```
square_root((3 * 3) + (4 * 4))
```

# ## 8.9.2 Reading RPN from a Text File

The next step is to get the program to open a text file and read RPN statements from that file. The “statements” can consist of a series of operators and numbers, like those shown in the previous example, but what should the program do after evaluating each one?

For now, let’s adopt a simple rule: If a line of text in the file to be read is blank, do nothing. But if there is any input on the line, then execute that line of RPN code and print the result, which should be available as `stack[-1]` (the “top” of the stack).

The new version of the program follows. Note that the bold lines represent new or strongly altered lines. Moreover, the `open_rpn_file` definition is new.

Click here to view code image

```
import re
import operator

stack = []   # Stack to hold the values.

# Scanner object. Isolate each token and take
# appropriate action: push a numeric value, but
```