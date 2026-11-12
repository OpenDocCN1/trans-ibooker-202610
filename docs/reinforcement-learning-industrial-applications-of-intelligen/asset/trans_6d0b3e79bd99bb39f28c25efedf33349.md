
# 使用模拟器进行调优

但我并没有直接用 `servos` 进行所有这些调优。当问题变得更难时，进行 `hyperparameter` 搜索会变得越来越困难。这主要是因为在真实 `environment` 中执行一个 `step` 所需的时间，但也存在安全和磨损的担忧。在这些实验中，我仅用 5 天开发时间就成功损坏了一个 `servo`；其 `positional feedback` 失效了。[图 6-9](#ch06.html#fig_fixed_goal_policy_2) 是损坏 `servo` 的图片，这提醒我们在现实生活中过度运动会导致磨损。

![](assets/rele_0608.png)

因此，构建一个 `simulator` 来进行初始 `tuning` 是合理的。它评估起来可能比真实环境快得多、便宜得多。所以我添加了一个基本的 `servo` `simulation` 并进行了一些 `hyperparameter tuning`。

以下是 `tuning` 时的一些提示：

*   `learning rate` 与所需的 `training time` 成反比。较小、较慢的 `learning rates` 需要更长的训练时间。
*   规模较小的 `minibatches` 泛化效果更好。
*   尝试在每次 `update` 中包含多个 `episodes`，以打破 `sample correlation`。
*   独立调优 `value estimation` 和 `policy networks`。
*   不要 `update` 过快，否则 `training` 会震荡。
*   `training speed` 和 `training stability` 之间存在权衡。如果你想要更稳定的 `training`，就要降低学习速度。

但老实说，对于这个问题，你使用什么 `hyperparameters` 并不重要。任何算法都能解决。我们在这里所做的一切只是试图控制 `learning` 的稳定性并提高速度。即便如此，仍然需要大约 1,000 个 `steps` 才能获得一个相当稳定的 `policy`。主要因素是 `learning rate`。你可以降低它以获得更快的 `training`，但 `policy` 倾向于震荡。

## 最终策略

[图 6-10](#ch06.html#fig_trained_policy_sim) 展示了在 `simulated environment` 上 `training` 四个不同阶段（250、500、750 和 1,000 `environment steps`，对应 `servo` 的模拟移动）的训练 `policy`。你可以通过让 `policy` 预测给定 `goal`（`x-axis`）和 `position`（`y-axis`）`observation` 下的 `action`（值由 `shading` 表示）来生成这样的图表。完美的 `policy` 应该看起来像平滑的垂直条纹，之所以平滑是因为这是 `continuous action`，之所以垂直是因为我们想要一个直接映射到请求 `goal` 的 `action`；`position observation` 是无关的。

![](assets/rele_0609.png)

在 `training` 开始时，所有预测均为零。随着时间的推移，`agent` 学会了在 `goal` 较高时向上移动，在 `goal` 较低时向下移动。最终 `policy` 中存在一条微妙的对角线，因为 `model` 有足够的自由度做出这种选择，而各种 `random samples` 共同导致了这种 `policy` 而非完美的 `policy`。给予更多的 `training time` 并进行平均，这最终会变得完美。

[图 6-11](#ch06.html#fig_trained_policy_real) 展示了当让 `agent` 在真实 `servo` 上训练时非常相似的结果。对角线更明显，但这可能只是由于选定的特定 `random samples` 所致。但 `actions` 也不是那么明显。黑色不那么黑，白色不那么白。就好像在现实生活中，正确 `action` 存在更多的不确定性，我将其归因于请求的移动并不完美。例如，如果我请求 `position` 为 0.5，它实际上会移动到 0.52。我认为这种 `positional error` 表现为略微不确定的 `policy`。

![](assets/rele_0610.png)

当然，和以前一样，给予更多的 `training time` 并进行平均，将会产生一个完全可行且有用的 `policy`。

# 其他策略梯度算法

`Policy gradients`，尤其是 `off-policy PG algorithms`，是一个热门研究领域，因为直接从经验中学习 `policy` 具有内在吸引力。但在某些 `environments` 中，尤其是具有复杂 `state spaces` 的 `environments` 中，性能落后于 `value methods`。在本节中，我将介绍两条主要研究路线：稳定 `off-policy updates` 的其他方法，你已经看到了 `TRPO` 和 `PPO` 的形式；以及 `emphatic methods`，它们试图使用 `Bellman error` 的梯度，而不是 `advantages`。

## Retrace(λ)

`Retrace(λ)` 是一种早期的 `off-policy PG algorithm`，它使用加权和裁剪的 `importance sampling` 来帮助控制 `update` 中包含多少 `off-policy` 成分。^([25](#ch06.html#idm46428616407240))

Munos 等人将 `importance sampling ratio` 截断为最大值 1，这类似于 `TRPO/PPO` 中的悲观界，并约束了对 `policy` 的改进。在最好的情况下，差异很小，因此 `update` 是标准的 `A2C`。在最坏的情况下，`update` 可以忽略不计，因为两个 `policies` 差异太大。裁剪的原因是防止 `variance explosion`（重复的高值导致 `policy` 发生剧烈变化）。缺点是这会增加 `bias`；结果更稳定，但探索能力变弱。

回报还由 `λ` 加权，即来自[“资格迹”](#ch03.html#sec_eligibility_traces)的 `eligibility traces`。这根据之前的访问更新 `policy`，提供了一个 `hyperparameter` 来控制 `bias–variance trade-off`。

## 具有经验回放的 Actor-Critic (ACER)

`ACER` 通过包含 `experience replay`、`trust region policy optimization` 和 `dueling network architecture` 的组合来扩展 `Retrace(λ)`。^([26](#ch06.html#idm46428616398120))

首先，Wang 等人重新构建 `Retrace(λ)` 以补偿截断 `importance sampling` 引入的 `bias`。换句话说，`Retrace(λ)` 防止 `policy` 发生大幅变化，这使其更稳定（偏差更大），但探索能力变弱。`compensating factor` 按照 `Retrace(λ)` 中 `truncation` 的比例来补偿这种探索损失。

他们还包含一个更高效的 `TRPO` 实现，它维护一个由 `neural network` 预测的 `average policy`。如果你将 `KL divergence` 编码为惩罚项而非约束条件，则可以使用 `DL framework` 的 `auto-differentiation`。

最后，他们使用 `experience replay`（见[“经验回放”](#ch04.html#sec_experience_replay)）提高样本效率。对于 `continuous action spaces`，他们包含 `dueling architecture`（见[“决斗网络”](#ch04.html#sec_dueling_networks)）以减少 `variance`。这导致了两个独立的算法：一个用于 `discrete actions`，一个用于 `continuous`。

结果是一种算法，其性能接近带有 `prioritized experience replay`（见[“优先经验回放”](#ch04.html#sec_prioritized_replay)）的 `DQN`，但也能适用于 `continuous action spaces`。然而，所有的修正导致算法相当复杂，因此 `neural network architecture` 也很复杂。

## 使用 Kronecker 分解信任区域的 Actor-Critic (ACKTR)

在[“自然策略梯度和信任区域策略优化”](#ch06.html#sec_npg_trpo)中，你已经看到使用 `natural policy gradient` 并通过 `KL divergence` 约束是有益的，因为 `gradient` 独立于 `approximating function` 的 `parameterization`，且 `KL constraint` 提高了稳定性。然而，计算 `gradient` 需要昂贵的计算来计算 `Fisher information matrix`。`TRPO` 的 `linear projection` 是另一个低效来源。

2017 年，Wu 等人提出使用近似来加速计算。^([27](#ch06.html#idm46428616383656)) 他们使用 `Kronecker-factored approximation (K-FAC)`，即 `Fisher information matrix` 的一种近似，来提高计算性能，并使用该近似的 `rolling average` 代替 `linear projection`，以减少 `agent` 与 `environment` 交互的次数。
