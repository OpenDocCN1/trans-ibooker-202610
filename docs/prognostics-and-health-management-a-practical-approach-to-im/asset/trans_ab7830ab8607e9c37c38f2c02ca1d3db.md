

Pecht, M. (2008). *电子产品预测与健康管理*. Hoboken, NJ: Wiley. Pratt, J.W., Edgeworth, F.Y., and Fisher, R.A. (1976). On the efficiency of maximum likelihood estimation. *Annals of Statistics* 4 (3): 501–514. Ross, M.S. (1987). *工程师和科学家概率与统计导论*. New York: Wiley. Ross, M.S. (1996). *随机过程*, 2e. New York: Wiley. Ross, M.S. (2000). *概率模型*, 7e. San Diego, CA: Academic Press. Saxena, A., Celaya, J., Saha, B. et al. (2009). On applying the prognostic performance metrics. Annual Conference of the Prognostics and Health Management Society (PHM09), San Diego, California, US, 27 Sep.–1 Oct. Speaks, S. (2005). Reliability and MTBF overview. Vicor Reliability Engineering. http://www.vicorpower.com/documents/quality/Rel_MTBF.pdf (accessed August 2015). Szidarovszky, F., Gershon, M., and Duckstein, L. (1986). *系统管理中多目标决策制定的技术*. Amsterdam: Elsevier. Valdez-Flores, C. and Feldman, R.M. (1989). A survey of preventive maintenance models for stochastically deteriorating single-unit systems. *Naval Research Logistics* 36: 419–446. Wang, H.Z. (2002). A survey of maintenance policies of deteriorating systems. *European Journal of Operational Research* 139 (3): 469–489. Yakowitz, S. and Szidarovszky, F. (1989). *数值计算导论*. New York: Macmillan.

## 进一步阅读

Filliben, J. and Heckert, A. (2003). Probability distributions. In: *Engineering Statistics Handbook*. National Institute of Standards and Technology. http://www.itl.nist.gov/div898/handbook/eda/section3/eda36.htm. Hofmeister, J., Wagoner, R., and Goodman, D. (2013). Prognostic health management (PHM) of electrical systems using conditioned-based data for anomaly and prognostic reasoning. *Chemical Engineering Transactions* 33: 992–996. Tobias, P. (2003a). Extreme value distributions. In: *Engineering Statistics Handbook*. National Institute of Standards and Technology. https://www.itl.nist.gov/div898/handbook/apr/section1/apr163.htm. Tobias, P. (2003b). How do you project reliability at use conditions? In: *Engineering Statistics Handbook*. National Institute of Standards and Technology. https://www.itl.nist.gov/div898/handbook/apr/section4/apr43.htm.

## 2

## 预测与健康管理/监测(PHM)的方法

### 2.1 预测与健康管理/监测(PHM)方法导论

你在第1章学到，预测的目的在于能够准确检测并报告系统中未来的故障——预测故障进展。用于实现该目的的预测与健康管理/监测(PHM)中的预测方法可分为三大类：经典、基于使用和基于状态 [Hofmeister et al. 2017; Pecht 2008; Kumar and Pecht 2010; O’Connor and Kleyner 2012; Sheppard and Wilmering 2009]。经典预测方法可分为基于模型、数据驱动或混合驱动，如图2.1所示。

### 2.1.1 基于模型的预测方法

基于模型的预测方法包括建模和使用与可靠性、概率和失效物理(PoF)模型相关的表达式。此类模型用于研究和比较，例如材料、制造和利用对产品可靠性、鲁棒性和强度关系，通常在结构化、设计、受控的实验和寿命试验中进行。这样的建模可能提供较好的精度，但难以应用于复杂、已部署的系统 [Speaks 2005]。这些模型包括分布和概率模型；可靠性理论基础；以及基于可靠性测试的模型，如第1章中介绍的加速因子(AF)。其他方法将在后面讨论。

### 2.1.2 数据驱动的预测方法

数据驱动的预测方法包括统计和机器学习(ML)方法；与基于模型的预测方法相比通常更易应用；但在预测估计中可能产生较低的精度和准确性。统计方法包括参数和非参数模型，如 (Ross 1987) 和 (Hollander et al. 2014) 中所示；以及 K-最近邻(KNN)，一种用于根据邻居对对象进行分类或回归的非参数方法 [Medjaher and Zerhouni 2013]。机器学习包括线性判别分析(LDA)用于表征或分离多个对象、隐马尔可夫模型(HMM)用于建模具有隐藏状态的系统，以及主成分分析(PCA)用于将观测值转换为线性不相关的变量。

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_93_0.png)

