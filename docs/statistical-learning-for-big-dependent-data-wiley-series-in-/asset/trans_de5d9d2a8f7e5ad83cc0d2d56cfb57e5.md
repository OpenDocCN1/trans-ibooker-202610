

(AR) 表示中的时间序列 \(\pi(B)y_t = a_t\) 或移动平均 (MA) 表示中的时间序列 \(y_t = \psi(B)a_t\)，其中理解为 \(\pi(B) = \phi(B)(1-B)^d/\theta(B)\) 和 \(\psi(B) = \theta(B)/[\phi(B)(1-B)^d]\) 且 \(\pi(B)\psi(B) = 1\)。在实践中，\(\pi_i\) 和 \(\psi_j\) 均可通过对 \(B\) 中的两个多项式进行长除法得到。

我们假设在时间 \(t = h\) 发生了大小为 \(w_0\) 的异常事件。在此情况下，通过使用公式 (4.1) 中的指示变量，我们可以将观测时间序列写为

\(z_t = w_0 I_t^{(h)} + y_t = w_0 I_t^{(h)} + \psi(B)a_t. \quad (4.3)\)

换言之，我们有

\(z_t = \left\{ \begin{array}{ll} y_t, & \text{if } t \neq h, \\ y_h + w_0, & \text{if } t = h. \end{array} \right.\)

表示 (4.3) 是第 3 章中以输入变量 \(I_t^{(h)}\) 介绍的传递函数的特例。同样，在时间 \(t = h\) 发生的异常事件也可能在多个时间段内影响正在研究的时间序列。例如，芝加哥的一场大冬季暴风雪可能会导致奥黑尔国际机场的航空交通中断数天。在此情况下，我们可以将观测时间序列写为

\(z_t = \sum_{i=0}^m w_i I_t^{(h+i)} + \psi(B)a_t, \quad (4.4)\)

其中 \(m\) 是有限正整数，\(w_i\) 是该事件对时间段 \(t = h + i\)（\(i = 0, ..., m\)）的影响。此处我们使用参数 \(w_i\) 来表示该事件在 \(t = h + i\) 时对序列的影响。在实践中，可能很难 *事先* 知道 \(m\) 的值。此外，影响可能会随时间流逝而平滑衰减。因此，为 \(z_t\) 假设以下模型是合理的：

\(z_t = \frac{w_0}{1 - \delta B} I_t^{(h)} + \psi(B)a_t = \frac{w_0}{1 - \delta B} I_t^{(h)} + y_t, \quad (4.5)\)

其中 \(w_0\) 是异常事件的初始影响，\(0 < \delta < 1\) 表示影响的折现率。这是因为 \(w_0/(1 - \delta B) = w_0(1 + \delta B + \delta^2 B^2 + \cdots)\)。更具体地说，公式 (4.5) 显示

\(z_t = \left\{ \begin{array}{ll} y_t, & \text{if } t < h, \\ y_h + w_0, & \text{if } t = h, \\ y_{h+1} + w_0\delta, & \text{if } t = h+1, \\ y_{h+2} + w_0\delta^2, & \text{if } t = h+2, \\ \vdots & \vdots \end{array} \right.\)

其中 \(y_t\) 是未观测到的未受污染的时间序列。

通常，用于描述在时间索引 \(t = h\) 处异常事件对时间序列影响的灵活模型是

\(z_t = \frac{w(B)}{\delta(B)} I_t^{(h)} + y_t, \quad (4.6)\)

其中 \(w(B) = w_0 + w_1 B + \cdots + w_m B^m\) 和 \(\delta(B) = 1 - \delta_1 B - \cdots - \delta_r B^r\) 是 \(B\) 中的两个多项式，阶数分别为有限的 \(m\) 和 \(r\)，且 \(y_t = \psi(B)a_t\) 是未受污染的时间序列。\(w(B)/\delta(B)\) 的灵活性可见 Box and Tiao (1975) 和第 3 章。显然，我们要求 \(w(B)\) 和 \(\delta(B)\) 在公式 (4.6) 中没有公因式。

在实际应用中，时间序列可能会受到发生在时间 \(t = h_i\)（\(i = 1, \dots, g\)）的多个异常事件的影响，其中 \(g\) 是非负整数。在此情况下，我们为 \(z_t\) 假设以下模型：

\( z_t = \sum_{i=1}^g \frac{w_i(B)}{\delta_i(B)} I_t^{(h_i)} + \psi(B)a_t, \tag{4.7} \)

其中 \( w_i(B) = w_{i,0} + w_{i,1}B + \cdots + w_{i,m_i}B^{m_i} \) 和 \( \delta_i(B) = 1 - \delta_{i,1}B - \cdots - \delta_{i,r_i}B^{r_i} \)，且 \(m_i\) 和 \(r_i\) 为非负整数。

标量时间序列上异常事件影响建模的前面讨论可以轻易推广到向量情况。例如，对于观测到的向量系列，公式 (4.3) 中的模型将变为

\( z_t = \boldsymbol{w}_0 I_t^{(h)} + \boldsymbol{y}_t = \boldsymbol{w}_0 I_t^{(h)} + \boldsymbol{\psi}(\boldsymbol{B})\boldsymbol{a}_t, \)

其中 \( \boldsymbol{w}_0 = (w_{01}, \dots, w_{0k})' \) 是事件影响的向量，\( w_{0i} \) 是对第 \(i\) 个分量 \(z_{it}\) 的影响，\( \boldsymbol{y}_t \) 是相应的未受污染向量时间序列，而 \( \boldsymbol{\psi}(\boldsymbol{B}) = [\boldsymbol{\Phi}(\boldsymbol{B})]^{-1}\theta(\boldsymbol{B}) \) 表示 \( \boldsymbol{y}_t \) 的 \(\psi\)-权重矩阵，其遵循 VARMA 模型 \( \boldsymbol{\Phi}(\boldsymbol{B})\boldsymbol{y}_t = \theta(\boldsymbol{B})\boldsymbol{a}_t \)。关于 \(\psi\)-权重矩阵的进一步讨论见第 3 章。同样，公式 (4.6) 中的模型可以推广为

\( \boldsymbol{z}_t = [\boldsymbol{\delta}(\boldsymbol{B})]^{-1} \boldsymbol{w}(\boldsymbol{B}) I_t^{(h)} + \boldsymbol{y}_t, \tag{4.8} \)

其中 \( \boldsymbol{w}(\boldsymbol{B}) = \boldsymbol{w}_0 + \boldsymbol{w}_1 \boldsymbol{B} + \cdots + \boldsymbol{w}_m \boldsymbol{B}^m \) 和 \( \boldsymbol{\delta}(\boldsymbol{B}) = \boldsymbol{I} - \boldsymbol{\delta}_1 \boldsymbol{B} - \cdots - \boldsymbol{\delta}_r \boldsymbol{B}^r \) 分别是阶数为有限的 \(m\) 和 \(r\) 的两个矩阵多项式，且 \( \boldsymbol{w}_i \) 和 \( \boldsymbol{\delta}_i \) 是 \(k \times k\) 实矩阵，\(k\) 为 \( \boldsymbol{z}_t \) 的维度。此处我们要求 \( \boldsymbol{w}(\boldsymbol{B}) \) 和 \( \boldsymbol{\delta}(\boldsymbol{B}) \) 左互素。公式 (4.8) 在应用中有用的一个特例是

\( \boldsymbol{z}_t = (1 - \delta \boldsymbol{B})^{-1} \boldsymbol{w}_0 I_t^{(h)} + \boldsymbol{y}_t, \)

在此例中，异常事件对 \( \boldsymbol{z}_t \) 每个分量的影响以相同的速率衰减。

## 4.1.2 阶跃函数干预

在某些应用中，异常事件可能会永久影响正在研究的时间序列。例如，考虑某产品 A 品牌的市场份额，以及其制造商在时间索引 \(t = h\) 处进行的大型促销活动。如果促销在促销生效时间之前就已宣布，消费者可能会预期价格下降而推迟购买。随后，他们在促销期间购买了大量 A 品牌产品，导致促销结束后销售额立即下降。理论上，这种大型促销可能会永久改变 A 品牌的市场份额。为了描述这种复杂的市场反应，使用指示变量 \( I_t^{(h)} \) 会变得更加困难。相反，可以使用公式 (4.2) 中的阶跃函数来简化模型。此类干预的一个候选模型是

\( z_t = (w_{-1}F + w_0 + w_1 B) S_t^{(h)} + y_t, \)

其中 \(F = B^{-1}\) 是前移算子，\(y_t\) 是没有促销时的基础市场份额，\(w_i\) 是用于量化促销效果的参数。在这种情况下，人们会期望 \(w_{-1} < 0\)、\(w_0 > 0\) 和 \(w_1 < 0\)，而促销的永久效应由 \(w_{-1} + w_0 + w_1\) 给出。

转向向量时间序列的情况。始于 \(t = h\) 的异常事件的影响可以建模为

$$z_t = w_0 S_t^{(h)} + \Psi(B) a_t,$$

与之前一样，\(w_0 = (w_{0,1}, \dots, w_{0,k})'\)，其中 \(k\) 为 \(z_t\) 的维度。此处容易看出，当 \(t < h\) 时有 \(z_t = y_t\)，当 \(t \ge h\) 时有 \(z_t = w_0 + y_t\)，其中 \(y_t\) 是未受污染的向量时间序列。显然，我们可以将模型推广到包含多个干预的情况，每个干预对基础时间序列都有复杂的影响。

因为

$$I_t^{(h)} = S_t^{(h)} - S_{t-1}^h = \nabla S_t^{(h)},$$ (4.9)

在实际应用中，为给定干预选择使用指示变量还是阶跃函数可能并不关键。可以让数据选择要使用的适当变量。例如，如果我们估计模型

$$z_t = (w_0 - w_1 B) S_t^{(h)} + \Psi(B) a_t,$$ 并发现 \(\hat{w}_0 \simeq \hat{w}_1\)，那么可以将模型简化为

$$z_t = \hat{w}_0 \nabla S_t^{(h)} + \Psi(B) a = \hat{w}_0 I_t^{(h)} + \Psi(B) a_t,$$

表明干预效应仅发生在 \(t = h\)。如果在使用指示变量和阶跃函数之间存在疑问，始终可以使用模型选择方法（如信息准则）来选择合适的模型。

## 4.1.3 一般外生变量干预

干预分析不仅限于使用指示变量或阶跃函数。事实上，只要变量在时间 \(t\) 已知，就可以使用任何外生变量进行干预分析。例如，设 \(x_t\) 为公司在其产品上的月度广告支出，\(z_t\) 为公司的月度总销售额。那么，广告对销售的影响可以建模为

$$z_t = v(B)x_t + y_t,$$

其中 \(y_t\) 是没有任何广告时的月度销售额，而 \(v(B) = v_0 + v_1 B + v_2 B^2 + \cdots\) 是传递函数。在实践中，\(v(B)\) 通常可以用两个多项式的比值很好地近似，例如 \(v(B) = w(B)B^b/\delta(B)\)，其中 \(w(B) = w_0 + w_1 B + \cdots + w_m B^m\) 和 \(\delta(B) = 1 - \delta_1 B - \cdots - \delta_r B^r\)，\(m\) 和 \(r\) 为非负整数，而 \(b\) 是一个通常被称为干预 *滞后* 的整数。这属于第 3 章讨论的传递函数模型。

## 4.1.4 构建干预模型

再次，我们从在时间 \(t = h\) 处发生单次干预的标量情况开始，并考虑模型

$$z_t = v(B) I_t^{(h)} + \psi(B) a_t. \quad (4.10)

