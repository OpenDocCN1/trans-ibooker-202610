# Exercise 3.15

**Optional:** If you know the Lisp or Scheme programming languages, how would you define *car* or *cdr*? Do one version of each that uses list unpacking and another that uses slicing.

I think list unpacking is an underused feature in Python.

# 3.2.10 Reversing and sorting lists

Let’s create a list of years when our guitar models were introduced.

```
years = [1954, 1950, 1952, 1958, 1987]
years
```

[1954, 1950, 1952, 1958, 1987]

The *reverse* method flips the order of items in the **list**. It changes the list itself; it doesn’t make a copy.

```
years.reverse()
years
```

[1987, 1958, 1952, 1950, 1954]

By default, *sort* puts the list elements in ascending order.

```
years.sort()
years
```

[1950, 1952, 1954, 1958, 1987]

With an optional keyword second parameter, you can sort them in descending order.

```
years.sort(reverse=True)
years
```

[1987, 1958, 1954, 1952, 1950]

Though I’ve shown methods and operations that physically change the **list** and its contained items, some similar functions leave the original intact.