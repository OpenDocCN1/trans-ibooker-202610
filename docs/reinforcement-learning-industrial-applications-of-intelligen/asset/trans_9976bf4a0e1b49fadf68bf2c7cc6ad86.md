

[公式 2-8](#ch02.html#eq_state_value_function) 将状态价值函数定义为从给定状态开始的预期回报，你已经看到了如何在 [公式 2-2](#ch02.html#eq_online-value_function) 中将其转换为在线算法。[公式 3-1](#ch03.html#eq_mc_state_value_function) 是蒙特卡洛（MC）状态价值函数估计的一种在线实现。关键部分是回报 `G` 的指数加权移动平均估计。随着时间的推移，你更新状态价值估计以使其更接近真实的预期回报，而 `α` 控制更新的速率。

### 公式 3-1. 在线蒙特卡洛状态价值函数

$$\begin{array}{cl} {V_{\pi}\left( s \right)} & {\doteq {\mathbb{E}}_{\pi}\left\lbrack G \middle| s \right\rbrack} \\ & \left. \leftarrow{\mathbb{E}}_{\pi}\left\lbrack {V_{\pi}\left( s \right) + \alpha\left( {G - V_{\pi}\left( s \right)} \right)\left| s \right.} \right\rbrack \right. \\ \end{array}$$

回想一下，`G` 是整条轨迹的总回报。因此，[公式 3-1](#ch03.html#eq_mc_state_value_function) 的问题在于，你需要等到一个回合结束才能得到 `G` 的值。所以是的，它是在线的，因为它一次更新一步，但你仍然必须等到回合结束。

[“动态规划中的价值迭代”](#ch02.html#sec_dynamic_programming) 中的价值估计提供了状态价值函数略有不同的解释。你不需要等到回合结束来观察回报，而是可以使用下一步的折扣估计来迭代更新当前的状态价值估计。[公式 3-2](#ch03.html#eq_dp_state_value_function) 展示了动态规划（DP）的一个在线版本。根据定义，它是在线的，因为它始终向前看一步。请注意，该公式从与 MC 版本相同的状态价值函数定义开始。

### 公式 3-2. 在线动态规划状态价值函数

$$\begin{array}{cl} {V_{\pi}\left( s \right)} & {\doteq {\mathbb{E}}_{\pi}\left\lbrack G \middle| s \right\rbrack} \\ & \left. \leftarrow\max\limits_{a}\sum\limits_{s^{'},r}p\left( s^{'},r \mid s,a \right)\left\lbrack {r + \gamma V\left( s^{'} \right)} \right\rbrack \right. \\ \end{array}$$

你已经知道这里的问题是你不知道转移概率。但请注意 [公式 3-2](#ch03.html#eq_dp_state_value_function) 最后一行，DP 版本使用了一个有趣的 `G` 定义：`r + γV(s')`。这表示回报等于当前奖励加上下一状态预测的折扣值。换句话说，回报是当前奖励加上所有未来奖励的估计。能否在蒙特卡洛算法中使用这个 `G` 的定义，这样你就不必等到回合结束？可以！请看 [公式 3-3](#ch03.html#eq_td_state_value_function)。

### 公式 3-3. 时序差分状态价值函数

```
V[π](s) ≐ 𝔼[π][V[π](s) + α(r + γV[π](s') − V[π](s)) | s]
```

[公式 3-3](#ch03.html#eq_td_state_value_function) 是预期回报的 TD 估计。它结合了自举法的优势和 MC 滚动平均的采样效率。请注意，你可以重新构建此公式以估计动作价值函数。

最后，我可以将其转换为一个在线算法，该算法假设指数加权平均是期望算子的合适估计，从而得到 [公式 3-4](#ch03.html#eq_online_td_state_value_function) 中的在线 TD 状态价值估计。

### 公式 3-4. 在线时序差分状态价值函数

```
V[π](s) ← V[π](s) + α(r + γV[π](s') − V[π](s))
```

花点时间消化一下这些公式；试着用你自己的话来解释它们。它们表明，你可以根据任意的奖励定义，通过在环境中“穿梭”并根据 [公式 3-3](#ch03.html#eq_td_state_value_function) 迭代更新状态价值函数，来学习一个最优策略。对状态价值函数（进而对策略）的更新是即时的，这意味着你的下一个动作应该比之前的更好。

## Q-Learning

1989 年，一种称为 `Q-learning` 的 TD 学习实现使得数学家更容易证明收敛性。该算法被认为点燃了强化学习（RL）的热潮。你可以重新构建 [公式 3-3](#ch03.html#eq_td_state_value_function) 以提供动作价值函数（[公式 2-9](#ch02.html#eq_action_value_function)）。此外，[公式 3-5](#ch03.html#eq_q_learning_update_function) 通过为给定状态有意选择最佳动作（方式同 [公式 2-10](#ch02.html#eq_optimal_value_function)）实现了一个简单的策略。

### 公式 3-5. 用于估计预期回报的 Q-learning 在线更新规则

$$\left. Q\left( s,a \right)\leftarrow Q\left( s,a \right) + \alpha\left( {r + \gamma\operatorname{argmax}\limits_{a_{s} \in \mathcal{A}{(s)}}Q\left( s^{'},a_{s} \right) - Q\left( s,a \right)} \right) \right\rbrack$$

回想一下，`Q(s,a)` 是动作价值函数，即 [公式 2-9](#ch02.html#eq_action_value_function) 中的那个函数，它告诉你每个动作的预期回报。[公式 3-5](#ch03.html#eq_q_learning_update_function) 作为一个在线更新规则实现，这意味着你应该通过覆盖之前的估计来迭代优化 `Q(s,a)` 的估计。将 `α` 右侧的所有内容视为一个 `delta`，即估计值与实际发生情况相比有多错误的度量。而 delta 中实现的 TD 单步前瞻允许算法向前投射一步。这使得智能体能够询问：“哪个是最佳动作？”

这是最重要的改进。之前的尝试仅利用该状态的知识来估计每个状态的预期回报。[公式 3-5](#ch03.html#eq_q_learning_update_function) 允许智能体向前看，以查看哪些未来轨迹是最优的。这被称为 `rollout`（展开/模拟）。智能体展开轨迹中的状态和动作（最多直到回合结束，类似于 MC 方法）。智能体可以创建不仅局部最优，而且在未来也是最优的策略。智能体可以创建能够预测未来的策略。

不过，这种动作价值函数的实现之所以特别，是因为一个简单的添加。[公式 3-5](#ch03.html#eq_q_learning_update_function) 中的 `argmax` 告诉算法扫描该状态的所有可能动作，并选择预期回报最好的那个。

这种对智能体的主动引导被称为 Q-learning，并实现在 [算法 3-1](#ch03.html#alg_q_learning) 中。

### 算法 3-1. Q-learning（离策略 TD）

