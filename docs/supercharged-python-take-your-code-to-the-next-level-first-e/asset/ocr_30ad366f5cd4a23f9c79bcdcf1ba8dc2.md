```
array_name[ i1, i2, i3,... iN ]
```

In this syntax, each of the arguments _i_1 through _iN_ may be either a scalar value—which must be an index in range—or a slice. You can use at most N such arguments, where N is the number of dimensions (the rank) of the array. For each scalar used, the dimensionality of the result is reduced by one.

Therefore, slicing a two-dimensional array as `A[2, 1:4]` produces a one-dimensional array as a result. Slicing it as `A[2:3, 1:4]` would get the same elements but would be two-dimensional, even though it would have only one row that was not empty. (This issue matters, because most operations on arrays must match in size and number of dimensions.)

Any of the _i_ values in this syntax may be omitted; in each such case, its value is assumed to be the colon (`:`), which says, “Select all the elements in this dimension.” If there are fewer than N arguments, then the first M dimensions (where M is the number of arguments) get the values assigned, and the last N–M dimensions assume the colon as their default.

Table 12.5 lists a series of examples. In this table, A is a two-dimensional array and A3D is a three-dimensional array.

# Table 12.5. Examples of “numpy” Indexing and Slicing

| Example | Description |
|---------|-------------|