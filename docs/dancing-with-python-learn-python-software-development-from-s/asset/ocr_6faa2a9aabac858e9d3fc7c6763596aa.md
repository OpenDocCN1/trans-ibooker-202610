Now we wrap this code in a function and allow arguments.

```
```python
def least_first(items):
    for index in range(1, len(items)):
        if items[0] > items[index]:
            items[0], items[index] = items[index], items[0]
    return items

least_first([3, 2, 1, 4])
```
```

```
```text
[1, 3, 2, 4]
```
```

```
```python
least_first([5, -1, 8, -3, 0])
```
```

```
```text
[-3, 5, 8, -1, 0]
```
```

This technique worked well, but what if the list has only one item or is empty? Will we get an error?

```
```python
least_first([10])
```
```

```
```text
[10]
```
```

```
```python
least_first([])
```
```

```
```text
[]
```
```

These *edge cases* worked because of the way *range* works and because we did not access `items[0]` for the empty list.

The variables `items` and `index` are both *local to* `least_first`. I have not defined them in the Python environment, and they are not known there.

![](bbox=[0.186, 0.655, 0.813, 0.798])

Figure 2.10: Local variables in the “least_first” function

```
```text
items
```
```

```
```text
NameError: name 'items' is not defined
```
```