```
>>> sns.scatterplot(data=africa, x="Literacy", y="IMR")
>>> plt.axline( (x_bar,y_bar), slope = m, color="black")
>> plt.title("IMR vs Literacy: Line of best fit")
```

Figure 2.11. Literacy rates and infant mortality, with regression line

![](bbox=[0.17, 0.24, 0.76, 0.68])

For comparison, here are the data, the regression line, and the SD line in one scatterplot, with their intersection at the point of averages clearly marked:

```
sns.scatterplot(data=africa, x="Literacy", y="IMR")
plt.axline( (x_bar,y_bar), slope = m, color="black")
plt.axline( (x_bar, y_bar), slope = m_sd, linestyle='--', color="
plt.title("IMR vs Literacy: Regression line (solid) and SD line (
```