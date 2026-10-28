```
figure = plt.figure()
axes = plt.axes(projection='3d')

# draw the scatter plot
the_plot = axes.scatter(xs, ys, zs, color="black")
```

<Figure size 432x288 with 1 Axes>

```
plt.show()
```

![](bbox=[0.27, 0.32, 0.68, 0.58])

Consider using a grid or different markers or colors to make the plot easier to interpret.

# Exercise 13.19

By calling scatter twice, draw this scatter plot so that points with z values less than 20 use a '+' marker and otherwise use an '*' marker.

# Exercise 13.20

Draw this scatter plot so that points with x + y > 10 are shown in red and otherwise in green.