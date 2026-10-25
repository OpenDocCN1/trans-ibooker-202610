# Figure 12.3. How “axis=0” and “axis=1” work

Let’s take another example; this one is easier to see how its effects work. Let’s start with an array in which each element is equal to its column number.

Click here to view code image

```
```
B = np.fromfunction(lambda r,c: c, (4, 5),
                    dtype=np.int32)
```
```

# Printing this array produces

```
```
[[0 1 2 3 4]
 [0 1 2 3 4]
 [0 1 2 3 4]
 [0 1 2 3 4]]
```
```

Summing along axis 0 (producing column totals) should give us a multiple of 4 each time. Summing along axis 1 (producing row totals) should give us 10 each time.
And, in fact, that’s what we get.

Click here to view code image

```
```
>>> np.sum(B, axis = 0)      # row, totaling cols.
array([ 0,  4,  8, 12, 16])
>>> np.sum(B, axis = 1)      # col, totaling rows.
array([10, 10, 10, 10])
```
```

This is admittedly confusing because axis=0, which should refer to rows, actually sums all the dimensions except rows (in this case, columns). And axis=1 actually sums all the