```
>>> grid[1][2]
10
```

Working with columns is a little trickier than working with rows, because each column is spread over multiple lists. To access a column, we need to aggregate one value from each row. We can do that with a loop, which incrementally builds a new list representing a column. Here, I obtain column 1:

```
>>> column = []
>>> for i in range(len(grid)):
...     column.append(grid[i][1])
...
>>> column
[13, 12, 6, 8]
```

Notice how the first index (the row) varies, but the second (the column) does not ①. This picks out each value with the same column index.

What about summing rows and columns? To sum a row, we can use the sum function. Here’s the sum of row 0:

```
>>> sum(grid[0])
26
```

We can also use a loop, like this:

```
>>> total = 0
>>> for value in grid[0]:
...     total = total + value
...
>>> total
26
```

Using sum is the easier option, so we’ll use that.

To sum a column, we can build a column list and use sum on that, or we can calculate it directly without making a new list. Here’s the latter approach for column 1:

```
>>> total = 0
>>> for i in range(len(grid)):
...     total = total + grid[i][1]
...
```