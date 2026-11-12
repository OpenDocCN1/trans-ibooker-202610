# 附录 B. 习题答案

## 第 1 章

- 1. 62.6738 是有理数，因为它有有限位小数，因此可以表示为分数 626738 / 10000。
- 2. 10^7 × 10^-5 = 10^(7-5) = 10^2 = 100
- 3. 81^(1/2) = √(81) = 9
- 4. 25^(3/2) = (25^1/2)^3 = 5^3 = 125
- 5. 最终金额将为 $1161.47。Python 脚本如下。

示例 B-1。

```python
from math import exp

p = 1000
r = .05
t = 3
n = 12

a = p * (1 + (r/n))**(n * t)

print(a) # prints 1161.4722313334678
```

- 6. 最终金额将为 $116.18。Python 脚本如下

示例 B-2。

```python
from math import exp

p = 100 # principal, starting amount
r = .05 # interest rate, by year
t = 3.0 # time, number of years

a = p * exp(r*t)

print(a) # prints 116.1834242728283
```

- 7. 导数计算结果为 6x，因此 x = 3 处的斜率为 18。SymPy 代码如下。

示例 B-3。

```python
from sympy import *

# Declare 'x' to SymPy
x = symbols('x')

# Now just use Python syntax to declare function
f = 3*x**2 + 1

# Calculate the derivative of the function
dx__f = diff(f)
print(dx__f) # prints 6*x
print(dx__f.subs(x, 3)) # 18
```

- 8. 0 到 2 之间曲线下的面积为 10。SymPy 代码如下

示例 B-4。

```python
from sympy import *

# Declare 'x' to SymPy
x = symbols('x')

# Now just use Python syntax to declare function
f = 3*x**2 + 1

# Calculate the integral of the function with respect to x
# for the area between x = 0 and 2
area = integrate(f, (x, 0, 2))

print(area) # prints 10
```

## 第 2 章

- 1. $0.3 \times 0.4 = 0.12$，参阅有关联合概率的部分
- 2. $(1 - 0.3) + 0.4 - (0.3 \times 0.4) = 0.98$，参阅并集概率部分，请记住我们要找的是不下雨，因此要从 1.0 中减去该概率。
- 3. $0.3 \times 0.2 = 0.06$，参阅条件概率部分
- 4. 下面的 Python 代码计算结果为 .822，累加了 50 名或更多乘客不出场的概率。

示例 B-5。

```python
from scipy.stats import binom

n = 137
p = .40

p_50_or_more_noshows = 0.0

for x in range(50, 138):
    p_50_or_more_noshows += binom.pmf(x, n, p)

print(p_50_or_more_noshows) # 0.822095588147425
```

- 5. 使用如下面 SciPy 代码所示的贝塔分布，计算直到 .5 的面积并从 1.0 中减去。结果约为 .98，因此这枚硬币极不可能是公平的。

示例 B-6。

```python
from scipy.stats import beta

heads = 8
tails = 2

p = 1.0 - beta.cdf(.5, heads, tails)

print(p) # 0.98046875
```

## 第 3 章

- 1. 均值为 1.752，标准差约为 0.02135。Python 代码如下。

示例 B-7。

```python
from math import sqrt

sample = [1.78, 1.75, 1.72, 1.74, 1.77]

def mean(values):
    return sum(values) / len(values)

def variance_sample(values):
    mean = sum(values) / len(values)
    var = sum((v - mean) ** 2 for v in values) / len(values)
    return var

def std_dev_sample(values):
    return sqrt(variance_sample(values))

mean = mean(sample)
std_dev = std_dev_sample(sample)

print("MEAN: ", mean) # 1.752
print("STD DEV: ", std_dev) # 0.021354156550406264
```

- 2. 使用 CDF 获取 30 个月和 20 个月之间的值，面积约为 .06。Python 代码如下。

示例 B-8。

```python
from scipy.stats import norm

mean = 42
std_dev = 8

x = norm.cdf(30, mean, std_dev) - norm.cdf(20, mean, std_dev)

print(x) # 0.06382743803380352
```

- 3. 一卷平均丝径在 1.7026 和 1.7285 之间的概率为 99%。Python 代码如下。

示例 B-9。

