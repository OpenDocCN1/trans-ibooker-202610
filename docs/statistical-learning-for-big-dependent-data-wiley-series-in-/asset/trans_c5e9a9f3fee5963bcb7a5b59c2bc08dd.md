
对所有 99 个时间序列。此命令汇总了命令 arimaSpec 的结果。

这三个命令使用 R 的内置命令 `arima` 进行时间序列估计，它为给定的时间序列估计指定的 ARIMA 模型。因此，我们简要说明 R 的 `arima` 命令。该命令执行 ARIMA 模型的 ML 估计。必须给出模型的阶数。对于常规 ARIMA 模型，阶数由子命令 `order=c (p, d, q)` 指定。对于季节性 ARIMA 模型，还需要使用以下子命令指定模型的季节部分：

```
seasonal=list(order=c(P,D,Q),period=s) .
```

关于 `arima` 命令需要提及三个重要问题。首先，该命令使用略有不同的模型公式。例如，ARIMA(1,0,1) 模型写作

(1 - \phi_1 B)(z_t - \mu) = (1 + \theta_1 B)a_t.

因此，该命令的 MA 参数输出是书中使用的 MA 系数的负值。也就是说，MA 系数涉及符号变化。其次，均值 \(\mu\) 在 `arima` 命令的输出中称为 *截距*。第三，如果 d = 1 或 D = 1，`arima` 命令自动将 \(\mu\) = 0。因此，当数据中存在漂移时，使用 `arima` 命令必须小心。

接下来，我们使用示例 2.5 中世界股票指数的第一系列来演示所提议的自动建模过程。R 命令如下：

```
> lx=log(x)
> out1=arimaSpec(lx[,1])
include.mean: FALSE
Selected order: (p,d,q):  1 1 1
minimum criterion:  -24707.47
> out2=arima(lx[,1],order=c(1,1,1)) ## R estimation
> out2
Call: arima(x = lx[, 1], order = c(1, 1, 1))
Coefficients:
          ar1      ma1
      0.4847  -0.5686
s.e.  0.1430   0.1352

sigma^2 estimated as 0.0001534: log likelihood=12370.39,aic=-24734.78
```

```
> out3=arima(lx[,1],order=c(3,1,2)) ## Try another model
> out3
Call:
arima(x = lx[, 1], order = c(3, 1, 2))

Coefficients:
          ar1      ar2      ar3      ma1      ma2
      0.3428   0.2127   0.0291  -0.4277  -0.2305
s.e.   0.3831   0.2038   0.0217   0.3831   0.2255

sigma^2 estimated as 0.0001533: log likelihood=12371.53,aic=-24731.06
> p1 = c(1,-c(out3$coef[1:3])) ## AR polynomial
> q1 = c(1,-c(out3$coef[4:5])) ## MA polynomial
> polyroot(p1)
[1] 1.420945+0.000000i -4.359908+2.268511i -4.359908-2.268511i
> polyroot(q1)
[1] -0.927846+1.865029i -0.927846-1.865029i
```

```
#The best model selected by arimaSpec is ARIMA(1,1,1). The fitted model is
# (1-0.48B) (1-B) z(t) = (1-0.57B)a(t). Also, a more complicated
# ARIMA(3,1,1) is estimated, the coefficients are not significant at
# the 5% level, indicating over fitting. The roots of the AR and MA
# polynomials are shown. Next, the output of the program SummaryModel is
# presented
#> out1=SummaryModel(lx)
```

所有序列均为非平稳
经过一阶差分的序列数量：99
d=1

| | q=0 | q=1 | q=2 | q=3 |
|---|---|---|---|---|
| p=0 | 18 | 4 | 0 | 0 |
| p=1 | 18 | 3 | 2 | 0 |
| p=2 | 14 | 2 | 3 | 0 |
| p=3 | 6 | 0 | 2 | 0 |
| p=4 | 1 | 0 | 2 | 0 |
| p=5 | 19 | 1 | 3 | 1 |

