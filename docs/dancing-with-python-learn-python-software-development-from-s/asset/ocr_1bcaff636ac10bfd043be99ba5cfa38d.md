```
b
```

10

That assigned the same value to the variables a and b. We can also assign multiple values to multiple variables.

```
m, n = 17, 6
```

# Exercise 2.1

What are the values of m and n after you run the following?

```
m, n = 17, 6
m, n = n, m
```

This is simultaneous assignment.

# Exercise 2.2

Does the following work the same as the last example? Why or why not?

```
m, n = 17, 6
m = n
n = m
```

This is sequential assignment.

# 2.5 True and False

In section 1.1, we saw how you could treat the bits 0 and 1 like the Booleans false and true. In Python, we represent these as False and True.

The not operator inverts them.

```
not True
```

False

```
not False
```

True