```python
from math import sqrt
from scipy.stats import norm

def critical_z_value(p, mean=0.0, std=1.0):
    norm_dist = norm(loc=mean, scale=std)
    left_area = (1.0 - p) / 2.0
    right_area = 1.0 - ((1.0 - p) / 2.0)
    return norm_dist.ppf(left_area), norm_dist.ppf(right_area)


def ci_large_sample(p, sample_mean, sample_std, n):
    # Sample size must be greater than 30

    lower, upper = critical_z_value(p)
    lower_ci = lower * (sample_std / sqrt(n))
    upper_ci = upper * (sample_std / sqrt(n))

    return sample_mean + lower_ci, sample_mean + upper_ci


print(ci_large_sample(p=.99, sample_mean=1.715588,
sample_std=0.029252, n=34))
# (1.7026658973748656, 1.7285101026251342)
```

- 4. 营销活动的 p 值为 .01888，表明活动有效。Python

示例 B-10。

```python
from scipy.stats import norm

mean = 10345
std_dev = 552

p1 = 1.0 - norm.cdf(11641, mean, std_dev)

# Take advantage of symmetry
p2 = p1

# P-value of both tails
# I could have also just multiplied by 2
p_value = p1 + p2

print("2-tailed P-value", p_value)
if p_value <= .05:
    print("Passes 2-tailed test")
else:
    print("Fails 2-tailed test")
```




双尾 P 值 0.01888333596496139
通过双尾检验

## 第 4 章

- 1. 向量落在 [2 3] 上，Python 代码如下

### 示例 B-11.

```python
from numpy import array

v = array([1,2])

i_hat = array([2, 0])
j_hat = array([0, 1.5])

# fix this line
basis = array([i_hat, j_hat])

# transform vector v into w
w = basis.dot(v)

print(w) # [2. 3.]
```

- 2. 向量落在 [0 -3] 上，Python 代码如下

### 示例 B-12.

```python
from numpy import array

v = array([1,2])

i_hat = array([-2, 1])
j_hat = array([1, -2])

# fix this line
basis = array([i_hat, j_hat])

# transform vector v into w
w = basis.dot(v)

print(w) # [ 0 -3]
```

- 3. 行列式为 2.0，Python 代码如下

### 示例 B-13.

```python
import numpy as np
from numpy.linalg import det

i_hat = np.array([1, 0])
j_hat = np.array([2, 2])
basis = np.array([i_hat,j_hat]).transpose()
determinant = det(basis)
print(determinant) # 2.0
```

- 4. 是的，因为矩阵乘法允许我们将多个矩阵组合成一个表示单一综合变换的矩阵。

- 5. x=19.8, y=-5.4, z=-6。代码如下。

### 示例 B-14.

```python
from numpy import array
from numpy.linalg import inv

A = array([
  [3, 1, 0],
  [2, 4, 1],
  [3, 1, 8]
])

B = array([
  54,
  12,
  6
])

X = inv(A).dot(B)

print(X) # [19.8 -5.4 -6. ]
```

- 6. 是的，它是线性相关的。尽管使用 NumPy 时会有一些浮点数精度问题，但行列式实际上为 0。

### 示例 B-15.

```python
from numpy.linalg import det
from numpy import array

i_hat = array([2, 6])
j_hat = array([1, 3])

basis = array([i_hat, j_hat]).transpose()
print(basis)
determinant = det(basis)
print(determinant) # -3.330669073875464e-16
```

为了解决浮点数问题，请使用 SymPy，这样你将得到 0。

### 示例 B-16.

```python
from sympy import *
basis = Matrix([
    [2,1],
    [6,3]
])
determinant = det(basis)
print(determinant) # 0
```

## 第 5 章

- 1. 正如我们在第 5 章中学到的，有许多工具和方法可以执行线性回归，但这里是使用 Scikit-Learn 的解决方案。
斜率为 1.75919315，截距为 4.69359655。

### 示例 B-17.

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression

# Import points
df = pd.read_csv('https://bit.ly/3C8JzrM', delimiter=", ")

# Extract input variables (all rows, all columns but last column)
X = df.values[:, :-1]

# Extract output column (all rows, last column)
Y = df.values[:, -1]

# Fit a line to the points
fit = LinearRegression().fit(X, Y)

# m = 1.75919315, b = 4.69359655
m = fit.coef_.flatten()
b = fit.intercept_.flatten()
print("m = {0}".format(m))
print("b = {0}".format(b))

# show in chart
plt.plot(X, Y, 'o') # scatterplot
plt.plot(X, m*X+b) # line
plt.show()
```

- 2. 我们得到的相关系数高达 0.92421，检验值为 23.8355，统计显著范围为 $\pm 1.9844$。此
相关性肯定有用且统计显著。代码如下。

### 示例 B-18.

```python
import pandas as pd

