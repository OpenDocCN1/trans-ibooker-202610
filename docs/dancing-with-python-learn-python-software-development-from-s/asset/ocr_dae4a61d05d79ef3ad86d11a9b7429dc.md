We use the **numpy** functions *max* and *min* to compute the maximum and minimum data values.

```
np.max(ages), np.min(ages)
```

(52, 29)

In section 13.3, we saw how to draw histograms using **matplotlib**. Here is a histogram of the age data, with the mean labeled:

```
import matplotlib.pyplot as plt
plt.hist(ages, bins=33, color="lightgray", edgecolor="black")

plt.xlabel("Age")
plt.ylabel("Count")
plt.title("Histogram of concert attendee ages")

mean = np.mean(ages)
plt.axvline(mean, color="black", linestyle="dotted")
plt.text(mean + 0.1, 11, "mean")
```

Text(40.72, 11, 'mean')
<Figure size 432x288 with 1 Axes>

```
plt.draw()
```

![](bbox=[0.32, 0.6, 0.69, 0.85])