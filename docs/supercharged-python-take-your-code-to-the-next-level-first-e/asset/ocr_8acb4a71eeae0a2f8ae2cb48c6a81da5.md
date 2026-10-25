```
with open('file.txt', 'w') as f:
    f.write('To be or not to be\n')
    f.write('That is the question.\n')
    f.write('Whether tis nobler in the mind\n')
    f.write('To suffer the slings and arrows\n')

with open('file.txt', 'r') as f:
    print(f.read())
```

This example writes out a series of strings as separate lines and then prints the contents directly, including the newlines.

> Click here to view code image

![](bbox=[0.194, 0.406, 0.844, 0.519])

Reading this same file with either **readline** or **readlines** —each of which recognizes newlines as separators—likewise reads in the newlines at the end of each string. Here’s an example that reads in one line at a time and prints it.

> Click here to view code image

```
with open('file.txt', 'r') as f:
    s = ' ' # Set to a blank space initially
    while s:
        s = f.readline()
        print(s)
```

The **readline** method returns the next line in the file, in which a “line” is defined as the text up to and including the next