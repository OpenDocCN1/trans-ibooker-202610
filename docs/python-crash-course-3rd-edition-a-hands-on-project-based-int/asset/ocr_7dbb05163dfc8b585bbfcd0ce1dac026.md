![](bbox=[0.126, 0.092, 0.873, 0.511])

Figure 15-11: A walk with 50,000 points

# ## Altering the Size to Fill the Screen

A visualization is much more effective at communicating patterns in data if it fits nicely on the screen. To make the plotting window better fit your screen, you can adjust the size of Matplotlib’s output. This is done in the `subplots()` call:

```
fig, ax = plt.subplots(figsize=(15, 9))
```

When creating a plot, you can pass `subplots()` a `figsize` argument, which sets the size of the figure. The `figsize` parameter takes a tuple that tells Matplotlib the dimensions of the plotting window in inches.

Matplotlib assumes your screen resolution is 100 pixels per inch; if this code doesn’t give you an accurate plot size,