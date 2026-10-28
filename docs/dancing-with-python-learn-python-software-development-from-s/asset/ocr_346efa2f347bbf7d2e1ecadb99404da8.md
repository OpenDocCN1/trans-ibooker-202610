```
46, 48, 45, 39, 46, 43, 35, 38, 43, 41, 36, 40, 34, 44,
42, 44, 40, 49, 47, 51, 52, 45, 44, 47, 39, 38, 43, 39,
45, 40, 36, 43, 38, 43, 32, 35, 36, 42, 40, 38, 37, 36,
41, 41, 31, 39, 51, 38, 42, 36, 35, 36, 40, 40, 37, 43,
39, 42, 44, 50, 39, 38, 37, 33, 52, 35, 44, 29, 42, 39,
40, 42]
)

plt.title("Box plot of concert attendee ages")
plt.ylabel("Age in years")

# medianprops is a dict with the
# drawing options for the median line
plt.boxplot(ages, medianprops={"color": "black", "linewidth": 2})

quantiles = np.quantile(ages, [0.0, 0.25, 0.5, 0.75, 1.0])
labels = ["minimum", "Q1 = first quartile",
          "mean", "Q3 = third quartile", "maximum"]

for (q, label) in zip(quantiles, labels):
    plt.axhline(q, color="gray", linewidth=0.5)
    plt.text(0.55, q + 0.2, f"{label} = {q}", fontsize=9)

IQR = quantiles[3] - quantiles[1]

# Draw the lower whisker extreme. Below this are outliers.
q = math.ceil(quantiles[1] - 1.5 * IQR)
plt.axhline(q, color="gray", linewidth=0.5)
plt.text(0.55, q + 0.2, f"Q1 - 1.5 * IQR = {q}", fontsize=9)

# Draw the upper whisker extreme. Above this are outliers.
q = math.floor(quantiles[3] + 1.5 * IQR)
plt.axhline(q, color="gray", linewidth=0.5)
plt.text(0.55, q + 0.2, f"Q3 + 1.5 * IQR = {q}", fontsize=9)

Text(0.55, 50.2, 'Q3 + 1.5 * IQR = 50')
<Figure size 432x288 with 1 Axes>
```