
![](img/05efaadbca64ad00f9e8d44c1db8cc7b_171_0.png)

**图 3.32** 使用非线性和线性退化率的仿真CBD图。

在图3.32的右侧，当退化率的非线性进一步增强时：例如，dP = (0.75/200)t^2.0。

3.18中引入的DNL项以及相关术语积分非线性（INL）都是对两个模拟值之间偏差的度量：在本书中，这两个模拟值分别是理想（直线）特征和输入到预测算法的数据特征（如FFS）（Texas Instruments 1995）。在本书中，我们引入一个新术语——FFS非线性（FNL），它衡量理想RUL和理想SoH转移曲线与来自此类预测算法的RUL和SoH估计实际曲线之间的偏差（Hofmeister et al. 2018）。

### 示例 3.19

将你为图3.32右侧图创建的仿真数据转换为DPS数据，假设退化率恒定。这样操作时，你会创建一条非线性DPS转移曲线，如图3.33所示；图中还显示了理想DPS以作比较。尽管该DPS是非线性的，但它比其FFP特征数据更接近线性（可回顾图3.32右侧图）。

## 3.7 故障进展特征与系统节点

在第1章中，我们使用了一个用于CBM的PHM系统框架的示例说明。参考图3.34，该框架包括（i）传感器框架，（ii）特征向量框架，（iii）预测框架，（iv）健康管理框架，（v）性能验证框架，以及（vi）控制与数据流框架。传感器框架连接到一个节点；在一个系统中可能有数百或数千个节点，每个节点都是系统中CBD的一个来源：声学、电气、磁、机械应力与应变、振动、辐射、光学等。因此，采用基于节点的架构作为PHM系统的锚点是合理的，该系统收集CBD以实现可靠的工况监测，从而支持CBM。

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_172_0.png)

**图 3.33** 非恒定退化率下理想DPS与来自数据的DPS的比较。

电子设备的PHM：框架图

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_172_1.png)

**图 3.34** 用于支持故障进展特征的基于节点的框架。

在本章中，我们介绍并讨论了连接在节点上用于监测信号的传感器，利用噪声容限隔离CBD和FD；校准作为FD标称值的使用；以及与CBD特征、FFP特征、DPS数据、FFS数据和预测（预测框架）相关的各种变换和数据处理。使用基于节点的架构作为PHM系统的框架是合理的，而不是例如在控制与数据流框架中为每个节点设计和开发细节。由传感器和驱动器组成的传感器框架在节点处捕获信号，执行数据调理，并产生FD。由处理器组成的特征向量框架执行额外的调理和变换，以创建FFS，作为产生预测信息的预测框架的输入数据。健康管理框架处理预测信息，以生成诊断、预测和后勤指令与行动，从而维持系统的健康与可靠性。性能验证框架提供产生预测性能指标的手段和方法，以评估预测信息的准确性。控制与数据流框架管理PHM的功能和行动（CAVE3 2015; Hofmeister et al. 2017; Kumar and Pecht 2010; Pecht 2008）。

## 3.8 故障进展特征：小结

本章阐述了将CBD特征转换为其他特征（FFP、DPS和FFS）的基本原理。你了解到，FFP和DPS都可以转换为FFS。你看到FFP是CBD的归一化版本，并且FFP大大减少了特征模型的数量；我们还证明了将FFP转换为FFS会产生一种非常适合输入到预测算法的特征：

- 1. 幅度值以百分比表示。
- 2. 幅度为0或更小表示未检测到退化。
- 3. 幅度为100或更大表示功能失效。

功能失效定义为一种SoH状态，在该状态下，预测目标不再在规范范围内运行。

你了解到，可以通过相对于参数标称未退化值（*P*₀）的参数值变化（*dP*）来求解特征，从而将FFP转换为DPS：*DPS* = (*dP/P*₀)；你还了解到，与FFP相比，DPS的弯曲程度更低，并且*DPS*有两种形式：相对于时间恒定和非恒定的退化率：

*f(DPS, t) = (d/dt)DPS*

你了解到，DPS与FFP一样可以转换为FFS，并且基于DPS的FFS最适合由预测算法处理——即使相对于时间的退化率不是恒定的。

你还了解到，需要根据所建模的特征来验证建模选择，并随着退化的进展，判断特征斜率角是减小还是增大，以帮助确定特征对应的退化函数：

*FD*ᵢ = *FD*₀*f(dP*ᵢ, *P*₀)

本章介绍了评估FFS曲线非线性的两个定量度量：逐点非线性和总非线性，使用式（3.23）和（3.24）：

FNL_i = FFS_i - IDEAL_FFS_i
FNL_E = max(abs({FNL_i}))

在下一章中，你将了解到，特征可以由有限数量的特性曲线有效建模：10个幂函数和4个指数函数。你还将了解到，启发式建模方法对于预测而言足够准确：基于传统方法（如FMEA和PoF）的正式建模并非必需。

## 参考文献

Carr, J.J. and Brown, J.M. (2000). Introduction to Biomedical Equipment Technology, 4e. Upper Saddle River, New Jersey: Prentice Hall.

Erickson, R. (1999). Fundamentals of Power Electronics. Norwell, MA: Kluwer Academic Publishers.

Hofmeister, J., Wagoner, R., and Goodman, D. (2013). Prognostic health management (PHM) of electrical systems using conditioned-based data for anomaly and prognostic reasoning. Chemical Engineering Transactions 33: 992–996.

Hofmeister, J., Goodman, D. and Wagoner, R. (2016). Advanced anomaly detection method for condition monitoring of complex equipment and systems. 2016 Machine Failure Prevention Technology, Dayton, Ohio, US, 24–26 May.

Hofmeister, J., Szidarovszky, F., and Goodman, D. (2017). An approach to processing condition-based data for use in prognostic algorithms. 2017 Machine Failure Prevention Technology, Virginia Beach, Virginia, US, 15–18 May.

Hofmeister, J.P, Goodman, D.L., and Szidarovszky, F. (2018). Transforming condition-based data signatures into functional failure signatures, IEEE 2018 Aerospace Conference, Big Sky, Montana, US, 3–9 March.

Jenq, Y.C. and Li, Q. (2002). Differential non-linearity, integral non-linearity, and signal to noise ratio of an analog to digital converter. Portland, Oregon: Department of Electrical and Computer Engineering, Portland State University.

Judkins, J.B. and Hofmeister, J.P. (2007). Non-invasive prognostication of switch mode power supplies with feedback loop having gain, IEEE Aerospace Conference 2007, Big Sky, Montana, US, 4–9 Mar.

Judkins, J.B., Hofmeister, J., and Vohnout, S. (2007). A prognostic sensor for voltage regulated switch-mode power supplies. IEEE Aerospace Conference 2007, Big Sky, Montana, US, 4–9 Mar, Track 11–0804, 1–8.

Kumar, S. and Pecht, M. (2010). Modeling approaches for prognostics and health management of electronics. International Journal of Performability Engineering 6 (5): 467–476.

Medjaher, K. and Zerhouni, N. (2013). Framework for a hybrid prognostics. Chemical Engineering Transactions 33: 91–96. https://doi.org/10.3303/CET1333016.

National Science Foundation Center for Advanced Vehicle and Extreme Environment Electronics at Auburn University (CAVE3). (2015). Prognostics health management for
