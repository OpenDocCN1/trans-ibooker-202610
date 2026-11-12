

## 资格迹 Actor-Critic 算法

```latex
\begin{array}{cl} 
{\text{1:}} & {\mathbf{i}\mathbf{n}\mathbf{p}\mathbf{u}\mathbf{t}\text{:}\text{a}\text{differentiable}\text{parameterized}\text{policy}\pi\left( a \mid s,\theta \right),} \\ 
& {\text{a}\text{differentiable}\text{parameterized}\text{state-value}\text{function} V\left( s,w \right),} \\ 
& {\text{step}\text{sizes} 0 < \alpha_{w},\alpha_{\theta} < 1,\text{and}\text{trace-decay}\text{rates} 0 \leq \lambda_{\theta},\lambda_{w} \leq 1} \\ 
{\text{2:}} & {\text{Initialize}\theta\text{and} w\text{arbitrarily}} \\ 
{\text{3:}} & {\mathbf{l}\mathbf{o}\mathbf{o}\mathbf{p}\text{for}\text{each}\text{episode:}} \\ 
{\text{4:}} & \left. \text{Initialize} t\leftarrow 0,I\leftarrow 1\text{and} s, \right. \\ 
& \left. z_{w}\leftarrow 0\text{(same}\text{dimensionality}\text{as} w\text{)},\text{and} \right. \\ 
& \left. z_{\theta}\leftarrow 0\text{(same}\text{dimensionality}\text{as}\theta\text{)} \right. \\ 
{\text{5:}} & {\mathbf{w}\mathbf{h}\mathbf{i}\mathbf{l}\mathbf{e} s\text{is}\text{not}\text{terminal:}} \\ 
{\text{6:}} & {\text{Choose} a\text{from} s\text{using}\pi\text{,}\text{breaking}\text{ties}\text{randomly}} \\ 
{\text{7:}} & {\text{Take}\text{action} a\text{,}\text{observe} s^{'},r} \\ 
{\text{8:}} & \left. \delta\leftarrow r + \gamma V\left( s^{'},w \right) - V\left( s,\theta \right) \right. \\ 
{\text{9:}} & \left. z_{w}\leftarrow\gamma\lambda_{w}z_{w} + \nabla V\left( s,w \right) \right. \\ 
{\text{10:}} & \left. z_{\theta}\leftarrow\gamma\lambda_{\theta}z_{\theta} + I\nabla\ln\pi\left( a \mid s,\theta \right) \right. \\ 
{\text{11:}} & \left. w\leftarrow w + \alpha_{w}\delta z_{w} \right. \\ 
{\text{12:}} & \left. \theta\leftarrow\theta + \alpha_{\theta}I\delta z_{\theta} \right. \\ 
{\text{13:}} & \left. I\leftarrow\gamma I \right. \\ 
{\text{14:}} & \left. s\leftarrow s' \right. \\ 
\end{array}
```

### 示例：资格迹 Actor-Critic 在 CartPole 环境上的应用

我对[“示例：CartPole 环境上的 n-step Actor-Critic”](#ch05.html#sec_example_actor_critic_cartpole)中的示例进行了微小修改，以加入衰减资格迹。该实验使用 `α_θ` = `α_w` = `λ_θ` = `λ_w` ≐ 0.1，你可以在[图 5-8](#ch05.html#fig_cartpole_eligibility)中观察结果。

![](assets/rele_0508.png)

性能基本保持不变。这表明，在 `CartPole` 环境中，成功的智能体不需要远见。观察下一步潜在的失败就足够了。

改变 `λ` 的值影响不大，除了值为 0 的情况（此时没有时序差分更新）和值为 1 的情况（此时梯度爆炸）。在我的实验中，介于两者之间的所有值都导致了相似的结果。

在奖励稀疏或需要大量规划的环境中，多步时序差分估计表现出色。

## 基础策略梯度算法对比

本章介绍了策略梯度算法的四种不同实现。它们都使用梯度下降来迭代改进策略，但每种实现的价值估计方式不同。更好的价值估计能减少不必要的大幅更新；它能降低梯度的方差。

你可以在[公式 5-11](#ch05.html#eq_pg_equivalent_forms)中看到不同的形式。策略的梯度等于参数梯度乘以回报的期望。这些算法之间唯一的区别在于如何量化回报。我发现这种对策略梯度算法的高层解读很有启发性，因为它阐明了不同实现之间微妙的变化。

### 公式 5-11. 策略梯度算法的等价实现

```latex
\begin{array}{clr} 
{\nabla_{\theta}J\left( \theta \right)} & {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)G} \right\rbrack} & \text{(REINFORCE)} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}{\left( a \mid s \right)\left( G - \right.}V_{w}\left( s \right)} \right\rbrack} & {\text{(REINFORCE}\text{with}\text{Baseline)}} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)Q_{w}\left( s,a \right)} \right\rbrack} & \text{(Actor-Critic)} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)A_{w}\left( s,a \right)} \right\rbrack} & {\text{(Advantage}\text{Actor-Critic)}} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)\delta} \right\rbrack} & {\text{(Temporal-Difference}\text{Actor-Critic)}} \\ 
& {\doteq {\mathbb{E}}_{\pi_{\theta}}\left\lbrack {\nabla_{\theta}\ln\pi_{\theta}\left( a \mid s \right)\delta\left( \lambda \right)} \right\rbrack} & {\text{(Eligibility}\text{Actor-Critic)}} \\ 
\end{array}
```

# 工业示例：为客户自动采购产品

最近的一种商业模式趋势是自动向客户配送产品，并允许他们退回不感兴趣的商品。Stitch Fix 是订阅盒模式业务中最著名的公司之一。在提交了关于体型和个人偏好的大量初始信息后，Stitch Fix 的客户会收到一个装满新衣服的包裹。客户可以自由退回不喜欢的衣服，并为保留的商品付费（外加服务费）。

这个想法听起来可能很新颖，但公司早已通过“无承诺”订阅模式这样做了很长时间。例如，惠普会乐意以规定的费率为你的打印机配送墨水。你可以在亚马逊上订阅特定产品。Hello Fresh 会向你配送食材。无承诺模式与订阅盒模式的区别在于，如果客户选错了计划（例如，收到的墨水太多），他们很可能会取消订阅。惠普有动力以合适的速率配送墨水，以最大化客户终身价值。

我想强调这一挑战是多么令人印象深刻。在这里，一个无需人工干预的智能体可以自动向你发送产品，并迭代地学习你的偏好。你无需下单，就能在需要时收到商品。更妙的是，智能体知道你可能喜欢什么商品，即使你自己从未想过购买它们。从市场的角度来看，这展示了为什么强化学习可能如此有利可图。供应商能卖给你更多东西，而你会真心高兴它们做到了这一点！

## 环境：`Gym-Shopping-Cart`

该示例使用公开可用的数据来体现订阅盒模式。我创建了[一个环境](https://gitlab.com/winderresearch/gym-shopping-cart)，它使用 Instacart 的数据来模拟购买行为。

