

### 4. 寻找新的残差：

```
\(\eta_i = \eta_{i-1} - o_i p'_i, \quad f_i = f_{i-1} - o_i q'_i.\)
```

从步骤 1 和 4 可以清楚地看出，该算法在每一步 m 都给出了两种表示

```
\(X = \mu_X + p_1 o'_1 + \cdots + p_m o'_m + \eta_m, \quad Y = \mu_Y + q_1 o'_1 + \cdots + q_m o'_m + f_m. \quad (7.45)\)
```

从最小二乘回归的性质来看，根据步骤 3 和 4 可以清楚地看出，残差 \(\eta_i\) 与 \(o_i\) 不相关，因此得分 \(o_1, \dots, o_m\) 也不相关。此外，可以证明权重 \(w_1, \dots, w_m\) 也不相关。从算法中也可以清楚地看出，**_Y_** 与残差 \(\eta_i\) 之间的线性依赖关系被用来构建得分 **_X_**，然后用于预测 **_Y_**。

最后，有多个 R 包可用于 PLS 回归。包 **pls** 和 **plsdepot** 提供 PLS 分析。**plsdof** 考虑 PLS 的自由度，**plsRglm** 给出广义线性模型的 PLS 分析，而 **plsVarSel** 提供 PLS 的变量选择。

### 示例 7.7

同样，考虑月度宏观数据集，以美国工业生产指数（IPI）的月度增长率作为因变量，所有变量的前六个滞后值作为预测变量，如我们在示例 7.5 和 7.6 中所做的那样。将 PLS 成分的最大数量设置为 30 并使用交叉验证，如果使用观测到的预测变量，则 PLS 回归为该数据选择 _m_ = 5。参见图 7.23。PLS 回归的摘要表明，使用 30 个成分时，模型解释了约 54% 的 IPI 增长率的方差。图 7.24 显示了带有 30 个成分的 PLS 的交叉验证预测。图中的直线表示目标线。该图显示，带有 30 个成分的 PLS 回归提供了合理的拟合。如果对 732 个预测变量进行标准化，带有交叉验证的 PLS 回归会选择一个 PLS 成分。

### PLS 的 R 演示：使用包 pls.

```
> da <- read.csv("FREMDApril19.csv",header=TRUE)
> x <- as.matrix(da)
> Y <- x[7:710,6]
> X <- cbind(x[6:709,],x[5:708,],x[4:707,],x[3:706,],x[2:705,],x[1:704,])
> name <- paste(name,"Y",1:122,sep="")
> n1 <- paste(name,"L1",sep="")
> n2 <- paste(name,"L2",sep="")
> n3 <- paste(name,"L3",sep="")
> n4 <- paste(name,"L4",sep="")
> n5 <- paste(name,"L5",sep="")
> n6 <- paste(name,"L6",sep="")
> colnames(X) <- c(n1,n2,n3,n4,n5,n6)
> require(pls)
> m.plsr <- plsr(Y~.,data=data.frame(X),ncomp=30,validation="CV")
```

![](img/d4c84ae3c2178f4c506af3dd2ed13122_417_0.png)

Figure 7.23 PLS 回归的交叉验证结果。因变量是 IPI 的增长率，预测变量由所有变量的前六个滞后值组成。

![](img/d4c84ae3c2178f4c506af3dd2ed13122_417_1.png)

Figure 7.24 宏观数据集中带有 30 个成分的 PLS 回归的交叉验证预测。因变量是 IPI 的增长率，预测变量由所有变量的前六个滞后值组成。

```
> summary(m.plsr)
Data: X dimension: 704 732
Y dimension: 704 1
Fit method: kernelpls
Number of components considered: 30

VALIDATION: RMSEP
Cross-validated using 10 random segments.
(Intercept) 1 comps 2 comps 3 comps 4 comps 5 comps 6 comps
CV 0.007435 0.006880 0.006851 0.006842 0.006843 0.006837 0.006839
adjCV 0.007435 0.006874 0.006846 0.006837 0.006837 0.006832 0.006839
7 comps 8 comps 9 comps 10 comps 11 comps 12 comps 13 comps
CV       0.006778 0.006642 0.006754 0.006878 0.006935 0.007024 0.007057
adjCV    0.006775 0.006633 0.006724 0.006824 0.006873 0.006945 0.006978
14 comps 15 comps 16 comps 17 comps 18 comps 19 comps 20 comps
CV       0.007126 0.007145 0.007226 0.007299 0.007423 0.007653 0.007956
adjCV    0.007031 0.007041 0.007123 0.007195 0.007301 0.007506 0.007779
21 comps 22 comps 23 comps 24 comps 25 comps 26 comps 27 comps
CV       0.008149 0.008298 0.008255 0.008169 0.008145 0.008042 0.007974
adjCV    0.007962 0.008099 0.008053 0.007982 0.007963 0.007866 0.007796
28 comps 29 comps 30 comps
CV      0.007825 0.007891 0.007988
adjCV   0.007658 0.007724 0.007806

TRAINING: % variance explained
1 comps 2 comps 3 comps 4 comps 5 comps 6 comps 7 comps 8 comps
X      15.26   40.45   49.27   72.16   82.47   98.50   99.97   99.98
Y      16.32   17.08   17.43   17.44   17.48   17.69   19.77   26.04
9 comps 10 comps 11 comps 12 comps 13 comps 14 comps 15 comps
X      99.98   99.98   99.98   99.98   99.99   99.99   99.99
Y      31.61   35.26   36.85   38.88   39.81   41.43   42.81
16 comps 17 comps 18 comps 19 comps 20 comps 21 comps 22 comps
X      99.99   99.99   99.99   99.99   99.99   99.99   99.99
Y      43.50   44.18   45.48   46.61   47.43   48.05   48.63
23 comps 24 comps 25 comps 26 comps 27 comps 28 comps 29 comps
X      99.99   99.99   99.99   99.99   99.99   99.99   99.99
Y      49.38   49.94   50.65   51.37   52.14   52.80   53.28
30 comps
X      99.99
Y      53.97
plot(m.plsr,ncomp=30,asp=1,line=TRUE)
> selectNcomp(m.plsr,"randomization",plot=TRUE)
[1] 5
```

