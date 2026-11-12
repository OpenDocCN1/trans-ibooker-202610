

## 附录 A. 附录

### 使用 SymPy 进行 LaTeX 渲染

当你对数学符号越来越熟悉时，将 SymPy 表达式转换为数学符号显示会很有帮助。

最快的方法是在你的表达式上使用 SymPy 的 `latex()` 函数，然后将结果复制到 Math LaTeX 查看器中。

示例 A-1 展示了如何将一个简单的表达式转换为 LaTeX 字符串。当然，我们也可以对导数、积分以及其他 SymPy 操作的结果进行 LaTeX 渲染。不过，为了保持示例简洁，我们只做最简单的展示。

### 示例 A-1. 使用 SymPy 将表达式转换为 LaTeX

```
from sympy import *
x,y = symbols('x y')
z = x**2 / sqrt(2*y**3 - 1)
print(latex(z))
# prints
# \frac{x^{(2)}}{\sqrt{2 y^{(3)} - 1}}
```

这个 `\frac{x^{(2)}}{\sqrt{2 y^{(3)} - 1}}` 字符串是格式化后的数学 LaTeX，有多种工具和文档格式可以对其进行适配以支持显示。但如果你只想渲染数学 LaTeX，可以去一个 LaTeX 方程编辑器。以下是我常用的两个在线编辑器：

- https://lateditor.lagrida.com/
- https://latex.codecogs.com/

在图 A-1 中，我使用 Lagrida 的 LaTeX 编辑器来渲染数学表达式。

![](img/8e0dcc1636d032e7728a07e97751e446_407_0.png)

![](img/8e0dcc1636d032e7728a07e97751e446_407_1.png)

图 A-1. 使用数学 LaTeX 编辑器查看 SymPy LaTeX 输出

如果你想省去复制粘贴的步骤，可以将 LaTeX 直接作为参数追加到 CodeCogs LaTeX 编辑器的 URL 中，如示例 A-2 所示，这样浏览器就会显示渲染后的数学方程。

### 示例 A-2. 使用 CodeCogs 打开数学 LaTeX 渲染

```python
import webbrowser
from sympy import *

x,y = symbols('x y')

z = x**2 / sqrt(2*x*y**3 - 1)

webbrowser.open("https://latex.codecogs.com/png.image?\dpi(200)" + latex(z))
```

如果你使用 Jupyter，也可以使用插件来渲染数学 LaTeX。

### 从零开始实现二项分布

如果你想从零开始实现二项分布，示例 A-3 中包含了你需要的所有部分。

### 示例 A-3. 从零开始构建二项分布

```python
# Factorials multiply consecutive descending integers down to 1
# EXAMPLE: 5! = 5 * 4 * 3 * 2 * 1
def factorial(n: int):
    f = 1
    for i in range(n):
        f *= (i + 1)
    return f

# Generates the coefficient needed for the binomial distribution
def binomial_coefficient(n: int, k: int):
    return factorial(n) / (factorial(k) * factorial(n - k))

# Binomial distribution calculates the probability of k events out of n trials
# given the p probability of k occurring
def binomial_distribution(k: int, n: int, p: float):
    return binomial_coefficient(n, k) * (p ** k) * (1.0 - p) ** (n - k)

# 10 trials where each has 90% success probability
n = 10
p = 0.9

for k in range(n + 1):
    probability = binomial_distribution(k, n, p)
    print("{0} - {1}".format(k, probability))
```

使用 `factorial()` 和 `binomial_coefficient()`，我们可以从零开始构建二项分布函数。阶乘函数将 1 到 n 的连续整数范围相乘。例如，5! 的阶乘是 1*2*3*4*5 = 120。

二项系数函数允许我们从 n 种可能性中选取 k 种结果，而不考虑顺序。如果 k = 2 且 n = 3，那么将分别生成集合 (1,2) 和 (1,2,3)。在这两个集合之间，可能的不同组合为 (1,3)、(1,2) 和 (2,3)。共有 3 种组合，因此二项系数为 3。当然，使用 `binomial_coefficient()` 函数，我们可以通过阶乘和乘法来避免所有排列工作。

在实现 `binomial_distribution()` 时，请注意我们如何取二项系数，并将其乘以成功概率 p 发生 k 次（即指数）的概率。然后我们将其乘以相反的情况：失败概率 1.0 - p 发生 n - k 次的概率。这样我们就可以考虑事件在多次试验中发生与不发生的概率 p。

