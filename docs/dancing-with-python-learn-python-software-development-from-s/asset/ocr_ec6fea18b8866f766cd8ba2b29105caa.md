+   9. **Don’t** include spaces around the equal sign in a keyword or default argument.
+   
+   10. In arithmetic expressions, **don’t** put a space between a negative sign and a number, variable, or expression. **Don’t** put spaces around “**” for exponentiation. **Do** put spaces around “+”, “-”, and “*” for addition, subtraction, and multiplication. However, you may leave out the spaces when multiplying a variable by an integer or when grouping terms closely makes your expression easier to read. For example, 2*x - x*y. Use your judgment, but be consistent.

# 6.8 Nested functions

You can define a function inside another function. We say that the inner function is *nested* inside the outer.

```
```python
def f(x, y):
    w = 10
    def g(z):
        return z**3 + w
    return g(x + y)
```
```

```
```python
f(2, 3)
```

135
```

The nested function g on line 3 can see and use w outside its definition but within *f*.

You cannot call a nested function by name outside its enclosing function.

```
```python
g(3)
```

`NameError: name 'g' is not defined`
```

To avoid confusion, do not use a parameter name for a nested function that you also use for the containing function. You cannot return from a nested function all the way out of the function in which you defined it. You may code this behavior using an exception via try and except.

You may define more than one nested function, and a nested function may contain additional function definitions within it.