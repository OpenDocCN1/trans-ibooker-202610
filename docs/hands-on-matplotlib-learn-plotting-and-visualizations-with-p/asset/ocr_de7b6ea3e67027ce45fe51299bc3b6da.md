Figure 4-2 shows the output.

![](bbox=[0.26, 0.135, 0.74, 0.335])

Figure 4-2. A simple single-line graph with arange()

Let’s visualize the quadratic graph y = f(x) = x³+1. The code is as follows:

```
plt.plot(x, [(y**3 + 1) for y in x])
plt.show()
```

Figure 4-3 shows the output.

![](bbox=[0.26, 0.515, 0.74, 0.715])

Figure 4-3. y = f(x) = x³+1