### 从零开始实现β分布

如果你很好奇如何从零开始构建β分布，你需要重用用于二项分布的 `factorial()` 函数，以及上一章中构建的 `approximate_integral()` 函数。

就像我们在上一章中所做的那样，我们将感兴趣范围内的矩形堆积在曲线下方，如图 A-2 所示。

![](img/8e0dcc1636d032e7728a07e97751e446_409_0.png)

图 A-2. 在曲线下方堆积矩形以求面积/概率

这里只使用了 6 个矩形，如果使用更多矩形，精度会更高。让我们从零开始实现 `beta_distribution()`，并在 .9 到 1.0 之间积分 1000 个矩形，如示例 A-4 所示。

### 示例 A-4. β分布从零开始

```python
# Factorials multiply consecutive descending integers down to 1
# EXAMPLE: 5! = 5 * 4 * 3 * 2 * 1
def factorial(n: int):
    f = 1
    for i in range(n):
        f *= (i + 1)
    return f
```

```python
def approximate_integral(a, b, n, f):
    delta_x = (b - a) / n
    total_sum = 0

    for i in range(1, n + 1):
        midpoint = 0.5 * (2 * a + delta_x * (2 * i - 1))
        total_sum += f(midpoint)

    return total_sum * delta_x
```

```python
def beta_distribution(x: float, alpha: float, beta: float) -> float:
    if x < 0.0 or x > 1.0:
        raise ValueError("x must be between 0.0 and 1.0")

    numerator = x ** (alpha - 1.0) * (1.0 - x) ** (beta - 1.0)
    denominator = (1.0 * factorial(alpha - 1) * factorial(beta - 1)) / (1.0 *
    factorial(alpha + beta - 1))

    return numerator / denominator
```

```python
greater_than_90 = approximate_integral(a=.90, b=1.0, n=1000, f=lambda x:
beta_distribution(x, 8, 2))
less_than_90 = 1.0 - greater_than_90

print("GREATER THAN 90%: {}, LESS THAN 90%: {}".format(greater_than_90,
less_than_90))
```

你会发现，在 `beta_distribution()` 函数中，我们提供一个给定概率 x、一个量化成功的 alpha 值，以及一个量化失败的 beta 值。该函数将返回观察到给定可能性 x 的可能性。但同样，为了获得观察概率 x 的概率，我们需要在 x 值的范围内找到一个区域。幸运的是，我们已经从上一章中定义好了 `approximate_integral()` 函数，可以直接使用。我们可以计算成功率大于 90% 以及小于 90% 的概率，如上文最后几行所示。

### 推导贝叶斯定理

如果你想理解贝叶斯定理为什么有效，而不仅仅是听我说，让我们做一个思想实验。假设我有 100,000 人的人口。用给定的概率乘以它，得到喝咖啡的人数和患癌症的人数。

```
N = 100,000

P(Coffee Drinker) = .65

P(Cancer) = .005

Coffee Drinkers = 65,000

Cancer Patients = 500
```

我们有 65,000 名咖啡饮用者和 500 名癌症患者。在这 500 名癌症患者中，有多少人喝咖啡？我们得到了条件概率 P(Coffee|Cancer)，可以将其乘以这 500 人，这应该能得出 425 名喝咖啡的癌症患者。

```
P(Coffee Drinker|Cancer) = .85

Coffee Drinkers with Cancer = 500×.85 = 425
```

那么，喝咖啡的人中患癌症的百分比是多少？我们要除以哪两个数字？我们已经知道了既喝咖啡又患癌症的人数。因此，我们将这个数字与咖啡饮用者的总数进行比例计算。

```
P(Cancer|Coffee Drinker) = \frac{Coffee Drinkers with Cancer}{Coffee Drinkers}

P(Cancer|Coffee Drinker) = \frac{425}{65,000}

P(Cancer|Coffee Drinker) = 0.006538
```

等等，我们刚刚翻转了条件概率吗？是的，我们做到了！我们从 P(Coffee Drinker|Cancer) 开始，最终得到了 P(Cancer|Coffee Drinker)。通过取出人群的两个子集（65,000 名咖啡饮用者和 500 名癌症患者），然后应用我们拥有的条件概率的联合概率，我们最终得到了 425 名既喝咖啡又患癌症的人。然后我们将其除以咖啡饮用者的人数，得到给定某人是咖啡饮用者时患癌症的概率。

