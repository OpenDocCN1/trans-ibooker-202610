
尽管*偏见*可以指代任何形式的偏好（无论公平与否），但我们的关注点在于*不良偏见*或*歧视*，即特定优势群体被置于系统性优势地位，而特定劣势群体被置于系统性劣势地位。这涉及种族、性别、年龄和性取向等属性。

# 应该使用哪些指标？

AIF360 目前包含 77 个公平性指标来衡量偏见（参见[图 1-2](#ch01.xhtml#fig_2_how_bias_is_measured)），因此很难理解在每个用例中应使用哪些指标。由于不存在适用于所有情况的单一最佳指标，建议使用多个指标，并在您组织内的主题专家和关键利益相关者的指导下谨慎选择。AIF360 拥有指标的差值版和比率版，它们本质上传达相同的信息，因此您可以根据查看结果的用户的习惯进行选择。

AIF360 的 [API 文档](https://oreil.ly/LSBZO) 允许您查看其所有指标和算法，以及每个指标、算法或解释器的定义；其所基于的研究；其源代码；以及其参数和异常。以下列出的 API 类别中列出了几类指标。如果您的用例需要针对训练数据的指标，请使用 `DatasetMetric` 类中的指标（及其子类，如 `BinaryLabelDatasetMetric`）。如果应用程序需要针对模型的指标，请使用 `ClassificationMetric` 类。以下是相关类：

**`BinaryLabelDatasetMetric`**：基于单个二元标签数据集计算指标的类

**`ClassificationMetric`**：基于两个二元标签数据集计算指标的类

**`SampleDistortionMetric`**：基于两个结构化数据集计算指标的类

**`Utility Functions`**：用于实现指标的辅助脚本

![](Images/aifa_0102.png)

## 个体公平性指标与群体公平性指标

如果您的用例关注个体公平性，请使用 `SampleDistortionMetric` 类中的指标或一致性指标。如果您关注群体公平性，请使用 `DatasetMetric` 类（及其子类，如 `BinaryLabelDatasetMetric`）以及 `ClassificationMetric`。如果您的用例同时关注*个体和群体*公平性，且需要使用单一指标，请使用 `ClassificationMetric` 类中的广义熵指数及其特化形式——泰尔指数和变异系数。不过，可以同时检查多个指标，包括来自个体公平性和群体公平性的指标。

## 世界观与指标

如果您的用例遵循 WAE 世界观，请使用人口统计学平价指标（如 `disparate_impact` 和 `statistical_parity_difference`）。如果您的用例遵循 WYSIWYG 世界观，请使用机会均等指标（如 `average_odds_difference` 和 `average_abs_odds_difference`）。其他群体公平性指标（通常标记为“机会均等”）介于这两种世界观之间。相关指标包括以下内容：

*   `false_negative_rate_ratio`
*   `false_negative_rate_difference`
*   `false_positive_rate_ratio`
*   `false_positive_rate_difference`
*   `false_discovery_rate_ratio`
*   `false_discovery_rate_difference`
*   `false_omission_rate_ratio`
*   `false_omission_rate_difference`
*   `error_rate_ratio`
*   `error_rate_difference`

## 数据集类

`Dataset` 类及其子类是处理所有形式数据的关键抽象。训练数据用于学习分类器，而测试数据用于进行预测和比较指标。除了机器学习流水线的这些标准方面外，公平性应用还要求将受保护属性与数据中的每个实例或记录相关联。为了保持通用格式，独立于所应用的算法或指标，AIF360 构建 `Dataset` 类的方式使得所有相关属性（特征、标签、受保护属性及其各自的标识符）都存在于该类的每个实例中并可从中访问。子类添加了进一步的属性来区分数据集，并规定其可与哪些算法和指标交互。

结构化数据是公平性文献中研究的主要数据集类型，由 `StructuredDataset` 类表示。`BinaryLabelDataset`（二元标签数据集）作进一步区分——它是一种结构化数据集，每个实例只有一个标签，且只能取两个值中的一个：有利或不利。非结构化数据可以通过构建一个与 `StructuredDataset` 类平行的类来适配 AIF360 架构，而不会影响不适用于非结构化数据的现有类或算法。

# 偏见指标的透明度

我们现在将重点转向改进偏见指标透明度的更广泛问题。

## 解释器类

AIF 360 工具包优先考虑解释公平性指标的需求，因此 IBM 研究人员开发了 `Explainer` 类，以配合 `Metrics` 类提供关于计算出的公平性指标的进一步见解。可以创建扩展 `Explainer` 类的不同复杂度的子类，以输出对不同类型用户有意义的解释。AIF360 首个版本中实现的解释器功能是通过打印 JSON 输出进行基本报告。未来的版本可能包括偏见的细粒度定位、可操作的补救分析和反事实公平性。

## AI 事实表

阻碍 AI 公平性和信任的一个障碍是缺乏标准实践来记录 AI 服务是如何创建、测试、训练、部署和评估的。组织正在要求 AI 服务的透明度，以及它们应该和不应该如何被使用。为了满足这一需求，[AI 事实表](https://oreil.ly/Bvo10)^([1](#ch01.xhtml#idm46444778451176)) 以类似于食品营养标签或家电规格表的方式，提供关于机器学习模型重要特征的信息。它们将包含系统操作、训练数据、底层算法、测试设置和结果、性能基准、公平性和鲁棒性检查、预期用途、维护和再训练等信息。示例问题如下：

*   用于训练服务的数据集是否有数据表或数据声明？
*   是否对数据集和模型进行了偏见检查？如果是，请描述所检查的偏见策略、偏见检查方法和结果。
*   是否对数据集执行了任何偏见缓解？如果是，请描述缓解方法。
*   算法输出是否可解释或可阐释？如果是，请解释如何实现（例如，通过直接可解释的算法、局部可解释性或基于示例的解释）。
*   解释的目标用户是谁（机器学习专家、领域专家、普通消费者、监管机构等）？
*   服务是否在任何额外数据集上进行了测试？它们是否有数据表或数据声明？
*   描述测试方法。
*   是否保留服务运行中的使用数据？
*   如果输入偏离训练或测试数据，预期会有什么行为？
*   数据到 AI 服务的整体工作流是如何被追踪的？

这些信息将帮助用户了解服务如何工作、其优势和局限性，以及它是否适合他们正在考虑的应用。

^([1](#ch01.xhtml#idm46444778451176-marker)) M. Arnold et al., “FactSheets: Increasing Trust in AI Services through Supplier’s Declarations of Conformity” in *IBM Journal of Research and Development* 63, no. 4/5, (July/September 2019): 6, [*https://oreil.ly/Bvo10*](https://oreil.ly/Bvo10)。

# 第 2 章. 偏见缓解算法
