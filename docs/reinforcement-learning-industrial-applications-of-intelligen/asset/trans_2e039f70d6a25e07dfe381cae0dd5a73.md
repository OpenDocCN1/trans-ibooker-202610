

步骤 (13) 更新一个指向 `n` 步之前的状态-动作对的变量。最初，这将在 `t` = 0 之前，因此它会进行检查以防止步骤 (14) 中出现索引越界错误。如果 `τ >= 0`，则 [算法 3-3](#ch03.html#alg_n_step_sarsa) 开始使用预期回报更新动作-价值函数。

步骤 (15) 计算 [公式 3-7](#ch03.html#eq_2_step_expected_return) 中定义的 `n` 步预期回报的奖励部分。步骤 (16) 检查算法是否已超过回合结束点加上 `n`。如果没有，则将下一状态的预测值加到预期回报中，正如 [公式 3-7](#ch03.html#eq_2_step_expected_return) 右侧所定义的那样。

步骤 (18) 与之前相同；它使用 `n` 步预期价值估计来更新动作-价值函数。最后，步骤 (19) 增加步数计数器。在更新直到终止的所有步骤后，算法在步骤 (20) 返回主循环。

## 网格环境上的 n 步算法

我在网格环境上实现了一个 `n` 步 SARSA 智能体，并将步数设置为 2 和 4。所有其他设置与标准 SARSA 实现相同。您可以在 [图 3-7](#ch03.html#fig_n_step_sarsa_comparison) 中看到标准 SARSA 与 `n` 步 SARSA 的区别。

从 [图 3-7](#ch03.html#fig_n_step_sarsa_comparison) 中可以明显看出，`n` 步算法比其 `1` 步对应算法能更快地学习最优策略。这在直觉上讲得通。如果你能观察到“未来”（加引号是因为现实是你在延迟更新），那么你就能更早地学习到最优轨迹。`n` 的最优值取决于具体问题，应视为一个需要调整的超参数。这些方法的主要缺点是额外的计算和内存负担。

本节重点关注在线策略 SARSA。当然，你也可以实现离线策略和非 SARSA 的 `n` 步 TD 学习。您可以在 [“延伸阅读”](#ch03.html#ch03-further-reading) 中列出的书籍里阅读更多相关内容。

![](assets/rele_0307.png)

# 资格迹

[“n 步算法”](#ch03.html#n_step_algorithms) 开发了一种方法，以同时提供自举法和前向规划的优势。这一想法的延伸是对不同的 `n` 值进行平均，例如 `2` 步和 `4` 步 SARSA 的平均。你可以将这一想法推向极致，对所有 `n` 值进行平均。

当然，对不同的 `n` 步更新（[公式 3-9](#ch03.html#eq_n_step_td_update_rule)）进行迭代会增加大量的计算复杂度。此外，随着 `n` 的增加，智能体在能够开始更新第一个时间步之前必须等待越来越长的时间。在极端情况下，这将像蒙特卡洛方法一样糟糕，必须等到回合结束才能进行任何更新。

迄今为止见到的算法的问题在于，它们试图向前看时间，这是不可能的，因此它们转而延迟更新，假装自己在向前看。一种解决方案是向后看时间，而不是向前。如果你试图以最快时间爬上一座山，你可以尝试几条路线，然后回顾你的行动以找到最佳轨迹。

换句话说，智能体需要一种新机制将状态-动作对标记为“已接触”，以便它能回来并用未来的更新来更新该状态。你可以使用*示踪剂*来标记感兴趣事物的位置。放射性示踪剂有助于诊断疾病和定向给药。地球物理学家在水力压裂过程中将它们注入岩石，以提供裂缝大小和位置的证据。你可以使用虚拟示踪剂来提醒智能体哪些状态-动作估计将来需要更新。

接下来，你需要用所有 `n` 步回报的平均值来更新状态-动作函数。一步更新是 TD 误差更新（[公式 3-6](#ch03.html#eq_td_action_value_function)）。你可以使用指数加权移动平均来近似平均值，这是平均值的在线等价形式（如 [“策略评估：价值函数”](#ch02.html#value-function) 所示）。感兴趣的读者可以在 [“延伸阅读”](#ch03.html#ch03-further-reading) 中找到其等价于平均多个 `n` 步回报且收敛的证明。

将一步更新与移动平均相结合，再加上示踪剂的想法，提供了一种在线 SARSA，它具有零延迟（无直接 `n` 步前瞻），但拥有前向规划和自举法的所有好处。既然你已经见过这些数学原理，让我们直接深入了解 [算法 3-4](#ch03.html#alg_lambda_sarsa) 中的算法。

### 算法 3-4. SARSA(λ)

```
\begin{array}{cl} {\text{1:}} & {\mathbf{input: a policy that uses the action-value function, } \pi(a \mid s, Q(s,a)), \text{ step size } 0 < \alpha < 1, \text{ a tracer decay rate } 0 \leq \lambda \leq 1} \\ {\text{2:}} & \left. \text{Initialize } Q(s,a) \leftarrow 0, \text{ for all } s \in \mathcal{S}, a \in \mathcal{A}(s) \right. \\ {\text{3:}} & {\mathbf{loop for each episode:}} \\ {\text{4:}} & \left. \text{Initialize } z(s,a) \leftarrow 0, \text{ for all } s \in \mathcal{S}, a \in \mathcal{A}(s), \text{ and default state } s \right. \\ {\text{5:}} & {\text{Choose } a \text{ from } s \text{ using } \pi, \text{ breaking ties randomly}} \\ {\text{6:}} & {\mathbf{do:}} \\ {\text{7:}} & {\text{Take action, } a, \text{ and observe } r, s'} \\ {\text{8:}} & {\text{Choose } a' \text{ from } s' \text{ using } \pi, \text{ breaking ties randomly}} \\ {\text{10:}} & \left. \delta \leftarrow r + \gamma Q(s',a') - Q(s,a) \right. \\ {\text{11:}} & \left. z(s,a) \leftarrow z(s,a) + 1 \right. \\ {\text{12:}} & {\mathbf{for each } s \in \mathcal{S}:} \\ {\text{13:}} & {\mathbf{for each } a \in \mathcal{A}(s):} \\ {\text{14:}} & \left. Q(s,a) \leftarrow Q(s,a) + \alpha \delta z(s,a) \right. \\ {\text{15:}} & \left. z(s,a) \leftarrow \gamma \lambda z(s,a) \right. \\ {\text{16:}} & \left. s \leftarrow s', a \leftarrow a' \right. \\ {\text{17:}} & {\mathbf{while } s \text{ is not terminal}} \\ \end{array}
```

[算法 3-4](#ch03.html#alg_lambda_sarsa) 中的大部分代码重复了 [算法 3-2](#ch03.html#alg_sarsa)，所以我就不重复了。不同之处始于步骤 (10)，它暂时计算一步 TD 误差以供后续使用。步骤 (11) 通过增加代表当前状态-动作对的表格中的单元格来实现示踪剂。这就是算法“记住”在轨迹中接触过哪些状态和动作的方式。

步骤 (12) 和 (13) 循环遍历所有状态和动作，以在步骤 (14) 中更新动作-价值函数。这里的新增部分是它用当前的示踪剂值对 TD 误差进行加权，该值在步骤 (15) 中呈指数衰减。换句话说，如果智能体很久以前接触过该状态-动作对，则更新幅度可忽略不计。如果它在上一个时间步接触过，则更新幅度较大。

