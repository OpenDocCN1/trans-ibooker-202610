newline or end of file. It returns an empty string only if the end- of-file condition (EOF) has already been reached. But the **print** function automatically prints an extra newline unless you use the **end** argument to suppress that behavior. The output in this case is

![](bbox=[0.1816, 0.2445, 0.4141, 0.2602])

```
```
To be or not to be.

That is the question.

Whether tis nobler in the mind

To suffer the slings and arrows
```
```

A **print** function argument of `end=' '` would avoid printing the extra newline. Alternatively, you can strip the newlines from the strings that are read, as follows.

![](bbox=[0.1816, 0.568, 0.4141, 0.5836])

```
```
with open('file.txt', 'r') as f:
    s = ' '    # Set to a blank space, initially
    while s:
        s = f.readline()
        s = s.rstrip('\n')
        print(s)
```
```

This is starting to look complicated, despite what should be a simple operation. A simpler solution may be to use the **readlines** method (note the use of the plural) to read the entire file into a list, which can then be read as such. This