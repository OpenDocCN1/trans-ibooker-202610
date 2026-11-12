

其中 $\lambda_i \ge 0$，使得当所有点的约束条件都满足时 $F_L \le w'w/2$，且如果对变量 $(y_j, x_j)$ 不满足约束条件 $y_i(w'x_i + w_0) \ge 1$，则会增加 $F_L$。对 $w$ 和 $w_0$ 求导

$$\frac{\partial F_L}{\partial w} = 0 = w - \sum_{i=1}^n \lambda_i y_i x_i,$$ (5.64)

$$\frac{\partial F_L}{\partial w_0} = 0 = \sum_{i=1}^n \lambda_i y_i,$$ (5.65)

并将这些方程代入公式 (5.63)，我们得到对偶问题

$$F_D = \frac{1}{2}w'w - w'w + \sum_{i=1}^n \lambda_i = \sum_{i=1}^n \lambda_i - \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \lambda_i \lambda_j y_i y_j x_i' x_j,$$ (5.66)

这是一个关于 $n$ 个变量 $\lambda_i$ 的问题，这些变量满足限制条件 $\lambda_i \ge 0$ 和 $\sum_{i=1}^n \lambda_i y_i = 0$。它被称为对偶问题，最小化 $F_L$ 以找到定义超平面的 $p+1$ 个变量及其限制条件，等价于在满足各自限制条件的 $n$ 个变量 $\lambda_i$ 条件下最大化 $F_D$。第二个问题更容易求解，且目标函数仅通过特征向量的标量积依赖于数据。这是一个具有二次准则和线性约束的凸优化问题，可通过标准优化方法求解。参见，例如，Hastie et al. (2001)。

还要注意，超平面由样本点之间的标量积定义，因为根据公式 (5.64) 它可以写成

$$f(x) = w'x + w_0 = \sum_{i=1}^n \lambda_i y_i x_i' x + w_0.$$ (5.67)

超平面由 $\lambda_i > 0$ 的点定义，而 $\lambda_i = 0$ 的点在超平面方程中不起作用。$\lambda_i > 0$ 的点称为支持向量，并满足 $y_i(w'x_i + w_0) = 1$。参见图 5.32。分类是根据 $w'x_i + w_0$ 的符号进行的。请注意，限制条件定义了一个由限制超平面 $w'x_i + w_0 = 1$ 和 $w'x_i + w_0 = -1$ 所界定的空间，该空间内不存在数据点。包含在这些限制超平面中的数据是支持向量，它们起着关键作用，因为如果它们被修改，分类将会改变，这一特性仅适用于支持向量。这是与 Fisher 的 LDF 或逻辑回归的重要区别。在这些方法中，对任意点的微小扰动（从 $x_i$ 到 $x_i + \epsilon u$，其中 $\epsilon$ 很小）可能会导致分类变化。在支持向量机 (SVM) 中，这种情况会发生在靠近限制超平面的点上，但对远离两个超平面的点则没有影响。

对于具有相同协方差矩阵的正态可分数据，Fisher LDF 提供的解是最优的，但当协方差矩阵不同且数据不服从正态分布时，SVM 提供了一个有用的替代方案。通常在这些情况下，逻辑回归会提供与 SVM 类似的解。

### 5.6.2.2 非线性可分问题

如果数据不可线性分离，我们可以修改问题，找到一个超平面，使其在大多数数据上最大化类群间的分离，但允许少数点被错误分类。为了达到这个目的，我们为每个点引入一个变量 $\alpha_i \ge 0$，如果点被正确分类则该变量为零，否则为正。公式 (5.62) 的优化问题修改为

$$
\min \left( \frac{1}{2} \|w\|^2 + C \sum_{i=1}^n \alpha_i \right) , \tag{5.68}
$$

\[
\text{subject to } y_i(w'x_i + w_0) \ge 1 - \alpha_i, \quad i = 1, \dots, n, \\
\alpha_i \ge 0, \tag{5.69}
\]

其中需要给定参数 $C$。通常会尝试不同的值，并选择分类误差最小的那个。它也可以通过交叉验证 (CV) 获得，如第 5.4.2 节所述。拉格朗日函数为

$$
F_L = \frac{1}{2} w'w + C \sum_{i=1}^n \alpha_i - \sum_{i=1}^n \lambda_i [y_i(w'x_i + w_0) - (1 - \alpha_i)] - \sum_{i=1}^n \mu_i \alpha_i \tag{5.70}
$$

其中 $\lambda_i \ge 0$。对 $w$、$w_0$ 和 $\alpha_i$ 求导，我们有

$$
\frac{\partial F_L}{\partial w} = 0 = w - \sum_{i=1}^n \lambda_i y_i x_i, \\
\frac{\partial F_L}{\partial w_0} = 0 = \sum_{i=1}^n \lambda_i y_i, \\
\frac{\partial F_L}{\partial \alpha_i} = 0 = C - \lambda_i - \mu_i
$$

