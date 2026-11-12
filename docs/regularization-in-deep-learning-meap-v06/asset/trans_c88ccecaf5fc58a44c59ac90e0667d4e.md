
共线性存在于原始设计矩阵 $\Phi$ 中。此外，权重因子 $\lambda$（从技术上讲应为 $\sqrt{\lambda}$）越大，这些扩展列向量就越正交，从而使得正规方程的解也更加稳定。

请注意，我们也可以基于一阶条件，通过将梯度设为零，采用相同的方法推导出正规方程：

```
\frac{\partial Q(\mathbf{w})}{\partial \mathbf{w}} = \frac{\partial(\mathbf{y}-\Phi\mathbf{w})^T(\mathbf{y}-\Phi\mathbf{w}) + \lambda \mathbf{w}^T \Omega \mathbf{w}}{\partial \mathbf{w}} = -2 \Phi^T \mathbf{y} + 2 \Phi^T \Phi \mathbf{w} + 2 \lambda \Omega \mathbf{w} = 0
```

整理各项后，这会给出相同的正规方程 $(\Phi^T\Phi + \lambda \Omega)\mathbf{w}=\Phi^T \mathbf{y}$。不过，与前一种方法相比，这种方法无法提供相同的洞察。

另外，请注意矩阵乘法通常以对第一个矩阵转置开始。这取决于我们如何安排和解释矩阵的行和列，以便乘法结果与预期形状相匹配。

## 3.2.4 实现平滑样条模型

为了使用平滑样条模型观察“双下降”现象，我们将创建以下函数来表示真实的底层模式：

$$
y = ||x - 0.4| - 0.2| + \frac{x}{2} - 0.1
$$

这是使用平滑样条模型进行近似的真实分段线性函数。下面的代码清单实现了待近似的底层函数。

### 清单 3.1 定义真实的底层函数

```
def get_true_y(x):
    return np.abs(np.abs(x - 0.4) - 0.2) + x/2 - 0.1
```

然后，我们可以采样若干个等距输入点并获取相应的输出以进行绘图。这通过以下代码片段完成。

```
import math
import matplotlib.pyplot as plt
import numpy as np
from numpy import random
random.seed(1)
X_vals = np.linspace(0, 1, 100).reshape(-1,1)
y_vals = get_true_y(X_vals)
plt.plot(y_vals, color = 'r')
plt.title('Underlying true function')
plt.xlabel('X')
plt.ylabel('y')
plt.show()
```

- #A 导入必要的包
- #B 设置种子以确保可重复性
- #C 创建一组等距的 X 值
- #D 计算相应的真实 y 值
- #E 绘制真实函数

生成的输出如图 3.8 所示，它展示了一个由四个线性片段组成的分段线性函数。我们将构建一个具有不同数量多项式项的平滑样条模型，以近似底层的真实函数。

![](img/ee98e91e6845f41077b163bec833ee40_86_0.png)

图 3.8 平滑样条模型待近似的真实底层分段线性函数。

我们首先从转换函数入手，该函数将原始单变量输入 X 映射至新的设计矩阵 $\Phi$，最高不超过预设的多项式次数。这与第 2 章中定义的函数相同。

### 清单 3.2 定义多项式转换函数

```
def polynomial(x, num_basis=2):
    Phi = np.zeros((x.shape[0], num_basis))
    for i in range(num_basis):
        Phi[:, i:i+1] = x**i
    return Phi
```

- #A 基于指定数量的多项式基生成新设计矩阵的函数
- #B 创建零矩阵作为占位符
- #C 基于多项式序列填充每一列

至于惩罚项 $J(\mathbf{w}) = \iint (m''(x))^2 dx$，在转换为 $J(\mathbf{w}) = \mathbf{w}^T \Omega \mathbf{w}$ 时，可以通过离散的二维块进行近似。具体地，$J(\mathbf{w})$ 使用以下公式近似：

$J(\mathbf{w}) = \sum_{q=2}^p \sum_{r=2}^q \beta_{q,r} w_q w_r$

其中 $\beta_{q,r}$ 表示 $\mathbf{\Omega}$ 的元素，计算如下：

$\beta_{q,r} = \frac{(q-1)q(r-1)r}{q+r-3}$

### 清单 3.3 求解平滑样条模型的解析解

