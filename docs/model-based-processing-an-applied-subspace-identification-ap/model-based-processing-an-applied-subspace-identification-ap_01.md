## 前言

本文涵盖了模型化信号处理方法的核心思想，即在进行处理之前，首先获得模型这一常被忽视但必要的先决条件。本文重点介绍利用子空间辨识技术实现基于模型的辨识 MBID 来设计基于模型信号处理器 MBSP 的模型开发，并结合验证和统计分析方法来评估其整体性能 [1]。它提出了一种不同的方法，将系统辨识问题的解决方案作为基于模型信号处理器 卡尔曼滤波器 的不可分割的一部分，该处理器可应用于大量应用，但除非有可靠的模型可用或能够适应变化的环境，否则很难取得成功 [2]。在此，使用子空间方法，可以非常快速地辨识模型并将其纳入各种处理问题，例如状态估计、跟踪、检测、分类、控制和通信等 [3, 4]。处理器的模型可以以多种方式演化，要么从第一性原理出发并估计其固有不确定参数，如参数自适应方案中那样 [5]，要么通过直接优化方法提取受约束的模型集 [6]，要么简单地将黑箱结构拟合到噪声数据上 [7, 8]。一旦模型从受控实验数据、大量测量数据甚至从高度复杂的真实模型中合成出来，就可以开发长期处理器以直接应用 [1]。由于许多实际应用寻求实时解决方案，我们主要集中于开发快速、可靠的辨识方法，以实现此类实现 [9–11]。模型提取/开发必须随后进行验证和测试，以确保模型可靠地代表了潜在的现象学——糟糕的模型只能导致失败！

系统辨识 [6] 为从测量数据序列中提取模型提供了解决方案，这些序列可以是时间序列、频率数据或简单地是有序的索引值集合。模型可以有多种形式，从简单的多项式到从非线性分布系统演化而来的高度复杂的结构。从数据中提取模型对于大量应用至关重要，这些应用从在变化的海洋中检测潜艇，到在乳腺组织中定位肿瘤，到精确定位破坏性地震的震中，或简单地监测驱动关键系统部件的电机的状况 [1]。这些应用都需要对建模和对潜在现象学的基本理解 当可能时 以及提取数据的测量仪器以及随之而来的不确定性。有些问题可以用“黑箱”表示来简单解决，该表示以某种方式忠实地再现数据，而无需捕捉潜在的动力学 例如常见的支票簿条目 或“灰箱”模型，即已提取的模型，但其参数具有极大兴趣 例如未知有毒物质的质量。然而，当真正需要获得对潜在现象学的准确表示时，例如飞机机翼的结构动力学或核电站涡轮机不合时宜的振动，则需要更复杂的系统和不确定性表示。在这种情况下，必须开发能够捕捉动力学的模型并将其“拟合”到数据中，以便执行诸如结构状况监测或旋转机器故障检测/预测等应用。这里的模型可以从由常微分方程组控制的集总表征中演化，线性或非线性，或从偏微分方程组演化的分布表示中演化。所有这些表示都有一个共同点，当需要执行关键任务时——它们都由数学模型表示，该模型捕捉其潜在现象学，必须以某种方式从噪声测量中提取出来。这就是本文要解决的基本问题，但我们必须将注意力限制在更易管理的表示集合上，因为许多专著已经解决了针对特定应用的问题集 [12, 13]。

事实上，这种专门解决方案的概念引导我们进入系统理论和控制的通用状态空间模型。这里的基本思想是，系统的所有理论特性都由这组基本模型表征，从而使理论得以发展并应用于任何可以在状态空间中表示的系统。许多模型自然地在状态空间中演化，因为它本质上是一组 n 阶微分方程 普通或偏微分，线性或非线性，时不变或时变，标量或多变量 的表示，这些方程被转换为一组一阶方程，每个方程都是一个状态。例如，由单个质量、弹簧、阻尼器构成的简单机械系统，其特征是一组二阶、线性、时不变微分方程，可以简单地以状态空间形式表示为一组两个一阶方程，每个方程代表一个状态：一个用于位移，一个用于速度 [12]。

我们在此文中使用状态空间表示，并在第 2 章和第 3 章中提供足够的背景知识。

