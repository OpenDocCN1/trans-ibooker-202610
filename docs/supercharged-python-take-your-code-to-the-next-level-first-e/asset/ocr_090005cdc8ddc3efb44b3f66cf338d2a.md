```
`(0.1, 10.0)`
```

The second point is formed in the same way, combining the next value in A with its reciprocal in the second set. Here are some points that could be plotted.

> Click here to view code image

```
`(0.1, 10.0), (0.2, 5.0), (0.3, 3.3) ...`
```

A less interesting, but illustrative, example is to plot a handful of points and connect them. Let’s specify five such values:

```
`(0, 1)
(1, 2)
(2, 4)
(3, 5)
(4, 3)`
```

The plot for this graph would be created by the following statement:

> Click here to view code image

```
`plt.plot([0, 1, 2, 3, 4], [1, 2, 4, 5, 3])`
```

If the X argument is omitted, the default is [0, 1, 2, ... N–1], where N is the length of the Y array. Therefore, this example could be written as

```
`plt.plot([1, 2, 4, 5, 3])`
```