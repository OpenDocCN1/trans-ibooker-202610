```
plt.show()
```

![](bbox=[0.3, 0.13, 0.7, 0.33])

Notice that the title is larger and in italics. I set the title’s size and style via the fontsize and fontstyle keyword arguments for set_title.

Let’s update our initialization to remove spines:

```
def initialize_plot(x_label, y_label, title_label):
    plt.clf()                # clear the figure

    figure, axes = plt.subplots()   # set the plot layout

    axes.grid()                 # include a background grid

    axes.axhline(color="black")   # draw a horizontal axis in black
    if x_label:
        axes.set_xlabel(x_label)    # label the horizontal axis

    axes.axvline(color="black")   # draw a vertical axis in black
    if y_label:
        axes.set_ylabel(y_label)    # label the vertical axis

    if title_label:
        axes.set_title(title_label) # set the title

    for position in ("top", "right", "bottom", "left"):
        # turn off the extra lines around the plot
        axes.spines[position].set_visible(False)

    return figure, axes
```