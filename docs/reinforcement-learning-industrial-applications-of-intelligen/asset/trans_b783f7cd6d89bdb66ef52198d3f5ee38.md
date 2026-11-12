
## 参考文献

^([28](#ch06.html#idm46428616378696-marker)) Henderson, Peter, Riashat Islam, Philip Bachman, Joelle Pineau, Doina Precup, and David Meger. 2019. ["重要的深度强化学习"](`https://oreil.ly/zgyQ4`). ArXiv:1709.06560，1月。

^([29](#ch06.html#idm46428616370040-marker)) Sutton, Richard S., A. Rupam Mahmood, and Martha White. 2015. ["针对离策略时序差分学习问题的强调方法"](`https://oreil.ly/95_Z3`). ArXiv:1503.04269，4月。

^([30](#ch06.html#idm46428616368344-marker)) Zhang, Shangtong, Wendelin Boehmer, and Shimon Whiteson. 2019. ["广义离策略演员-评论家"](`https://oreil.ly/tHEt8`). ArXiv:1903.11329，10月。

^([31](#ch06.html#idm46428616366168-marker)) Gelada, Carles, and Marc G. Bellemare. 2019. ["通过自举协变量偏移进行离策略深度强化学习"](`https://oreil.ly/1TKVs`). ArXiv:1901.09455，1月。

^([32](#ch06.html#idm46428616362648-marker)) Zhang, Shangtong, Wendelin Boehmer, and Shimon Whiteson. 2019. ["广义离策略演员-评论家"](`https://oreil.ly/NCo2a`). ArXiv:1903.11329，10月。

^([33](#ch06.html#idm46428620414856-marker)) Richter, Oliver, and Roger Wattenhofer. 2019. ["通过分位数回归学习策略"](`https://oreil.ly/6I-C9`). ArXiv:1906.11941，9月。

^([34](#ch06.html#idm46428620403608-marker)) Henderson, Peter, Riashat Islam, Philip Bachman, Joelle Pineau, Doina Precup, and David Meger. 2019. ["重要的深度强化学习"](`https://oreil.ly/F9Xvc`). ArXiv:1709.06560，1月。

# 第 7 章 使用熵方法学习所有可能的策略

深度强化学习（RL）因其处理和逼近复杂观测的能力而成为标准工具，这会产生精细的行为。然而，许多深度 RL 方法优化的是确定性策略，因为如果拥有完全可观测性，则只存在一种最优策略。但通常期望学习随机策略或概率行为，以提高鲁棒性并应对随机环境。

## 什么是熵？

`Shannon entropy`（以下简称 `entropy`）是衡量随机变量所包含信息量的指标，其中信息量按编码所有可能状态所需的比特数计算。[公式 7-1](#ch07.html#eq_entropy) 以方程形式展示了这一点，其中 `X` ≐ {`x`[0],`x`[1],⋯,`x`[`n` − 1]} 是随机变量，`H` 是熵，`I` 是信息量，`b` 是所用对数的底数（`b` ≐ 2 时常用比特，`b` ≐ 10 时用班，`b` ≐ `e` 时用纳特）。比特是最常用的底数。

### 公式 7-1. 随机变量的信息量

```
\mathcal{H}\left( X \right) \doteq {\mathbb{E}}\left\lbrack I\left( X \right) \right\rbrack = - \sum\limits_{x \in X}p\left( x \right)\log_{b}p\left( x \right)
```

例如，硬币有两种状态，假设它不会竖立着落地。这两种状态可用 0 和 1 编码，因此以比特为单位的熵衡量的硬币所含信息量为 1。骰子有六种可能状态，因此需要三个比特来描述所有这些状态（真实值为 2.5849…）。

最优控制的概率解是随机策略。为了准确表示动作-概率分布，必须采样足够多的状态和动作。你可以测量已访问的状态和动作数量，就像在 `UCB` 中那样（参见[“改进 `ϵ`-贪婪算法”](#ch02.html#improving-e-greedy)）。但这并不直接耦合到策略；`UCB` 是一种探索策略，而非策略的一部分。相反，你可以使用策略分布均匀程度的代理指标（如熵），并将其作为惩罚项包含在目标函数中。

## 最大熵强化学习

最大化策略的熵会迫使智能体访问所有状态和动作。策略不再旨在寻找最大化奖励的确定性行为，而是可以学习*所有*行为。换句话说，智能体不再学习执行任务的最佳方式，熵项迫使智能体学习解决任务的*所有*方法。这产生的策略能够直观地探索，并在面对对抗性攻击时（例如向机器人扔砖头）提供更鲁棒的行为。请参阅 [PPO 博客文章中的视频](`https://oreil.ly/ckDQo`) 以获取一些趣味内容。我在[第 10 章](#ch10.html#ch_operational_rl)中详细讨论这一点。

[公式 7-2](#ch07.html#eq_maximum_entropy_objective_function) 展示了最大熵 RL 的目标函数，其中 `α` 是一个超参数（称为 `temperature`，关联回熵的热力学定义），它决定了熵项相对于奖励的重要性，并控制最优策略的随机性。

### 公式 7-2. 最大熵目标函数

```
\begin{array}{r} {\pi^{\*} \doteq \operatorname{argmax}\limits_{\pi}\sum\limits_{s \in \mathcal{S},a \in \mathcal{A}}{\mathbb{E}}\left\lbrack {r + \alpha\mathcal{H}\left( \pi\left( a \mid s \right) \right)} \right\rbrack} \\ \end{array}
```

[公式 7-2](#ch07.html#eq_maximum_entropy_objective_function) 表明，最优策略被定义为使由策略熵正则化的回报最大化的策略。这类似于最优策略的标准定义，不同之处在于它增加了一个奖励项以促进非确定性策略，这与确定性策略梯度截然相反。

`α` 超参数允许你控制奖励项的大小。接近零的值倾向于确定性，较大的值则促进非确定性动作，从而鼓励更多探索。

## 软演员-评论家

软演员-评论家（`SAC`）算法是最大熵目标函数的离策略策略梯度近似。^([1](#ch07.html#idm46428620337112)) “软”一词用于强调算法中的非确定性元素；不含熵项的标准贝尔曼方程可视为“硬”，因此 `Q-learning` 和 `DDPG` 是相应的“硬”算法。

Haarnoja 等人首先将评论家定义为早期论文中提出的动作价值函数的软近似。^([2](#ch07.html#idm46428620334744)) 回顾一下，目标是生成一个最大化熵的策略，这意味着智能体应比其他状态更重视高熵状态。根据[公式 7-2](#ch07.html#eq_maximum_entropy_objective_function)，[公式 7-3](#ch07.html#eq_maximum_entropy_state_value_function) 表明，状态的价值等于动作价值加上该新状态熵的奖励。我简化了符号以匹配本书其余部分；[公式 7-3](#ch07.html#eq_maximum_entropy_state_value_function) 假设你正在遵循某个策略，但我隐藏了该信息以使数学表达更易理解。`V` 和 `Q` 分别代表状态价值函数和动作价值函数。

### 公式 7-3. 最大熵状态价值函数

```
\begin{array}{cl} {V\left( s \right)} & {= {\mathbb{E}}\left\lbrack {Q\left( s,a \right) + \alpha\mathcal{H}\left( {\pi\left( a \mid s \right)} \right)} \right\rbrack} \\ & {= {\mathbb{E}}\left\lbrack {Q\left( s,a \right) - \alpha\log\pi\left( a \mid s \right)} \right\rbrack} \\ \end{array}
```
