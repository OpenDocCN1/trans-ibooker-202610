the array present between the two bounds defining the slice, with possible application
of an incrementation step, as shown in example 6.16.

# EXAMPLE 6.16.–

```
```python
>>> t = [0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24]
>>> t[2:10]
[4, 6, 8, 10, 12, 14, 16, 18]
```
```

The constructed slice begins with the element of index 2 and ends at index 10
excluded. Let us continue this example.

# EXAMPLE 6.17.–

```
```python
>>> t[2:10:3]
[4, 10, 16]
```
```

With an incrementation step of 3, the slice still begins with the element of index 2
and runs to index 10 excluded, but it jumps two elements from the initial array each
time (i.e. the index is incremented by 3 each time).

The general syntax of a slice is array[start:end:increment]: any of the three
components may be absent, positive, or negative, and in the latter case, the array is
processed in reverse order. The first slice from the array t in example 6.18 fetches all
elements of the array in reverse order, starting with the fifth from the end and working
back to the first element of the array. In other words, it fetches all elements in reverse
order, with the exception of the final four values in the initial array. The second slice
only takes one element out of two.

# EXAMPLE 6.18.–

```
```python
>>> t[5::-1]
[16, 14, 12, 10, 8, 6, 4, 2, 0]
>>> t[5::-2]
[16, 12, 8, 4, 0]
```
```

This flexibility in expressing arrays comes at a cost, as the meaning of these
syntactic variants of array operations are more involved. Purely for entertainment
purposes, readers may wish to determine the result of t[-6:2:-3], t[6:-2:-1] or
t[6:-2:1].