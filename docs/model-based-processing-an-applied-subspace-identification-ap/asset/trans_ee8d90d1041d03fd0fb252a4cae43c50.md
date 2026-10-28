

## 附录 A 概率与统计概述

如果我们将正面赋值为 1，背面赋值为 0，则随机变量 $X$ 执行映射

$$X(\omega = H) = x(H) = 1,\qquad X(\omega = T) = x(T) = 0$$

其中 $x(\cdot)$ 被称为随机变量 $X$ 的样本值或实现。

概率质量函数以随机变量为基础定义，即

$$P_X(x_i) = \Pr(X(\omega_i) = x_i) \tag{A.2}$$

而概率分布函数定义为

$$F_X(x_i) = \Pr(X(\omega_i) \le x_i) \tag{A.3}$$

它们之间的关系为

$$p_X(x) = \sum_i P_X(x_i)\,\delta(x - x_i) \tag{A.4}$$

$$F_X(x) = \sum_i P_X(x_i)\,\mu(x - x_i) \tag{A.5}$$

其中 $\delta$ 和 $\mu$ 分别为单位脉冲和阶跃函数。

可以很容易地证明分布函数是一个单调递增函数（详见 Ref. [1]），满足以下性质：

$$\lim_{x_i \to -\infty} F_X(x_i) = 0$$

以及

$$\lim_{x_i \to \infty} F_X(x_i) = 1$$

这些性质可用于证明质量函数满足

$$\sum_i P_X(x_i) = 1$$

分布函数或概率质量函数都可以完整描述随机变量的性质。给定其中任一函数，我们可以计算随机变量取实数线上任意事件集合的值的概率。为了完成我们的抛硬币示例，如果我们将正面出现的概率定义为 $p$，则可以按以下示例所示计算分布和质量函数。

### 示例 A.3

考虑抛硬币实验并计算相应的质量和分布函数。由前一示例可知，

$$P_X(x) = \begin{cases} 1 - p & x = 0 \\ p & x = 1 \\ 0 & \text{otherwise} \end{cases}$$

![](img/4bb66282061cdabd9d492d7740bb0233_488_0.png)

![](img/4bb66282061cdabd9d492d7740bb0233_488_1.png)

**图 A.1.** 抛硬币实验的概率质量和分布函数。

我们有

- 样本空间：$\Omega = \{H, T\}$
- 事件：$B = \{\emptyset, \{H\}, \{T\}\}$
- 概率：$P_X(x_1 = H) = p$，$P_X(x_0 = T) = 1 - p$
- 随机变量：$X(\omega_1 = H) = x_1 = 1$，$X(\omega_2 = T) = x_2 = 0$
- 分布：
$$F_X(x_i) = \begin{cases} 1 & x_i \ge 1 \\ 1 - p & 0 \le x_i \le 1 \\ 0 & x_i < 0 \end{cases}$$

该示例的质量和分布函数如图 A.1 所示。注意质量函数值的总和必须为 1，且分布函数的最大值必须为 1，满足前面提到的性质。

如果我们将随机变量扩展为时间的函数，则可以定义随机过程，如第 2 章所述。更正式地，随机或随机过程是 $t$ 和 $\omega$ 的二维函数：

$$X(t, \omega), \qquad \omega \in \Omega,\ t \in T \tag{A.6}$$

其中 $T$ 是索引参数的集合（连续或离散），$\Omega$ 是样本空间。

我们列出概率论中的一些主要定理，并将读者引向更详细的文献 [1, 2]。

- 单变量：$\Pr(X) = P_X(x)$
- 双变量：$\Pr(X, Y) = P_{XY}(x, y)$
- 边际：$\Pr(X) = \sum_y P_{XY}(x, y)$
- 独立：$\Pr(X, Y) = P_X(x)P_Y(y)$
- 条件：$\Pr(X \mid Y) = P_{XY}(x, y) / P_Y(y)$
- 链式法则：$\Pr(X, Y, Z) = \Pr(X \mid Y, Z)\Pr(Y \mid Z)\Pr(Z)$

对于随机变量，我们可以根据概率质量函数定义基本统计量。随机变量 $X$ 的 *期望值* 或 *均值* 由下式给出

$$m_x = E\{X\}$$

并被视为给定数据集的典型或代表性值。因此，均值被称为集中趋势的度量。数值数据围绕期望值扩散的程度通常由 *方差* 或等价地 *自协方差* 衡量，给定为

$$R_{xx} = E\{(X - m_x)^2\}$$

这些基本统计量被称为 *集合统计量*，因为它们是在集合（$i = 1, 2, \ldots$）的数据值上测量的，即期望总是假设在实现集合上。我们总结这些统计量与其质量函数的表达式如下

| Statistic | Definition | Mass-function expression |
|---|---|---|
| Expected value | $m_x = E\{X\}$ | $= \sum_i X_iP_X(x_i)$ |
| Nth moment | $E\{X^n\}$ | $= \sum_i X_i^nP_X(x_i)$ |
| Nth moment about the mean | $E\{(X - m_x)^n\}$ | $= \sum_i (X_i - m_x)^nP_X(x_i)$ |
| Mean squared ($N = 2$) | $E\{X^2\}$ | $= \sum_i X_i^2P_X(x_i)$ |
| Variance | $R_{xx} = E\{(X - m_x)^2\}$ | $= \sum_i (X_i - m_x)^2P_X(x_i)$ |
| Covariance | $R_{xy}$ | $= E\{(X_i - m_x)(Y_i - m_y)\}$ |
| Standard deviation | $\sigma_{xx}$ | $= \sqrt{R_{xx}}$ |
| Conditional mean | $E\{X \mid Y\}$ | $= \sum_i X_iP_X(x_i \mid Y)$ |
| Joint conditional mean | $E\{X \mid Y, Z\}$ | $= \sum_i X_iP_X(x_i \mid Y, Z)$ |
| Conditional variance | $R_{x \mid y}$ | $= E\{(X - E\{X \mid Y\})^2 \mid Y\}$ |

