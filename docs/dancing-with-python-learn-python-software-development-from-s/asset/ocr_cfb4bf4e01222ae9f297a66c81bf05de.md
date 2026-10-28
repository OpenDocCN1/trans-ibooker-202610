```
axes.set_xlabel('x', fontsize="16")
axes.set_ylabel('y', fontsize="16")
axes.set_zlabel('z', fontsize="16")

# set the limits of the values shown on the axes
axes.set_xlim(0, 1)
axes.set_ylim(0, 1)
axes.set_zlim(0, 1)

# label the origin
axes.text(0.1, 0, 0.03, "(0, 0, 0)")

# draw the axes
axes.plot([0, 0], [0, 0], [0, 1], linewidth=2, color="black")
axes.plot([0, 0], [0, 1], [0, 0], linewidth=2, color="black")
axes.plot([0, 1], [0, 0], [0, 0], linewidth=2, color="black")

# draw the point at the origin
the_plot = axes.plot([0, 0], [0, 0], [0, 0], marker='o',
                    markersize=6, color="black")
```

<Figure size 432x288 with 1 Axes>

```
plt.show()
```

![](bbox=[0.25, 0.62, 0.78, 0.88])