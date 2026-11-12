
# Q-learning 和 SARSA 算法

## 算法 3-1：Q-learning

### 算法

```latex
\begin{aligned}
{\text{1:}} & {\mathbf{i}\mathbf{n}\mathbf{p}\mathbf{u}\mathbf{t}\text{:}\text{a}\text{policy}\text{that}\text{uses}\text{the}\text{action-value}\text{function,}\pi\left( {a \mid s,Q\left( s,a \right)} \right)} \\
{\text{2:}} & \left. \text{Initialize} Q\left( s,a \right)\leftarrow 0,\text{for}\text{all} s \in \mathcal{S},a \in \mathcal{A}\left( s \right) \right. \\
{\text{3:}} & {\mathbf{l}\mathbf{o}\mathbf{o}\mathbf{p}\text{:}\text{for}\text{each}\text{episode}} \\
{\text{4:}} & {\text{Initialize}\text{environment}\text{to}\text{provide} s} \\
{\text{5:}} & {\mathbf{d}\mathbf{o}\text{:}} \\
{\text{6:}} & {\text{Choose} a\text{from} s\text{using}\pi\text{,}\text{breaking}\text{ties}\text{randomly}} \\
{\text{7:}} & {\text{Take}\text{action,} a\text{,}\text{and}\text{observe} r,s'} \\
{\text{8:}} & \left. Q\left( s,a \right)\leftarrow Q\left( s,a \right) + \alpha\left\lbrack {r + \gamma\operatorname{argmax}\limits_{a_{s} \in \mathcal{A}{(s)}}Q\left( s^{'},a_{s} \right) - Q\left( s,a \right)} \right\rbrack \right. \\
{\text{9:}} & \left. s\leftarrow s' \right. \\
{\text{10:}} & {\mathbf{w}\mathbf{h}\mathbf{i}\mathbf{l}\mathbf{e} s\text{is}\text{not}\text{terminal}} \\
\end{aligned}
```

### 说明

