+   **Exercise 3.8**
+   
+   Write a Python function to generate all possible questions for this book with negative, zero, and positive integer values of `start`, `end`, and `step`. Just kidding, but do experiment.

+   **Exercise 3.9**
+   
+   Devise formal rules for how slicing works with allowable values of `start`, `end`, and `step`.

# 3.2.7 Slicing: Changing sub-lists

Just as we saw that we could **change** and **remove** list items, we can use slice notation with assignment to alter a list’s structure.

The idea is that we create a slice expression to specify part of the list, and then use assignment to substitute another list for the slice.

```
the_list = ["a", "b", "c", "d", "e", "f"]
```

```
the_list[:1] = [1, 2, 3]
the_list
```

```
[1, 2, 3, 'b', 'c', 'd', 'e', 'f']
```

```
the_list[6:] = [7, 8, 9]
the_list
```

```
[1, 2, 3, 'b', 'c', 'd', 7, 8, 9]
```

```
the_list[4:6] = ["alpha", "beta", "gamma"]
the_list
```

```
[1, 2, 3, 'b', 'alpha', 'beta', 'gamma', 7, 8, 9]
```

+   **Exercise 3.10**
+   
+   While these examples work fine, Python requires the slice and the list to the right of the assignment to have the same length when you use a step other than one. Experiment with positive and negative steps to see what works and what doesn’t.