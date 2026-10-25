![](bbox=[0.217, 0.089, 0.811, 0.454])

**Figure 13.1.** A sine graph

This is simple code, but let’s step through each part of it. The first thing the example does is import the needed packages.

Click here to view code image

```
```
import numpy as np
import matplotlib.pyplot as plt
```
```

Next, the example calls the **numpy linspace** function. This function, remember, generates a set of values, including the two specified endpoints, to get a total of N evenly spaced values. By default, N is 50.

Click here to view code image

```
```
A = np.linspace(0, 2 * np.pi)
```
```