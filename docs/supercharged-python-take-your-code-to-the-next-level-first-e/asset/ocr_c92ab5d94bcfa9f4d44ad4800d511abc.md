That array contains the edges of the bins—that is, the lowest value of the subrange represented by each bin.

That may sound complicated, but it’s only a matter of adding a couple of lines of code. In this case, X represents the edges of the bins—and then X is modified to contain the median point of those bins. In this way, the frequencies get plotted against values in the distribution (centered at 100) rather than bin numbers.

# Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt

A = np.random.standard_normal(2000000)
A = A * 10 + 100
B, X = np.histogram(A, 50)
X = (X[1:] + X[:-1]) / 2   # Use bin centers rather than
                          # edges.
plt.plot(X, B)             # Plot against values rather
                           # than
plt.show()                 #   bin numbers.
```

The X values are calculated by getting the median value of each subrange— by taking the bottom and top edges (which are one off in position) and averaging them. The expression X[1:] shifts one position, because it starts with the second element. The expression X[:-1] excludes the last element to make the lengths equal.

# Click here to view code image

```
X = (X[1:] + X[:-1]) / 2   # Use bin centers rather than
                          # edges
```