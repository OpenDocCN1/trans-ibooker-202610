

在本小节中，我们展示了如何利用规范的状态空间结构从 MIMO 系统演化而来的马尔可夫序列或脉冲响应矩阵中提取唯一的模型。在结束本节之前，重要的是要理解输入/输出数据可以直接用于执行这些操作，如 [27] 中所述，而非先估计脉冲响应然后再应用相似变换。在下一节中，我们将发展子空间技术，该技术可直接从 MIMO 数据中提取状态空间模型，但与此处发展的规范结构不同——它们是“黑箱”模型，可通过相似变换（如 Eq. (6.45) 中用于 Luenberger 规范形式的变换）转换为规范结构。这是子空间实现的基本哲学，“拟合一个黑箱状态空间模型并将其变换到期望的坐标系”（例如规范形式）以进行处理。接下来我们将研究这些子空间方法。

## 6.4 确定性子空间实现：正交投影

在本小节中，我们发展了从输入/输出数据中识别状态空间表示的基本子空间实现方法，将实现从脉冲响应扩展到输入/输出数据——仍假设为确定性。为了理解当演化的是输入/输出序列而非脉冲响应序列时确定性实现问题如何求解，重要的是要理解大多数子空间实现技术首先要求直接从采集数据中提取扩展可观测矩阵，然后第二步提取系统模型。现有方法在可观测矩阵的估计方式及其提取系统模型的方法上有所不同。为了提取状态空间模型 Σ_ABCD，只需获得可观测矩阵的扩展列空间或状态序列的行空间即可，两者都可通过 SVD 轻松获得，即子空间辨识的关键要素基于提取：

𝒪_K = [ C ; CA ; ⋮ ; CA^{K-1} ]_{Column space} ;   𝒳_k = [ x(k)  x(k+1)  …  x(K+k-1) ]_{Row space}

也许经典实现理论采用脉冲响应序列后进行 Hankel 矩阵分解与从输入/输出序列的子空间实现理论之间的主要“联系”在于

内在的系统理论“秩条件”定义了状态空间的最小维数。子空间实现理论主要依赖于输入序列的持续激励概念以及解的最小状态空间维数 [9]，即可观测矩阵张成 $N_x$-维状态空间，该空间包含由于状态而产生的输出部分，不包括输入。为了看到这一点，我们首先构造输入/输出数据矩阵（如前所述）。

回想一下，离散 LTI 解由 Eq. (3.33) 给出

$$x(t)=A^t x(0)+\sum_{\ell=0}^{t-1}A^{t-\ell-1}Bu(\ell),\qquad t=0,1,\ldots,K$$

测量或输出系统由

$$y(t)=CA^t x(0)+\sum_{\ell=0}^{t-1}CA^{t-\ell-1}Bu(\ell)+D\delta(t)$$

给出
将此关系进一步在 $k$-个采样上展开并整理项，我们得到

$$\begin{bmatrix}y(0)\\y(1)\\\vdots\\y(k-1)\end{bmatrix}=\begin{bmatrix}C\\CA\\\vdots\\CA^{k-1}\end{bmatrix}x(0)+\begin{bmatrix}D&0&\cdots&0\\CB&D&\cdots&0\\\vdots&\vdots&\ddots&\vdots\\CA^{k-2}B&\cdots&CB&D\end{bmatrix}\begin{bmatrix}u(0)\\u(1)\\\vdots\\u(k-1)\end{bmatrix}$$

其中 $\mathcal{O}_k\in\mathbb{R}^{kN_y\times N_x}$ 是可观测矩阵，$\mathcal{T}_k\in\mathbb{R}^{kN_y\times kN_u}$ 是脉冲响应矩阵——一个 Toeplitz 矩阵 [4]。

将这些关系在时间上平移 ($0\to t$) 得到

$$\begin{bmatrix}y(t)\\y(t+1)\\\vdots\\y(t+k-1)\end{bmatrix}=\mathcal{O}_k x(t)+\mathcal{T}_k\begin{bmatrix}u(t)\\u(t+1)\\\vdots\\u(t+k-1)\end{bmatrix}$$

导出向量输入/输出关系

$$\mathbf{y}_k(t)=\mathcal{O}_k x(t)+\mathcal{T}_k\mathbf{u}_k(t)$$

