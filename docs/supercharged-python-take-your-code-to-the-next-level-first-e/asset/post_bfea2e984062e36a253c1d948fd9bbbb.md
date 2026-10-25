

Y. But most often, you’ll want to include both X and Y arguments.

The action is to take pairs of points from X and Y, in which X and Y are the same length, and to plot them on a graph. Each value in X is matched to the corresponding value in Y to get an (x, y) point. All the points are then plotted and connected.

Here’s a simple example, using the **np.linspace** and **np.sin** functions.

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt

A = np.linspace(0, 2 * np.pi)
plt.plot(A, np.sin(A))
plt.show()
```

If you’ve downloaded both **numpy** and **matplotlib**, and if you enter this code as shown, your computer should display the window shown in Figure 13.1. The window remains visible until you close it.

![](img/6e45221a8147f2442c653a3d4a866570_795_0.png)

**Figure 13.1.** A sine graph

This is simple code, but let’s step through each part of it. The first thing the example does is import the needed packages.

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt
```

Next, the example calls the **numpy linspace** function. This function, remember, generates a set of values, including the two specified endpoints, to get a total of N evenly spaced values. By default, N is 50.

Click here to view code image

```
A = np.linspace(0, 2 * np.pi)
```

Therefore, array A contains floating-point numbers beginning with 0, ending in 2 * pi, and 48 other values evenly spaced between these two values.

The call to the **plot** function specifies two arrays: A, which contains 50 values along the X axis, and a second array, which contains the sine of each element in A.

```
plt.plot(A, np.sin(A))
```

The function looks at each element in A and matches it with a corresponding value in the second array, to get 50 (x, y) pairs. Finally, the **show** function tells the software to display the resulting graph onscreen.

We can, of course, just as easily graph the cosine function instead, by deleting the call to **np.sin** and replacing it with **np.cos**.

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt

A = np.linspace(0, 2 * np.pi)
plt.plot(A, np.cos(A))
plt.show()
```

In this version, each value in A is matched with its cosine value to create an (x, y) pair. Figure 13.2 shows the resulting graph.

![](img/6e45221a8147f2442c653a3d4a866570_797_0.png)

But you aren’t limited to trigonometric functions. The flexibility of **numpy** arrays—particularly how they can be operated on as a unit—makes the plotting power of **matplotlib** both simple and versatile.

For example, what if you wanted to plot a graph of the *reciprocal* function—that is, 1/N? If N is 5, its reciprocal is 1/5, and vice versa.

We start by creating a range of values for X. This is why the **np.linspace** function is so useful; it creates a source of values (as many as you want) from the desired domain. Often these values will be monotonically increasing along the X axis.

Starting with the value 0 would be a problem, because then 1/N would cause division by 0. Instead, let’s start with the value 0.1 and run to values as high as 10. By default, 50 values are generated.

```
A = np.linspace(0.1, 10)
```

Now it’s easy to plot and show the results by using A and 1/A to provide values for the (x, y) pairs. Each value in A gets matched with its reciprocal.

```
plt.plot(A, 1/A)
plt.show()
```

Figure 13.3 shows the results.

![](img/6e45221a8147f2442c653a3d4a866570_798_0.png)

The function creates points by combining values from A and 1/A. So, for example, the first (x, y) pair is

```
(0.1, 10.0)
```

The second point is formed in the same way, combining the next value in A with its reciprocal in the second set. Here are some points that could be plotted.

> Click here to view code image

```
(0.1, 10.0), (0.2, 5.0), (0.3, 3.3) ...
```

A less interesting, but illustrative, example is to plot a handful of points and connect them. Let’s specify five such values:

```
(0, 1)
(1, 2)
(2, 4)
(3, 5)
(4, 3)
```

The plot for this graph would be created by the following statement:

> Click here to view code image

```
plt.plot([0, 1, 2, 3, 4], [1, 2, 4, 5, 3])
```

If the X argument is omitted, the default is [0, 1, 2, ... N–1], where N is the length of the Y array. Therefore, this example could be written as

```
plt.plot([1, 2, 4, 5, 3])
```

In either case, calling the **show** function puts the graph on screen, as illustrated by Figure 13.4.

![](img/6e45221a8147f2442c653a3d4a866570_800_0.png)

**Figure 13.4.** A primitive graph of five points

Note that you don’t necessarily have to use ascending values. You can use any points to create arbitrary lines. Here’s an example:

Click here to view code image

```
plt.plot([3, 4, 1, 5, 2, 3], [4, 1, 3, 3, 1, 4])
```

The points to be plotted would be

Click here to view code image

```
(3, 4), (4, 1), (1, 3), (5, 3), (2, 1), (3, 4)
```

Those points, in turn, form a pentagram (more or less), as shown in Figure 13.5. All the points are plotted, and then line segments are drawn between one point and the next.

![](img/6e45221a8147f2442c653a3d4a866570_801_0.png)

**Figure 13.5.** Plot of a pentagram

The final example in this section shows that you can graph formulas as complex as you like. This is the beauty of being able to operate directly on **numpy** arrays. It’s easy to graph complex polynomials. Here’s an example:

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt
A = np.linspace(-15, 20)
plt.plot(A, A ** 3 - (15 * A ** 2) + 25)
plt.show()
```

These statements, when run, graph a polynomial as shown in Figure 13.6.

![](img/6e45221a8147f2442c653a3d4a866570_802_0.png)

**Figure 13.6.** Plotting a polynomial

## 13.4 PLOTTING MORE THAN ONE LINE

What if you’d like to show more complex graphs, such as a graph that contains both a sine function and a cosine function so you can compare them? This is easy. You can, for example, make two calls to the `plot` function before showing the results.

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt

A = np.linspace(0, 2 * np.pi)
plt.plot(A, np.sin(A))
plt.plot(A, np.cos(A))
plt.show()
```

Alternatively, two **plot** statements could have been combined into one by placing four arguments in a single statement.

Click here to view code image

```
plt.plot(A, np.sin(A), A, np.cos(A))
```

In either case, Python responds by displaying the graph shown in Figure 13.7.

![](img/6e45221a8147f2442c653a3d4a866570_804_0.png)

**Figure 13.7.** Sine and cosine plotted together

The `matplotlib` package automatically plots curves of two different colors: orange and blue. These show up nicely on a computer screen, but in a black-and-white printout, you may not be able to see the difference.

Fortunately, the plotting software provides other ways of differentiating curves through the formatting arguments, which we look at next.

The more complete syntax for the `plot` function is shown here.

![](img/6e45221a8147f2442c653a3d4a866570_805_0.png)

Click here to view code image

```
plt.plot( X1, Y1, [fmt1,] X2, Y2, [fmt2,] ... )
```

What this syntax indicates is that you can include any number of X, Y pairs of arrays. From each such pair, a set of (x, y) points is created by combining pairs of corresponding elements. You can then add optional format arguments.

The _fmt_ arguments are useful for differentiating lines by giving them contrasting colors and styles. For example, the following **plot** statement causes the plot for the cosine function to consist of small circles.

Click here to view code image

```
plt.plot(A, np.sin(A), A, np.cos(A), 'o')
plt.show()
```

The sine curve has no format specifier, so it takes on a default appearance. The resulting graph differentiates between the two curves nicely, as you see in Figure 13.8. By default, different colors are assigned to the two lines.

