Figure 17-16 shows the output.

![](bbox=[0.28, 0.15, 0.78, 0.45])

Figure 17-16. A simple bivariate histogram with color

You can also customize the size of bins and add ticks on the x- and y-axes (known as a rug plot) as follows:

```
sns.displot(x='total_bill', y='tip',
    data=tips, rug=True,
    kind='hist', bins=30)
plt.show()
```