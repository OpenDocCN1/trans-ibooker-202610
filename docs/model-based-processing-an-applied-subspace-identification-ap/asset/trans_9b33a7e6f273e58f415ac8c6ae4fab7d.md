

图3.2 一阶过程的高斯-马尔可夫仿真：(a) 状态/量测均值，(b) 状态及其均值的95%置信区间，(c) 量测及其均值的95%置信区间。

![](img/4bb66282061cdabd9d492d7740bb0233_109_0.png)

![](img/4bb66282061cdabd9d492d7740bb0233_109_1.png)

![](img/4bb66282061cdabd9d492d7740bb0233_109_2.png)

利用上述置信区间，我们预计95%的样本将落在 \((m_x \to 0) \pm 3.03\;(1.96 \times \sqrt{2.286})\) 之内。从图中可以看出，100个样本中仅有2个超出该界限，表明该仿真在统计上是可以接受的。对于仿真量测，我们观察到类似的结果。稳态方差由下式给出

\[
R_{yy}=P+R_{vv}=2.286+4=6.286
\]

因此，在稳态下，我们预计95%的量测样本将落在 \((m_y \to 0) \pm 5.01(1.96 \times \sqrt{6.286})\) 之内。本例到此结束。

应当注意，如果超出界限的样本超过5%（即落在界内的样本不足95%），则必须选择新的随机种子并重新执行仿真，直到满足界限条件，以表明这是一个有效的高斯-马尔可夫仿真。

## 3.6 新息模型

在本节中，我们简要推导与高斯-马尔可夫表示相关的新息模型。该模型的重要性将在全书后续展开，但现在我们借此机会展示它与基本高斯-马尔可夫表示的关系。我们首先将原始的高斯-马尔可夫表示扩展到过程噪声与量测噪声相关的情形，然后说明新息模型如何成为该结构的一个特例。

相关过程噪声与量测噪声的标准高斯-马尔可夫模型由下式给出

\[
x(t)=Ax(t-1)+Bu(t-1)+W(t-1)w^*(t-1)
\]
\[
y(t)=Cx(t)+v^*(t) \tag{3.70}
\]

其中 \(R^*(t,k):=R^*\delta(t-k)\)，且

\[
R^*=
\begin{bmatrix}
R_{ww^*} & R_{wv^*}\\
R_{v^*w^*} & R_{v^*v^*}
\end{bmatrix}
=
\begin{bmatrix}
WR_{ww}W' & WR_{wv}\\
R_{vw}W' & R_{vv}
\end{bmatrix}
\]

这里我们观察到，在标准高斯-马尔可夫模型中，\((N_x+N_v)\times(N_x+N_v)\) 分块协方差矩阵 \(R^*\) 是满的，其非对角线上为互协方差矩阵 \(R_{w^*v^*}\)。通常的标准模型假设它们为零（不相关）。使用这种形式的高斯-马尔可夫模型对具有相关 \(w(t)\) 和 \(v(t)\) 的系统进行仿真更为复杂，因为必须首先对 \(R^*\) 进行因式分解，使得

\[
R^*=
\begin{bmatrix}
R_1^*\\
R_2^*
\end{bmatrix}
\begin{bmatrix}
R_1^{*'} & R_2^{*'}
\end{bmatrix}
\tag{3.71}
\]

其中 \(R_i^*\) 为矩阵平方根 [6, 7]。一旦完成因式分解，就可以通过对不相关噪声源 \(w(t)\) 和 \(v(t)\) 进行“着色”来合成相关噪声，如下所示

\[
\begin{bmatrix}
w^*(t)\\
v^*(t)
\end{bmatrix}
=
\begin{bmatrix}
R_1^*w(t)\\
R_2^*v(t)
\end{bmatrix}
\tag{3.72}
\]

新息模型是相关高斯-马尔可夫描述的一种受约束形式。如果我们假设 \(\{e(t)\}\) 是一个零均值、白色高斯序列，即 \(e \sim \mathcal{N}(0,R_{ee})\)，则*新息模型* [11–15] 可表示为

\[
x(t)=A(t-1)x(t-1)+B(t-1)u(t-1)+K(t-1)e(t-1)
\]
\[
y(t)=C(t)x(t)+e(t)
\tag{3.73}
\]

其中 \(e(t)\) 是 \(N_y\) 维新息向量，\(K(t-1)\) 是 \((N_x \times N_y)\) 加权矩阵，新息协方差由下式给出

