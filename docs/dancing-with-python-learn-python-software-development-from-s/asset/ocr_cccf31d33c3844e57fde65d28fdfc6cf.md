```
plt.show()
```

![](bbox=[0.28, 0.14, 0.7, 0.38])

# 13.1.8 Plotting the sine function

The technique of applying a function to each element of an array works for many mathematical operations, though you should use them from the numpy module and not the math module. Here I draw the sine function from x = −2π to 2π:

```
import matplotlib.pyplot as plt
from mpl_helper_2 import initialize_plot
import numpy as np

figure, axes = initialize_plot('x', 'y', "y = sin(x)")

# set the aspect ratio of vertical to horizontal
axes.set_aspect('equal')

# --------------------------------------------------------------------------
# Generate 1000 equally spaced points between -2 pi and 2 pi
x_values = np.linspace(-2*np.pi, 2*np.pi, 1000)
# --------------------------------------------------------------------------

the_plot = plt.plot(x_values, np.sin(x_values))
```

<Figure size 432x288 with 0 Axes>
<Figure size 432x288 with 1 Axes>