## 7.6 提升法

提升算法最初被提出作为集成方法，即结合多个模型（或方法）的预测以获得最优预测的方法。提升的基本思想是从调整后的数据生成多个预测，然后使用线性组合或多数投票将其聚合，以构建最终的估计器或预测。它类似于模型平均。关于提升的良好参考文献是 Bühlmann 和 van de Geer (2011, 第 12 章) 以及 Hastie 等人 (2009)。Friedman 等人 (2000) 和 Friedman (2001) 发展了一个更一般的统计框架，将提升直接解释为函数估计的统计方法。

给定数据 \(\{(\mathbf{x}_t, y_t) | t = 1, \dots, n\}\)，提升包含两个关键要素。第一个要素称为 *弱学习器*，这是一种易于计算的统计预测方法。弱学习器的示例包括简单回归树或简单线性回归。第二个要素是一种数据调整方法，其中要拟合的变量在每次迭代中进行调整，这种调整通常取决于弱学习器。考虑模型

```
\(y_t = f(\mathbf{x}_t) + \epsilon_t,\)  \(\text{(7.46)}\)
```

其中 \(\epsilon_t\) 是误差项，\(f(.)\) 是感兴趣的未知实值函数。提升算法的工作流程如下：

- 调整后的数据 1 \(\rightarrow\) weak learner \(\hat{f}^{(1)}(.)\)
- 调整后的数据 2 \(\rightarrow\) weak learner \(\hat{f}^{(2)}(.)\)
- \(\vdots \quad \vdots\)
- 调整后的数据 \(r\rightarrow\) weak learner \(\hat{f}^{(r)}(.)\)

其中 \(r\) 是预先指定的正整数。然后，聚合结果为

\(\text{聚合} : \hat{f}_A = \sum_{i=1}^r \alpha_i \hat{f}^{(i)}(.), \quad (7.47)\)

其中 \(\alpha_i\) 是合适的系数或权重。显然，弱学习器的选择、迭代次数以及权重 \(\alpha_i\) 的选择是提升在应用中发挥作用的重要问题。

从数学角度来看，为了估计方程 (7.46) 中的函数 \(f(.)\)，我们通常采用适当的损失函数，并在某个可测函数空间上对其进行最小化。即，

\(f_o(.) = \text{arg min}_{f \in S} E[L(f(X), y)] \quad (7.48)\)

其中 \(S\) 表示一个可测函数空间，\(L(f(.), y)\) 是一个假设对 \(f(.)\) 可微且凸的损失函数。对于平方误差损失，\(L(f(.), y) = (y - f(.))^2\)，相关的总体最小化器是 \(f_o(x) = E[Y|X=x]\)。

Friedman (2001) 提出了一种函数梯度下降（FGD）算法来估计 \(f(.)\)。该算法如下：

### FGD 算法：

- 1. 初始化 \(\hat{f}^{(0)}(.)\)，例如 \(\hat{f}^{(0)}(.) \equiv 0\) 或

\(\hat{f}^{(0)} = \text{arg min}_c \frac{1}{n} \sum_{i=1}^n L(c, y_i).\)

设置 \(r=0\)。

- 2. 将 \(r\) 增加 1，即 \(r \leftarrow r+1\)。通过计算负梯度 \(-\frac{dL(f,y)}{df}\) 并在 \(\hat{f}^{(r-1)}(x_t)\) 处对其求值来生成一组新的响应变量。即，计算

\(v_t = -\frac{\partial L(f, y_t)}{\partial f} |_{f=\hat{f}^{(r-1)}(x_t)}, \quad t=1, \dots, n.\)

- 3. 使用解释变量 \(x_1, \dots, x_n\) 通过弱学习器拟合新的响应变量 \(v_1, \dots, v_n\)，即

\((x_t, v_t) \rightarrow \text{弱学习器 } g^{(r)}(.).\)

- 4. 更新 \(\hat{f}^{(r)}(.) = \hat{f}^{(r-1)}(.) + \delta \times \hat{g}^{(r)}(.),\) 其中 \(0 < \delta \le 1\) 是步长参数。

- 5. 重复步骤 2 到 4，直到 \(r = r_o\)，其中 \(r_o\) 是预先指定的正整数。

