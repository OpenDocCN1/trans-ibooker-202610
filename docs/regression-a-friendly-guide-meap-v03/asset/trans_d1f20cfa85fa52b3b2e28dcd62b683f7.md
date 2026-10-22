

由于试验是独立的，总体概率是个体结果概率的乘积：

P(Y = 0) ≈ (1 − Π)^n   and   P(Y = n) ≈ Π^n.

类似地，在前 k 次尝试中取得成功而在剩余的 n − k 次尝试中失败的概率为 Π^k * (1 − Π)^(n − k)。事实上，这也是 k 次成功和 n − k 次失败的任意特定顺序的概率；剩下的就是要确定这些成功和失败可以重新排列的方式数。把这组成功和失败看作 n 张纸条，其中 k 张写有 1，n − k 张写有 0。忽略 0 和 1，有 n * (n − 1) * (n − 2) * ... * 2 * 1 = n! 种排列这些纸条的方式，因为有 n 种选择第一张纸条的方式，n − 1 种选择第二张纸条的方式，依此类推。对于纸条的任何特定排列，重新排列 1 或 0 实际上不会改变任何东西，所以我们必须考虑大量相同的排列。由于对于纸条的特定排列，1 的排列数为 k!，0 的排列数为 (n − k)!，成功和失败的不同排列总数为

$$\frac{n!}{k!(n-k)!}\,.$$

这个量是*二项式系数* $\binom{n}{k}$；按惯例，

$0! = 1$, 所以 $\binom{n}{0} = \binom{n}{n} = 1$. 把这一切放在一起，我们发现

$P(Y = k) = \binom{n}{k} \pi^{k} (1 - \pi)^{n - k}, \quad \text{for } 0 \le k \le n.$

这个*概率分布*定义了二项随机变量 $Y \sim B(n, p)$，并且不出所料，被称为

*二项分布*，参数为 n 和 p。当只有一次试验时，$n = 1$，我们得到一个成功概率为 $p$ 的伯努利随机变量。

每当你遇到一个新的概率分布时，你都想更好地了解它——具体来说，你想知道它的平均值（即期望值）和方差。暴力法是一种计算二项随机变量平均值的方法，但这不必要地困难且丑陋。相反，我们将利用一个巧妙的技巧：二项随机变量 $Y \sim B(n, p)$ 是 $n$ 个独立

伯努利随机变量之和，每个都对应于实验中的一次试验。因此，$Y$ 的平均值只是伯努利变量平均值之和，所以 $E(Y)=n\pi$。使用同样的技巧，$Y$ 的方差为 $Var(Y)=n\pi(1-\pi)$，其标准差为 $\sqrt{n\pi(1-\pi)}$。当概率 $p$ 接近 0.5 或试验次数 $n$ 较大时，二项分布大致呈正态，因此 $B(n,p)\approx N\left(n\pi,\sqrt{n\pi(1-\pi)}\right)$。当我们绘制 $B(20,.5)$ 和 $B(250,.1)$ 时，例如，将近似的正态分布叠加在每个之上（以洋红色显示）就可以很容易地看出这一点：

```
fig, (ax1, ax2) = plt.subplots(1, 2)

x = np.linspace(0,20,num=1000)
ax1.plot(x, st.norm.pdf(x,loc=10,scale=np.sqrt(5)), color='m', lw
x = np.linspace(0,20,num=21)
ax1.plot( x, st.binom.pmf(x,n=20,p=0.5), 'bo')
ax1.vlines(x, 0, st.binom.pmf(x, n=20, p=0.5), color='b', lw=3, a
ax1.set_xlabel("Number of successes")# ax1.ylabel("Probability")
ax1.set_ylabel("Probability")
ax1.set_title("Binomial distribution: n=20, p=0.5")

x = np.linspace(0,50,num=1000)
ax2.plot(x, st.norm.pdf(x,loc=25,scale=np.sqrt(22.5)), color='m',
```

```
x = np.linspace(0,50,num=51)
ax2.plot( x, st.binom.pmf(x,n=250,p=0.1), 'bo')
ax2.vlines(x, 0, st.binom.pmf(x, n=250, p=0.1), color='b', lw=2,
ax2.set_xlabel("Number of successes")
ax2.set_title("Binomial distribution: n=250, p=0.1")
```

图 5.1. 当 n 较大或 p,p50% 时，B(n,p) 接近正态

![](img/f25e2c651d2b8852d1c723f946646ff0_182_0.png)

另一方面，当试验次数较少或成功概率接近 0 或 1 时，二项分布就不能很好地用正态分布近似。例如，这里是

