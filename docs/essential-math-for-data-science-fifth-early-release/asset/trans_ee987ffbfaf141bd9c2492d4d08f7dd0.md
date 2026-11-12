

但此处的贝叶斯定理在哪里呢？让我们专注于 $P(\text{Cancer}|\text{Coffee Drinker})$ 表达式，并用我们先前计算的所有表达式来展开它。

$P(\text{Cancer}|\text{Coffee Drinker}) = \frac{100,000 \times P(\text{Cancer}) \times P(\text{Coffee Drinker}|\text{Cancer})}{100,000 \times P(\text{Coffee Drinker})}$

请注意，100,000 的总体 N 既出现在分子中也出现在分母中，因此会被约掉。现在这看起来熟悉了吗？

$P(\text{Cancer}|\text{Coffee Drinker}) = \frac{P(\text{Cancer}) \times P(\text{Coffee Drinker}|\text{Cancer})}{P(\text{Coffee Drinker})}$

果然，这应该与贝叶斯定理相符！

$P(\text{A}|\text{B}) = \frac{P(\text{B}|\text{A})*P(\text{B})}{P(\text{A})}$

$P(\text{Cancer}|\text{Coffee Drinker}) = \frac{P(\text{Cancer}) \times P(\text{Coffee Drinker}|\text{Cancer})}{P(\text{Coffee Drinker})}$

因此，如果你对贝叶斯定理感到困惑，或难以掌握其背后的直觉，不妨根据给定的概率对固定总体进行子集抽取。然后你就可以沿着思路，将条件概率“翻转”过来。

## 从零开始实现 CDF 和逆 CDF

为了计算正态分布下的面积，我们当然可以使用第 1 章学到、并在第 2 章中应用于β分布的矩形填充法。它不需要累积分布函数（CDF），只是在概率密度函数（PDF）下方堆放矩形。使用这种方法，我们可以找到一只金毛寻回犬体重在 61 到 62 磅之间的概率，如示例 A-5 所示，使用 1000 个堆叠的矩形与正态 PDF 进行计算。

### 示例 A-5. Python 中的正态分布函数

```
import math
```

```
def normal_pdf(x: float, mean: float, std_dev: float) -> float:
    return (1.0 / (2.0 * math.pi * std_dev ** 2) ** 0.5) *
           math.exp(-1.0 * ((x - mean) ** 2 / (2.0 * std_dev ** 2)))
```

```
def approximate_integral(a, b, n, f):
    delta_x = (b - a) / n
    total_sum = 0
    for i in range(1, n + 1):
        midpoint = 0.5 * (2 * a + delta_x * (2 * i - 1))
        total_sum += f(midpoint)

    return total_sum * delta_x
```

```
p_between_61_and_62 = approximate_integral(a=61, b=62, n=7,
                                   f=lambda x: normal_pdf(x, 64.43, 2.99))
print(p_between_61_and_62) # 0.0825344984983386
```

这将给我们大约 8.25% 的概率，即一只金毛寻回犬的体重在 61 到 62 磅之间。如果我们想利用已经为我们积分好的累积分布函数，且无需任何矩形堆叠，我们可以像示例 A-6 所示从零开始声明它。

### 示例 A-6. 在 Python 中使用逆 CDF（称为 ppf()）

```
import math
def normal_cdf(x: float, mean: float, std_dev: float) -> float:
    return (1 + math.erf((x - mean) / math.sqrt(2) / std_dev)) / 2
mean = 64.43
std_dev = 2.99
x = normal_cdf(66, mean, std_dev) - normal_cdf(62, mean, std_dev)
print(x) # prints 0.49204501470628936
```

math.erf() 被称为误差函数，常用于计算累积分布。最后，要从零开始实现逆 CDF，你需要使用 erf() 函数的逆函数，称为 erfinv()。示例 A-7 使用从零开始编码的逆 CDF 计算了 1000 个随机生成的金毛寻回犬体重。

### 示例 A-7. 生成随机金毛寻回犬体重

```
import random
from scipy.special import erfinv
def inv_normal_cdf(p: float, mean: float, std_dev: float):
    return mean + (std_dev * (2.0 ** 0.5) * erfinv((2.0 * p) - 1.0))
mean = 64.43
std_dev = 2.99
for i in range(0, 1000):
    random_p = random.uniform(0.0, 1.0)
    print(inv_normal_cdf(random_p, mean, std_dev))
```

## 使用 e 来预测随时间变化的事件概率

让我们再看一个 e 的实用用例。假设你是一家丙烷罐制造商。显然你不希望罐体泄漏，否则可能会造成危险，尤其是在明火和火花附近。在测试一种新罐体设计时，你的工程师报告称，在任何给定年份，它泄漏的概率为 5%。

