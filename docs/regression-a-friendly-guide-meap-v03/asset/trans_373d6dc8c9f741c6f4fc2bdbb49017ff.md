

没有芽（左边的直方图）的拟合概率小于 50%。区分系数用一个数字概括了我们所看到的内容——如果我们令 π̄_S 和 π̄_F 分别表示成功和失败的拟合概率的平均值，则

```
R_D^2 = π̄_S - π̄_F.
```

对于紫色数据，区分系数与 R_M^2 和 R_S^2 非常吻合：

```
fit_avgs = chasm.groupby("Bud").agg(Fit_average=('Fit_prob', 'mean'))
fit_avgs

Fit_average
Bud
    0    0.139026
    1    0.807023

R2_D = fit_avgs["Fit_average"][1] - fit_avgs["Fit_average"][0]
R2_D

0.6679966136273958
```

正如名称所示，区分系数衡量了 logistic 模型区分成功和失败的能力；这让人想起基于 Photoperiod 的二元分类器。与其只选择其中一种诊断统计量，不如尝试全部三种——它们应该表现相似，并证实似然比检验的结果。

## 4.4 Examples

### 4.4.1 Intubation and throat pain

在《生物统计学百科全书》的一篇文章中，David Collett 提供了 35 名患者术后咽喉疼痛的以下数据：

```
throat = pd.read_csv("throat.csv")
throat

Age  Sex  Lubricant  Duration  Device  SoreThroat
0   48    1         0        45       0          0
```

|  | Age | Sex | Lubricant | Duration | Device | SoreThroat |
|---|---|---|---|---|---|---|
| 1 | 48 | 1 | 0 | 15 | 0 | 0 |
| 2 | 39 | 0 | 1 | 40 | 0 | 1 |
| 3 | 59 | 1 | 0 | 83 | 1 | 1 |
| 4 | 24 | 1 | 1 | 90 | 1 | 1 |
| 5 | 55 | 1 | 1 | 25 | 1 | 1 |
| 6 | 35 | 0 | 1 | 35 | 0 | 1 |
| 7 | 23 | 1 | 1 | 65 | 0 | 1 |
| 8 | 57 | 0 | 1 | 95 | 0 | 1 |
| 9 | 34 | 1 | 1 | 35 | 0 | 1 |
| 10 | 56 | 0 | 1 | 75 | 0 | 1 |
| 11 | 35 | 0 | 0 | 45 | 1 | 1 |
| 12 | 37 | 0 | 1 | 50 | 1 | 0 |
| 13 | 30 | 1 | 1 | 75 | 1 | 1 |
| 14 | 45 | 1 | 1 | 30 | 0 | 0 |
| 15 | 60 | 1 | 0 | 25 | 0 | 1 |
| 16 | 35 | 1 | 1 | 20 | 1 | 0 |
| 17 | 41 | 1 | 0 | 60 | 1 | 1 |
| 18 | 67 | 0 | 1 | 70 | 1 | 1 |
| 19 | 25 | 0 | 0 | 30 | 0 | 1 |
| 20 | 63 | 0 | 1 | 60 | 0 | 1 |
| 21 | 26 | 0 | 1 | 61 | 0 | 0 |
| 22 | 47 | 0 | 0 | 65 | 0 | 1 |
| 23 | 27 | 0 | 0 | 15 | 1 | 0 |
| 24 | 18 | 0 | 1 | 20 | 1 | 0 |
| 25 | 64 | 0 | 0 | 45 | 0 | 1 |
| 26 | 48 | 0 | 0 | 15 | 1 | 0 |
| 27 | 28 | 1 | 0 | 25 | 0 | 1 |
| 28 | 54 | 1 | 0 | 15 | 1 | 0 |
| 29 | 58 | 1 | 1 | 30 | 0 | 1 |
| 30 | 59 | 1 | 1 | 40 | 0 | 1 |
| 31 | 67 | 1 | 0 | 15 | 1 | 0 |
| 32 | 43 | 1 | 1 | 135 | 1 | 1 |
| 33 | 63 | 1 | 0 | 20 | 1 | 0 |
| 34 | 41 | 0 | 0 | 40 | 1 | 0 |

我们将使用此数据来估计在以下变量取值下，术后出现 SoreThroat 的概率（0=否，1=是），给定这些变量的值：

- 患者的 Age 年龄，
- 患者的 Sex 性别（0=女性，1=男性），
- 手术的 Duration 时长，单位为分钟，
- 手术期间用于保持患者气道开放的 Device 类型（0=喉罩气道，1=气管导管），以及
- Device 上是否使用 Lubricant 润滑剂（0=否，1=是）。

相对于本章迄今为止我们查看的另外两个数据集而言，该数据集的一个明显新颖之处在于其规模很小；我们稍后会看到这如何影响模型评估。即使我们有一个小数据集，模型构建的第一阶段与以往一样。首先，为了研究 SoreThroat 如何依赖于 Duration，我们将开始计算空模型，使用 glm 拟合 logistic 模型，然后应用似然比检验进行初步评估：

```
pi_null = throat["SoreThroat"].mean()
pi_null

0.6285714285714286

throat_null = np.log( pi_null / (1 - pi_null) )
throat_null

0.5260930958967791

throat_model = smf.glm("SoreThroat ~ Duration", data=throat, family=Binomial())
throat_fit = throat_model.fit()
throat_fit.params

Intercept  -2.213582
Duration    0.070383
dtype: float64
```

根据基于此数据的空模型，患者术后出现咽喉疼痛的几率为 62.86%；这相当于对数几率为 0.526，如图所示。如果我们将 Duration 作为预测变量纳入，简单的 logistic 模型为

