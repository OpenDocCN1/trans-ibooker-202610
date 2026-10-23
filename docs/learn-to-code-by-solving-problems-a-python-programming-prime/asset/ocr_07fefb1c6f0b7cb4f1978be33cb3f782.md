```
>>> x[0] = 99
>>> x
[99, 2, 3, 4, 5]
>>> y
[1, 2, 3, 4, 5]
```

Observe this time that the y list didn’t change when the x list changed. They’re separate lists.

# CONCEPT CHECK

What is the output of the following code?

```
lst = ['abc','def','ghi']
lst[1] = 'wxyz'

print(len(lst))
```

+   A. 3
+   B. 9
+   C. 10
+   D. 4
+   E. This code produces an error

Answer: A. Changing a list value is allowed (because lists are mutable). But changing the value at index 1 to a longer string doesn’t change the fact that the list has three values.

# Learning About Methods

Like strings, lists have many useful methods. I’ll show you some of them in the next section, but first I’d like to show you how you can learn about methods on your own.