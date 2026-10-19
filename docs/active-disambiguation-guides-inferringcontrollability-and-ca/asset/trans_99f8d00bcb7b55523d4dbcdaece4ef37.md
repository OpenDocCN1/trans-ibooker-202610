

session, 一个场图（体素大小 = 2.4×2.4×2.4 mm, TR = 482.0ms, TE1 = 4.92ms, TE2 = 7.38ms）被采集以减少空间畸变。对扫描直接进行了偏置校正。我们在任务期间采集了生理记录以测量参与者的脉搏、呼吸和皮肤电导。

影像数据使用 FMRIB 软件库（FSL）⁸⁷ 进行分析。预处理阶段包括运动校正、通过应用场图校正空间畸变、大脑提取、空间平滑（使用半高全宽为 3 mm），以及高通时间滤波。功能图像首先与个体 T1 结构图像共配准，然后使用 12 个自由度非线性配准到蒙特利尔神经学研究所（MNI）模板。

## fMRI 全脑分析

一级分析使用 FSL FEAT（版本 6.00）进行。首先，使用 FSL FILM 对数据进行预白化以考虑时间自相关。模型中包含了时间导数、标准运动校正参数和生理噪声回归变量（使用 FSL PNM 工具箱⁸⁸）。结果采用自动离群值降权获得，并使用 FSL FLAME1，聚类校正阈值为 z > 3.1 且 *p* < 0.05⁸⁹。

我们对全脑分析使用了一个单一的 GLM，并将试验的三个阶段（评定、行动、结果）全部纳入 GLM。常数和参数回归变量与相关试验事件的起始时间锁定，并具有相应事件的持续时间。所有参数回归变量均标准化为均值为零、标准差为 1。所有回归变量均在 FEAT 中与双伽马 HRF 卷积。回归变量之间的相关性显示在补充图 12 中。

### Rating phase.
评定阶段始于评定屏幕出现的时间，并持续到参与者完成所有评定为止。为考虑 Self-Other 与 Control-Other 阶段之间的差异，控制评定和自我评定相关的回归变量为每个任务阶段单独定义。其他评定相关的回归变量在两个任务阶段中均定义，因为参与者在两个任务阶段中都需要以类似的方式学习他人的表现水平。

- Control rating related regressors, time locked to rating screen onset:
  - constant for control rating
  - parametric control rating
- Self rating related regressors, time locked to self rating onset (in the Self-Other phase, when the true control was shown, this was time locked to the rating screen onset + 1.23sec, the mean RT for making a rating (computed from the Self-only trials)):
  - constant for self rating
  - parametric self rating
- Other rating related regressors, time locked to other rating onset
  - constant for other rating
  - parametric other rating

为每个评定的反应时（RT）纳入了无兴趣的参数回归变量（按任务阶段分开，控制评定 RT 仅用于 Control-Other 阶段）。

### Action phase.
行动阶段始于参与者进行游戏时，并持续到游戏结束。回归变量在 Self-Other 和 Control-Other 阶段中定义，因为行动阶段在任务阶段之间没有差异。

- game-type specific constants (4 in total, 1 for each of the 4 games)
- active disambiguation (binary, prior to normalizing: 1 for AD trial, 0 for normal trial)
- parametric uncertainty (extracted from Active learning model)
- switch trial (binary, prior to normalizing: 1 for switch trial, 0 for stay trial)

此外，还纳入了一个行动阶段 RT 的无兴趣参数回归变量，以控制 AD 试验上的 RT 差异。另一个无兴趣参数回归变量用于游戏块中的试验编号，以控制试验编号（作为块内时间的度量）对不确定性的影响。

### Outcome phase.
结果阶段始于反馈出现在屏幕上时，并持续到参与者按下‘Continue’按钮为止。回归变量根据任务阶段定义，因为我们假设每个任务阶段的学习机制不同。

- Constant for outcome phase
- Parametric regressors in Self-Only phase:
  - sPE (on normal trials only)
  - oPE (on normal trials only)
  - signed uncertainty change (from the current to the next trial)
- Parametric regressors in the Control-Other phase:
  - Feedback difference (defined as normal − AD feedback, on switch trials only)
  - signed uncertainty change (from the current to the next trial)

我们在结果阶段纳入了无兴趣回归变量。为考虑与运动相关的脑活动，我们纳入了一个常数，该常数与参与者按下‘Continue’按钮的时间锁定，持续时间为 1 秒。对每个任务阶段分别而言，我们纳入了切换试验和主动消歧的二元回归变量（这两个回归变量的定义与行动阶段中相应回归变量类似）。

## fMRI ROI 时间进程分析

### ROI selection and time series extraction.
与之前的工作³,¹⁷,³⁸,⁹⁰,⁹¹ 类似，基于全脑分析中显著激活簇构建了半径为三个体素（5 mm）的球形 ROI。ROI 以行动时不确定性效应的峰值体素为中心，并且不确定性相关簇与行动时 AD 效应重叠之处。这适用于三个脑区，我们因其位于枕叶皮层而将其排除在分析之外。剩下的两个 ROI 位于左侧 SMG（MNI: x=-62, y=-24, z=36）和左侧顶叶上小叶的 7 区（MNI: x=-12, y=-64, z=64）。为确保 ROI 中仅包含脑组织，我们将左侧 SMG 的中心向内侧移动两个体素，将左侧 7 区向下方移动一个体素。

球形 ROI 掩膜使用标准到结构变形场从标准空间转换到每个参与者的结构空间，并使用结构到功能仿射矩阵从结构空间转换到功能空间。得到的转换后掩膜被阈值化和二值化。然后，从滤波后的功能数据中剔除运动和生理噪声。每个 ROI 的时间序列从残差滤波功能数据中提取，并跨体素取平均。时序数据被标准化，并使用三次样条插值上采样 20 倍。所得数据被分割为 17 秒窗口，即每个感兴趣事件（见下文 GLM 部分所述）前 2 秒后 15 秒。我们对每个时间点应用普通最小二乘 GLM，从而为每个回归变量得到一组 beta 权重的时程。所有参数回归变量均已标准化（均值为 0，标准差为 1）。在以下所有 ROI-GLM 中，我们除了下面描述的参数回归变量外还纳入了常数回归变量。

### Time course GLMs (ROI-GLMs).
我们假设这些参与不确定性和 AD 行动生成处理的区域也可能参与反馈结果的处理。因此，我们的时间进程分析独立于用于定义 ROI 的全脑分析。所有 ROI-GLM 的回归变量之间的相关性显示在补充图 13 中。

我们的总体假设是，大脑基于当前观察到的反馈差异和先前的逆自我估计来表示推断的控制信念（图 6a）。首先，我们检验 SMG 和 7 区是否表示推断的控制估计及其更新。因此，我们运行了以下 ROI-GLM1，与当前试验的结果时间锁定（图 6b）：

```
BOLD ~ control + control update
(26)
```

其中 control 为下一试验的控制评定，因此是参与者从当前观察到的反馈中推断出的控制水平的度量。Control update 为下一试验与当前试验控制评定的差值，因此是参与者基于当前观察到的结果改变其控制评定的程度的度量。此 ROI-GLM1 包含 Control-Other 阶段的所有试验。

我们下一个 GLM 的目标是检验 ROI 活动是否与反馈差异和逆自我相关，而这些是控制推断所必需的成分。在此，我们检验了两个竞争假设（见图 6c-d），即我们的 ROI 可能在结果时如何表示反馈差异。首先，我们假设神经活动可能表示