结果表明，所有对数股票指数系列均需要一阶差分，且大多数差分后的系列要么是白噪声（18/99=18%），要么采用低阶模型。每个时间序列的模型具体信息作为命令 SummaryModel 的矩阵 M2 的输出给出。此信息未打印，但我们可以使用 SelectedSeries 找出遵循各模型的系列，SelectedSeries 获取 SummaryModel 输出中矩阵 M2 的信息，并为每个模型给出系列，如下所示。

```
> SelectedSeries(out1$M2,c(1,1,0))
There are 18 of order: 1 1 0
Names of the series:
[1] "x21" "x33" "x41" "x47" "x49" "x50" "x51" "x52" "x59" "x60"
"x62" "x64" "x67" "x71" "x79" "x89" "x93" "x98"
```

```
> SelectedSeries(out1$M2,c(0,1,1))
There are 4 of order: 0 1 1
Names of the series:
[1] "x56" "x87" "x92" "x99"
```

```
> SelectedSeries(out1$M2,c(5,1,2))
There are 3 of order: 5 1 2
Names of the series:
[1] "x23" "x38" "x77"
```

## 2.9 诊断检验

诊断检验是对拟合时间序列模型进行某些残差分析。其目的是验证建模过程中的假设没有出错的迹象。如果发现任何缺陷，它还提供了模型改进或完善的机会。检验不仅关注估计参数，还关注拟合模型的残差。理想情况下，参数估计应具有统计显著性，残差应形成白噪声系列，且拟合模型对于研究的案例应具有意义。对于拟合的 ARIMA 模型，AR 和 MA 多项式之间不应有公因式。此外，AR 和 MA 多项式不应包含单位圆内的根。我们将讨论诊断检验中常用的一些有用工具。

### 2.9.1 残差图

标准化残差 \(\hat{a}_t/\hat{\sigma}_a^2\) 的时间图在模型检验中非常重要。该图可用于 (a) 识别异常观测值，如幅值大于 3.5 的标准化残差，(b) 检查结构性断点，(c) 验证方差齐性，以及 (d) 检验序列相关性。理想情况下，标准化残差应呈随机分布，均值为零且方差恒定。它们还应大体介于 -3.5 和 3.5 之间。在第 4 章中，我们将学习如何分析残差以识别异常值和结构性断点，并修正模型以消除其影响。

### 2.9.2 残差序列相关性的箱-皮尔森检验

指定良好的 ARIMA 模型的残差应形成白噪声系列。为此，最广泛使用的模型检验统计量是使用残差自相关函数（ACF）的箱-皮尔森检验。设 h 为正整数，感兴趣的原假设是 \(H_0 : \rho_1 = \cdots = \rho_h = 0\) 与备择假设 \(H_a : \rho_i \neq 0\) 对某些 \(i \in \{1, ..., h\}\) 相对立，其中 \(\rho_i\) 是残差的滞后-i ACF。设 \(\hat{r}_j\) 为残差的滞后-j 样本 ACF。如果残差没有序列相关性，则 \(\hat{r}_j\) 渐近正态分布，均值为零，方差为 \((T-j)/T(T+2)\)，其中 T 是样本量。因此，\(H_0\) 的箱-皮尔森检验统计量为

```
Q(h) = T(T + 2) \sum_{j=1}^h \frac{\hat{r}_j^2}{T - j}, \quad (2.109)
```

在 \(H_0\) 下，它渐近服从自由度为 \(h-m\) 的 \(\chi^2_{h-m}\) 分布，其中 m 是模型中使用的参数个数。因此，如果 \(Q(h) > \chi^2_{h-m}(\alpha)\)，则拒绝原假设 \(H_0\)，其中 \(\chi^2_{h-m}(\alpha)\) 是自由度为 \(h-m\) 的卡方分布的 \(100(1-\alpha)\) 分位数。通常使用 \(\alpha = 0.05\) 或 0.01。箱-皮尔森检验统计量的稳健版本是将 \(\hat{r}_j\) 替换为基于秩的对应量。参见 Tsay (2010) 及其参考文献。

