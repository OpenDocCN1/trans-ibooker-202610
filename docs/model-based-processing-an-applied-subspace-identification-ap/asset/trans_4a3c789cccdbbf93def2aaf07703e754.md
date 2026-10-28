

总和表示为

$$\underbrace{[1\ e^{-j\Omega_m}\ \cdots\ e^{-jN\Omega_m}]}_{\mathbf{V}'(\Omega_m)}\underbrace{\begin{bmatrix}h(0)\\h(1)\\\vdots\\h(N)\end{bmatrix}}_{\mathbf{h}}=\mathbf{V}'(\Omega_m)\mathbf{h}=1$$

MVDR估计器通过最小化方差获得

$$\min_{\mathbf{h}} J=\mathbf{h}'\mathbf{R}_{yy}\mathbf{h}$$

并满足约束条件 $\mathbf{V}'(\Omega_m)\mathbf{h}=1$。该估计器可以使用拉格朗日乘数法推导，通过将拉格朗日乘数 $\lambda$ 增广到代价函数中并最小化，从而将有约束问题转化为无约束问题 [25]

$$\min_{\mathbf{h}} J=\mathbf{h}'\mathbf{R}_{yy}\mathbf{h}+\lambda(1-\mathbf{V}'(\Omega_m)\mathbf{h})$$

执行最小化并代入约束条件，得到滤波器权重为

