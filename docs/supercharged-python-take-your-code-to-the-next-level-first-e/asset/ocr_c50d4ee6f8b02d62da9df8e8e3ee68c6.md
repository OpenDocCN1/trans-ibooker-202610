# Here’s the procedure.

+   • Multiply each item in the first row of A by each corresponding item
+   in the first column of B. Get the sum (10). This becomes C[0,0].

+   • Multiply each item in the first row of A by each corresponding item
+   in the second column of B. Get the sum (13). This becomes C[0,1].

+   • Multiply each item in the second row of A by each corresponding item
+   in the first column of B. Get the sum (28). This becomes
+   C[1,0].

+   • Multiply each item in the second row of A by each corresponding item
+   in the second column of B. Get the sum (40). This becomes
+   C[1,1].

You can also take the dot product of a one-dimensional array
combined with a two-dimensional array. The result is that the
array shapes are evaluated as if they had the following shapes:

```
`(1, X) * (X, Y) => (1, Y)`
```

For example, you could take the dot product of [10, 15,
30] and the following array, which we’ll call B:

```
`[[0 1]
[2 3]
[4 5]]`
```

This next statement shows the dot product between a one-
dimensional array and the two-dimensional array, B. The
resulting dot product has a shape of (1, 2) :

Click here to view code image