[算法 3-1](#ch03.html#alg_q_learning) 展示了 `Q-learning` 的一种实现。你应该能认出该算法的大部分内容，因为它与 [算法 2-3](#ch02.html#alg_on-policy-mc-agent) 和 [算法 2-4](#ch02.html#alg_dp-policy) 密切相关。和之前一样，步骤 (1) 首先传入一个策略，用于决定采取哪个动作的概率（例如，可能实现来自 [算法 2-1](#ch02.html#alg_e-greedy) 的 `ϵ-greedy` 探索）。步骤 (2) 初始化一个数组以存储所有动作-价值估计。这可以通过一个自动默认为设定值的映射来高效实现。

步骤 (3) 遍历所有回合，步骤 (5) 遍历回合中的每一步。步骤 (4) 初始化起始状态。环境通常提供一个函数来 "`reset`" 智能体到默认状态。

步骤 (6) 使用外部策略函数来决定采取哪个动作。选择概率最高的动作。我比文献中典型的表述更明确地说明这一点，因为我想清楚地表明：永远不存在单一的最优动作。相反，一系列动作都有一定概率是最优的。`Q-learning` 选择概率最高的动作，这通常源自最大的 `Q(s,a)` 值。例如，你可以通过在一定比例的时间内将所有动作设置为相同概率来实现 `ϵ-greedy`。

步骤 (7) 在环境中使用选定的动作，并观察奖励和下一状态。步骤 (8) 根据在环境中观察到的新行为，更新给定状态和动作的当前动作-价值估计。请记住，期望回报计算为当前奖励与下一状态期望值之和。因此，差值（delta）就是这个和减去当前状态的期望值。步骤 (8) 还使用 `α` 参数对差值进行指数衰减，这有助于对嘈杂的更新进行平均。

步骤 (9) 设置下一状态，最后步骤 (10) 重复此过程，直到环境发出状态为终止状态的信号。随着足够多的回合 [步骤 (3)]，动作-价值估计将趋近于正确的期望回报。

请看 [算法 3-1](#ch03.html#alg_q_learning) 末尾的步骤 (8)。注意右边的 TD 误差项。`argmax_{a_s ∈ A(s)} Q(s', a_s)` 会检查该下一状态的所有可能动作，并选择期望回报最高的那个。这与步骤 (6) 中选择的动作无关。相反，智能体正在使用未来的动作规划一条路径。

这是一个 `off-policy`（离策略）智能体的首个实际示例。该算法是离策略的，因为更新仅影响当前策略。它不使用更新来指导智能体。智能体仍然必须从当前策略中推导出动作。这是一个微妙但至关重要的改进，直到最近才被利用。你可以在本书后面找到更多离策略算法的示例。

另一点值得注意的是步骤 (2) 中数组的使用。它在表中存储每个状态-动作对的期望价值估计，因此这通常被称为 `tabular`（表格型）方法。这一区别很重要，因为这意味着当你拥有连续状态或动作时，无法使用 [算法 3-1](#ch03.html#alg_q_learning)，那会导致数组长度无限。你将在 [第 4 章](#ch04.html#ch_deep_q_networks) 中看到能原生处理连续状态的方法，或在 [第 5 章](#ch05.html#ch_policy_gradient_methods) 中看到处理连续动作的方法。你可以在 [第 9 章](#ch09.html#ch_practical_rl) 中学习如何将连续值映射为离散值。

## SARSA

`SARSA` 在 `Q-learning` 之后不久被开发出来，旨在为 `TD learning`（时序差分学习）提供一个更通用的解决方案。与 `Q-learning` 的主要区别在于差值（delta）中缺少 `argmax`。相反，它通过对所有运行进行平均（以在线方式）来计算期望回报。

从 [公式 3-6](#ch03.html#eq_td_action_value_function) 可以看出，这种特定 `TD learning` 实现的名称源于计算动作-价值函数所需的状态、动作和奖励。`SARSA` 的算法如 [算法 3-2](#ch03.html#alg_sarsa) 所示。

### 公式 3-6：SARSA 动作-价值函数的在线估计

```latex
Q(s,a) ← Q(s,a) + α(r + γ Q(s',a') - Q(s,a))
```

[算法 3-2](#ch03.html#alg_sarsa) 和 [算法 3-1](#ch03.html#alg_q_learning) 都有超参数。第一个是 `γ`，即折扣期望回报的因子，在 [公式 2-7](#ch02.html#eq_discounted_expected_return) 中引入。第二个是动作-价值学习率 `α`，其值应在 0 和 1 之间。较高的值学习更快，但平均效果较差。

### 算法 3-2：SARSA（On-policy TD，同策略时序差分）

```latex
\begin{aligned}
{\text{1:}} & {\mathbf{i}\mathbf{n}\mathbf{p}\mathbf{u}\mathbf{t}\text{:}\text{a}\text{policy}\text{that}\text{uses}\text{the}\text{action-value}\text{function,}\pi\left( {a \mid s,Q\left( s,a \right)} \right)} \\
{\text{2:}} & \left. \text{Initialize} Q\left( s,a \right)\leftarrow 0,\text{for}\text{all} s \in \mathcal{S},a \in \mathcal{A}\left( s \right) \right. \\
{\text{3:}} & {\mathbf{l}\mathbf{o}\mathbf{o}\mathbf{p}\text{:}\text{for}\text{each}\text{episode}} \\
{\text{4:}} & {\text{Initialize}\text{environment}\text{to}\text{provide} s} \\
{\text{5:}} & {\mathbf{d}\mathbf{o}\text{:}} \\
{\text{6:}} & {\text{Choose} a\text{from} s\text{using}\pi\text{,}\text{breaking}\text{ties}\text{randomly}} \\
{\text{7:}} & {\text{Take}\text{action,} a\text{,}\text{and}\text{observe} r,s'} \\
{\text{8:}} & {\text{Choose} a'\text{from} s'\text{using}\pi\text{,}\text{breaking}\text{ties}\text{randomly}} \\
{\text{9:}} & \left. Q\left( s,a \right)\leftarrow Q\left( s,a \right) + \alpha\left\lbrack {r + \gamma Q\left( s^{'},a' \right) - Q\left( s,a \right)} \right\rbrack \right. \\
{\text{10:}} & \left. s\leftarrow s',a\leftarrow a' \right. \\
{\text{11:}} & {\mathbf{w}\mathbf{h}\mathbf{i}\mathbf{l}\mathbf{e} s\text{is}\text{not}\text{terminal}} \\
\end{aligned}
```