其中 $\mathbf{y}\in\mathbb{R}^{kN_y\times1}$，$\mathbf{x}\in\mathbb{R}^{N_x\times1}$，和 $\mathbf{u}\in\mathbb{R}^{kN_u\times1}$。

我们堆叠这些向量以创建批数据（块 Hankel）矩阵以获得随后的“数据方程”。即，定义块输出数据矩阵 $(\mathcal{Y}_{0|k-1})$ 和块输入数据矩阵 $(\mathcal{U}_{0|k-1})^4$ 在 $K$-个采样上为

$$\mathcal{Y}_{0|k-1}:=\begin{bmatrix}\mathbf{y}(0)&\mathbf{y}(1)&\cdots&\mathbf{y}(K-1)\\\mathbf{y}(1)&\mathbf{y}(2)&\cdots&\mathbf{y}(K)\\\vdots&\vdots&&\vdots\\\mathbf{y}(k-1)&\mathbf{y}(k)&\cdots&\mathbf{y}(K+k-2)\end{bmatrix}$$

4 记号“$0|k-1$”定义了 $K$-列数据矩阵的第一行和最后一行元素。

和

𝒰₀|k−1 := [ u(0) u(1) ⋯ u(K−1); u(1) u(2) ⋯ u(K); ⋮ ⋮ ⋮ ⋮; u(k−1) u(k) ⋯ u(K+k−2) ] (6.60)

或更紧凑地

𝒴₀|k−1 = [yₖ(0) ⋯ yₖ(K−1)] ∈ ℝ^{kN_y×K}
𝒰₀|k−1 = [uₖ(t) ⋯ uₖ(K−1)] ∈ ℝ^{kN_u×K} (6.61)

对应的状态向量定义为

𝒳₀ := [x(0) x(1) ⋯ x(K−1)] ∈ ℝ^{N_x×K} (6.62)

因此，我们有向量-矩阵输入/输出方程或数据方程，它将系统模型与数据（输入、状态和输出矩阵）联系起来

𝒴₀|k−1 = 𝒪ₖ𝒳₀ + 𝒯ₖ𝒰₀|k−1 (6.63)

总体目标是从确定性输入/输出数据中提取可观测矩阵并随后提取系统矩阵 Σ_ABCD。也就是说，我们寻求一个解，其列空间（值域）等价于 𝒪_K（例如 range(𝒴₀|k−1) ≡ range(𝒪_K)）。显然，如果脉冲响应矩阵 𝒯_K 已知，除了数据和输入矩阵之外，那么我们可以仅从数据方程中减去最后一项，我们将有

𝒴₀|k−1 = 𝒪ₖ𝒳₀

提供期望的结果，使我们在执行 SVD 以获得等价列空间后能够提取 Σ_ABCD，但不幸的是 𝒯_K 是未知的！因此，我们必须首先以某种方式估计脉冲响应矩阵，以便将其从数据方程中移除。

### 6.4.1 子空间实现：正交投影

提取可观测矩阵的一种方法是最小二乘 [7, 28–31]，即把问题表述为相对于 𝒯_k 最小化代价函数 𝒥

𝒥 = min_{𝒯_k} ‖ 𝒴₀|k−1 − 𝒯_k 𝒰₀|k−1 ‖ (6.64)

以便利用开发一个正交于输入空间 𝒰 的投影算子 𝒫 的思想来消除输入的影响，使得

$\langle \mathcal{U},\mathcal{P}^{\perp}\rangle=\mathbf{0}$。众所周知的方法存在于完成此估计，例如将梯度算子设为零（$\nabla_{\hat{\mathcal{T}}}\mathcal{J}=\mathbf{0}$）并求解 $\hat{\mathcal{T}}_k$ 或配方法 [7, 31]。因此，该问题的最小二乘估计为

$$\hat{\mathcal{T}}_k := \mathcal{Y}_{0|k-1}\mathcal{U}_{0|k-1}^{\prime}(\mathcal{U}_{0|k-1}\mathcal{U}_{0|k-1}^{\prime})^{-1}$$  
(6.65)

从数据方程中移除输入得到

