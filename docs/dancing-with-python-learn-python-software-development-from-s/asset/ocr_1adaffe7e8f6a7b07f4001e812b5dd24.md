Figure 5.4 shows the graphs of the sine *sin* and inverse sine *asin* functions.

![](bbox=[0.17, 0.13, 0.83, 0.38])

Figure 5.4: Plots of the sine and inverse sine functions

There are three essential things to note:

+   1. *sin* can take any real number represented by a **float** as its argument, but only produces values between −1.0 and 1.0 inclusive. Said another way, the *domain* of *sin* is the set of real numbers, and its *range* is the set of real numbers *y* such that −1 ≤ *y* ≤ 1.
+   2. The sine function is periodic: given a real number *x*, *sin*(*x*) = *sin*(*x* + 2*k*π) for any integer *k*. That’s why *sin* repeats itself as you move from one side of the graph to the other. We say that the sine is periodic with period 2π.
+   3. The inverse function *asin* has the domain −1 ≤ *x* ≤ 1 for any real number *x*, but we restrict the range to −π/2 ≤ *y* ≤ π/2.

```
math.asin(-1.0)
```

-1.5707963267948966

```
math.asin(-1.5)
```

ValueError: math domain error

# Exercise 5.18

The *cos* function also has a period of 2π. What is its domain and range? What are the domain and range for *acos*?