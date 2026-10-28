

## 光衰减器、隔离器和窄带通滤波器

光衰减器（A）、隔离器（I）和窄带通滤波器（F）组件协同工作以减少反射光量（见图 12-2）。在 QKD 环境中，它们负责衰减光子脉冲电平，以确保在量子特洛伊木马攻击的背景下信息泄露不是问题。

## 激光二极管、强度调制器和相位调制器

*激光二极管（LD）* 组件产生光子脉冲，即 QKD 系统中信息的主要单元（见图 12-2）。*强度调制器（IM）* 通过在流中加入诱饵态来提供对光子分裂攻击的安全保障。

此 QKD 解决方案中的 *相位调制器（PM）* 组件可将四个离散相位值编码到光子脉冲上；接收方设备上的 PM 然后用于解密这些值。

## 分束器和偏振分束器

分束器（BS）由接收方用于提取每个光子粒子的偏振数据。*偏振分束器（BMS）* 确保光子在干涉仪对中沿相反路径传播并始终到达最终分束器。

## 监测探测器

在接收方设备中实施 *监测探测器（MD）* 有助于抵御热致致盲攻击。该光功率监测器接收设备输入的约 1%。这足以检测热致致盲中典型的任何可疑热能流入。

## 延迟线

光学 *延迟线（DL）* 是一种保护相位调制器（PM）免受特洛伊木马攻击的组件。延迟线使得恶意行为者无法在调制光子被 Bob/接收方检测到之前检测到来自相位调制器单元的任何反射光。

## 电子偏振控制器

接收方设备中的 *电子偏振控制器（EPC）* 用于减轻 QKD 运行期间的环境干扰（见图 12-3）。EPC 置于接收方干涉仪之前，以补偿传输光纤中持续的偏振旋转。

## 光纤长度伸缩器

发射器和接收器中基于光纤的干涉仪的光程可能受温度波动的影响。为补偿此问题，电驱动的 *光纤长度伸缩器（FS）* 置于接收方干涉仪中。

## 雪崩光电二极管和自差分电路

所谓的雪崩光电二极管（APDs）用于 Alice/接收方设备上的光子探测。二极管只是一种将光转换为电的器件。APD 是一种具有信号放大特性的二极管。所呈现 QKD 系统中的 APDs 冷却至 -30° 摄氏度以获得最佳性能。

所呈现系统的一部分还包括 *自差分电路（SD）*，它允许雪崩光电二极管在所谓的盖革模式下有效工作，而不会产生过度的信号噪声。在此模式下，设备的输出脉冲产生“开/关”触发事件。在盖革模式下工作的雪崩光电二极管是目前可用于探测单光子的最优组件之一。

## 外部结构

所有这些先进技术都容纳在一个相当极简主义的钢制机箱下，如图 12-4 所示。发射器和接收器设备的宽度均为 19″/48 cm，高度均为 5.25″/13.3 cm。

![](img/df9b6c40c665f58cab0c435444990c63_256_0.png)

*图 12-4. QKD 发射器和接收器*

> 1 图片来源：www.ncbi.nlm.nih.gov/pmc/articles/PMC5434053。依据知识共享 4.0 国际许可使用（https://creativecommons.org/licenses/by/4.0/）。

## 结语

完成本章后，你将学到以下内容：

- QKD 领域的重大密码攻击
- 安全 QKD 系统由哪些硬件组件构成

在下一章中，我们将探讨 QKD 的其他当前实现，包括 *SwissQuantum*、*Tokyo QKD Network* 等。

## 参考文献

Dixon, A. R., Dynes, J. F., Lucamarini M. et al. Quantum key distribution with hacking countermeasures and long term field trial. 2017. www.ncbi.nlm.nih.gov/pmc/articles/PMC5434053

Luis Adrian Lizama-Pérez, José Mauricio López, and Eduardo De Carlos López. 2016. Quantum Key Distribution in the Presence of the Intercept-Resend with Faked States Attack. Entropy/MDPI 19.

