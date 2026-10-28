
## 14 Analyzing Data

> *It is a capital mistake to theorize before one has data.*
> 
> —Arthur Conan Doyle, Sherlock Holmes

While we can use fancy names like “data science,” “analytics,” and “artificial intelligence” to talk about working with data, sometimes you just want to read, write, and process files containing many rows and columns of information. People have been doing this interactively for years, typically using applications like Microsoft Excel® and online apps like Google Sheets™.

To “programmatically” manipulate data, I mean that we use Python functions and methods. This chapter uses the popular pandas library to create and manipulate these collections of rows and columns, called DataFrames. We will later introduce other methods in Chapter 15, Learning, Briefly. Before we discuss DataFrames, let’s review some core ideas from statistics.

**Topics covered in this chapter**

- 14.1 Statistics 528
- 14.2 Cats and commas 538
- 14.3 pandas DataFrames 539
- 14.4 Data cleaning 555
- 14.5 Statistics with pandas 558
- 14.6 Converting categorical data 561
- 14.7 Cats by gender in each locality 563
- 14.8 Are all tortoiseshell cats female? 569
- 14.9 Cats in trees and circles 571
- 14.10 Summary 574

### 14.1 Statistics

This section introduces several core statistical functions for analyzing data. The data we use is the ages in 2021 of one hundred attendees at a fictional concert of 1990s music cover bands.

```python
import numpy as np

ages = np.array(
    [40, 41, 39, 35, 42, 37, 45, 43, 42, 38, 39, 45, 37, 36,
     45, 41, 41, 31, 42, 40, 39, 38, 40, 39, 41, 46, 42, 44,
     46, 48, 45, 39, 46, 43, 35, 38, 43, 41, 36, 40, 34, 44,
     42, 44, 40, 49, 47, 51, 52, 45, 44, 47, 39, 38, 43, 39,
     45, 40, 36, 43, 38, 43, 32, 35, 36, 42, 40, 38, 37, 36,
     41, 41, 31, 39, 51, 38, 42, 36, 35, 36, 40, 40, 37, 43,
     39, 42, 44, 50, 39, 38, 37, 33, 52, 35, 44, 29, 42, 39,
     40, 42]
)
```

#### 14.1.1 Means and medians

The mean is the average of the data values. We often write the mean as the Greek letter mu = μ, though you may also see “m.”

```python
mean = np.mean(ages)
mean
```

```text
40.62
```

You can also compute the mean directly by dividing the sum of the values by the number of values.

```python
np.sum(ages)/ages.size
```

```text
40.62
```

We use the **numpy** functions *max* and *min* to compute the maximum and minimum data values.

```python
np.max(ages), np.min(ages)
```

```text
(52, 29)
```

In section 13.3, we saw how to draw histograms using **matplotlib**. Here is a histogram of the age data, with the mean labeled:

```python
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

```python
plt.draw()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_557_0.png)

The value where we split the data in the middle, with half above and half below, is called the *median*.

```python
np.median(ages)
```

```text
40.0
```

#### Exercise 14.1

How do you calculate the median if the number of data values is even? Odd?

![](img/4ffeafa3e3f1373f02971e43b0755ea6_558_0.png)

The first quartile, commonly called *Q1*, is the median of the data values less than the median. Approximately 25% of the smallest data values are in the first quartile. We compute it using the *quantile* function.

```python
Q1 = np.quantile(ages, 0.25)
```

The third quartile, commonly called *Q3*, is the median of the data values greater than the median. Approximately 25% of the largest data values are in the third quartile.

```python
Q3 = np.quantile(ages, 0.75)
Q1, Q3
```

```text
(38.0, 43.0)
```

#### Exercise 14.2

What is `np.quantile(ages, 0.1)`? What does it represent? What about `np.quantile(ages, 2.0/3.0)`?

![](img/4ffeafa3e3f1373f02971e43b0755ea6_558_1.png)

The interquartile range, commonly called *IQR*, is the difference between *Q3* and *Q1*.

```python
IQR = np.quantile(ages, 0.75) - np.quantile(ages, 0.25)
```

Data is often associated with units like meters, feet, hours, nanoseconds, gallons, and liters. The unit for each value in ages is “years.” Therefore, the unit for mean, median, and standard deviation is also years.

#### 14.1.2 Box and whisker plots

A *box plot*, also called a *whisker plot*, shows all these descriptive statistics. We can draw one with **matplotlib**:

```python
plt.boxplot(ages)
plt.draw()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_559_0.png)

The box in the middle ranges vertically from *Q1* to *Q3*, with the median marked by the horizontal line within. **matplotlib** shows the minimum and maximum points at the bottom and top.

Below the maximum is something that looks like a “T,” and above the minimum is an inverted “T.” These are the “whiskers.” The top whisker is at Q3 + 1.5 * IQR, rounded down if necessary, and the lower is at Q1 - 1.5 * IQR, rounded up if necessary. Beyond the whiskers are *outliers*, data values that you should inspect for correctness.

This plot is very plain, so let’s embellish it to see what the plot is describing. The following code adds lines and labels to the box plot to make it easier to understand:

```python
import matplotlib.pyplot as plt
import numpy as np
import math

ages = np.array(
    [40, 41, 39, 35, 42, 37, 45, 43, 42, 38, 39, 45, 37, 36,
     45, 41, 41, 31, 42, 40, 39, 38, 40, 39, 41, 46, 42, 44,
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
```

Text(0.55, 50.2, 'Q3 + 1.5 * IQR = 50')
<Figure size 432x288 with 1 Axes>

```python
plt.draw()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_561_0.png)

#### 14.1.3 Standard deviation and variance

Let’s look at two datasets of 99 numbers. The first contains each integer from 1 to 99. The second contains 99 copies of the integer 50. In each case, the mean is 50.

```python
data_1 = np.arange(1, 100)
data_2 = np.repeat(50, 99)
(np.mean(data_1), np.mean(data_2))
```

```text
(50.0, 50.0)
```

In the first case, the values are well spread out from the mean. In the second, every value is centered at the mean. We use the *standard deviation*, written as the Greek letter sigma = σ, to measure this spread. The **numpy function** *std* computes the standard deviation of a 1-dimensional array of values.

```python
(np.std(data_1), np.std(data_2))
```

```text
(28.577380332470412, 0.0)
```
