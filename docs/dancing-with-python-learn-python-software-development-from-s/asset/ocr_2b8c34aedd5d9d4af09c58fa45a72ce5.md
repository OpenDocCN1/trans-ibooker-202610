+   - 13.6 Moving to three dimensions ............................................................ 516
+   - 13.7 Summary ....................................................................................... 525

# 13.1 Function plots

This section introduces **matplotlib** and develops the techniques to create the plots in Figure 13.1.

![](bbox=[0.194, 0.286, 0.828, 0.544])

Figure 13.1: Plots of the sine and inverse sine functions

We use the **matplotlib.pyplot** module, which we always import as `plt`.

# 13.1.1 Plotting a point

In its most basic form, the *plot* function takes a point’s *x* coordinate, *y* coordinate, and some keyword arguments that state how to display the point.

```
import matplotlib.pyplot as plt

the_plot = plt.plot(1, 0.5, color="black", marker='o')
```

```
<Figure size 432x288 with 1 Axes>
```