式 (2.109) 中的检验统计量存在将所有残差 ACF 视为等同的缺点。在实践中，有人可能会认为某些滞后阶的 ACF 比其他阶更重要。例如，在季节性模型中，如果 \(s > 2\)，则 \(\hat{r}_s\) 可能比 \(\hat{r}_2\) 更重要。Peña 和 Rodriguez (2002, 2006) 提出了一个更强大的检验统计量来缓解这一缺点。该检验统计量基于残差的自相关矩阵：

```
\(R_m = \begin{bmatrix} 1 & \hat{r}_1 & \cdots & \hat{r}_{m-1} \\ \hat{r}_1 & 1 & \cdots & \hat{r}_{m-2} \\ \vdots & \vdots & \ddots & \vdots \\ \hat{r}_{m-1} & \hat{r}_{m-2} & \cdots & 1 \end{bmatrix}\)
```

并定义为

$$D_m = -\frac{T}{m+1} \ln |\hat{\mathbf{R}}_m|,$$

其渐近服从参数为

$$\alpha = \frac{3(m+1)\{m-2(p+q)\}^2}{2\{2m(2m+1) - 12(m+1)(p+q)\}},

\beta = \frac{3(m+1)\{m-2(p+q)\}}{2m(2m+1) - 12(m+1)(p+q)}.$$

的伽马分布。该分布的均值为 $\alpha/\beta = (m+1)/2 - (p+q)$，方差为：

$$\alpha/\beta^2 = (m+1)(2m+1)/3m - 2(p+q).$$

检验统计量 $D_m$ 的百分位数可通过使用上述公式计算伽马分布的参数轻松获得。或者，变量

$$ND_m^* = (\alpha/\beta)^{-1/4}(4/\sqrt{\alpha}) \left[ (D_m)^{1/4} - (\alpha/\beta)^{1/4} \left( 1 - \frac{1}{6\alpha} \right) \right], \quad (2.110)$$

近似服从标准正态分布。

可以证明，$D_m$ 统计量可写为

$$D_m = T \sum_{i=1}^m \frac{(m+1-i)}{(m+1)} \hat{\kappa}_i^2, \quad (2.111)$$

其中 $\hat{\kappa}_i$ 是残差的滞后-$i$ 偏自相关系数。因此，该检验可视为修正的 Ljung-Box 检验，其中我们不使用残差的样本 ACF，而是使用带有权重 $\frac{(m+1-i)}{m}$ 的残差 PACF。这些权重随滞后阶数线性递减，使得 $\hat{\kappa}_1^2$ 的权重为 1，$\hat{\kappa}_m^2$ 的权重为 $1/m$。

### 2.9.3 方差齐性检验

为检验方差齐性，可以将残差分为两部分，并对得到的两个方差应用传统的 $F$-检验。具体地，

$$F = \frac{\sum_{t=1}^{n_1} \hat{a}_t^2 / n_1}{\sum_{t=n_1+1}^T \hat{a}_t^2 / (T-n_1)} = \frac{s_1^2}{s_2^2},$$

其中 $n_1$ 约为 $T/2$。在方差齐性的原假设下，$F$ 统计量渐近服从自由度为 $n_1$ 和 $T - n_1$ 的 $F$ 分布。类似地，可以将残差分为 $h$ 个子样本，大小分别为 $n_1, \dots, n_h$，并计算检验统计量

$$\lambda = T \log \hat{\sigma}^2 - \sum_{i=1}^h n_i \log s_i^2,$$

其中 $\hat{\sigma}^2$ 是整个样本的残差方差，$s_i^2$ 是包含 $n_i$ 个观测值的第 $i$ 个子样本的方差。在所有子样本方差相同的假设下，检验统计量 $\lambda$ 渐近服从自由度为 $h-1$ 的卡方分布。有几种可用于检验条件异方差性的检验。例如，参见 Tsay (2010, chapter 3)。