系统辨识的广义之处在于，它不直接将问题限制在各种模型类中。例如，对于未知系统，选择一个模型集，认为它能够充分表示潜在现象学，然后直接从数据中辨识该集并验证其准确性。显然，有一个明确定义的程序可以捕捉这种方法来解决辨识问题 [6–15]。在某些情况下，模型的结构类可能先验已知，但阶数或等价地捕捉其演化的独立方程数量未知 例如海洋模式的数量。这里，阶数估计技术先于模型参数的拟合，然后是参数估计以提取所需模型 [14]。在其他情况下，阶数可从先验信息中得知，参数估计直接跟随 例如设计的机械结构。在任何情况下，这些约束都决定了解决辨识问题和提取模型以供应用的方法。存在许多应用，其中希望监测过程并跟踪随时间演化的各种参数 例如辐射检测，但为了在线完成此操作，基于模型的处理器必须顺序更新模型参数以完成其指定任务。我们在第 4 章和第 5 章中为线性和非线性模型开发这些处理器。

尽管本文主要设计为研究生教材，但它将对从事信号处理的专业人员和科学家有用，因为其中包含大量案例研究，展示了基于模型的子空间辨识方法对实际问题的适用性。此类教材的先决条件是本科线性代数 尤其是矩阵分解方法 、随机过程、线性系统以及一些基本的数字信号处理的融合。它在某种程度上是独特的，因为许多教材以零散的方式涵盖其部分主题。本文的底层基于模型的方法是贯穿算法、示例、应用和案例研究的线索。正是基于模型的主题，加上所开发的物理模型层次结构，结合了新的鲁棒子空间模型辨识方法，甚至使潜在的实时方法成为现实，才使其独具特色。本文源自四本以前的教材 [1, 5]，并通过大量实际应用扩展到现实世界的基于模型的问题。鲁棒子空间方法用于模型构建，这些方法在文献中已有很长时间，但需要更多的系统理论背景才能理解。我们通过首先开发基于模型的处理器，即模型的主要使用者，然后演进到联合估计信号和嵌入式参数的参数自适应处理器来引入这种辨识方法



导致谱估计的概念的提出，谱估计为原始辨识问题提供了基础性的基石。

接下来，状态空间模型被详细地介绍，从连续时间、采样数据到离散时间系统演进，从而引出随机创新模型，将经典的维纳滤波器与众所周知的卡尔曼滤波器联系起来 [2]。在此基础上，多变量（多输入/多输出）系统被简单地作为时间序列来发展，直至复杂的规范形式，从而引出矩阵分式传递函数描述。第3章以状态空间形式中的近似非线性高斯-马尔可夫表示而结束。

模型驱动处理器在接下来的第4章和第5章中被突出介绍，从线性表示的发展开始，引出最优卡尔曼滤波器 [2]。接下来，非线性处理器套件被发展，以线性化处理器为开端，引出扩展卡尔曼滤波器和无迹卡尔曼滤波器的特殊情况，并以源自贝叶斯方法 [5] 的新型粒子滤波器为顶点。这些技术被扩展到联合信号/参数估计问题，以创建参数自适应处理器。在这些章节中，示例和案例研究被引入以巩固这些基本思想。

接下来，我们介绍文本核心的基础——子空间辨识，首先约束于确定性系统。在这里，我们发展了提供子空间辨识基础的根本实现问题，使用汉克尔矩阵。卡尔曼 [18] 在20世纪60年代引入的许多底层系统理论结果都被汉克尔矩阵的性质所捕获。该问题通过结合输入/输出序列 [15] 被扩展到确定性辨识问题。也许对实现理论最重要贡献之一是平衡实现的概念，使稳健算法的演进成为可能。所有这些概念都在本章中被仔细发展。规范实现，即在唯一规范形式中辨识模型，是辨识中的一个重要概念 [16–18]。这里，多年来很大程度上忽略了这一努力，主要是因为唯一表示的概念在辨识模型时可能导致较大误差。然而，可以证明它们也可以被视为一种可行的方法，因为它们将汉克尔阵列变换为所谓的结构矩阵，从而使阶数和参数能够同时被辨识，导致不变的系统描述 [17, 19]。最后，我们引入投影理论的思想，展示正交/斜投影如何导致流行的确定性辨识技术 [9–11]。第6章以关于机械/结构系统辨识应用的详细案例研究而结束。

