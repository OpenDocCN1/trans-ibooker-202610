

# CartPole 环境与 DQN 分析

## 环境描述

该环境模拟在小车上平衡一根杆子。智能体可以向左或向右推动小车；这些就是动作。它用小车在 x 轴上的位置、小车的速度、杆尖的速度以及杆子的角度（0° 为竖直向上）来表示状态。智能体每走一步获得 1 的奖励。当杆子角度超过 ±12°、小车位置超过 ±2.4（显示区域边缘）或回合长度大于 200 步时，回合结束。要解决该环境，需要在连续 100 次试验中获得大于或等于 195 的平均奖励。

## Coach 预设与 DQN 配置

Coach 拥有[预设](https://oreil.ly/wDeC3)的概念，即已知有效的算法设置。[`CartPole_DQN`](https://oreil.ly/KTjRB) 预设提供了一个用 `DQN` 解决 `CartPole` 环境的方案。我采用了该预设并进行了一些修改，保留了以下参数：

*   每隔 100 个环境步骤，将目标网络权重复制到在线网络权重。
*   折扣因子设置为 0.99。
*   经验回放池的最大容量为 40,000 条经验。
*   使用恒定的 `ϵ`-greedy 策略，`ϵ` = 0.05（以使绘图结果一致）。
*   `NN` 使用基于 `MSE` 的损失函数，而非默认的 Huber 损失。
*   无环境“预热”来预填充经验池（以便从一开始就获得结果）。

在所有这些设置中，使用 `MSE` 损失（[公式 4-2](#ch04.html#eq_dqn_loss_function)）产生的影响最大。Huber 损失将损失截断为阈值以上的线性误差，否则为零。在更复杂的环境中，像 Huber 损失这样的绝对值损失有助于减轻离群值的影响。如果你使用 `MSE`，离群值会被平方，巨大的误差会淹没所有其他数据。但在这种情况下，环境简单且无噪声，所以 `MSE` 因其平方特性，有助于 `NN` 更快地学习，因为初始误差确实很大。因此，Huber 损失往往是 RL 中使用的默认损失函数。

### 提示

一般来说，我建议在有噪声的环境中使用绝对值损失，在更简单、无噪声的环境中使用平方损失。

## 基准智能体

为了给 `DQN` 算法提供基准，我在 Coach 中实现了两个新智能体。随机智能体在每一步随机选择一个动作。`Q-learning` 智能体实现了 [“Q-Learning”](#ch03.html#sec_q_learning) 中描述的 `Q-learning`。回想一下，表格型 `Q-learning` 无法处理连续状态。我通过将所有状态乘以 10、四舍五入到最接近的整数并转换为整数，成功用 `Q-learning` 解决了 `CartPole` 环境。我还仅使用杆子的角度和角速度（观测数组中的第三和第四个元素）。这导致 Q 值查找表中大约有 150 个状态。

[图 4-1](#ch04.html#fig_cartpole_episode_rewards) 展示了三个智能体的回合奖励。`DQN` 和 `Q-learning` 都能找到最优策略。随机策略能获得约 25 步的平均奖励。这是由环境物理特性决定的。杆子倒下需要那么长时间。`Q-learning` 智能体初期表现优于 `DQN`。这是因为 `DQN` 需要一定量的数据才能训练出一个合理的 Q 值模型。所需数据的确切数量取决于深度神经网络的复杂度和状态空间的大小。`Q-learning` 智能体有时表现不佳，是由于 `ϵ`-greedy 随机动作导致的。相比之下，`DQN` 能够泛化到未见过的状态，因此表现更稳定。

![](assets/rele_0401.png)

图 [4-2](#ch04.html#cartpole_example_random) 至 [4-4](#ch04.html#cartpole_example_dqn) 分别展示了随机、`Q-learning` 和 `DQN` 智能体的一个示例回合。`Q-learning` 和 `DQN` 的示例均持续 200 步，这是一个回合的最大步数。

![](assets/rele_04vid01.png)![](assets/rele_04vid02.png)![](assets/rele_04vid03.png)

```html
<figure id="cartpole_example_random"> 
  <video controls="controls" style="margin: 0 auto; max-width: 100%;" src="videos/cartpole_random.mp4" loop="loop" autoplay="autoplay">The video element is not supported</video> 
  <figcaption>随机智能体在 CartPole 环境上的示例</figcaption> 
</figure> 
<figure id="cartpole_example_q_learning"> 
  <video controls="controls" style="margin: 0 auto; max-width: 100%;" src="videos/cartpole_q_learning.mp4" loop="loop" autoplay="autoplay">The video element is not supported</video> 
  <figcaption>Q-learning 智能体在 CartPole 环境上的示例。</figcaption> 
</figure> 
<figure id="cartpole_example_dqn">
  <video controls="controls" style="margin: 0 auto; max-width: 100%;" src="videos/cartpole_dqn.mp4" loop="loop" autoplay="autoplay">The video element is not supported</video> 
  <figcaption>DQN 智能体在 CartPole 环境上的示例。</figcaption> 
</figure>
```

## 为什么要在线训练？

此时你可能会问，为什么要在线训练。你可以从环境中生成并存储 200 个回合的数据，然后以监督学习的方式离线训练模型。对于简单环境这虽可行，但效率低下。生成数据时，智能体仍需遵循某个策略。你可以选择随机策略，但这不太可能生成长回合。以 `CartPole` 环境为例。你认为随机策略需要多少次尝试才能达到 200 个时间步？很多。在大多数应用中，智能体需要在线学习，以便观察后续状态。

第二个原因是智能体必须产生*反事实*动作（可能并非最优的动作），以确认它确实拥有最优策略。这是探索的核心。动作产生新的、未见过的轨迹。其中之一将是最优的。如果你没有序列环境，或者不需要探索来寻找最优策略，则使用标准机器学习。我在[“从离线数据学习”](#ch04.html#sec_learning_from_offline_data)中进一步讨论这一点。

## 哪个更好？DQN 对比 Q-learning

结果表明，标准 `Q-learning` 与 `DQN` 一样好。而且我没有应用 [“Q-Learning 的扩展”](#ch03.html#sec_extentions_q_learning) 中看到的任何 `Q-learning` 改进。考虑到增加 `NN` 的复杂性，为什么要用 `DQN`？

为了让 `Q-learning` 奏效，我不得不大幅减少信息量。我移除了半数特征，并对剩余特征进行量化，仅留下约 150 个离散值。为了找到这些设置，我必须进行特征工程，包括尝试许多其他特征组合和量化级别——这项工作本身就能写成一本书。如果你想在生产环境中运行此类模型，这可能值得付出努力，因为简单性会使决策更易解释，模型也会更稳健、更高效、更稳定，等等。

使用 `DQN` 时，我什么都没改。`NN` 能够接收原始信息，弄清哪些特征重要，并告诉我哪个动作是最优的。模型可以融入连续值，使结果比表格方法更精细。如果你的领域更复杂且特征工程变得困难，`NN` 可以是一个高效的解决方案。但要注意所有常见的 `NN` 警示依然适用：它们需要大量数据，容易过拟合，对初始条件和超参数敏感，不具备韧性或鲁棒性，且易受对抗攻击（参见[“安全 RL”](#ch10.html#rl_secure_rl)）。

## 案例研究：降低建筑能耗

