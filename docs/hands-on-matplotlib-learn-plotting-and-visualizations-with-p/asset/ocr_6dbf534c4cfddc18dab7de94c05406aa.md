Figure 18-10 shows the output.

![](bbox=[0.24, 0.135, 0.71, 0.38])

# Figure 18-10. Multiline graph with Seaborn

You will now see how to create a multiple-bar graph with Matplotlib as follows:

```
df2 = pd.DataFrame([y1, y2, y3, y4])
df2.plot.bar()
plt.legend(x, loc='best')
plt.xticks(rotation=45)
plt.show()
```