Yusheng Zhao. 2018. Development of Quantum Key Distribution and Attacks against It. IOP Conf. Series: Journal of Physics: Conf. Series 1087.

Fabio Grazioso, Frédéric Grosshans. 2013. Photon-Number-Splitting-attack resistant Quantum Key Distribution Protocols without sifting. Physical Review A, American Physical Society.

J. Nitin, E. Anisimova, I. Khan et al. 2014. New Journal of Physics 16. Trojan-horse attacks threaten the security of practical quantum cryptography.

B. Ouchao, A. Jakimi. International Journal of Advanced Engineering, Management and Science, June 2018. Performance Evaluation of Secure Key Distribution Based on the B92 Protocol.

Yang-Yang Fei, Xiang-Dong Meng, Ming Gao, Hong Wang & Zhi Ma. 2018. Quantum man-in-the-middle attack on the calibration process of quantum key distribution. Scientific Reports volume 8.

## 第 13 章

## QKD 的实现

第 12 章提供了量子密钥分发（QKD）系统中硬件组件的基础概述。我们还探讨了为测试 QKD 威胁弹性而构建的东京 QKD 网络。本章将深入研究该技术的更大规模实现，探讨总共五个主要 QKD 网络的基础。

## DARPA 量子网络

世界首个量子网络的荣誉属于 *DARPA 量子网络*。该 QKD 系统由美国国防部 *国防高级研究计划局* 资助，于 2002 年至 2007 年间运行。DARPA 量子网络由 *BBN Technologies* 与哈佛大学和 *波士顿大学光子学中心（BUPC）* 合作构建。

DARPA 运行于波士顿和剑桥、马萨诸塞州街下未使用的光纤电缆（即暗光纤）上。该系统与传统互联网技术兼容。它由十个光节点组成，连接哈佛大学与波士顿大学。

## 第 13 章 QKD 的实现

DARPA 系统将几种 QKD 分发协议整合到单一协议栈中。隐私通过 *通用哈希* 得到增强，其中从一组哈希算法中随机选择算法。这种方法包含非常少的碰撞概率，并跨越整数、向量和字符串的数学域。DARPA 中的密钥筛选通过传统方式实现，例如游程编码（RLE），或使用 SARG04 协议。

SARG04 由 Scarani 等人提出，是无处不在的 BB84 协议的更稳健版本，提供对光子数分裂攻击的改进抵抗力。SARG04 在使用衰减激光脉冲而非单个光子的场景中效果最佳。

> *游程编码（RLE）* 是可追溯到 20 世纪 60 年代末的最简单的无损数据压缩形式。RLE 不包含重复的数据集，而是仅存储这些重复位集的单个集合，同时记录这些集合应重复的次数。

## 基于量子密码学的保密通信（SECOQC）

2003 年启动了一个宏大的欧洲密码学项目。SECOQC 的创建是为了研究量子密钥分发技术在 Q3P 协议背景下的实际应用。该项目的参与者包括来自欧盟、俄罗斯和瑞士的 41 个组织。接下来我们将审视这个庞大专家团队的研究成果。

现在，有两种主要类型的 QKD 网络方法。

- 1. **量子信道切换。** 发射器和接收器（例如 Alice 和 Bob）之间使用端到端量子信道。
- 2. **可信中继器。** 密钥通过许多节点（即中间位置）传输，这些节点被认为值得信赖。

SECOQC 项目的参与者专注于研究六节点可信中继网络的实现（见图 13-1）。这于 2008 年在维也纳实现。

![](img/df9b6c40c665f58cab0c435444990c63_260_0.png)

图 13-1. SECOQC 网络的节点布局

除圣珀尔滕（节点 1）外，所有节点均位于维也纳，圣珀尔滕由维也纳至德国慕尼黑通信线路上的中继站托管（见表 13-1）。

