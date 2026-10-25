Python doesn’t need to resolve all symbols in the definition until
the function is called. (This enables mutual self-references as
long as both functions are defined before either is called.)

```
```python
def function_name(args):
    statements
```
```

In this syntax, args is a list of zero or more arguments,
separated by commas if there are more than one:

```
```python
[arg1 [,arg2]...]
```
```

# For example:

> Click here to view code image

```
```python
def hypotenuse(side1, side2):
    total = sidel * sidel + side2 * side2
    return total ** 0.5   # Return square root of
total.
```
```

Once a function is defined, it may be executed (called) at any
time, but parentheses are always necessary in a function call,
whether or not there are arguments.

> Click here to view code image

```
```python
def floopy():
    return 100

print(floopy())       # Call floopy, no args! Print
100
print(hypotenuse(3, 5))
```
```