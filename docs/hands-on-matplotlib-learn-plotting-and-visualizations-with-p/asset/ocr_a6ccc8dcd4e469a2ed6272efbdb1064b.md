Figure 17-11 shows the output.

![](bbox=[0.22, 0.15, 0.72, 0.46])

Figure 17-11. Histogram combined with KDE

Now let’s use some real-life data, as follows:

```
tips = sns.load_dataset("tips")
sns.displot(x='total_bill', data=tips, kind='hist')
plt.show()
```