分布 $B(6, .3)$ 和 $B(20, .9)$：

```
fig, (ax1, ax2) = plt.subplots(1, 2)

x = np.linspace(0,6,num=1000)
ax1.plot(x, st.norm.pdf(x,loc=1.8,scale=np.sqrt(1.122)), color='m'
x = np.linspace(0,6,num=7)
ax1.plot( x, st.binom.pmf(x,n=6,p=0.3), 'bo')
ax1.vlines(x, 0, st.binom.pmf(x, n=6, p=0.3), color='b', lw=3, al
ax1.set_xlabel("Number of successes")# ax1.ylabel("Probability")
ax1.set_ylabel("Probability")
ax1.set_title("Binomial distribution: n=6, p=0.3")

x = np.linspace(0,20,num=1000)
ax2.plot(x, st.norm.pdf(x,loc=18,scale=np.sqrt(1.342)), color='m'
x = np.linspace(0,20,num=21)
ax2.plot( x, st.binom.pmf(x,n=20,p=0.9), 'bo')
ax2.vlines(x, 0, st.binom.pmf(x, n=20, p=0.9), color='b', lw=2, a
ax2.set_xlabel("Number of successes")
ax2.set_title("Binomial distribution: n=20, p=0.9")
```

图 5.2. 当 n 较小时或 p 远离 50% 时，B(n,p) 不大致呈正态

![](img/f25e2c651d2b8852d1c723f946646ff0_184_0.png)

尽管正态分布对这些拟合得并不好，但它们也不算*糟糕*。无论如何，二项分布与正态分布之间的关系在许多应用中是基础性的，正如我们在第 5.3 节中将看到的那样。

## 5.2.2 二项逻辑回归与偏差的再讨论

在许多实验和观察性研究中，受试者可以根据预测变量的值进行分组。这些预测值集合中的每一个都称为*协变量类*或*协变量模式*，并且每个 resulting 组都可以被视为一次二项实验。对于

2009 年 NAMCS 数据集，例如，我们可以将特定年龄的所有患者视为一个协变量类，从而产生 101 个这样的组：

```
cohorts = (chronic.groupby("Age", as_index=False)
    .agg(Number = ("Condition","size"),
          Successes = ("Condition","sum"),
          Percentage = ("Condition","mean")))
cohorts.insert(3,"Failures", cohorts["Number"] - cohorts["Success"]
cohorts
```

Age  Number  Successes  Failures  Percentage
0    0       796        21        775        0.026382
1    1       505        32        473        0.063366
2    2       320        29        291        0.090625
3    3       258        37        221        0.143411
4    4       299        30        269        0.100334
...  ...     ...        ...       ...        ...
96   96      13         12        1          0.923077
97   97      11         10        1          0.909091
98   98      6          5         1          0.833333
99   99      2          1         1          0.500000
100  100     6          5         1          0.833333

101 rows x 5 columns

遵循二项实验的术语，至少患有一种慢性病条件的患者是 Successes，而没有慢性病条件的患者是 Failures。Percentage 显然是特定年龄患者患有慢性病条件的百分比；我们在上一章开头看到，基于此数据的逻辑模型很好地拟合了这些年龄组百分比。当我们之前构建该模型时，我们直接使用了二元响应 Condition，因为我们当时使用的是伯努利随机变量。现在，我们将改变方法，直接使用协变量类进行工作：对于每个年龄，Successes 的数量是二项随机变量的一个观测值。为了使这一点精确，我们将使用以下记号：

-   X_i 表示定义第 i 个协变量类的 Age，i 从 1 到 101，
-   Y_i 表示第 i 个协变量类中观察到的 Successes 数量，以及
-   $n_i$ 表示第 $i^{th}$ 协变量类中的患者数量。

理论上——在被观察之前，换句话说——第 $i^{th}$ 协变量类中的成功次数是二项随机变量 $Y_i \sim B\left(n_i, \Pi_i\right)$，其中成功概率 $\Pi_i$ 依赖于预测变量 $X_i$：

$\log\left(\frac{\Pi_i}{1 - \Pi_i}\right) = \alpha + \beta x_i,$

参数 $\alpha$ 和 $\beta$ 必须通过将此模型拟合到可用数据来估计。观测值 $y_i$ 是 $Y$ 的一个实现，而计算出的 Percentage 是 $\Pi_i$ 的经验估计。正如我们之前看到的，拟合的逻辑模型将具有形式

$\log\left(\frac{\hat{\Pi}_i}{1 - \hat{\Pi}_i}\right) = a + b x_i,$

其中回归系数 $a$ 和 $b$ 是

