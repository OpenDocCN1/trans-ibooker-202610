

![](img/8e0dcc1636d032e7728a07e97751e446_296_0.png)

图 6-6. 逻辑函数

在某些教材中，你也可能会看到逻辑函数按如下形式声明。

$$p = \frac{e^{\beta_0 + \beta_1 x}}{1 + e^{\beta_0 + \beta_1 x}}$$

别担心，因为这只是同一个函数的不同代数表达方式。请注意，就像线性回归一样，我们也可以将逻辑回归扩展到多个输入变量（$x_1, x_2, ... x_n$），如下面的公式所示。我们只需添加更多的 $\beta_x$ 系数。

$$p = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x_1 + \beta_2 x_2 + ... \beta_n x_n)}}$$

## 逻辑曲线拟合

如何将逻辑曲线拟合到给定的训练数据集？首先，数据可以包含小数、整数和二进制变量的任意混合，但输出变量必须是二进制的（0 或 1）。当我们实际进行预测时，输出变量将在 0 到 1 之间，类似于概率。

数据提供了我们的输入和输出变量值，但我们需要求解 $\beta_0$ 和 $\beta_1$ 系数以拟合我们的逻辑函数。回想一下我们在第 5 章中如何使用最小二乘法。然而，这不适用于这里。相反，我们使用最大似然估计，正如其名称所示，最大化给定逻辑曲线输出观测数据的可能性。

要计算最大似然估计，实际上没有像线性回归那样的封闭形式方程。我们仍然可以使用随机梯度下降，或者让库为我们完成。让我们先从库 SciPy 开始，介绍这两种方法。

## 使用 SciPy

SciPy 的好处在于模型通常具有一组标准化的函数和 API，这意味着在许多情况下，你可以复制/粘贴代码，然后在不同模型之间重复使用。在示例 6-3 中，你将看到对我们的患者数据执行逻辑回归。如果你将其与第 5 章中的线性回归代码进行比较，你会发现它在导入、分离和拟合数据方面有着几乎相同的代码。主要区别在于，我为模型使用了 LogisticRegression() 而不是 LinearRegression()。

### 示例 6-3. 使用标准逻辑回归 SciPy

```python
import pandas as pd
from sklearn.linear_model import LogisticRegression

# Load the data
df = pd.read_csv('https://bit.ly/33ebs2R', delimiter=",")

# Extract input variables (all rows, all columns but last column)
X = df.values[:, :-1]

# Extract output column (all rows, last column)
Y = df.values[:, -1]

# Perform logistic regression
# Turn off penalty
model = LogisticRegression(penalty='none')
model.fit(X, Y)

# print beta1
print(model.coef_.flatten()) # 0.69267212

# print beta0
print(model.intercept_.flatten()) # -3.17576395
```

## 进行预测

要进行特定预测，请在 SciPy 的模型对象上使用 predict() 和 predict_prob() 函数，无论它是 LogisticRegression 还是任何其他类型的分类模型。predict() 函数将预测特定类（例如 True 1.0 或 False 1.0），而 predict_prob() 将输出每个类的概率。

在 SciPy 中运行模型后，我得到的逻辑回归为 $\beta_0 = -3.17576395$ 和 $\beta_1 = 0.69267212$。当我绘制它时，它应该看起来不错，如图 6-7 所示。

![](img/8e0dcc1636d032e7728a07e97751e446_298_0.png)

图 6-7. 绘制 Scikit-learn 逻辑回归

这里有几点需要注意。当我创建 `LogisticRegression()` 模型时，我没有指定 `penalty` 参数，该参数选择像 `l1` 或 `l2` 这样的正则化技术。虽然这超出了本书的范围，但我在“关于 SciPy 参数”框中包含了简要的见解，以便你手头有有用的参考。

最后，我将对系数和截距使用 `flatten()`，它们作为多维矩阵输出但只有一个元素。展平意味着将数字矩阵折叠为较少的维度，特别是在元素数量少于维度数量时。例如，我在这里使用 `flatten()` 来获取嵌套在二维矩阵中的单个数字，并将其作为单个值提取出来。然后我就得到了我的 $\beta_0$ 和 $\beta_1$ 系数。

## 关于 SciPy 参数

SciPy 在其回归和分类模型中提供了很多选项。不幸的是，没有足够的篇幅或页面来涵盖它们，因为这不是一本专注于机器学习的书。

然而，SciPy 文档写得很好，关于逻辑回归的页面可以在这里找到。

https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html

如果很多术语不熟悉，例如正则化和 l1 及 l2 惩罚，还有其他优秀的 O'Reilly 书籍探讨这些主题。我发现的一本更有帮助的文本是 Aurélien Géron 编写的 **Hands-On Machine Learning with Scikit-learn, Keras, and TensorFlow**。

