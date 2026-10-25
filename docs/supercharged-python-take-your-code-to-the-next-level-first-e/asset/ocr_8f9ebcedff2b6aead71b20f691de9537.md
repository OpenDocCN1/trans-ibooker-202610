In another language, such an action would require three
separate statements. But Python does not require this, because
—as just shown—it can do the swap all at once. Here is what
another language would require you to do:

```
temp = x
x = y
y = temp
```

# ## 4.2.6 Use Advanced Tuple Assignment

Tuple assignment has some refined features. For example, you
can unpack a tuple to assign to multiple variables, as in the
following example.

Click here to view code image

```
tup = 10, 20, 30
a, b, c = tup
print(a, b, c) # Produces 10, 20, 30
```

It’s important that the number of input variables on the left
matches the size of the tuple on the right. The following
statement would produce a runtime error.

Click here to view code image

```
tup = 10, 20, 30
a, b = tup # Error: too many values to unpack
```

Another technique that’s occasionally useful is creating a