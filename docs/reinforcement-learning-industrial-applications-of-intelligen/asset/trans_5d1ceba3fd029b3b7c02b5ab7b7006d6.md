
# TD3 算法伪代码

```
\begin{array}{cl} 
{\text{1:}} & {\text{input: an actor neural network, } \mu_{\theta_{\mu}}(s) \text{, with weights } \theta_{\mu} \text{,}} \\ 
& {\text{two critic neural networks, } Q_{\theta_{1}}(s,a), Q_{\theta_{2}}(s,a) \text{, with weights } \theta_{1}, \theta_{2} \text{,}} \\ 
& {\text{a replay buffer, } B \text{,}} \\ 
& {\text{the size of a training minibatch, } N \text{,}} \\ 
& {\text{the network update rate, } \tau \text{,}} \\ 
& {\text{the discount-rate parameter, } \gamma \text{,}} \\ 
& {\text{a noise clipping threshold, } c \text{,}} \\ 
& {\text{and a noise function for exploration, } \mathcal{N}} \\ 
{\text{2:}} & {\text{Initialize } \theta_{\mu}, \theta_{1}, \theta_{2} \text{,}} \\ 
& {\text{and the target weights, } \theta_{\mu}' \leftarrow \theta_{\mu} \text{, } \theta_{1}' \leftarrow \theta_{1} \text{, } \theta_{2}' \leftarrow \theta_{2}} \\ 
{\text{3:}} & {\text{loop: for each episode}} \\ 
{\text{4:}} & {\text{Initialize } s} \\ 
{\text{5:}} & {\text{while } s \text{ is not terminal:}} \\ 
{\text{6:}} & {a \leftarrow \mu_{\theta_{\mu}}(s) + \mathcal{N}} \\ 
{\text{7:}} & {\text{Take action, } a \text{, and observe } r, s'} \\ 
{\text{8:}} & {\text{Store } (s, a, r, s') \text{ in } B} \\ 
{\text{9:}} & {\text{Sample a random minibatch of } N \text{ transitions, } (s_i, a_i, r_i, s_i') \text{, from } B} \\ 
{\text{10:}} & {\tilde{a}_i \leftarrow \mu_{\theta_{\mu}'}(s_i') + \operatorname{clip}(\mathcal{N}, -c, c) \text{ (Target policy smoothing)}} \\ 
{\text{11:}} & {y_i \leftarrow r_i + \gamma \min_{j=1,2} Q_{\theta_j'}(s_i', \tilde{a}_i) \text{ (Clipped double Q-learning)}} \\ 
{\text{12:}} & {\text{Update both critics, } \theta_j \leftarrow 1,2 \text{, by minimizing: } \frac{1}{N} \sum_i (y_i - Q_{\theta_j}(s_i, a_i))^2} \\ 
{\text{13:}} & {\text{if } t \bmod d \text{ then: (Delayed policy update)}} \\ 
{\text{14:}} & {\text{Update the actor by minimizing: } \frac{1}{N} \sum_i (\nabla_{\theta_{\mu}} Q_{\theta_1}(s_i, \mu_{\theta_{\mu}}(s_i)))^2} \\ 
{\text{15:}} & {\text{Update both critics, } \theta_j \leftarrow 1,2 \text{: } \theta_j' \leftarrow \tau \theta_j + (1 - \tau) \theta_j'} \\ 
{\text{16:}} & {\theta_{\mu}' \leftarrow \tau \theta_{\mu} + (1 - \tau) \theta_{\mu}'} \\ 
{\text{17:}} & {\text{end if}} \\ 
{\text{18:}} & {s \leftarrow s'} \\ 
\end{array}
```

## 算法说明

在步骤 (1) 中，你现在需要传入两个评论家网络，分别用参数 `θ₁` 和 `θ₂` 表示。大多数实现期望你传入两个独立的深度学习网络，通常它们具有完全相同的架构。但不要因此而阻止你尝试不同的架构，或者可能的多头架构。

原始算法会裁剪动作噪声，以使结果保持接近原始动作。其意图是防止智能体选择无效动作。大多数实现将此值设置为与环境的动作空间匹配。我暂时建议，因为你控制噪声函数的设计，所以裁剪是不必要的。选择一个与你的动作空间和平滑目标相匹配的噪声函数，然后复用 DDPG 中的标准确定性动作函数 [步骤 (6)]。

在步骤 (11) 中请注意，预测值是来自目标网络的两个评论家中的最小值。换句话说，选择最低的动作价值估计；由于高估偏差，这更有可能是正确的。步骤 (12) 使用这个折扣奖励的预测来更新两个评论家。这有助于抑制那个正在高估的网络。

步骤 (13) 之后的内部循环每 `d` 次迭代发生一次；这延迟了策略更新。第 (14) 行很有趣，因为它使用第一个评论家来更新策略，而不是使用具有最小值的评论家。我怀疑这里的推理是：因为高估的网络受到步骤 (12) 的约束，从长远来看，无论你选择网络一还是网络二都没有区别。任一网络都同样可能高估。但你可能会发现，通过更智能地选择使用哪个网络，可以获得微小的性能提升。

所有其他行与 DDPG 相同。

### 提示

始终参考原始论文，并添加足够的监控代码来验证你的实现。我发现展开循环并手动计算是验证代码是否按预期工作的最佳方法。

# 案例研究：基于评论的推荐

在信息过载的时代，用户越来越依赖推荐算法来推荐相关内容或产品。最近，YouTube 和 TikTok 等服务开始利用交互式推荐系统，根据用户的反馈持续向个别用户推荐内容。最先进的非强化学习方法试图构建将用户映射到物品的模型，利用卷积滤波器挖掘局部相关模式，或使用基于注意力的序列预测模型。其中，序列模型表现最好，因为它们能适应反复的反馈。但这些方法在离线训练，且必须经常重新训练以提供最新的推荐。加上深度学习模型的复杂性，这导致了巨大的训练成本。

强化学习天然适合这类问题，因为反馈可以直接通过奖励定义来适应。Wang 等人提出了一个例子，他们通过建议使用文本作为反馈机制，而不是通常的点赞或点踩，使问题更有趣。他们建议，在现实世界中，稀疏投票会严重降低性能。相反，他们建议像评论和物品描述这样的文本信息包含更多信息，且对稀疏性不那么敏感。^([12](#ch06.html#idm46428616666584))

Wang 等人使用马尔可夫决策过程来形式化该问题，其中状态是用户与推荐系统之间的交互，动作是推荐的物品数量，奖励由用户反馈决定。

正如其他推荐论文所指出的，主要问题是用户和物品的巨大数量带来的高维度。Wang 等人采用词嵌入将物品文本和用户评论映射为特征向量，然后使用聚类方法将特征向量减少为可管理数量的组。
