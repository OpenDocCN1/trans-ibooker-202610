# 附录 A. 双动作逻辑斯蒂策略的梯度

公式 5-6 是一个双动作策略。为了更新策略，我需要计算策略自然对数的梯度（参见公式 5-4）。我在公式 A-1 中给出了该梯度。根据重构方式的不同，求导可以用几种不同的方法进行，因此结果看起来可能不同，尽管它们提供相同的结果。

### 公式 A-1. 双动作逻辑斯蒂策略梯度

$$
\nabla\ln\pi\left( a \mid s,\theta \right) = \begin{pmatrix} {\frac{\delta}{\delta\theta_{0}}\ln\left( \frac{1}{1 + e^{- \theta_{0}^{\intercal}s}} \right)} \\ {\frac{\delta}{\delta\theta_{1}}\ln\left( {1 - \frac{1}{1 + e^{- \theta_{1}^{\intercal}s}}} \right)} \\ \end{pmatrix}
$$

### 公式 A-2. 逻辑斯蒂函数的重构

$$
\begin{array}{cl} {\pi\left( x \right) \doteq \frac{1}{1 + e^{- x}}} & {= \frac{e^{x}}{e^{x}\left( 1 + e^{- x} \right)}} \\ & {= \frac{e^{x}}{e^{x}\left( 1 + e^{- x} \right)}} \\ & {= \frac{e^{x}}{e^{x} + e^{x}e^{- x}}} \\ & {= \frac{e^{x}}{e^{x} + e^{x - x}}} \\ & {= \frac{e^{x}}{e^{x} + e^{0}}} \\ & {= \frac{e^{x}}{e^{x} + 1}} \\ & {= \frac{e^{x}}{1 + e^{x}}} \\ \end{array}
$$

重构后的逻辑斯蒂函数关于动作 0 的导数见公式 A-3。

### 公式 A-3. 动作 0 的求导




# 附录 A. 逻辑策略的梯度

$$\\begin{array}{cl} {\\frac{\\delta}{\\delta\\theta\_{0}}\\ln\\pi\_{0}\\left( \\theta\_{0}^{\\intercal}s \\right)} & {= \\frac{\\delta}{\\delta\\theta\_{0}}\\ln\\left( \\frac{e^{\\theta\_{0}^{\\intercal}s}}{1 + e^{\\theta\_{0}^{\\intercal}s}} \\right)} \\\\ & {= \\frac{\\delta}{\\delta\\theta\_{0}}\\ln e^{\\theta\_{0}^{\\intercal}s} - \\frac{\\delta}{\\delta\\theta\_{0}}\\ln\\left( 1 + e^{\\theta\_{0}^{\\intercal}s} \\right)} \\\\ & {= \\frac{\\delta}{\\delta\\theta\_{0}}\\theta\_{0}^{\\intercal}s - \\frac{\\delta}{\\delta\\theta\_{0}}\\ln\\left( 1 + e^{\\theta\_{0}^{\\intercal}s} \\right)} \\\\ & {= s - \\frac{\\delta}{\\delta\\theta\_{0}}\\ln\\left( 1 + e^{\\theta\_{0}^{\\intercal}s} \\right)} \\\\ & {= s - \\frac{\\delta}{\\delta\\theta\_{0}}\\ln u\\text{where} u = 1 + e^{\\theta\_{0}^{\\intercal}s}} \\\\ & {= s - \\frac{1}{u}\\frac{\\delta}{\\delta\\theta\_{0}}u} \\\\ & {= s - \\frac{1}{1 + e^{\\theta\_{0}^{\\intercal}s}}\\frac{\\delta}{\\delta\\theta\_{0}}\\left( 1 + e^{\\theta\_{0}^{\\intercal}s} \\right)} \\\\ & {= s - \\frac{1}{1 + e^{\\theta\_{0}^{\\intercal}s}}\\frac{\\delta}{\\delta v}\\left( e^{v} \\right)\\frac{\\delta}{\\delta\\theta\_{0}}v\\text{where} v = \\theta\_{0}^{\\intercal}s} \\\\ & {= s - \\frac{1}{1 + e^{\\theta\_{0}^{\\intercal}s}}e^{\\theta\_{0}^{\\intercal}s}s} \\\\ & {= s - \\frac{se^{\\theta\_{0}^{\\intercal}}s}{1 + e^{\\theta\_{0}^{\\intercal}s}}} \\\\ & {= s - s\\pi\\left( \\theta\_{0}^{\\intercal}s \\right)} \\\\ \\end{array}$$

计算动作 1 的策略导数的方法见 公式 A-4。请注意，结尾部分的推导与 公式 A-3 相同。

## 公式 A-4. 动作 1 的求导

$$\\begin{array}{cl} {\\frac{\\delta}{\\delta\\theta\_{1}}\\ln\\pi\_{1}\\left( \\theta\_{1}^{\\intercal}s \\right)} & {= \\frac{\\delta}{\\delta\\theta\_{1}}\\ln\\left( {1 - \\frac{e^{\\theta\_{1}^{\\intercal}s}}{1 + e^{\\theta\_{1}^{\\intercal}s}}} \\right)} \\\\ & {= \\frac{\\delta}{\\delta\\theta\_{1}}\\ln\\left( {\\frac{1 + e^{\\theta\_{1}^{\\intercal}s}}{1 + e^{\\theta\_{1}^{\\intercal}s}} - \\frac{e^{\\theta\_{1}^{\\intercal}s}}{1 + e^{\\theta\_{1}^{\\intercal}s}}} \\right)} \\\\ & {= \\frac{\\delta}{\\delta\\theta\_{1}}\\ln\\left( \\frac{1 + e^{\\theta\_{1}^{\\intercal}s} - e^{\\theta\_{1}^{\\intercal}s}}{1 + e^{\\theta\_{1}^{\\intercal}s}} \\right)} \\\\ & {= \\frac{\\delta}{\\delta\\theta\_{1}}\\ln\\left( \\frac{1}{1 + e^{\\theta\_{1}^{\\intercal}s}} \\right)} \\\\ & {= \\frac{\\delta}{\\delta\\theta\_{1}}\\ln 1 - \\frac{\\delta}{\\delta\\theta\_{1}}\\ln\\left( 1 + e^{\\theta\_{1}^{\\intercal}s} \\right)} \\\\ & {= - \\frac{\\delta}{\\delta\\theta\_{1}}\\ln\\left( 1 + e^{\\theta\_{1}^{\\intercal}s} \\right)} \\\\ & {= \\cdots\\text{same}\\text{as}\\text{the}\\text{derivative}\\text{of}\\pi\_{0}} \\\\ & {= - s\\pi\\left( \\theta\_{1}^{\\intercal}s \\right)} \\\\ \\end{array}$$

## 公式 A-5. 逻辑策略

$$\\nabla\\ln\\pi\\left( a \\mid s,\\theta \\right) = \\begin{pmatrix} {s - s\\pi\\left( \\theta\_{0}^{\\intercal}s \\right)} \\\\ {- s\\pi\\left( \\theta\_{1}^{\\intercal}s \\right)} \\\\ \\end{pmatrix}$$
