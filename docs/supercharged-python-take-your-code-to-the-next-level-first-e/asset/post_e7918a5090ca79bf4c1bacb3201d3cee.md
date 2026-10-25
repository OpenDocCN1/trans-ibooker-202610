
The first argument, _A_, refers to a **numpy** array and is the only required argument.

The _bins_ argument determines the number of subranges, or bins. The default setting is 10, which means that the function determines the difference between the highest and lowest values and divides by 10 to get the size of each subrange. But you can specify settings other than 10.

Click here to view code image

```
plt.hist(A, bins=50) # Place results into 50 bins.
```

Other keyword arguments accepted by this function include **color**, which takes a string containing one of the characters shown in Table 13.2; **align**, which takes one of the values 'left', 'right', or 'mid'; and **cumulative**, which is a Boolean that indicates cumulative results are to be graphed. For more information, use help.

```
>>> help(plt.hist)
```

Another use of histograms is to graph a normal distribution curve. We showed how to do this in Chapter 11, but the approach here gives even better results.

We start by using the **numpy** randomization package to generate 200,000 data points in a normal distribution. This distribution has a mean of 0.0 and a standard deviation of 1.0. But by adding and multiplying, we can convert to an array of values with a mean of 100 and a standard deviation of 10.

Click here to view code image

```
A = np.random.standard_normal(200000)
A = A * 10 + 100
```

In graphing these results, we can rely on the default setting of 10 bins, but results are more satisfying if graphed into even smaller subranges. So we specify 80 bins. Let's also set the color to green while we're at it.

Click here to view code image

```
plt.hist(A, bins=80, color='g')
plt.title('Normal Distribution in 80 Bins')
plt.show()
```

The result is an appealing-looking normal-distribution graph, shown in Figure 13.12. If you can see it in green (as it is displayed on a computer screen), then it looks a little bit like a Christmas tree. Happy holidays!

![](img/6e45221a8147f2442c653a3d4a866570_819_0.png)

**Figure 13.12.** Histogram of a normal distribution

You might wonder: Can you present this data as a completely smooth line rather than as a series of bars?

You can. **numpy** provides a **histogram** function that enables you to generate frequency numbers for a series of subranges (bins). The general syntax, shown here, displays the two most important arguments.

![](img/6e45221a8147f2442c653a3d4a866570_819_1.png)

Click here to view code image

```
plt.histogram(A [, bins=10] [, keyword_args] )
```

The action of the function is to produce a new array; this array contains the result of the histogram. Each element in this new array is a frequency number corresponding to one of the bins. By default, the number of bins is 10. So for the resulting array:

- The first element contains the number of values from A that occur in the first bin (the first subrange).
- The second element contains the number of values from A that occur in the second bin (the second subrange).
- And so on.

The value returned is actually a tuple. The first element of this tuple contains the frequency numbers we want to plot. The second element contains the exact edges of the bin. So, to get the data we need, use the following syntax:

![](img/6e45221a8147f2442c653a3d4a866570_820_0.png)

```
plt.histogram(A, bins) [0]
```

We can now generate a smooth normal-distribution curve. Generate a large set of random numbers, and place the results in 50 bins. (You can pick another number of bins, but using between 50 and 100 tends to produce good results.) Finally, plot the frequency numbers of those bins. This example employs 2 million trials, but there still should be almost no noticeable delay, which is amazing.

Click here to view code image

```
import numpy as np
import matplotlib.pyplot as plt

A = np.random.standard_normal(2000000)
A = A * 10 + 100
B = np.histogram(A, 50)[0]
plt.plot(B)
plt.show()
```

This code specifies no argument for the “X-axis” array; instead, it’s handled by default. The plotting software uses 0, 1, 2 ... N–1 for the X coordinates, where N is the length of the B array, which contains the result of the `.histogram` function. Figure 13.13 shows the results produced by this example.

![](img/6e45221a8147f2442c653a3d4a866570_822_0.png)

Figure 13.13. Smooth drawing of a normal distribution curve

The resulting figure is a smooth, pleasing curve. The X axis, however, may not be what you expect. The numbers along the X axis show the bin numbers.

The range consists of the lowest random number generated to the highest; this range is then divided into 50 parts. The Y axis shows the frequency of hits in each bin.

But instead of placing the bin numbers along the X axis, it’s more useful to display values from the distribution itself. The simplest *correct* way to do that is to use “X-axis” values that represent the median points of each bin. The next example does that by using the second array returned by `np.histogram`.

That array contains the edges of the bins—that is, the lowest value of the subrange represented by each bin.

That may sound complicated, but it’s only a matter of adding a couple of lines of code. In this case, X represents the edges of the bins—and then X is modified to contain the median point of those bins. In this way, the frequencies get plotted against values in the distribution (centered at 100) rather than bin numbers.

Click here to view code image

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

Click here to view code image

```
X = (X[1:] + X[:-1]) / 2   # Use bin centers rather than
                          # edges
```

If you look at the revised plot of the histogram (Figure 13.14), you can see it plots values centered at 100 with a standard deviation of 10. A standard deviation of 10 means that roughly 95 percent of the area of the curve should fall within two deviations (80 to 120), and more than 99 percent of the area should fall within three standard deviations (70 to 130).

![](img/6e45221a8147f2442c653a3d4a866570_824_0.png)

**Figure 13.14.** Normal distribution curve centered at 100

The `histogram` function has other uses. For example, you can use it to replace some of the code in Chapter 11, demonstrating the Law of Large Numbers. The example in that section collected data in a series of bins. The `histogram` function does the same thing as the code in that section but does it many times as fast.

> Performance Tip
> You should observe through this chapter and Chapter 12 that many numpy functions echo actions that can be performed in Chapter 11, “The Random and Math Packages,” by importing random and math. But the numpy versions, especially with large data sets (such as the 2,000,000 random numbers generated for the most recent examples) will be many times as fast. So when you have a choice, prefer to use numpy, including its random sub-package, for large numeric operations.

Some of the other arguments are occasionally useful. To learn about all of them, you can get help from within IDLE.

```
>>> np.info(np.histogram)
```

## 13.7 CIRCLES AND THE ASPECT RATIO

Sometimes you’ll want to adjust the relative size of the X and Y axes; this is especially true when you draw a geometric shape. In this section, we’ll show how to draw a circle. Usually, you don’t want it to look like an ellipse, so we’ll adjust the aspect ratio between X and Y to be equal before showing the graph.

There’s more than one way to draw a circle; the other way is left as an exercise for the end of the chapter. In this section, we’ll use an approach utilizing trig functions. Imagine a little bug traveling around the outside of a circle, starting at o
