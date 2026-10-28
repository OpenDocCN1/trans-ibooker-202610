```
y_values = [x**2 - 3 for x in x_values]
# -----------------------------------------------------------------------------

the_plot = axes.plot(x_values, y_values)
```

<Figure size 432x288 with 0 Axes>
<Figure size 432x288 with 1 Axes>

Figure 13.2 shows the plot **matplotlib** generates when we call plt.show().

![](bbox=[0.28, 0.26, 0.7, 0.5])

Figure 13.2: Plot of the parabola y = x² - 3

To get better formatting in the title, I used LaTeX markup between the dollar signs for the polynomial equation. The LaTeX \mathregular expression told **matplotlib** to use the same font as the surrounding text.

To smooth out this graph, we need to calculate many more points. We can use lists, but it is much more efficient to use 1-dimensional NumPy arrays.

# 13.1.7 NumPy arrays

How big is a Python object if we measure it in bytes? Let’s look at the size of an empty string.

```
from sys import getsizeof

getsizeof("")
```