# Read data into Pandas dataframe
df = pd.read_csv('https://bit.ly/3C8JzrM', delimiter=",")

# Print correlations between variables
correlations = df.corr(method='pearson')
print(correlations)

# OUTPUT:
#       x         y
# x  1.00000  0.92421
# y  0.92421  1.00000

# Test for statistical significance
from scipy.stats import t
from math import sqrt

# sample size
n = df.shape[0]
print(n)
lower_cv = t(n - 1).ppf(.025)
upper_cv = t(n - 1).ppf(.975)

# retrieve correlation coefficient
r = correlations["y"]["x"]

# Perform the test
test_value = r / sqrt((1 - r ** 2) / (n - 2))

print("TEST VALUE: {}".format(test_value))
print("CRITICAL RANGE: {}, {}".format(lower_cv, upper_cv))

if test_value < lower_cv or test_value > upper_cv:
    print("CORRELATION PROVEN, REJECT H0")
else:
    print("CORRELATION NOT PROVEN, FAILED TO REJECT H0 ")

# Calculate p-value
if test_value > 0:
    p_value = 1.0 - t(n - 1).cdf(test_value)
else:
    p_value = t(n - 1).cdf(test_value)

# Two-tailed, so multiply by 2
p_value = p_value * 2
print("P-VALUE: {}".format(p_value))

"""
TEST VALUE: 23.835515323677328
CRITICAL RANGE: -1.9844674544266925, 1.984467454426692

CORRELATION PROVEN, REJECT H0
P-VALUE: 0.0 (extremely small)
"""
```

- 3. 在 x = 50 时，预测区间在 50.79 到 134.51 之间。代码如下。

### 示例 B-19.

```python
import pandas as pd
from scipy.stats import t
from math import sqrt

# Load the data
points = list(pd.read_csv('https://bit.ly/3C8JzrM',
delimiter=", ").itertuples())

n = len(points)

# Linear Regression Line
m = 1.75919315
b = 4.69359655

# Calculate Prediction Interval for x = 50
x_0 = 50
x_mean = sum(p.x for p in points) / len(points)

t_value = t(n - 2).ppf(.975)

standard_error = sqrt(sum((p.y - (m * p.x + b)) ** 2 for p in
points) / (n - 2))

margin_of_error = t_value * standard_error * \
sqrt(1 + (1 / n) + (n * (x_0 - x_mean) ** 2) \
/ \
(n * sum(p.x ** 2 for p in points) -
sum(p.x for p in points) ** 2))

predicted_y = m*x_0 + b

# Calculate prediction interval
print(predicted_y - margin_of_error, predicted_y +
margin_of_error)
# 50.792086501055955 134.51442159894404
```

- 4. 当测试数据集被分成三份并使用 k=3 的 k 折交叉验证进行评估时，表现相当不错。你大约会在三个数据集中得到 MSE 均值为 0.83，标准差为 0.03。

### 示例 B-20.

```python
import pandas as pd
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import KFold, cross_val_score

df = pd.read_csv('https://bit.ly/3C8JzrM', delimiter=",")

# Extract input variables (all rows, all columns but last column)
X = df.values[:, :-1]

# Extract output column (all rows, last column)
Y = df.values[:, -1]

# Perform a simple linear regression
kfold = KFold(n_splits=3, random_state=7, shuffle=True)
model = LinearRegression()
results = cross_val_score(model, X, Y, cv=kfold)
print(results)
print("MSE: mean=% .3f (stdev-%.3f)" % (results.mean(),
results.std()))

"""
[0.86119665 0.78237719 0.85733887]
MSE: mean=0.834 (stdev-0.036)
"""
```

## 第 6 章

- 1. 通过 Scikit-Learn 运行此操作时，准确率极高。当我运行时，在测试折中平均准确率至少为 99.9%。

### 示例 B-21.

```python
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix
from sklearn.model_selection import KFold, cross_val_score
# Load the data
df = pd.read_csv("https://bit.ly/3imidqa", delimiter=",")
X = df.values[:, :-1]
Y = df.values[:, -1]
kfold = KFold(n_splits=3, shuffle=True)
model = LogisticRegression(penalty='none')
results = cross_val_score(model, X, Y, cv=kfold)
print("Accuracy Mean: %.3f (stdev=%.3f)" % (results.mean(), results.std()))
```

- 2. 混淆矩阵将产生极高数量的真正例和真负例，以及极少的假正例和假负例。运行此代码你将看到。

### 示例 B-22.

```python
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import confusion_matrix
from sklearn.model_selection import train_test_split
# Load the data
df = pd.read_csv("https://bit.ly/3imidqa", delimiter=",")
# Extract input variables (all rows, all columns but last column)
X = df.values[:, :-1]
# Extract output column (all rows, last column)\
Y = df.values[:, -1]

