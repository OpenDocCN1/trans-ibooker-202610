Now we want to increase the value so that dollars refers to 251. This won’t work:

```
>>> dollars + 1
251
```

The result is 251, but that value is gone, not stored anywhere:

```
>>> dollars
250
```

What we need is an assignment statement that captures the result of dollars + 1:

```
>>> dollars = dollars + 1
>>> dollars
251
>>> dollars = dollars + 1
>>> dollars
252
```

It’s common for learners to think of the assignment symbol = as equality. But don’t do that! The assignment statement is a command to make a variable refer to the value of an expression, not a claim that two entities are equal.

# CONCEPT CHECK

What is the value of y after the execution of the following code?

```
>>> x = 37
>>> y = x + 2
>>> x = 20
```

+   A. 39
+   B. 22
+   C. 35
+   D. 20
+   E. 18