```
def estimate_coef(X, y, num_basis):
    Phi = polynomial(X, num_basis=num_basis)
    q = np.arange(2, num_basis).reshape(1, -1)
    r = q.reshape(-1, 1)
    omega = np.zeros((num_basis, num_basis))
    omega[2:, 2:] = (q - 1) * q * (r - 1) * r / (q+r-3)
    values, vectors = np.linalg.eig(omega)
    beta = vectors @ np.diag(values.clip(min = 0) ** 0.5)
    y_star = np.concatenate((y, np.zeros(omega.shape[0]).reshape(-1,1)), axis=0)
    Phi_star = np.concatenate((Phi, math.sqrt(rho) * beta.T), axis=0)
    w_star = np.linalg.lstsq(Phi_star, y_star, rcond=None)[0]
    return w_star
```

- #A 估计模型系数的函数
- #B 通过添加多项式扩展原始设计矩阵
- #C 使用 omega 矩阵近似惩罚项
- #D 在裁剪非负值后，将 omega 矩阵的平方根生成到 beta 矩阵中
- #E 分别创建拉长的设计矩阵和结果变量
- #F 使用 numpy 的最小二乘程序求解最优权重向量

接下来，我们创建一个函数来计算经验风险，即给定数据集和系数集的均方误差。

### 清单 3.4 计算均方误差

```
def calculate_mse(X, y, coef, num_basis):
    Phi = polynomial(X, num_basis=num_basis)
    f_star = Phi@coef
    resid = y - f_star
    Q = np.dot(resid.T, resid).mean()
    return Q
```

- #A 通过添加多项式扩展原始设计矩阵
- #B 生成模型预测值
- #C 计算残差
- #D 使用最优权重计算代价

请注意，即使模型足够复杂可以完美插值训练数据，计算过程中精确的 MSE 损失也不会体现为零。相反，它会是一个非常小的数，例如 $10^{-8}$，可以将其视为零。

最后，我们创建一个函数，允许对上述步骤进行多次运行，并根据预设的模拟次数报告中位数结果。具体地，我们首先创建两个占位符（`mse_train` 和 `mse_test`）来存储多次运行的结果。对于每次运行，我们将生成一个不同的训练数据集，如果需要，还可以选择用一定水平的噪声损坏目标。泛化性能通过 X_test 中一组等距点以及 y_test 中对应的真实目标值进行评估。接下来，我们通过依次评估最高达不同多项式次数的模型来改变模型复杂度，最高次数为 `num_basis_max`。每次评估包括估计模型系数并计算相应的代价。函数末尾，我们返回多次运行中训练集和测试集的中位数代价。

### 清单 3.5 计算多次运行的中位数代价

```
def compute_performance(num_train_samples, num_runs, num_basis_max, train_noise_std=0):
    mse_train = np.zeros((num_runs, num_basis_max))
    mse_test = np.zeros((num_runs, num_basis_max))
    for k in range(num_runs):
        X_train = random.randn(num_train_samples).reshape(-1, 1)
        y_train = get_true_y(X_train).reshape(-1,1)
        if train_noise_std > 0:
            y_train = y_train + np.random.normal(0, 0.01, y_train.shape[0]).reshape(-1,1)
        X_test = np.linspace(0, 1, 100, dtype=X_train.dtype).reshape(-1,1)
        y_test = get_true_y(X_test).reshape(-1,1)
        for num_basis in range(1, num_basis_max + 1):
            fitted_coef = estimate_coef(X_train, y_train, num_basis)
            mse_train[k, num_basis-1] = calculate_mse(X_train, y_train, fitted_coef, num_basis)
            mse_test[k, num_basis-1] = calculate_mse(X_test, y_test, fitted_coef, num_basis)
    return np.median(mse_train, axis = 0), np.median(mse_test, axis=0)
```

- #A 执行多次模拟并报告中位数结果的函数
- #B 用于存储多次运行 mse 的占位符矩阵
- #C 执行多次运行并报告中位数
- #D 为每次运行生成不同的训练集
- #E 如果启用，则用随机噪声损坏目标
- #F 生成用于得出泛化性能的测试集数据点列表
- #G 通过遍历每个多项式次数来改变模型复杂度
- #H 估计模型系数
- #I 存储多次运行的结果

在定义上述实用函数后，我们现在可以指定全局超参数并运行模拟。在以下代码清单中，我们将调参数 $\lambda$ 设置为一个非常小的数，该数值可进一步通过交叉验证进行评估。我们总共运行十次，每次包含十个训练样本和最高 30 次多项式。
