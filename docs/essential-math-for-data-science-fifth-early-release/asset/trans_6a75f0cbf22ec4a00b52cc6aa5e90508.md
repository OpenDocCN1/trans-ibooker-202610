

所以它大约会收敛到正确答案，但不会过度，以免在已经找到可接受解时浪费计算时间。

你可能还有另一个疑问，为什么我将 `best_loss` 初始化为一个极大的数值。我这么做是为了用一个我知道在搜索开始后会被覆盖的值来初始化最佳损失，然后它将与每次迭代的新损失进行比较，以查看是否带来改进。我本来也可以使用正无穷 `float( 'inf' )`，而不是一个非常大的数字。

## 爬山算法与逻辑回归

就像前面线性回归的例子一样，我们也可以将爬山算法应用于逻辑回归。如果你觉得微积分和偏导数一次性太多，也可以再次使用这种技术。

爬山算法的方法论是相同的：使用正态分布中的随机值调整 m 和 b。不过，我们有一个不同的目标函数，即第 6 章讨论过的最大似然估计。因此，我们只采取增加似然估计的随机调整，经过足够多的迭代后，我们应该能收敛到一个拟合的逻辑回归。

所有这些都在示例 A-10 中演示。

_示例 A-10. 为简单逻辑回归使用爬山算法._

```
import math
import random
import numpy as np
import pandas as pd

# Desmos graph: https://www.desmos.com/calculator/6cb10atg3l
points = [p for p in pd.read_csv("https://tinyurl.com/y2coco07").itertuples()]
best_likelihood = -10_000_000
b0 = .01
b1 = .01

# calculate maximum likelihood
def predict_probability(x):
    p = 1.0 / (1.0001 + math.exp(-(b0 + b1 * x)))
    return p

for i in range(1_000_000):
# Select b0 or b1 randomly, and adjust it randomly
random_b = random.choice(range(2))

random_adjust = np.random.normal()

if random_b == 0:
    b0 += -random_adjust
elif random_b == 1:
    b1 += random_adjust

# Calculate total likelihood
true_estimates = sum(math.log(predict_probability(p.x)) for p in points if p.y == 1.0)
false_estimates = sum(math.log(1.0 - predict_probability(p.x)) for p in points if p.y == 0.0)

total_likelihood = true_estimates + false_estimates

# If likelihood improves, keep the random adjustment. Otherwise revert.
if best_likelihood < total_likelihood:
    best_likelihood = total_likelihood
elif random_b == 0:
    b0 -= random_adjust
elif random_b == 1:
    b1 -= random_adjust

print("1.0 / (1 + exp(-({0} + {1}*x))".format(b0, b1))
print("BEST LIKELIHOOD: {0}".format(math.exp(best_likelihood)))
```

有关最大似然估计、逻辑函数以及我们使用 log() 函数的原因的更多详细信息，请参阅第 6 章。

## 线性规划简介

每个数据科学专业人士都应该熟悉的一种技术是 **线性规划**，它通过引入“松弛变量”来适应方程组求解不等式系统。当线性规划系统中的变量是离散整数或二进制变量（0 或 1）时，它被称为 **整数规划**。当同时使用线性连续变量和整数变量时，它被称为 **混合整数规划**。

尽管线性规划及其变体更多是算法驱动而非数据驱动，但它们可以用于解决大量经典的“人工智能”问题。虽然将线性规划系统称为“人工智能”听起来可能存疑，但许多供应商和公司都普遍采用这种做法，因为这会提升其感知价值。

在实践中，最好使用众多可用的求解器库来为你执行线性规划，但本节末尾将提供从头开始实现的资源。对于这些示例，我们将使用 PuLP，不过 Pyomo 也是一个选项。我们还将使用图形直觉，尽管超过三维的问题不容易可视化。

以下是我们的示例。你有两条产品线：iPac 和 iPac Ultra。iPac 每单位利润为 200 美元，而 iPac Ultra 每单位利润为 300 美元。

然而，装配线每天只能工作 20 小时，生产一个 iPac 需要 1 小时，生产一个 iPac Ultra 需要 3 小时。

每天只能提供 45 套组件，iPac 需要 6 套，而 iPac Ultra 需要 2 套。