$$\mathbf{h}_{\mathrm{MVDR}}=\frac{\mathbf{R}_{yy}^{-1}\mathbf{V}(\Omega_m)}{\mathbf{V}'(\Omega_m)\mathbf{R}_{yy}^{-1}\mathbf{V}(\Omega_m)}$$

将该表达式代入 $J$，得到在 $\Omega_m$ 处的 $MVDR$ 谱估计为

$$S_{\mathrm{MVDR}}(\Omega_m)=\frac{\Delta T}{\mathbf{V}'(\Omega_m)\mathbf{R}_{yy}^{-1}\mathbf{V}(\Omega_m)}=\Delta T\mathbf{h}'\mathbf{R}_{yy}\mathbf{h}$$

这意味着在给定频率处的谱估计是通过在期望频率处适当缩放输出协方差获得的。在实践中，MVDR比经典的相关/周期图估计器具有更高的分辨率，但低于AR（MEM）估计器。我们还注意到，对于窄带过程，MVDR谱估计的峰值与功率成正比，而MEM的峰值与功率的平方成正比。事实上，可以证明MVDR和MEM谱之间存在伯格公式的关系：[18]

$$\frac{1}{S_{\mathrm{MVDR}}(\Omega)}=\frac{1}{N_a}\sum_{i=1}^{N_a}\frac{1}{S_{\mathrm{AR}}(\Omega,i)}$$

其中 $S_{\mathrm{AR}}(\Omega,i)$ 是对应于第 $i$ 阶AR模型的MEM功率谱密度。因此，我们看到MVDR谱估计平均了所有较低阶（分辨率较低）和较高阶AR模型来获得其估计，从而解释了其较低的分辨率。实际上，实现该技术的一个建议方法是使用AR谱估计器，然后执行此平均。

这完成了对MVDR PSD估计技术的讨论。在下一节中，我们讨论主要设计用于从噪声中提取正弦信号的方法。

## 2.5.2.4 多信号分类（MUSIC）谱估计

在本节中，我们研究噪声中谐波（正弦）参数估计问题的解决方案，该问题在多种应用中普遍存在，尤其是在到达方向和目标定位问题的空间处理中 [26, 27]。

复谐波模型定义为

```
s(t) = Σ_{i=1}^{N_s} A_i e^{jΩ_i t + φ_i}  (2.69)
```

其中 $A_i$ 是复数，$A_i = |A_i| e^{-jθ_i}$，$\phi_i$ 是随机的且均匀分布为 $\phi \sim U(-\pi, \pi)$，$\Omega_i$ 是谐波频率。$N_s$ 个复振幅和谐波频率的集合 $[\{A_i\}, \{\Omega_i\}]$，$i = 1, ..., N_s$ 被假设为确定但未知的。

相应的测量模型由加性随机噪声中的复谐波信号表征

```
y(t) = s(t) + n(t)  (2.70)
```

其中 $n(t)$ 是方差为 $\sigma_n^2$ 的零均值随机序列。因此，其自相关函数为

```
R_yy(k) = R_ss(k) + R_nn(k) = Σ_{i=1}^{N_s} P_i e^{jΩ_i k} + σ_n^2  for P_i := |A_i|^2  (2.71)
```

由于信号与噪声不相关。将此关系扩展到包含 $N_s$ 个谐波，我们得到

```
R_yy(k) = { Σ_{i=1}^{N_s} P_i + σ_n^2 δ(k)  k = 0 ; Σ_{i=1}^{N_s} P_i e^{jΩ_i k}  k ≠ 0 }  (2.72)
```

有许多方法可以解决这个问题，例如最大似然参数估计、Prony技术、线性预测，但在这里我们选择使用功率方法 [20, 26]，即代价函数基于最大化信号中的输出功率，所得函数与其功率谱相关。频谱的所有峰值定位模型的关联谐波频率。

现在对式（2.72）在 $k$ 上展开，我们得到

$$R_{yy}(N) := R_{ss}+R_{nn}=V(\Omega)PV^H(\Omega)+\sigma_n^2I$$ (2.73)

其中 $V(\Omega)\in\mathbb{C}^{N\times N_s}$ 是谐波信号矩阵，对应的功率矩阵为 $P\in\mathbb{C}^{N_s\times N_s}$，且 $P=\operatorname{diag}[P_1\ \cdots\ P_{N_s}]$。由谐波模型构造的信号矩阵为

$$V(\Omega)=[v_1(\Omega)\mid v_2(\Omega)\mid\cdots\mid v_{N_s}(\Omega)]$$

其中 $v_i(\Omega):=[1\ e^{j\Omega_i}\ \cdots\ e^{j(N-1)\Omega_i}]^T$ (2.74)

因此，输出相关矩阵可以分解为对应的特征值 $(\Lambda)$ 和特征向量 $(E)$ 系统，即

$$R_{yy}(N)=E\,\Lambda\,E^H$$ (2.75)

或

$$R_{yy}(N)=E\begin{bmatrix}\lambda_1&0&\cdots&0\\0&\ddots&&\vdots\\\vdots&&\lambda_{N_s}&0\\0&\cdots&0&\lambda_N\end{bmatrix}E^H=V(\Omega)\begin{bmatrix}\lambda_1^s&0&\cdots&0\\0&\ddots&&\vdots\\\vdots&&\lambda_{N_s}^s&0\\0&\cdots&0&0\end{bmatrix}V^H(\Omega)+\sigma_n^2I$$ (2.76)

这源于存在 $N_s$ 个谐波的事实。因此，我们有 $\lambda_1>\lambda_2>\cdots>\lambda_N$ 且

$$\lambda_i=\begin{cases}\lambda_i^s+\sigma_n^2,&i=1,\ldots,N_s\\\sigma_n^2,&i=N_s+1,\ldots,N\end{cases}$$ (2.77)

输出相关也可以写成部分形式

$$R_{yy}(N)=[E(N_s)\mid E(N-N_s)]\begin{bmatrix}\Lambda(N_s)&0\\0&\Lambda(N-N_s)\end{bmatrix}\begin{bmatrix}E^H(N_s)\\E^H(N-N_s)\end{bmatrix}$$ (2.78)

其中 $E(N_s)\in\mathbb{C}^{N\times N_s}$ 和 $E(N-N_s)\in\mathbb{C}^{N\times(N-N_s)}$ 都是特征向量矩阵。将各部分相乘得到关系式

$$R_{yy}(N)=E(N_s)\Lambda(N_s)E^H(N_s)+E(N-N_s)\Lambda(N-N_s)E^H(N-N_s)$$ (2.79)

$$R_{yy}(N)=[e_1\ \cdots\ e_{N_s}]\begin{bmatrix}\lambda_1^s+\sigma_n^2&0&\cdots&0\\0&\ddots&&\vdots\\\vdots&&\lambda_{N_s}^s+\sigma_n^2&0\\0&\cdots&0&\lambda_{N_s}^s+\sigma_n^2\end{bmatrix}\begin{bmatrix}e_1^H\\\vdots\\e_{N_s}^H\end{bmatrix}+[e_{N_s+1}\ \cdots\ e_N]\begin{bmatrix}\sigma_n^2&0&\cdots&0\\0&\ddots&&\vdots\\\vdots&&\ddots&0\\0&\cdots&0&\sigma_n^2\end{bmatrix}\begin{bmatrix}e_{N_s+1}^H\\\vdots\\e_N^H\end{bmatrix}$$ (2.80)

这意味着分解

$$R_{yy}(N) = \sum_{i=1}^{N_s} (\lambda_i^s + \sigma_n^2)\mathbf{e}_i\mathbf{e}_i^H + \sum_{i=N_s+1}^{N} \sigma_n^2\mathbf{e}_i\mathbf{e}_i^H \tag{2.81}$$

根据这种*谱分解*，可以定义各种子空间，如下所示：

- 1) *信号子空间*定义为由*信号特征向量* $\{\mathbf{e}_i\}$；$i = 1, \dots, N_s$ 张成的 $N_s$ 维子空间，对应于 $N_s$ 个最大的特征值。
- 2) *噪声子空间*定义为由*噪声特征向量* $\{\mathbf{e}_i\}$；$i = N_s + 1, \dots, N$ 张成的 $(N - N_s)$ 维子空间，对应于剩余的 $(N - N_s)$ 个特征值。
- 3) *信号子空间*和*噪声子空间*是*正交*的。

