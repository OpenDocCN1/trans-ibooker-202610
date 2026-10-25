# Here's the Python code that implements this program logic:

Click here to view code image

```
the__stack = []

def push(v):
    the__stack.append(v)

def pop():
    return the__stack.pop()

def main():
    s = input('Enter RPN string: ')
    a__list = s.split()
    for item in a__list:
        if item in '+-*/':
            op2 = pop()
            op1 = pop()
            if item == '+':
                push(op1 + op2)
            elif item == '-':
                push(op1 - op2)
            elif item == '*':
                push(op1 * op2)
            else:
                push(op1 / op2)
        else:
            push(float(item))
    print(pop())

main()
```

This application, although not long, could be more compact. We’ve included dedicated push and pop functions operating on a global variable, the__stack. A few lines could have been saved by using methods of the__stack directly.