假设所有供应都能售出，我们应该销售多少 iPac 和 iPac Ultra 才能最大化利润？

首先，我们先记录第一个约束并将其拆解。

- ...装配线每天只能工作 20 小时，生产一个 iPac 需要 1 小时，生产一个 iPac Ultra 需要 3 小时。

我们可以将其表示为一个不等式，其中 x 是 iPac 的单位数量，y 是 iPac Ultra 的单位数量。两者都必须为正，图 A-5 显示我们可以相应地绘图。

$x + 3y \le 20(x \ge 0, y \ge 0)$

![](img/8e0dcc1636d032e7728a07e97751e446_422_0.png)

图 A-5. 绘制第一个约束

现在让我们看看第二个约束。

每天只能提供 45 套组件，iPac 需要 6 套，而 iPac Ultra 需要 2 套。

我们也可以如图 A-6 所示建模并绘图。

```
6x + 2y ≤ 45(x ≥ 0, y ≥ 0)
```

![](img/8e0dcc1636d032e7728a07e97751e446_423_0.png)

图 A-6. 绘制第二个约束

注意在图 A-6 中，我们现在有了这两个约束之间的重叠。我们的解位于该重叠区域内，我们称之为 *可行域*。最后，我们要最大化利润 Z，根据 iPac 和 iPac Ultra 各自的利润金额，可以如下表示：

```
Z = 200x + 300y
```

如果将此函数表示为一条直线，我们可以尽可能增加 Z，直到该直线刚好不再位于可行域内。然后我们记录 x 和 y 的值，如图 A-7 所示。

![](img/8e0dcc1636d032e7728a07e97751e446_424_0.png)

图 A-7. 增加我们的目标线，直到它不再位于可行域内

## 目标函数的 DESMOS 图

如果你需要以更具交互性和动画的方式可视化此内容，请查看 Desmos 上的此图表：
https://www.desmos.com/calculator/iildqi2vt7

当你尽可能增加利润 Z 时，当该线“刚好接触”可行域时，你将落在可行域的一个顶点或角上。该顶点提供将最大化利润的 x 和 y 值，如图 A-8 所示。

![](img/8e0dcc1636d032e7728a07e97751e446_426_0.png)

图 A-8. 我们线性规划系统的最大化目标。

虽然我们可以使用 NumPy 和大量矩阵运算来数值求解此问题，但使用如示例 A-11 所示的 PuLP 会更容易。请注意，`LpVariable` 定义要求解的变量。`LpProblem` 是使用 Python 运算符添加约束和目标函数的线性规划系统。然后通过在 `LpProblem` 上调用 `solve()` 来求解变量。

## 示例 A-11. 使用 Python PuLP 求解线性规划系统

```
# GRAPH: https://www.desmos.com/calculator/iildqi2vt7
from pulp import *

# declare your variables
x = LpVariable("x", 0)  # 0<=x
y = LpVariable("y", 0)  # 0<=y

# defines the problem
prob = LpProblem("factory_problem", LpMaximize)

# defines the constraints
prob += x + 3*y <= 20
prob += 6*x +2*y <= 45

# defines the objective function to maximize
prob += 200*x + 300*y

# solve the problem
status = prob.solve()
print(LpStatus[status])

# print the results x = 5.9375, y = 4.6875
print(value(x))
print(value(y))
```

现在你可能在想，生产 5.9375 和 4.6875 个单位是否合理？如果你可以容忍变量中的连续值，线性规划系统会高效得多，而且你也许之后可以将它们四舍五入。但某些类型的问题绝对要求离散地处理整数和二进制变量。

为了强制 x 和 y 变量被视为整数，请提供类别参数 `cat=LpInteger`，如示例 A-12 所示。

## 示例 A-12. 强制将变量求解为整数

```
# declare your variables
x = LpVariable("x", 0, cat=LpInteger) # 0<=x
y = LpVariable("y", 0, cat=LpInteger) # 0<=y
```

在图形上，这意味着我们用离散点而非连续区域来填充可行域。我们的解不一定落在顶点上，而更可能落在最接近顶点的点上，如图 A-9 所示。