第7章是将确定性辨识问题扩展到随机情形。在实现背景下，协方差矩阵取代了脉冲响应矩阵，而确定性输入/输出序列被噪声多通道序列取代——这就是现实世界的问题。

- 8 Söderström, T. and Stoica, P. (1989). *System Identification*. New York: Academic Press.
- 9 van Overschee, P. and De Moor, B. (1996). *Subspace Identification for Linear Systems: Theory, Implementation, Applications*. Boston, MA: Kluwer Academic Publishers.
- 10 Katayama, T. (2005). *Subspace Methods for System Identification*. London: Springer.
- 11 Verhaegen, M. and Verdult, V. (2007). *Filtering and System Identification: A Least-Squares Approach*. Cambridge: Cambridge University Press.
- 12 Juang, J. (1994). *Applied System Identification*. Upper Saddle River, NJ: Prentice-Hall PTR.
- 13 Aoki, M. (1990). *State Space Modeling of Time Series*, 2e. London: Springer.
- 14 Norton, J. (1986). *An Introduction to Identification*. New York: Academic Press.
- 15 Ho, B. and Kalman, R. (1966). Effective reconstruction of linear state variable models from input/output data. *Regelungstechnik* 14: 545–548.
- 16 Luenberger, D. (1967). Canonical forms for linear multivariable systems. *IEEE Trans. Autom. Control* AC-12: 290–293.
- 17 Candy, J., Warren, M., and Bullock, T. (1977). Realization of an invariant system description from Markov sequences. *IEEE Trans. Autom. Control* AC-23 (12): 93–96.
- 18 Chen, C. (1984). *Linear System Theory and Design*. New York: Holt, Rinehart & Winston.
- 19 Guidorzi, R. (1975). Canonical structures in the identification of multivariable systems. *Automatica* 11: 361–374.
- 20 Akaike, H. (1974). Stochastic theory of minimal realization. *IEEE Trans. Autom. Control* 19: 667–674.
- 21 Akaike, H. (1975). Markovian representation of stochastic processes by canonical variables. *SIAM J. Control* 13 (1): 162–173.
- 22 Faurre, P. (1976). Stochastic realization algorithms. In: *System Identification: Advances and Case Studies* (ed. R. Mehra and D. Lainiotis), 1–23. New York: Academic Press.
- 23 Larimore, W. (1990). Canonical variate analysis in identification, filtering and adaptive control. In: *Proceedings of the 29th Conference on Decision and Control*, Hawaii, USA, 596–604.
- 24 Tse, E. and Weinert, H. (1975). Structure determination and parameter identification for multivariable stochastic linear systems. *IEEE Trans. Autom. Control* 20: 603–613.
- 25 Glover, K. and Willems, J. (1974). Parameterizations of linear dynamical systems: canonical forms and identifiability. *IEEE Trans. Autom. Control* 19: 640–646.
- 26 Denham, M. (1974). Canonical forms for identification of multivariable linear systems. *IEEE Trans. Autom. Control* 19: 646–656.
- 27 Candy, J., Bullock, T., and Warren, M. (1979). Invariant system description of the stochastic realization. *Automatica* 15: 493–495.
- 28 Candy, J., Warren, M., and Bullock, T. (1978). Partial realization of invariant system descriptions. *Int. J. Control* 28 (1): 113–127.
- 29 Sullivan, E. (2015). *Model-Based Processing for Underwater Acoustic Arrays.* New York: Springer.

## Acknowledgements

我妻子 Patricia 的支持和鼓励是我着手开展这一事业所需的主要动力。我的家人、大家庭和朋友们忍受了许多遗憾，但仍然在我的种种借口下给予鼓励。当然，我的优秀同事和朋友们，尤其是 S. Lehman 博士、I. Lopez 博士、E. Sullivan 博士和 B. Beauchamp 先生，他们仔细审阅了手稿并提出了许多改进建议，其持续支持不容置疑地值得衷心致谢。

## Glossary

