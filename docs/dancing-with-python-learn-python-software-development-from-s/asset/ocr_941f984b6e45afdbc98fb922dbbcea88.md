We saw tuples when we did multiple assignments. We did not need parentheses then, but we could have included them if we wished.

```
a, b = 3, -2
print(f"{a = }    {b = }")

a, b = (9, 0.3)
print(f"{a = }    {b = }")

(a, b) = ("a", "b")
print(f"{a = }    {b = }")
```

a = 3    b = -2
a = 9    b = 0.3
a = 'a'  b = 'b'

# Exercise 3.18

What exception does Python raise if you try to assign a value to an item in a tuple via an index?

Use *tuple* to turn a list into a tuple and *list* to turn a tuple into a list.

```
t = 2, -4, 9
t
list(t)
tuple([-16, 25, -36])
```

(-16, 25, -36)

# Exercise 3.19

What are three ways to make an empty tuple?

The syntax gets confusing if you try to create a tuple with a single element.

```
(3.14)
```

3.14