你知道这个数字已经高到不可接受，但你想知道这个概率随时间如何复合。你现在问自己：“在 2 年内、5 年内、10 年内发生泄漏的概率是多少？”暴露的时间越长，看到罐体泄漏的概率不会只会越高吗？欧拉数 e 又可以来解救了！

```
P_{leak} = 1.0 - e^{-\lambda T}
```

上述函数模拟了随时间变化的事件概率，在此情况下即罐体在 T 时间后泄漏的概率。e 再次是欧拉数，lambda λ 是每个时间单位（每年）的故障率，T 是已过去的时间量（年数）。

如果我们将此函数作图，其中 T 为横轴，泄漏概率为纵轴，且 λ = .05，我们将得到图 A-3 中所示的结果：

![](img/8e0dcc1636d032e7728a07e97751e446_415_0.png)

图 A-3. 预测随时间变化的泄漏概率

以下是我们如何在 Python 中为 λ = .05 和 T = 5 年建模此函数，如示例 A-8 所示。

### 示例 A-8. 预测随时间变化的泄漏概率的代码

```
from math import exp

# Probability of leak in 1 year
p_leak = .05

# number of years
t = 5

# Probability of leak within 5 years
# 0.22119921692859512
p_leak_5_years = 1.0 - exp(-p_leak * t)

print("PROBABILITY OF LEAK WITHIN 5 YEARS: {}".format(p_leak_5_years))
```

罐体在 2 年后发生故障的概率约为 9.5%，5 年约为 22.1%，10 年约为 39.3%。时间一久，罐体泄漏的可能性就越大。我们可以将此公式推广，以预测任何在给定期间内具有概率的事件，并查看该概率在不同时间段内如何变化。

## 山爬算法与线性回归

如果你觉得在从零开始构建机器学习时微积分令人不知所措，可以尝试一种更蛮力的方法。让我们尝试一种山爬算法，在若干次迭代中通过添加随机值来随机调整 m 和 b。这些随机值可以是正数或负数（这会使加法操作实质上变成减法），我们只保留改善平方和的调整。

但是，我们就随意生成任何随机数作为调整量吗？我们希望更倾向于较小的移动，但偶尔可能会允许较大的移动。这样我们大多数时候进行微调，但在需要时偶尔也会做出大跳跃。实现此目的的最佳工具是使用标准正态分布，均值为 0，标准差为 1。回顾第 3 章，标准正态分布在 0 附近具有高值密度，而值距离 0 越远（无论是负方向还是正方向），该值出现的可能性就越小，如图 A-4 所示。

![](img/8e0dcc1636d032e7728a07e97751e446_417_0.png)

图 A-4. 标准正态分布中的大多数值都很小且接近 0，而较大的值在尾部出现的频率较低

回到线性回归，我们将 m 和 b 从 0 或某些其他起始值开始。然后在 `for` 循环中进行 150,000 次迭代，通过添加从标准正态分布中采样的值来随机调整 m 和 b。如果随机调整改善/减小了平方和，我们就保留它。但如果平方和增加了，我们就撤销该随机调整。换句话说，我们只保留改善平方和的调整。让我们在示例 A-9 中看一下。

### 示例 A-9.

```
from numpy.random import normal
import pandas as pd

# Import points from CSV
points = [p for p in pd.read_csv("https://bit.ly/2KF29Bd").itertuples()]

# Building the model
m = 0.0
b = 0.0

# The number of iterations to perform
iterations = 150000

# Number of points
n = float(len(points))

# Initialize with a really large loss
# that we know will get replaced
best_loss = 1000000000000.0

for i in range(iterations):
    # Randomly adjust "m" and "b"
    m_adjust = normal(0,1)
    b_adjust = normal(0,1)

    m += m_adjust
    b += b_adjust

    # Calculate loss, which is total sum squared error
    new_loss = 0.0
    for p in points:
        new_loss += (p.y - (m * p.x + b)) ** 2

    # If loss has improved, keep new values. Otherwise revert.
    if new_loss < best_loss:
        print("y = {0}x + {1}".format(m, b))
        best_loss = new_loss
    else:
        m -= m_adjust
        b -= b_adjust

print("y = {0}x + {1}".format(m, b))
```

你将看到算法的进展，但最终你应该得到一个拟合函数，大约为 y = 1.9395722046562853x + 4.731834051245578，左右可能略有不同。让我们验证一下这个答案。当我使用 Excel 或 Desmos 为我执行线性回归时，Desmos 给出了 y = 1.93939x + 4.73333。不赖！我已经非常接近了！

为什么我们需要 100 万次迭代？通过实验，我发现经过足够多的迭代后，解不再有多大改进，并且 m 和 b 的最优值收敛到可以最小化平方和的非常接近的位置。你会发现许多机器学习库和算法都会有一个执行迭代次数的参数，它正是起到这个作用。你需要有足够的