将此分解应用于谐波估计问题，我们看到*谐波信号向量* $\mathbf{v}_i(\Omega)$ 位于由信号特征向量 $\{\mathbf{e}_i\}$；$i = 1, \dots, N_s$ 张成的信号子空间中，而相应的*噪声向量*位于由 $\{\mathbf{e}_i\}$；$i = N_s + 1, \dots, N$ 张成的正交噪声子空间中。因此，谐波信号向量位于 $R_{yy}(N)$ 的前 $N_s$ 个特征向量张成的子空间中，并且与每个噪声特征向量*正交*，即

$$\mathbf{v}_i^H(\Omega)\mathbf{e}_j = 0 \quad \text{对于所有 } i = 1, \dots, N_s; j = N_s + 1, \dots, N \tag{2.82}$$

这也意味着

$$V^H(\Omega)\mathbf{e}_j = 0 \text{ 对于 } j = N_s + 1, \dots, N \quad \text{且} \quad V(\Omega) = [\mathbf{v}_1(\Omega) \mid \cdots \mathbf{v}_{N_s}(\Omega)] \tag{2.83}$$

作为信号和噪声子空间正交性的说明，考虑两个谐波信号向量（$N_s = 2$）和一个噪声向量（$N = 3$）的情况，则信号向量 $\{\mathbf{v}_1(\Omega), \mathbf{v}_2(\Omega)\}$ 位于由两个信号特征向量 $\{\mathbf{e}_1, \mathbf{e}_2\}$ 张成的子空间中。噪声特征向量与该空间正交，如图2.4所示。

谐波频率*功率估计器* [26] 直接来源于式（2.82）的正交性，为

$$P(\Omega) := \frac{1}{\sum_{j=N_s+1}^{N} |\mathbf{v}^H(\Omega)\mathbf{e}_j|^2} \quad \text{对于 } P(\Omega) \to \infty \quad \text{只要 } \Omega = \Omega_i, \ i = 1, \dots, N_s \tag{2.84}$$

其中我们定义了*频率向量* $\mathbf{v}(\Omega)$ 来表示相应的时域傅里叶频率。⁸ 因此我们看到，如果我们计算功率

8 我们在全文中使用此频率向量约定。还要注意，它与谐波信号向量的不同之处在于，它跨越所有 $\Omega$，而不仅仅是与估计谐波信号对应的那个。

