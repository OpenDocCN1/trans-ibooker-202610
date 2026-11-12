

梯度具有直观的解释。[公式 5-7](#ch05.html#eq_logistic_policy_gradient_two_actions) 表示观测状态与策略预期状态之间的差值。

## Softmax 策略

你可以将 softmax 函数视为 logistic 函数的多分类版本。这意味着你可以用它来表示从多个动作中选择某个动作的概率。Softmax 策略定义见 [公式 5-8](#ch05.html#eq_softmax_policy)。

### 公式 5-8. Softmax 策略

```
\pi_{\theta}\left( a \mid s \right) \doteq \frac{e^{\theta^{\intercal}s}}{\sum_{a}e^{\theta_{a}^{\intercal}s}}
```

换句话说，[公式 5-8](#ch05.html#eq_softmax_policy) 是一个类 logistic 函数，经过所有可能动作的归一化，以确保所有动作的概率之和为 1。对应的梯度如 [公式 5-9](#ch05.html#eq_softmax_policy_gradient) 所示，完整推导过程见 [附录 B](#app02.html#ch_derivative_of_softmax_policy)。

### 公式 5-9. Softmax 策略梯度

```
\nabla\ln\pi\left( a \mid s,\theta \right) = s - \sum\limits_{a}s\pi\left( \theta^{\intercal}s \right)
```

与[“Logistic 策略”](#ch05.html#sec_logistic_policy)类似，[公式 5-9](#ch05.html#eq_softmax_policy_gradient) 中所示的梯度结果为观测特征向量与期望特征向量之间的差值。

## 任意策略

手动计算策略的导数让我能向你精确展示如何通过迭代改进策略。但总体而言，这一过程既繁琐又晦涩。相反，你可以通过以下三种方式计算导数：有限差分、自动微分和符号微分。

有限差分计算参数空间中一小步范围内策略的差值，`∇f(x) = (f(x+h)−f(x))/2h`。这很简单，但因步长 `h` 而容易产生数值误差。

自动微分工具（如 `JAX`，或 `TensorFlow`/`PyTorch`/`MXNet`/其他中的反向传播方法）试图追踪参数在方程中的使用情况以构建有向无环图（`DAG`）。`DAG` 库将每一步映射到相应的导数，并通过链式法则将它们组合起来。

符号微分（使用如 `SymPy` 等工具）实际上与自动微分相同，只是它通常单向工作（前向）。符号微分工具更强调重构，这可能产生更易理解的导数，而自动方法可能导致链式调用层层嵌套。

换句话说，如果你能利用库，我建议你不要手动求导。使用适合你用例的库。

# 基础实现

“策略函数”一节向你展示了如何提供 [公式 5-4](#ch05.html#eq_policy_gradient_update_rule) 所需的梯度函数。剩下的细节是 `G` 的用法。回顾一下，这是从某一点直到回合结束（如果是情节式任务）或永远（如果是持续式任务）的总奖励（参见[“折扣奖励”](#ch02.html#discounted_rewards)）。

第 [2](#ch02.html#ch_mdp-dp-mc)–[4](#ch04.html#ch_deep_q_networks) 章展示了三种计算预期回报的不同方法，从蒙特卡洛、完整轨迹方法到近似、多步投影方法。这些实现细节是几种基础策略梯度实现（包括 `REINFORCE` 和 `actor-critic` 算法）之间的唯一区别。

## 蒙特卡洛 (REINFORSE)

第一个算法使用蒙特卡洛方法。它首先根据当前策略收集整个回合的轨迹。然后通过 [公式 5-4](#ch05.html#eq_policy_gradient_update_rule) 更新策略。

[算法 5-1](#ch05.html#alg_reinforce) 展示了 `REINFORCE` 算法。在第 (6) 步中，`G` 指从时间 `t` 到回合结束的折扣奖励，类似于 [算法 2-3](#ch02.html#alg_on-policy-mc-agent)。在第 (7) 步中，我使用 `G` 的值来加权策略的微分。微分计算的是由 `θ` 定义的选择某个动作概率需要调整的方向。`αγ^t G` 的集体加权定义了调整的幅度。我在第 (7) 步中也重新引入了折扣因子，它被从导数中提取出来，因为它不依赖于 `θ`。

### 算法 5-1. REINFORCE 算法

```
\begin{aligned} {\text{1:}} & {\mathbf{i}\mathbf{n}\mathbf{p}\mathbf{u}\mathbf{t}\text{:}\text{a}\text{differentiable}\text{parameterized}\text{policy}\pi\left( a \mid s,\theta \right)\text{,}\text{step}\text{size}\alpha > 0} \\ {\text{2:}} & {\text{Initialize}\theta\text{arbitrarily}} \\ {\text{3:}} & {\mathbf{l}\mathbf{o}\mathbf{o}\mathbf{p}\text{for}\text{each}\text{episode:}} \\ {\text{4:}} & {\text{Generate}\text{trajectory}\text{following}\pi\left( \cdot \mid \cdot ,\theta \right)} \\ {\text{5:}} & {\mathbf{f}\mathbf{o}\mathbf{r}\text{each}\text{step}\text{in}\text{episode} t\leftarrow 0,1,\cdots,T\text{:}} \\ {\text{6:}} & \left. G\leftarrow\sum\limits_{k = t + 1}^{T}\gamma^{k - t - 1}r_{k} \right. \\ {\text{7:}} & \left. \theta\leftarrow\theta + \alpha\gamma^{t}G\nabla\pi\left( a \mid s,\theta \right) \right. \\ {\text{8:}} & {\mathbf{e}\mathbf{n}\mathbf{d}\mathbf{f}\mathbf{o}\mathbf{r}} \\ \end{aligned}
```

### 示例：CartPole 环境上的 REINFORCE

策略梯度的优势体现在策略的低维度上。`CartPole` 环境有四个特征。线性策略有四个参数，分别加权每个特征的重要性。结果是对最佳行动方案的概率表示。如果杆子的角度接近倒下，则选择试图纠正该角度的动作的概率非常高。训练过程中获得的奖励如 [图 5-1](#ch05.html#fig_cartpole_reinforce_rewards) 所示。策略应着眼于更远的未来以获得良好的奖励，所以我设置 `γ` ≐ 0.99。我选择 `α` ≐ 0.01，因为在较高值下学习不稳定。

将此结果与[“示例：CartPole 环境上的 DQN”](#ch04.html#sec_cartpole_dqn)中的表格实现进行比较。在那里，我必须使用神经网络来近似状态空间。而这里，我只用四个参数就实现了线性映射。其优雅性、实用性令人难以置信。

![](assets/rele_0501.png)

## 带基线的 REINFORCE

一种更常见的实现包含基线预期回报，如 [“Dueling Networks”](#ch04.html#sec_dueling_networks) 中见到的优势函数（参见 [公式 4-4](#ch04.html#eq_advantage_function)）。这有助于改善因平均奖励大幅变化导致的梯度不稳定性。

