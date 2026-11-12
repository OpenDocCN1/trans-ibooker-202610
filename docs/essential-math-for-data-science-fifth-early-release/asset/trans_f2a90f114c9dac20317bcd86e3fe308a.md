


![](img/8e0dcc1636d032e7728a07e97751e446_271_0.png)

图 5-16. 线性回归假设正态分布围绕回归线分布

当我们有一条沿着线性回归线的正态分布时，我们面对的不再是单一变量，还有第二个变量在引导分布。每个 y 预测值周围都有一个置信区间，这被称为预测区间。

让我们通过兽医示例重新建立一些背景，估算狗的年龄和兽医就诊次数。我想知道一只 8.5 岁狗的兽医就诊次数在 95% 置信水平下的预测区间。这个预测区间的样子如图 5-17 所示。我们有 95% 的把握，一只 8.5 岁的狗兽医就诊次数会在 16.462 到 25.966 次之间。

![](img/8e0dcc1636d032e7728a07e97751e446_273_0.png)

图 5-17. 一只 8.5 岁狗在 95% 置信水平下的预测区间

我们该如何计算这个？我们需要得到误差范围，并将其在预测的 y 值周围进行加减运算。这是一个棘手的方程，涉及 t 分布的临界值以及估计的标准误。让我们来看一下：

![](img/8e0dcc1636d032e7728a07e97751e446_274_0.png)

我们感兴趣的 x 值指定为 $x_0$，在此例中为 8.5。以下是使用 Python 解决此问题的方法，如示例 5-20 所示。

## 示例 5-20. 计算一只 8.5 岁狗的兽医就诊次数预测区间

```
import pandas as pd
from scipy.stats import t
from math import sqrt

# Load the data
points = list(pd.read_csv('https://bit.ly/2KF29Bd', delimiter=",").itertuples())

n = len(points)

# Linear Regression Line
m = 1.939
b = 4.733

# Calculate Prediction Interval for x = 8.5
x_0 = 8.5
x_mean = sum(p.x for p in points) / len(points)

t_value = t(n - 2).ppf(.975)

standard_error = sqrt(sum((p.y - (m * p.x + b)) ** 2 for p in points) / (n - 2))

margin_of_error = t_value * standard_error * \
    sqrt(1 + (1 / n) + (n * (x_0 - x_mean) ** 2) / \
    (n * sum(p.x ** 2 for p in points) - sum(p.x for p in points) ** 2))

predicted_y = m*x_0 + b
# Calculate prediction interval
print(predicted_y - margin_of_error, predicted_y + margin_of_error)
# 16.462516875955465 25.96648312404537
```

哎呀！这需要大量计算，遗憾的是 SciPy 及其他主流数据科学库并不提供此功能。但如果你倾向于统计分析，这将是非常有用的信息。我们不仅基于线性回归做出预测（例如，一只 8.5 岁的狗会有 21.2145 次兽医就诊），还能够表达一个远非绝对的结论：一只 8.5 岁的狗有 95% 的概率兽医就诊次数在 16.46 到 25.96 次之间。很棒，对吧？这是一个更安全的说法，因为它捕捉的是一个范围而不是单一数值，从而考虑了不确定性。

## 参数的置信区间

仔细想想，线性回归线本身就是一个样本统计量，而我们试图推断的总体也有一条线性回归线。这意味着 m 和 b 等参数有自己的分布，我们可以分别围绕 m 和 b 建模置信区间，以反映总体的斜率和 y 截距。这超出了本书的范围，但值得注意的是，可以进行这种类型的分析。

你可以找到从头进行这些计算的资源，但使用 Excel 的回归工具或 Python 库可能更简单。

## 训练/测试划分

我刚才进行的相关系数、统计显著性和决定系数分析，不幸的是并非总是由从业者执行。有时他们面对的数据量太大，没有时间或技术能力去做。例如，一张 128x128 像素的图像至少有 16,384 个变量。你有时间对每个像素变量进行统计分析吗？可能没有！不幸的是，这导致许多数据科学家根本不学习这些统计指标。

