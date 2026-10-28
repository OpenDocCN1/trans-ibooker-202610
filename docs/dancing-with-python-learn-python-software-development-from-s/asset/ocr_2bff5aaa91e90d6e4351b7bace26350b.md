# 14.1.2 Box and whisker plots

A *box plot*, also called a *whisker plot*, shows all these descriptive statistics. We can draw one with **matplotlib**:

```
plt.boxplot(ages)
plt.draw()
```

![](bbox=[0.31, 0.26, 0.68, 0.48])

The box in the middle ranges vertically from *Q1* to *Q3*, with the median marked by the horizontal line within. **matplotlib** shows the minimum and maximum points at the bottom and top.

Below the maximum is something that looks like a “T,” and above the minimum is an inverted “T.” These are the “whiskers.” The top whisker is at Q3 + 1.5 * IQR, rounded down if necessary, and the lower is at Q1 - 1.5 * IQR, rounded up if necessary. Beyond the whiskers are *outliers*, data values that you should inspect for correctness.

This plot is very plain, so let’s embellish it to see what the plot is describing. The following code adds lines and labels to the box plot to make it easier to understand:

```
import matplotlib.pyplot as plt
import numpy as np
import math

ages = np.array(
    [40, 41, 39, 35, 42, 37, 45, 43, 42, 38, 39, 45, 37, 36,
     45, 41, 41, 31, 42, 40, 39, 38, 40, 39, 41, 46, 42, 44,
```