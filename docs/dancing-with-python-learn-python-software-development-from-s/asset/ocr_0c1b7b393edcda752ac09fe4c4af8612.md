+   14.7 Cats by gender in each locality ......................................................... 563
+   14.8 Are all tortoiseshell cats female? ......................................................... 569
+   14.9 Cats in trees and circles ......................................................... 571
+   14.10 Summary ......................................................... 574

# 14.1 Statistics

This section introduces several core statistical functions for analyzing data. The data we use is the ages in 2021 of one hundred attendees at a fictional concert of 1990s music cover bands.

```
import numpy as np

ages = np.array(
    [40, 41, 39, 35, 42, 37, 45, 43, 42, 38, 39, 45, 37, 36,
     45, 41, 41, 31, 42, 40, 39, 38, 40, 39, 41, 46, 42, 44,
     46, 48, 45, 39, 46, 43, 35, 38, 43, 41, 36, 40, 34, 44,
     42, 44, 40, 49, 47, 51, 52, 45, 44, 47, 39, 38, 43, 39,
     45, 40, 36, 43, 38, 43, 32, 35, 36, 42, 40, 38, 37, 36,
     41, 41, 31, 39, 51, 38, 42, 36, 35, 36, 40, 40, 37, 43,
     39, 42, 44, 50, 39, 38, 37, 33, 52, 35, 44, 29, 42, 39,
     40, 42]
)
```

# 14.1.1 Means and medians

The mean is the average of the data values. We often write the mean as the Greek letter mu = μ, though you may also see “m.”

```
mean = np.mean(ages)
mean
```

40.62

You can also compute the mean directly by dividing the sum of the values by the number of values.

```
np.sum(ages)/ages.size
```

40.62