| Abbreviation | Definition |
|---|---|
| ADC | 模数转换 |
| AIC | 赤池信息准则 |
| AR | 自回归（模型） |
| ARMA | 自回归滑动平均（模型） |
| ARMAX | 自回归滑动平均外生输入（模型） |
| ARX | 自回归外生输入（模型） |
| AUC | 曲线下面积（ROC 曲线） |
| BSP | 贝叶斯信号处理 |
| BW | 带宽 |
| CD | 中心差分 |
| CDF | 累积分布函数 |
| CM | 条件均值 |
| CRLB | 克拉美-罗下界 |
| C-Sq | 卡方（分布或检验） |
| CT | 连续时间 |
| CTD | 浓度-温度-密度（测量） |
| CVA | 典型变量分析 |
| EKF | 扩展卡尔曼滤波器 |
| EM | 期望最大化 |
| FPE | 终极预测误差 |
| GLRT | 广义似然比检验 |
| G-M | 高斯混合 |
| GM | 高斯-马尔可夫 |
| G-S | 高斯和 |
| HD | 亥灵格距离 |
| HPR | 高概率区域 |
| IEKF | 迭代扩展卡尔曼滤波器 |
| i.i.d. | 独立同分布（样本） |
| KD | 库尔贝克散度 |
| KL | 库尔贝克-莱布ler |
| KLD | 库尔贝克-莱布ler 散度 |
| KSP | 卡尔曼-塞格-波波夫（方程） |
| LD | 下三角（矩阵）分解 |
| LE | Lyapunov 方程 |
| LKF | 线性卡尔曼滤波器 |
| LMS | 最小均方 |
| LS | 最小二乘 |
| LTI | 线性时不变（系统） |
| LZKf | 线性化卡尔曼滤波器 |
| MA | 滑动平均（模型） |
| MAICE | 最小赤池信息准则 |
| MAP | 最大后验 |
| MATLAB® | 数学软件包 |
| MBID | 基于模型的辨识 |
| MBP | 基于模型的处理器 |
| MBSP | 基于模型的信号处理 |
| MC | 蒙特卡洛 |
| MDL | 最小描述长度 |
| MIMO | 多输入/多输出（系统） |
| MinE | 最小错误概率 |
| ML | 最大似然 |
| MOESP | 多变量输出误差状态空间算法 |
| MMSE | 最小均方误差 |
| MSE | 均方误差 |
| MV | 最小方差 |
| N4SID | 用于子空间状态空间系统辨识的数值算法 |
| NMSE | 归一化均方误差 |
| N-P | 奈曼-皮尔逊（检测器） |
| ODP | 最优决策（阈值）点 |
| PDF | 概率密度函数（连续） |
| P-E | 错误概率（检测器） |
| PEM | 预测误差法 |
| PF | 粒子滤波器 |
| PI-MOESP | 过去输入多变量输出误差状态空间算法 |
| PMF | 概率质量函数（离散） |
| PO-MOESP | 过去输出多变量输出误差状态空间算法 |
| PSD | 功率谱密度 |
| RC | 电阻电容（电路） |
| REBEL | 递归贝叶斯估计库 |
| RLC | 电阻-电感-电容（电路） |
| RLS | 递归最小二乘 |
| RMS | 均方根 |
| RMSE | 均方根误差 |





| 缩写 | 全称 |
| --- | --- |
| ROC | 接收机工作特性（曲线） |
| RPE | 递归预测误差 |
| RPEM | 递归预测误差方法 |
| SID | 子空间辨识 |
| SIR | 序贯重要性采样-重采样 |
| SIS | 序贯重要性采样 |
| SMC | 序贯马尔可夫链 |
| SNR | 信噪比 |
| SPRT | 序贯概率比检验 |
| SPT | 西格玛点变换 |
| SSIS | 序贯采样重要性采样 |
| SSP | 状态空间处理器 |
| SSQE | 和平方误差 |
| SVD | 奇异值（矩阵）分解 |
| UD | 上三角矩阵分解 |
| UKF | 无迹卡尔曼滤波器 |
| UT | 无迹变换 |
| WSSR | 加权和平方残差统计检验 |
| W-test | 白噪声检验 |
| Z | Z 变换 |
| Z-M | 零均值统计检验 |
