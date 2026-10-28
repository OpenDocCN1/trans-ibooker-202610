

## 14 分析数据

> *在没有数据之前就进行理论化是一个致命的错误。*
> 
> —亚瑟·柯南·道尔，《夏洛克·福尔摩斯》

虽然我们可以用“数据科学”、“分析”和“人工智能”等花哨的名字来谈论数据工作，但有时你只是想读取、写入和处理包含许多行和列信息的文档。人们多年来一直以交互方式进行此操作，通常使用 Microsoft Excel® 和 Google Sheets™ 等在线应用等应用程序。

“以编程方式”操作数据，我的意思是我们使用 Python 函数和方法。本章使用流行的 pandas 库来创建和操作这些行列集合，称为 DataFrames。我们将在第 15 章《学习，简要》中介绍其他方法。在讨论 DataFrames 之前，让我们回顾一下统计学的一些核心概念。

**本章涵盖的主题**

- 14.1 统计学 528
- 14.2 猫和逗号 538
- 14.3 pandas DataFrames 539
- 14.4 数据清洗 555
- 14.5 使用 pandas 进行统计 558
- 14.6 转换分类数据 561
- 14.7 各地猫的性别 563
- 14.8 所有虎斑猫都是雌性吗？ 569
- 14.9 树上和圆圈里的猫 571
- 14.10 总结 574

### 14.1 统计学

本节介绍用于分析数据的几个核心统计函数。我们使用的数据是 2021 年在一场虚构的 1990 年代音乐翻唱乐队音乐会上的一百名与会者的年龄。

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

#### 14.1.1 均值和中位数

均值是数据值的平均值。我们通常将均值写为希腊字母 mu = μ，尽管你也可能看到“m。”

```python
mean = np.mean(ages)
mean
```

```text
40.62
```

你也可以通过将值的总和除以值的数量来直接计算均值。

```python
np.sum(ages)/ages.size
```

```text
40.62
```

我们使用 **numpy** 函数 *max* 和 *min* 来计算最大和最小数据值。

```python
np.max(ages), np.min(ages)
```

```text
(52, 29)
```

在第 13.3 节中，我们看到了如何使用 **matplotlib** 绘制直方图。以下是年龄数据的直方图，并标出了均值：

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

将数据在中间分割，使一半在上方一半在下方的值称为 *中位数*。

```python
np.median(ages)
```

```text
40.0
```

#### 练习 14.1

如果数据值的数量是偶数，奇数时如何计算中位数？

![](img/4ffeafa3e3f1373f02971e43b0755ea6_558_0.png)

第一四分位数，通常称为 *Q1*，是小于中位数的数据值的中位数。大约 25% 的最小数据值在第一四分位数中。我们使用 *quantile* 函数计算它。

```python
Q1 = np.quantile(ages, 0.25)
```

第三四分位数，通常称为 *Q3*，是大于中位数的数据值的中位数。大约 25% 的最大数据值在第三四分位数中。

```python
Q3 = np.quantile(ages, 0.75)
Q1, Q3
```

```text
(38.0, 43.0)
```

#### 练习 14.2

`np.quantile(ages, 0.1)` 是什么？它代表什么？`np.quantile(ages, 2.0/3.0)` 呢？

![](img/4ffeafa3e3f1373f02971e43b0755ea6_558_1.png)

四分位距，通常称为 *IQR*，是 *Q3* 与 *Q1* 的差值。

```python
IQR = np.quantile(ages, 0.75) - np.quantile(ages, 0.25)
```

数据通常与米、英尺、小时、纳秒、加仑和升等单位相关联。ages 中每个值的单位是“年”。因此，均值、中位数和标准差的单位也是年。

#### 14.1.2 箱线图和须线图

*箱线图*，也称为 *须线图*，显示了所有这些描述性统计。我们可以用 **matplotlib** 绘制一个：

```python
plt.boxplot(ages)
plt.draw()
```

![](img/4ffeafa3e3f1373f02971e43b0755ea6_559_0.png)

中间的箱子垂直范围从 *Q1* 到 *Q3*，中位数由箱内的水平线标记。**matplotlib** 在底部和顶部显示最小值和最大值。

最大值的下方看起来像一个“T”，最小值的上方是一个倒置的“T”。这些是“须线”。上须线位于 Q3 + 1.5 * IQR，若有必要则向下取整，下须线位于 Q1 - 1.5 * IQR，若有必要则向上取整。须线之外的是 *异常值*，应检查其正确性。

此图非常简陋，因此让我们对其进行修饰以了解图所描述的内容。以下代码向箱线图添加线条和标签，使其更容易理解：

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

#### 14.1.3 标准差和方差

让我们来看两个包含 99 个数字的数据集。第一个包含从 1 到 99 的每个整数。第二个包含 99 个整数 50 的副本。在每种情况下，均值都是 50。

```python
data_1 = np.arange(1, 100)
data_2 = np.repeat(50, 99)
(np.mean(data_1), np.mean(data_2))
```

```text
(50.0, 50.0)
```

在第一种情况下，值从均值广泛分布。在第二种情况下，每个值都集中在均值。我们使用 *标准差*，写为希腊字母 sigma = σ，来衡量这种离散程度。**numpy 函数** *std* 计算一维数组值的标准差。

```python
(np.std(data_1), np.std(data_2))
```

```text
(28.577380332470412, 0.0)
```

