You can see that the indices are represented on the x-axis, and magnitudes are marked on the y-axis. This is an unstacked vertical bar graph. You can create a stacked variation of it by just passing a simple argument as follows:

```
plt.figure()
df2.plot.bar(stacked=True)
plt.show()
```

Figure 16-4 shows the output.

![](bbox=[0.25, 0.31, 0.72, 0.51])

Figure 16-4. Visualizing vertically stacked bar graphs

You can even create horizontal stacked and unstacked bar graphs too. Let’s create a horizontally stacked bar graph with the routine barh() as follows:

```
plt.figure()
df2.plot.barh(stacked=True)
plt.show()
```