并且，将这些方程代入公式 (5.70)，我们得到对偶函数

$$
F_D = \sum_{i=1}^n \lambda_i - \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \lambda_i \lambda_j y_i y_j x'_i x_j, \tag{5.71}
$$

该函数需在限制条件 $\mu_i \ge 0, 0 \le \lambda_i \le C$ 和 $\sum_{i=1}^n \lambda_i y_i = 0$ 下最大化。该问题与公式 (5.66) 类似，同样，函数 $F_D$ 以及超平面都通过标量积 $x'_i x_j$ 依赖于数据，如公式 (5.67) 所示。

除了允许存在一些不可线性分离的点外，SVM 方法还可以通过增加空间维度来处理非线性。这通过引入以前特征的非线性函数作为新特征，并在该更大空间中寻找最优线性分离来实现。这种方法与经典程序所采用的方法相反，经典程序将数据投影到较小空间并使用非线性函数来分离状态。例如，在图 5.33 中，两个组在一维空间中无法分离，但在二维空间中是线性可分的。当我们引入新变量时，重要的是要意识到，如公式 (5.66) 所示，目标函数仅依赖于观测值之间的标量积。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_304_0.png)

**Figure 5.33** 可在二维空间中线性分离但不能在一维空间中分离的两个变量。

因此，如果我们引入特征的非线性变换，并从变量 $\mathbf{x} \in R^p$ 转到一组新变量 $\boldsymbol{h}(\mathbf{x}) \in R^q$，其中 $q > p$，我们需要计算标量积 $\boldsymbol{h}(\mathbf{x})'\boldsymbol{h}(\mathbf{x})$。一种快速方法是使用内积核函数，它直接给出标量积。例如，多项式内积核函数是

$$
K(\mathbf{x}_i, \mathbf{x}'_j) = (1 + \mathbf{x}'_i \mathbf{x}'_j)^d = \boldsymbol{h}(\mathbf{x}_i)' \boldsymbol{h}(\mathbf{x}_j),
$$

它提供了两个向量 $\boldsymbol{h}(\mathbf{x}_i)'\boldsymbol{h}(\mathbf{x}_j)$ 的标量积，该标量积包含了 $\mathbf{x}$ 中每个 $p$ 个特征上的 $d$ 阶多项式。例如，假设 $d=2$ 和 $p=2$。那么，$\mathbf{x}_i = (x_{1i}, x_{2i})'$ 和 $\mathbf{x}'_i \mathbf{x}_i = x_{1i}^2 + x_{2i}^2$，且

$$
K(\mathbf{x}_i, \mathbf{x}_i) = (1 + x_{1i}^2 + x_{2i}^2)^2 = (1 + 2x_{1i}^2 + 2x_{2i}^2 + 2x_{1i}^2 x_{2i}^2 + x_{1i}^4 + x_{2i}^4).
$$

这个内积包括原始变量中 $d=2$ 次多项式的所有项。请注意，$K(\mathbf{x}_i, \mathbf{x}_i)$ 是向量 $\boldsymbol{h}(\mathbf{x}_i) = (1, \sqrt{2}x_{1i}, \sqrt{2}x_{2i}, \sqrt{2}x_{1i}x_{2i}, x_{1i}^2, x_{2i}^2)$ 的平方范数，其中 $\boldsymbol{h}(\mathbf{x}_i) \in \mathbf{R}^6$。一般来说，$K(\mathbf{x}_i, \mathbf{x}_j)$ 给出了向量 $\boldsymbol{h}(\mathbf{x}_i)$ 和 $\boldsymbol{h}(\mathbf{x}_j)$ 的标量积，包括所有直到 $d$ 阶的项。因此，如果我们使用这个核来增加特征空间的维度，超平面是

$$
f(\boldsymbol{h}(\mathbf{x})) = \boldsymbol{w}' \boldsymbol{h}(\mathbf{x}) + w_0 = \sum_{i=1}^n \lambda_i y_i K(\mathbf{x}_i, \mathbf{x}) + w_0,
$$

其中现在 $\mathbf{x} \in \mathbf{R}^q$ 和 $\boldsymbol{h}(\mathbf{x}) \in \mathbf{R}^q$，且需要求解的优化问题是采用新变量 $\boldsymbol{h}(\mathbf{x}_i)$ 的公式 (5.71)，其形式为

$$
\max F_D = \sum_{i=1}^n \lambda_i - \frac{1}{2} \sum_{i=1}^n \sum_{j=1}^n \lambda_i \lambda_j y_i y_j K(\mathbf{x}_i, \mathbf{x}_j),
$$

$$
C \ge \lambda_i \ge 0, \quad \mu_i \ge 0, \quad \sum_{i=1}^n \lambda_i y_i = 0.
$$

其他可用的内积核函数包括基于径向基、样条、傅里叶系数、Sigmoid 等的核函数（Cherkassky and Mulier, 1998）。