这些基本统计量具有各种性质，使其可用于分析随机变量上的运算；其中较重要的¹有

- 线性：$E\{ax + b\} = aE\{x\} + b = am_x + b$
- 独立性：$E\{xy\} = E\{x\}E\{y\}$
- 方差：$R_{xx}(ax + b) = a^2R_{xx}$
- 协方差：
  - 不相关：$E\{xy\} = E\{x\}E\{y\}$ ($R_{xy} = 0$)
  - 正交：$E\{xy\} = 0$

注意期望值运算意味着对于随机过程，这些基本统计量是 *跨* 集合计算的。例如，如果我们想计算过程的均值，即

$$m_x(t) = E\{X(t, \omega_i)\}, \qquad X(t, \omega_i) = x_i(t)$$

我们只需取 $t = 0, 1, \ldots$ 的值并对每个时间值在集合（$i = 1, 2, \ldots$）上计算均值。处理随机过程与处理随机变量类似，只是我们必须考虑时间索引（详见第 2 章）。

接下来让我们定义随机变量所包含的概率信息的一些概念。我们将随机变量 $X(\omega_i) = x_i$ 出现所包含的（自）*信息* 定义为

$$I(x_i) = -\log_b P_X(x_i) \tag{A.7}$$

其中 $b$ 是对数的底，导致信息度量的单位不同（底 $= 2 \rightarrow$ 位），而 $X(\omega_i)$ 的 *熵* 或 *平均信息* 定义为

$$H(x_i) = -E\{I(x_i)\} = \sum_i P_X(x_i)\log_b P_X(x_i) \tag{A.8}$$

考虑存在多个随机变量的情况。然后我们将 $N$ 维随机变量的 *联合* 质量和分布函数定义为

$$P_X(x_1, \ldots, x_N), \qquad F_X(x_1, \ldots, x_N)$$

所有基本统计定义保持不变，只是将标量替换为联合函数。显然，如果我们把随机过程看作有序随机变量的序列，那么我们就是处理联合概率函数，即一组时间索引的随机变量。假设我们有两个随机变量，$x_1$ 和 $x_2$，并且我们知道后者已经取了一个特定值；那么我们可以定义在 $X(\omega_2) = x_2$ 已发生条件下 $x_1$ 的 *条件* 概率质量函数为

$$\Pr(x_1 \mid x_2) := P_X(X(\omega_1) = x_1 \mid X(\omega_2) = x_2) \tag{A.9}$$

并且可以从基本概率公理证明（见 Ref. [1]）

$$\Pr(x_1 \mid x_2) = \frac{\Pr(x_1, x_2)}{\Pr(x_2)} \tag{A.10}$$

注意该表达式也可以写成

$$\Pr(x_1, x_2) = \Pr(x_2 \mid x_1)\Pr(x_1) \tag{A.11}$$

将该方程代入式 (A.10) 得到贝叶斯法则，即

$$\Pr(x_1 \mid x_2) = \frac{\Pr(x_2 \mid x_1)\Pr(x_1)}{\Pr(x_2)} \tag{A.12}$$

如果我们使用联合质量函数的定义并代入前面的定义，则可以得到概率链式法则 [1–3]，

$$\Pr(x_1, \ldots, x_N) = \Pr(x_1 \mid x_2, \ldots, x_N)\Pr(x_2 \mid x_3, \ldots, x_N)\cdots\Pr(x_{N-1} \mid x_N)\Pr(x_N) \tag{A.13}$$

与这些定义一起出现的是条件期望的概念，即

$$E\{x_i \mid x_j\} = \sum_i X_i\Pr(x_i \mid x_j) \tag{A.14}$$

在条件期望定义后，我们列出其基本性质：

- 1) $E_X\{X \mid Y\} = E\{X\}$，如果 $X$ 和 $Y$ 独立
- 2) $E\{X\} = E_Y\{E[X \mid Y]\}$
- 3) $E_X\{g(Y)X \mid Y\} = g(Y)E\{X \mid Y\}$
- 4) $E_{X,Y}\{g(Y)X\} = E_Y\{g(Y)E\{X \mid Y\}\}$
- 5) $E_X\{c \mid Y\} = c$
- 6) $E_X\{g(Y) \mid Y\} = g(Y)$
- 7) $E_{X,Y}\{cX + dY \mid Z\} = cE\{X \mid Z\} + dE\{Y \mid Z\}$

信息和熵的概念也可以扩展到多个随机变量的情况。我们将两个随机变量 $x_i$ 和 $x_j$ 之间的 *互信息* 定义为

$$I(x_i; x_j) = \log_b\left[\frac{P_X(x_i \mid x_j)}{P_X(x_i)}\right] \tag{A.15}$$

而 $X(\omega_i)$ 和 $X(\omega_j)$ 之间的平均互信息为

$$I(X_i; X_j) = E_{X_i,X_j}\{I(x_i, x_j)\} = \sum_i\sum_j P_X(x_i, x_j)I(x_i, x_j) \tag{A.16}$$

这导致联合熵的定义为

$$H(X_i; X_j) = -\sum_i\sum_j P_X(x_i, x_j)\log_b P_X(x_i, x_j) \tag{A.17}$$

至此概率论部分结束。接下来让我们考虑一个重要的多变量分布及其性质。

