Figure 13-11 shows the output.

![](bbox=[0.3, 0.14, 0.7, 0.31])

# Figure 13-11. XKCD histogram

Another example is as follows:

```
y = np.random.randn(1000)
plt.xkcd()
plt.hist(y, bins = 30,
    range=[-3.5, 3.5],
    facecolor='r',
    alpha=0.6,
    edgecolor='k')
plt.grid()
plt.show()
```

Figure 13-12 shows the output.

![](bbox=[0.31, 0.62, 0.72, 0.785])

# Figure 13-12. Another XKCD histogram