**图 2.1** 显示PHM三种方法的框图。来源：基于 Pecht (2008)。

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_93_1.png)

**图 2.2** 精度和复杂度：经典PHM方法的相对比较。

### 2.1.3 混合预测方法

混合方法同时采用模型驱动和数据驱动方法，以进一步提高准确性并/或更好地理解参数和对象的关系 [Medjaher and Zerhouni 2013]。缺点是计算处理和复杂度增加（见图2.2）。我们将在本章后面回到这些及其他方法。

### 2.1.4 本章目标

本章的目标是介绍支持PHM预测的经典方法论，然后介绍一种用于PHM的基于状态维护(CBM)的方法：一种基于CBD特征的方法，为第3章奠定基础。

### 2.1.5 本章组织

本章其余部分组织如下，用于介绍和讨论支持PHM预测的经典建模方法，并介绍我们对CBM的方法：

2.2 基于模型的预测

本节介绍基于模型的预测方法，包括分析建模、分布建模、PoF和可靠性建模、加速因子、与可靠性建模相关的复杂度、失效分布、失效率和随时间发生的失效，以及基于模型预测的优缺点。

### 2.3 数据驱动预测

本节介绍数据驱动预测方法，包括统计方法和机器学习——分类和聚类。

### 2.4 混合驱动预测

本节介绍混合驱动预测方法：基于模型与数据驱动预测相结合。

### 2.5 基于状态维护(CBM)的方法

本节介绍CBM方法，包括CBD特征建模、比较寿命消耗和PoF，以及CBD特征方法论。包含CBD特征建模的示例。

### 2.6 PHM方法：总结

本节总结本章所呈现的材料。

## 2.2 基于模型的预测

基于模型的方法使用分析模型和PoF模型。分析模型包括使用、统计和概率模型；并且可通过PoF模型和/或基于可靠性的模型进行验证。基于可靠性的模型与测试相关，例如加速寿命试验(ALTs)和回归分析：Pecht 偏爱PoF，其中生命周期载荷和失效机制被建模并应用于评估可靠性并评估新材料、结构和技术 [Pecht 2008]。需要指出的是，通常PoF建模在应用于系统时计算上不可行 [Sheppard and Wilmering 2009]。如图2.3所示的基于模型的预测简化方法包括模型开发和模型使用。

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_94_0.png)

**图 2.3** 模型开发和使用的基于模型方法。

![](img/05efaadbca64ad00f9e8d44c1db8cc7b_95_0.png)

**图 2.4** 模型使用图。来源：基于 Medjaher and Zerhouni (2013)。

模型开发包括以下内容：(i) 识别、选择和/或开发模型；(ii) 通过仿真和/或实验产生数据以评估和验证模型；以及 (iii) 对数据进行表征以便后续数据测量、收集并输入模型，在模型使用时产生预测信息。模型使用包括以下内容（见图2.4）：(i) 获取数据，(ii) 处理数据，(iii) 检测故障，(iv) 执行诊断，(v) 执行预测，(vi) 做出决策，以及 (vii) 发布维护和后勤指令 [Medjaher and Zerhouni 2013]。

参考图2.4和2.5，标记为数据采集、数据处理、故障检测、诊断处理和预测处理的模块功能体现在传感器框架、特征向量框架、预测框架和性能验证框架中。预测信息传递给故障管理(FM)框架和/或写入输出文件，用于维护、后勤和图形用户界面相关的延迟决策和操作 [CAVE3 2015; Hofmeister et al. 2017]。

### 2.2.1 分析建模

分析模型，也称为物理模型，采用如表2.1所示的载荷参数来估计系统中特定预测目标如何从100%健康（未受损）状态变化到零健康（失效）状态，随着损伤累积 [Pecht 2008; Hofmeister et al. 2016, 2017; Vichare 2006; Vichare et al. 2007]。PHM系统执行健康监测，检测不健康状况，并使用例如故障树或状态图分析来识别并确定导致故障的最可能预测目标的位置。分析方法可分为两大类：*归纳*和*演绎*方法。

#### 分析建模：归纳方法

*归纳*方法基于推理，使用定性数据，从个别案例推导出一般结论。例如，这种方法可用于确定