\[
R_{ee}^* := \operatorname{cov}\left(
\begin{bmatrix}
Ke(t)\\
e(t)
\end{bmatrix}
\right)
=
\begin{bmatrix}
KR_{ee}K' & KR_{ee}\\
R_{ee}K' & R_{ee}
\end{bmatrix}
\delta(t-k)
\]

重要的是要注意，新息模型在维纳-卡尔曼滤波（谱分解）中具有重要意义，因为 \(R_{ee}^*\) 可以直接用权重和新息协方差矩阵表示为*因式分解*或平方根形式 \((R:=\sqrt{R}\sqrt{R}')\)，即

\[
R_{ee}^* :=
\begin{bmatrix}
K\sqrt{R_{ee}}\\
\sqrt{R_{ee}}
\end{bmatrix}
\begin{bmatrix}
\sqrt{R_{ee}}K' & \sqrt{R_{ee}}
\end{bmatrix}
\delta(t-k)
\tag{3.74}
\]

将新息模型与高斯-马尔可夫模型进行比较，我们看到它们都等价于 \(w\) 和 \(v\) 相关的情形。接下来，我们证明各种模型集与这一族状态空间表示的等价性。

## 3.7 状态空间模型结构

本节讨论文献中通常称为“标准形”的特殊状态空间结构，因为它们代表了特别有用的独特状态构造。我们将模型限制为SISO形式，而更复杂的多变量结构将在第6章中展开。这里我们首先回顾第2章中带外生输入的自回归滑动平均（ARMAX）模型，然后给出其在状态空间形式中的等价表示。

### 表3.2 ARMAX表示

| 传播 | 方程 |
|---|---|
| 输出传播 | \(y(t) = (1 - A(q^{-1}))y(t) + B(q^{-1})u(t) + C(q^{-1})e(t)\) |
| 均值传播 | \(m_y(t) = (1 - A(q^{-1}))m_y(t) + B(q^{-1})u(t) + C(q^{-1})m_e(t)\) |
| 脉冲传播 | \(h(t) = (1 - A(q^{-1}))h(t) + C(q^{-1})\delta(t)\) |
| 方差/协方差传播 | \(R_{yy}(k) = R_{ee}\sum_{i=0}^{\infty} h(i)h(i+k), \quad k \ge 0\) |

| 变量 | 描述 |
|---|---|
| \(y\) | 输出或量测序列 |
| \(u\) | 输入序列 |
| \(e\) | 方差为 \(R_{ee}\) 的过程（白色）噪声序列 |
| \(h\) | 脉冲响应序列 |
| \(\delta\) | 幅度为 \(\sqrt{R_{ee}}\) 的脉冲输入 |
| \(m_y\) | 均值输出或量测序列 |
| \(m_e\) | 均值过程噪声序列 |
| \(R_{yy}\) | 滞后 \(k\) 处的平稳输出协方差 |
| \(A\) | \(N_a\) 阶系统特征（极点）多项式 |
| \(B\) | \(N_b\) 阶输入（零点）多项式 |
| \(C\) | \(N_c\) 阶噪声（零点）多项式 |

下面给出一个示例，展示如何应用这些模型来实现一个随机信号。首先，我们考虑ARMAX到状态空间的变换。

回顾式(3.77)，ARMAX模型的一般差分方程形式由下式给出

\[
y(t) = -\sum_{i=1}^{N_a} a_i y(t-i) + \sum_{i=0}^{N_b} b_i u(t-i) + \sum_{i=0}^{N_c} c_i e(t-i)
\tag{3.78}
\]

或等价地，在频域中表示为

\[
Y(z) = \left( \frac{b_0 + b_1 z^{-1} + \cdots + b_{N_b} z^{-N_b}}{1 + a_1 z^{-1} + \cdots + a_{N_a} z^{-N_a}} \right) U(z) + \left( \frac{c_0 + c_1 z^{-1} + \cdots + c_{N_c} z^{-N_c}}{1 + a_1 z^{-1} + \cdots + a_{N_a} z^{-N_a}} \right) E(z)
\tag{3.79}
\]

其中 \(N_a \ge N_b\)、\(N_a \ge N_c\)，且 \(\{e(t)\}\) 是零均值白色序列，其频谱由 \(R_{ee}\) 给出。

可以直接证明（参见 [5]），ARMAX模型可以表示为*观测器标准形*：

\[
x(t) = A_0 x(t-1) + B_0 u(t-1) + W_0 e(t-1)
\]
\[
y(t) = C_0' x(t) + b_0 u(t) + c_0 e(t)
\tag{3.80}
\]

其中 \(x\)、\(u\)、\(e\) 和 \(y\) 分别为 \(N_a\) 维状态向量、标量输入、噪声和输出，且

\[
A_0 :=
\begin{bmatrix}
0 & \cdots & 0 & -a_{N_a}\\
1 & \cdots & 0 & -a_{N_a-1}\\
\vdots & \ddots & \vdots & \vdots\\
0 & \cdots & 1 & -a_1
\end{bmatrix}
\]

\[
B_0 :=
\begin{bmatrix}
-a_{N_a}b_0\\
\vdots\\
-a_{N_b+1}b_0\\
b_{N_b}-a_{N_b}b_0\\
\vdots\\
b_1-a_1b_0
\end{bmatrix}
\]

\[
W_0 :=
\begin{bmatrix}
-a_{N_a}c_0\\
\vdots\\
-a_{N_c+1}c_0\\
c_{N_c}-a_{N_c}c_0\\
\vdots\\
c_1-a_1c_0
\end{bmatrix}
\]

\[
C_0' :=
\begin{bmatrix}
0 & \cdots & 0 & 1
\end{bmatrix}
\]

注意该结构，我们看到每个矩阵或向量元素 \(\{A_{i,N_a}, B_i, W_i, C_i\}\)（\(i = 1, \ldots, N_a\)）都可以由以下关系确定

\[
\begin{aligned}
A_{i,N_a} &= -a_i \quad i = 1, \ldots, N_a\\
B_i &= b_i - a_i b_0\\
W_i &= c_i - a_i c_0\\
C_i &= \delta(N_a - i)
\end{aligned}
\tag{3.81}
\]

其中

\[
b_i = 0 \quad \text{for } i > N_b,\qquad
c_i = 0 \quad \text{for } i > N_c
\]

且 \(\delta(i-j)\) 是克罗内克δ函数。

考虑以下示例来说明这些关系。

**例3.3.** 设 \(N_a = 3\)、\(N_b = 2\)、\(N_c = 1\)，则相应的ARMAX模型为

\[
y(t) = -a_1 y(t-1) - a_2 y(t-2) - a_3 y(t-3) + b_0 u(t) + b_1 u(t-1) + b_2 u(t-2) + c_0 e(t) + c_1 e(t-1)
\tag{3.82}
\]

利用式(3.80)的观测器标准形，我们有

\[
x(t) =
\begin{bmatrix}
0 & 0 & -a_3\\
1 & 0 & -a_2\\
0 & 1 & -a_1
\end{bmatrix}
x(t-1)
+
\begin{bmatrix}
-a_3b_0\\
b_2-a_2b_0\\
b_1-a_1b_0
\end{bmatrix}
u(t-1)
+
\begin{bmatrix}
-a_3c_0\\
-a_2c_0\\
c_1-a_1c_0
\end{bmatrix}
e(t-1)
\]

\[
y(t) =
\begin{bmatrix}
0 & 0 & 1
\end{bmatrix}
x(t) + b_0 u(t) + c_0 e(t)
\]

本例到此结束。

重要的是要认识到，如果我们假设 \(\{e(t)\}\) 是高斯的，那么ARMAX模型等价于第3.6节的新息表示，即

\[
x(t) = A x(t-1) + B u(t-1) + W e(t-1)
\]
\[
y(t) = C' x(t) + b_0 u(t) + c_0 e(t)
\tag{3.83}
\]

在这种情况下，\(K \to W\)、\(D \to b_0\)、\(1 \to c_0\)。同时，相应的协方差矩阵变为

\[
R_{ee}^* := \operatorname{cov}\left(
\begin{bmatrix}
W e(t)\\
c_0 e(t)
\end{bmatrix}
\right)
=
\begin{bmatrix}
W R_{ee} W' & W R_{ee} c_0\\
c_0 R_{ee} W' & c_0 R_{ee} c_0
\end{bmatrix}
\delta(t-k)
\]

至此，关于一般ARMAX与状态空间等价性的讨论到此结束。接下来，我们推导第2.4节中给出的ARMAX模型某些特殊情况的状态空间等价模型。

我们从滑动平均（MA）开始

\[
y(t) = e(t) + \sum_{i=1}^{N_c} c_i e(t-i)
\]

或

\[
Y(z) = C(z)E(z) = \left(1 + c_1 z^{-1} + \cdots + c_{N_c} z^{-N_c}\right)E(z)
\]

定义状态变量为

\[
x_i(t-1) := e(t-i-1), \quad i = 1, \ldots, N_c
\tag{3.84}
\]