$$\mathcal{Y}_{0|k-1}-\hat{\mathcal{T}}_k\mathcal{U}_{0|k-1}=\mathcal{Y}_{0|k-1}-\mathcal{Y}_{0|k-1}\mathcal{U}_{0|k-1}^{\prime}(\mathcal{U}_{0|k-1}\mathcal{U}_{0|k-1}^{\prime})^{-1}\mathcal{U}_{0|k-1}$$

并因式分解我们有

$$\mathcal{Y}_{0|k-1}-\hat{\mathcal{T}}_k\mathcal{U}_{0|k-1}=\mathcal{Y}_{0|k-1}(\mathbf{I}-\mathcal{U}_{0|k-1}^{\prime}(\mathcal{U}_{0|k-1}\mathcal{U}_{0|k-1}^{\prime})^{-1}\mathcal{U}_{0|k-1})=\mathcal{Y}_{0|k-1}\mathcal{P}^{\perp}_{\mathcal{U}_{0|k-1}}$$

(6.66)

其中

$$\mathcal{P}^{\perp}_{\mathcal{U}_{0|k-1}}=\mathbf{I}-\mathcal{P}_{\mathcal{U}_{0|k-1}}=\mathbf{I}-\mathcal{U}_{0|k-1}^{\prime}(\mathcal{U}_{0|k-1}\mathcal{U}_{0|k-1}^{\prime})^{-1}\mathcal{U}_{0|k-1}$$  
(6.67)

是正交投影 $\langle\mathcal{U}_{0|k-1},\mathcal{P}^{\perp}_{\mathcal{U}_{0|k-1}}\rangle$ 将 $\mathcal{U}_{0|k-1}$ 的（列）空间投影到 $\mathcal{U}$ 的零空间上，使得

$$\mathcal{U}_{0|k-1}\times\mathcal{P}^{\perp}_{\mathcal{U}_{0|k-1}}=\mathcal{U}_{0|k-1}-(\mathcal{U}_{0|k-1}\mathcal{U}_{0|k-1}^{\prime})(\mathcal{U}_{0|k-1}\mathcal{U}_{0|k-1}^{\prime})^{-1}\mathcal{U}_{0|k-1}=\mathbf{0}$$  
(6.68)

这里解依赖于假设 $\mathcal{U}_{0|k-1}$ 是“持续激励”的或等价地它是满秩的，$\rho(\mathcal{U})=kN_u$。

将投影算子应用于 Eq. (6.63) 的数据方程将“过去数据”投影到 $(\perp)$ $\mathcal{U}$ 的零空间，由 $\mathcal{P}^{\perp}_{\mathcal{Y}_{0|k-1}\mid\mathcal{U}_{0|k-1}}$ 定义，从而得到期望的结果

$$\mathcal{P}^{\perp}_{\mathcal{Y}_{0|k-1}\mid\mathcal{U}_{0|k-1}}:=\mathcal{Y}_{0|k-1}\times\mathcal{P}^{\perp}_{\mathcal{U}_{0|k-1}}=\mathcal{O}_k\mathcal{X}_0\mathcal{P}^{\perp}_{\mathcal{U}_{0|k-1}}+\mathcal{T}_k\mathcal{U}_{0|k-1}\mathcal{P}^{\perp}_{\mathcal{U}_{0|k-1}}=\mathcal{O}_k\mathcal{X}_0\mathcal{P}^{\perp}_{\mathcal{U}_{0|k-1}}$$  
(6.69)

从而消除输入并提取可观测矩阵（见图 6.6）。

接下来，我们必须确保数据和可观测矩阵的列空间是相同的，以便提取 $\Sigma_{ABCD}$。

从 Eq. (6.63) 的数据方程，我们可以构造 *增广（输入/输出）数据矩阵*，$\mathcal{D}\in\mathbb{R}^{k(N_u+N_y)\times k}$

$$\mathcal{D}_{0|k-1}:=\begin{bmatrix}\mathcal{U}_{0|k-1}\\\mathcal{Y}_{0|k-1}\end{bmatrix}=\begin{bmatrix}\mathbf{I}_{kN_u}&\mathbf{0}\\\mathcal{T}_k&\mathcal{O}_k\end{bmatrix}\begin{bmatrix}\mathcal{U}_{0|k-1}\\\mathcal{X}_0\end{bmatrix}$$  
(6.70)