log(咽喉疼痛的几率) = −2.213582 + 0.070383 * Duration ,

这表明手术时间越长，咽喉疼痛越可能发生，因为 Duration 的系数为正。似然比检验的 p 值很小，约为 .04% ：

```
1 - chi2.cdf(throat_fit.null_deviance - throat_fit.deviance, df=1)

0.0004007973384960728
```

这意味着有强有力的证据表明该模型优于空模型。我们将在下一章学习其他用于评估该模型的工具，

它们都将证实似然比检验所传达的内容。接下来，让我们直观地检查拟合情况：

```
sns.regplot(data=throat, x="Duration", y="SoreThroat", logistic=True)
plt.axvline( transition, linestyle='--')
plt.xlim(10,140)
```

# Figure 4.10. Visualizing Collett’s throat data with the fitted logistic model

![](img/f25e2c651d2b8852d1c723f946646ff0_148_0.png)

垂直虚线标记了出现咽喉疼痛可能性较低（π̂ < .5）与可能性较高（π̂ > .5）之间的转变；它发生在 Duration 略多于半小时时：

```
transition = -throat_fit.params[0] / throat_fit.params[1]
transition

31.450620812871517
```

作为练习，你应该使用此 logistic 模型构建一个二元分类器，然后分析 ROC 曲线以探索其他可能分类阈值的有效性。你还应通过比较不包含该异常值与包含该异常值的模型来探索该图右上角异常值（手术持续 135 分钟）的影响——请参阅本章末尾的练习。

为了完成此初始建模阶段，让我们通过检查前面讨论的三个 R² 类比来比较空模型和 logistic 模型：

```
R2_M = 1 - (throat_fit.deviance / throat_fit.null_deviance )
R2_M

0.2712976002620783
```

```
throat["Null_residual"] = throat["SoreThroat"] - pi_null
throat["Residual"] = throat["SoreThroat"] - throat_fit.fittedvalues
throat["Difference"] = throat_fit.fittedvalues - pi_null
R2_S = np.sum(throat["Difference"]**2) / np.sum(throat["Null_residual"]**2)
R2_S

0.31224924747701394
```

```
throat["Fit_prob"] = throat_fit.fittedvalues
sns.displot( data=throat, x="Fit_prob", col="SoreThroat", binwidth=0.1)
```

Figure 4.11. Comparing predicted probabilities for both groups

![](img/f25e2c651d2b8852d1c723f946646ff0_150_0.png)

```
fit_avgs = throat.groupby("SoreThroat").agg(Fit_average=('Fit_prob', 'mean'))
fit_avgs
```

Fit_average
SoreThroat
0    0.433820
1    0.743652

```
R2_D = fit_avgs["Fit_average"][1] - fit_avgs["Fit_average"][0]
R2_D
```

0.3098313956744167

这三个度量值相当，并且都大于 0.2，因此到目前为止一切看起来都很好——包含 Duration 的模型比排除它的空模型具有更大的预测能力。更进一步，我们如何精确地量化和衡量模型的拟合度？这些是将在下一章仔细探讨的话题。

### 4.4.2 Extinction risks for cartilaginous fish

在上一章的 Example 5.2 中，我们利用鱼类长度与鱼类重量的对数之间的线性关系来推断 FishBase 中软骨鱼类缺失的重量，从而创建了 sharks 数据集。让我们加载数据并重新熟悉它：

```
sharks = pd.read_csv("sharks.csv")
sharks
```

| Species | Length | Weight | Category |
|---|---|---|---|
| Acroteriobatus annulatus | 140.0 | 20463.398751 |  |
| Acroteriobatus blochii | 100.0 | 8302.228700 | Le |
| Acroteriobatus leucospilus | 120.0 | 13535.898740 |  |
| Acroteriobatus salalah | 78.0 | 4264.716085 | Near |
| Acroteriobatus variegatus | 75.0 | 3839.035532 | Critically |
| ... | ... | ... | ... |
| Urotrygon simulatrix | 26.7 | 240.778211 |  |
| Urotrygon venezuelae | 25.5 | 212.849595 |  |
| Zameus squamulosus | 84.0 | 5202.119234 | Le |
| Zapteryx brevirostris | 53.4 | 758.000000 |  |
| Zapteryx xyster | 78.0 | 4264.716085 |  |

809 rows × 4 columns

请记住，Length 是物种的特征长度，单位为厘米，weight 是物种的特征重量，单位为克。现在我们知道了简单的 logistic 回归，我们将构建一个模型，将软骨鱼类的灭绝风险与其重量联系起来。为此，我们首先将因子 Category 转换为二元响应变量 Threatened，该因子有 5 个水平，直接来自 IUCN 红色名录，具体如下：

```
threatened = ["Critically Endangered","Endangered","Vulnerable"]

sharks["Threatened"] = sharks["Category"].isin( threatened ).astype(int)
sharks = sharks.drop(columns = "Category")
sharks
```

| Species | Length | Weight | Threatened |
|---|---|---|---|
| Acroteriobatus annulatus | 140.0 | 20463.398751 | 1 |
| Acroteriobatus blochii | 100.0 | 8302.228700 | 0 |
| Acroteriobatus leucospilus | 120.0 | 13535.898740 | 1 |
| Acroteriobatus salalah | 78.0 | 4264.716085 | 0 |
| Acroteriobatus variegatus | 75.0 | 3839.035532 | 1 |
| ... | ... | ... | ... |
| Urotrygon simulatrix | 26.7 | 240.778211 | 1 |
| Urotrygon venezuelae | 25.5 | 212.849595 | 1 |
| Zameus squamulosus | 84.0 | 5202.119234 | 0 |
| Zapteryx brevirostris | 53.4 | 758.000000 | 1 |

