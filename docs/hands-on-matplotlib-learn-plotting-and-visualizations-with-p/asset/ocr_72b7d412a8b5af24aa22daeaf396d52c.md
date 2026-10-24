The output of both the methods will be as follows:

```
[[1 0 0 0 0]
[0 1 0 0 0]
[0 0 1 0 0]
[0 0 0 1 0]
[0 0 0 0 1]]
```

The function np.ones() returns the matrix of the given size that has all the elements as 1s.

```
x = np.ones((2, 5, 5), dtype=np.int16)
print(x)
```

Run the code and you will see the following output:

```
[[[1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]]

 [[1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]
  [1 1 1 1 1]]]
```

The function np.zeros() returns a matrix of a given size with all the element as 0s.

```
x = np.zeros((2, 5, 5, 2), dtype=np.int16)
print(x)
```

Run the code and check the output.

The function np.full() returns a new array of a given shape and type, filled with the passed argument. Here’s an example:

```
x = np.full((3, 3, 3), dtype=np.int16, fill_value = 5)
print(x)
```