https://learning.oreilly.com/library/view/hands-on-machine-learning/9781492032632/

## 使用最大似然和梯度下降

正如我们在本书中一贯所做的，我旨在提供从头构建技术的见解，即使库可以为我们完成。为了自己拟合逻辑回归，有几种方法，但所有方法通常都求助于最大似然估计（MLE）。MLE 最大化给定逻辑曲线输出观测数据的可能性。它与平方和不同，但我们仍然可以应用梯度下降或随机梯度下降来求解。

我将尝试简化数学术语，并在此尽量减少线性代数。基本上，其思路是找到 $\beta_0$ 和 $\beta_1$ 系数，使我们的逻辑曲线尽可能贴近这些点，表明它最有可能生成了这些点。如果你回想一下第 2 章我们学习概率时，我们通过将多个事件的概率（或似然）相乘来组合它们。在此应用中，我们正在计算对于给定的逻辑回归曲线，我们会观察到所有这些点的可能性。

应用联合概率的概念，每个患者都有基于拟合逻辑函数表现出症状的可能性，如图 6-8 所示。

![](img/8e0dcc1636d032e7728a07e97751e446_299_0.png)

图 6-8. 逻辑曲线上每个输入值都有对应的似然值

我们从逻辑回归曲线上每个点上方或下方获取每个似然值。如果点在逻辑回归曲线下方，我们需要将结果概率从 1.0 中减去，因为我们也想最大化负例。

给定系数 $\beta_0 = -3.17576395$ 和 $\beta_1 = 0.69267212$，示例 6-4 展示了如何在 Python 中计算此数据的联合似然。

### 示例 6-4. 计算给定逻辑回归观察所有点的联合似然

```python
import math
import pandas as pd
patient_data = pd.read_csv('https://bit.ly/33ebs2R', delimiter=",").itertuples()
b0 = -3.17576395

b1 = 0.69267212

def logistic_function(x):
    p = 1.0 / (1.0 + math.exp(-(b0 + b1 * x)))
    return p

# Calculate the joint likelihood
joint_likelihood = 1.0

for p in patient_data:
    if p.y == 1.0:
        joint_likelihood *= logistic_function(p.x)
    elif p.y == 0.0:
        joint_likelihood *= (1.0 - logistic_function(p.x))

print(joint_likelihood) # 4.7911180221699105e-05
```

这里有一个数学技巧，可以压缩那个 `if` 表达式。正如我们在第 1 章中介绍的，当你将任何数字设为 0 次幂时，它将始终为 1。看一下这个公式，并注意指数中真（1）和假（0）情况的处理。

joint likelihood = \(\prod_{i=1}^{n} \left( \frac{1.0}{1.0 + e^{-(\beta_0 + \beta_1 x_i)}} \right)^{y_i} \times \left( \frac{1.0}{1.0 + e^{-(\beta_0 + \beta_1 x_i)}} \right)^{1.0-y_i}\)

要在 Python 中执行此操作，请将该 for 循环内的所有内容压缩到示例 6-5 中：

### 示例 6-5. 不使用 `if` 表达式压缩联合似然计算

```python
for p in patient_data:
    joint_likelihood *= logistic_function(p.x) ** p.y * (1.0 - logistic_function(p.x)) ** (1.0 - p.y)
```

我到底做了什么？请注意，这个表达式有两个部分，一个用于 \(y = 1\)，另一个用于 \(y = 0\)。当任何数字被提升到 0 次幂时，结果将为 1。因此，无论 \(y\) 是 1 还是 0，都会导致另一侧的相反条件计算为 1，并在乘法中没有影响。我们得以表达 `if` 表达式，但完全使用数学表达式来完成。我们无法对使用 `if` 的表达式进行求导，因此这将很有帮助。

请注意，计算机在将多个小小数相乘时可能会不堪重负，这被称为 **浮点下溢**。这意味着随着小数越来越小（在乘法中可能发生这种情况），计算机在跟踪那么多小数位时会遇到限制。有一个巧妙的数学技巧可以绕过这个问题。你可以对每个正在相乘的小数取 `log()`，然后将它们相加。这要归功于我们在第 1 章中介绍的对数的加法特性。然后你可以调用 `exp()` 函数将总和转换回乘积。

让我们修改代码以使用对数加法而不是乘法。请注意，`log()` 函数默认以底数 *e* 为底，虽然任何底数从技术上讲都可以工作，但这样做更可取，因为 *e^x* 是自身的导数，从计算角度来看更高效。