model = LogisticRegression(solver='liblinear')
X_train, X_test, Y_train, Y_test = train_test_split(X, Y,
test_size=.33)
model.fit(X_train, Y_train)
prediction = model.predict(X_test)

"""
The confusion matrix evaluates accuracy within each category.
[truepositives falsenegatives]
[falsepositives truenegatives]]

The diagonal represents correct predictions,
so we want those to be higher
"""

matrix = confusion_matrix(y_true=Y_test, y_pred=prediction)
print(matrix)
```

- 3. 一个用于测试用户输入颜色的交互式 shell 如下。考虑测试黑色 (0,0,0) 和白色 (255,255,255)，以查看深色和浅色字体是否分别被正确预测。

### 示例 B-23.

```python
import pandas as pd
from sklearn.linear_model import LogisticRegression
import numpy as np
from sklearn.model_selection import train_test_split

# Load the data
df = pd.read_csv("https://bit.ly/3imidqa", delimiter=",")

# Extract input variables (all rows, all columns but last
column)
X = df.values[:, :-1]

# Extract output column (all rows, last column)
Y = df.values[:, -1]

model = LogisticRegression(solver='liblinear')

X_train, X_test, Y_train, Y_test = train_test_split(X, Y,
test_size=.33)
model.fit(X_train, Y_train)
prediction = model.predict(X_test)
```





```
# Test a prediction
while True:
    n = input("Input a color {red},{green},{blue}: ")
    (r, g, b) = n.split(",")
    x = model.predict(np.array([[int(r), int(g), int(b)]]))
    if model.predict(np.array([[int(r), int(g), int(b)]]))[0] == 0.0:
        print("LIGHT")
    else:
        print("DARK")
```

- 4. 是的，逻辑回归在为给定背景色预测浅色或深色字体方面非常有效。准确率不仅极高，而且混淆矩阵在从右上到左下的对角线上数值较高，而其他单元格数值较低。

## 第 7 章

显然，你可以尝试大量的实验和“炼金术”，比如使用不同的隐藏层、激活函数、不同的测试数据集大小等。我尝试使用带有 3 个节点和 ReLU 激活的 1 个隐藏层，但在测试数据集上难以获得良好的预测结果。混淆矩阵和准确率始终较差，我尝试的任何配置更改效果同样糟糕。

神经网络可能失败的原因是：1）测试数据集对于神经网络（这种极其贪婪的数据模型）来说远远太小；2）针对这类问题，还有像逻辑回归这样更简单、更有效的模型。这并不是说你找不到可行的配置，但你必须小心，不要通过 p-hack（p 值操纵）来获得一个对现有少量训练和测试数据过拟合的良好结果。

我使用的 Scikit-Learn 代码如下。

### 示例 B-24.

```
import pandas as pd
# load data
from sklearn.metrics import confusion_matrix
from sklearn.model_selection import train_test_split
from sklearn.neural_network import MLPClassifier
df = pd.read_csv('https://tinyurl.com/y6r7qjrp', delimiter=",")
# Extract input variables (all rows, all columns but last column)
X = df.values[:, :-1]
# Extract output column (all rows, last column)
Y = df.values[:, -1]
# Separate training and testing data
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=1/3)
nn = MLPClassifier(solver='sgd',
                   hidden_layer_sizes=(3,),
                   activation='relu',
                   max_iter=100_000,
                   learning_rate_init=.05)
```

```
nn.fit(X_train, Y_train)
```

```
print("Training set score: %f" % nn.score(X_train, Y_train))
print("Test set score: %f" % nn.score(X_test, Y_test))
```

```
print("Confusion matrix:")
matrix = confusion_matrix(y_true=Y_test, y_pred=nn.predict(X_test))
print(matrix)
```

## 关于作者

**托马斯·奈德尔** 是奈德尔咨询集团（Nield Consulting Group）的创始人，也是 O'Reilly Media 和南加州大学的讲师。他喜欢让不熟悉或畏惧技术内容的人觉得技术内容平易近人且相关。托马斯经常教授数据分析、机器学习、数学优化和实用人工智能等课程。他撰写了两本书，包括 *SQL 入门*（O'Reilly）和 *学习 RxJava*（Packt）。

