
减小状态空间（即强化学习算法的输入）将可靠地将 RL 训练时间从数天缩短到几分钟级别。我在[“变换（降维、自编码器和世界模型）”](#ch09.html#sec_world_models)中详细讨论了这一点。

## 显著动作

在某些环境中，可以施加显著改变环境的动作。以 `DonkeyCar` 为例，转向动作的大数值会导致赛车旋转、打滑或以其他方式偏离最优轨迹。但当然，赛车需要转向，有时还需要通过急弯。

但这些动作及随后的负面奖励可能导致训练陷入停滞，因为它持续打滑，永远无法捕获足够的优质观测值。在 `DonkeyCar` 环境中，当允许动作出现大幅跳变时，这种影响会变得更糟。例如，当你骑自行车时，你知道在高速下急转方向在技术上是可能的，但我怀疑你从未尝试过。你可能尝试过幅度小得多的转向并有过糟糕的体验，因此你永远不会尝试类似将方向转 90 度这样的操作。[图 7-12](#ch07.html#fig_original_action_space) 展示了保持原始动作空间不变时的一些训练示例画面。

```
![](assets/rele_07vid02.png)<figure id="fig_original_action_space"> <video controls="controls" style="margin: 0 auto; max-width: 100%;" src="videos/original_action_space.mp4" loop="loop" autoplay="autoplay">The video element is not supported</video> <figcaption>A video of DonkeyCar training when the action space is left unaltered.</figcaption> </figure>
```

你可以通过在 RL 模型中包含这些先验信息来影响智能体的训练。你可以修改奖励信号以鼓励（或抑制）特定状态。或者，如本例所示，你可以过滤智能体的动作，使其表现得更安全。

如果你尝试使用完整范围的转向角度来训练智能体（试试看！），你会发现经过很长一段时间后，它学会了避开边缘，但方式过于猛烈，导致赛车陷入三轮漂移。尽管这看起来很酷，但无助于完成赛道。你需要让转向平滑过渡。

约束智能体行为最简单的方法之一是修改动作空间。就像在机器学习中一样，约束问题会使其更易求解。在[图 7-13](#ch07.html#fig_simplified_action_space) 所示的训练中，我将油门限制在最大 0.15（原为 5.0），将转向角限制在绝对最大 0.5（原为 1.0）。低速防止赛车打滑，较小的转向角有助于缓解转向突变。请注意，训练性能极大地受这些参数影响。请自行尝试修改它们。

```
![](assets/rele_07vid03.png)<figure id="fig_simplified_action_space"> <video controls="controls" style="margin: 0 auto; max-width: 100%;" src="videos/simplified_action_space.mp4" loop="loop" autoplay="autoplay">The video element is not supported</video> <figcaption>A video of DonkeyCar training after simplifying the action space.</figcaption> </figure>
```

## 超参数搜索

最大油门和转向设置是问题的超参数；它们不会被直接调优。问题定义的变化往往对性能影响最大，但 `SAC` 算法还有许多其他超参数，如策略模型的架构。目标是找到能带来最佳奖励的参数组合。为此，你需要执行超参数搜索，编写代码遍历所有排列并重新运行训练。你可以在[“超参数调优”](#ch10.html#sec_hyperparameter-tuning)中阅读更多相关内容。

我使用 `optuna` 库运行了优化的网格搜索来调优 `SAC` 算法的超参数，并在 `TensorBoard` 中查看了所有结果。我发现可以将学习率提高一个数量级，从而略微加快学习速度。调整熵系数也有帮助；默认情况下 `stable-baselines` 实现从 1.0 开始并学习最优值。但在本例中，智能体训练步数很少，没有时间学习最优系数。从 0.1 开始并学习最优系数效果更好。但总体而言，我对超参数对性能的影响程度感到失望（这是件好事！）。变分自编码器的超参数修改和动作空间的改变对性能的影响大得多。

## 最终策略

完成所有工程工作、代码编写和超参数搜索后，我现在获得了一个可行的策略，其训练环境步数在 5,000 到 50,000 之间，具体取决于环境以及你希望策略多完美。这在我的旧款 2014 Macbook Pro 上大约耗时 10–30 分钟。[图 7-14](#ch07.html#fig_final_policy_track) 和 [图 7-15](#ch07.html#fig_final_policy_road) 分别展示了 `donkey-generated-track-v0` 和 `donkey-generated-road-v0` 环境的示例策略。

```
![](assets/rele_07vid04.png)![](assets/rele_07vid05.png)<figure id="fig_final_policy_track"> <video controls="controls" style="margin: 0 auto; max-width: 100%;" src="videos/final_policy_track.mp4" loop="loop" autoplay="autoplay">The video element is not supported</video> <figcaption>A video of the final DonkeyCar policy for the `donkey-generated-track-v0` environment.</figcaption> </figure> <figure id="fig_final_policy_road"> <video controls="controls" style="margin: 0 auto; max-width: 100%;" src="videos/final_policy_road.mp4" loop="loop" autoplay="autoplay">The video element is not supported</video> <figcaption>A video of the final DonkeyCar policy for the `donkey-generated-road-v0` environment.</figcaption> </figure>
```

但请注意，整个过程中我几乎未涉及 RL 算法本身。所有艰苦的工作都投入到了特征工程、超参数搜索以及约束问题以使其简化上。这些正是工业 ML 项目中耗时最多的任务。同样像 ML 一样，RL 算法将随时间变得标准化，这将导致你完全不需要接触 RL 算法的局面。

## 进一步改进

你可以对该策略进行多方面的改进。等你读到这些文字时，其他工程师早已将我这微薄的努力远远甩在身后。你可以花时间改进自编码，或尝试[“最小化训练时间”](#ch07.html#sec_minimizing_training)中建议的其他想法。你可以进一步修改动作空间，比如增加最大动作直到其失效。我曾想尝试退火动作空间限制，即动作空间开始时很小，并在训练过程中逐渐增加到最大值。你也可以尝试修改奖励；也许速度不应获得过高奖励。惩罚突变（jerkiness）也很有意义。

你大概能想象出更多改进，但如果你在解决工业问题，则需要更谨慎：小步快跑，确保评估正确，确保操作安全，拆分问题，约束问题，并进行更多测试。你可以在[第 9 章](#ch09.html#ch_practical_rl)中阅读更多相关内容。

# 总结

RL 旨在寻找一种策略，以最大化从采样状态、动作和奖励中获得的期望回报。这一定义意味着不顾后果地最大化期望回报。研究表明，对策略更新进行边界约束或限制能带来更稳定的学习和更全面的策略。