在一个晦涩的在线论坛上，我曾读到一篇帖子，说统计回归像手术刀，而机器学习像链锯。在处理海量数据和变量时，你无法用手术刀筛选所有内容。你不得不求助于链锯，虽然你失去了可解释性和精确性，但至少可以扩展规模，对更多数据做出更广泛的预测。话虽如此，抽样偏差和过拟合等统计问题并不会消失。但有一些做法可以用于快速验证。

## 为什么 Scikit-learn 中没有置信区间和 P 值？

Scikit-learn 不支持置信区间和 P 值，因为这两种技术对于高维数据来说是未解决的问题。这只强调了统计学家和机器学习从业者之间的差距。Scikit-learn 的维护者之一 Gael Varoquaux 说过：“……一般来说，计算正确的 P 值需要对数据做出一些假设，而机器学习使用的数据并不满足这些假设（没有多重共线性，与维度相比有足够的数据）……P 值是人们期望被仔细检查的东西（它们是医学研究中的一道防线）。实现它们是在找麻烦……我们只能在非常狭窄的设置中[变量很少]给出 P 值。”

如果你想深入探究，GitHub 上有一些有趣的讨论：

- https://github.com/scikit-learn/scikit-learn/issues/6773
- https://github.com/scikit-learn/scikit-learn/issues/16802

如前所述，`statsmodels` 是一个为统计分析提供有用工具的库。请注意，由于上述原因，它可能无法扩展到更高维的模型。

- https://www.statsmodels.org/stable/regression.html

机器学习从业者用来缓解过拟合的一种基本技术叫做训练/测试划分，通常将 1/3 的数据留作测试，另外 2/3 用于训练（也可以使用其他比例）。**训练数据集**用于拟合线性回归，而**测试数据集**用于衡量线性回归在未见过的数据上的表现。这种技术通常用于所有受监督机器学习，包括逻辑回归和神经网络。图 5-18 展示了我们如何将数据分为 2/3 用于训练和 1/3 用于测试的可视化。

## 这是一个小型数据集

稍后我们将了解，将训练/测试数据集划分为 2/3 和 1/3 之外还有其他方法。如果你的数据集非常小，使用 9/10 和 1/10 搭配交叉验证，或者甚至只使用留一法交叉验证，可能会更好。参见 *训练/测试划分必须是三分之一吗？* 了解更多。

数据

训练

测试

![](img/8e0dcc1636d032e7728a07e97751e446_278_0.png)

图 5-18. 划分训练/测试数据。回归线是通过最小二乘法拟合到训练数据（蓝色）上的，而测试数据（红色）随后被分析，以查看预测在之前未见过的数据上偏离了多少

示例 5-21 展示了如何使用 Scikit-learn 执行训练/测试划分，其中 1/3 的数据留作测试，另外 2/3 用于训练。

> 训练就是拟合回归
> 
> 请记住，“拟合”回归与“训练”同义。后者一词由机器学习从业者使用。

## 示例 5-21. 对线性回归执行训练/测试划分

```python
import pandas as pd
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split

# Load the data
df = pd.read_csv('https://bit.ly/3cIH97A', delimiter=",")

# Extract input variables (all rows, all columns but last column)
X = df.values[:, :-1]

# Extract output column (all rows, last column)
Y = df.values[:, -1]

# Separate training and testing data
# This leaves a third of the data out for testing
X_train, X_test, Y_train, Y_test = train_test_split(X, Y,
                                                test_size=1/3)

model = LinearRegression()
model.fit(X_train, Y_train)
result = model.score(X_test, Y_test)
print("R^2: %.3f" % result)
```

请注意 train_test_split() 会获取我们的数据集（X 和 Y 列），将其打乱，然后根据我们的测试数据集大小返回训练和测试数据集。我们使用 LinearRegression 的 fit() 函数在训练数据 X_train 和 Y_train 上进行拟合。然后我们使用

