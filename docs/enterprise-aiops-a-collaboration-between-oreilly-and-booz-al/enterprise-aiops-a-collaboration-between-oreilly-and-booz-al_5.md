# Chapter 4. Data: Your Most Precious Asset

When it comes to your organization’s data, you can’t manage what you don’t see, and you certainly can’t protect what you aren’t tracking.

AI relies on large volumes of historical data and advanced ML methods to generate insights. Data is the starting point for AI development. It’s critical you establish data management as a fundamental competency, ensuring your teams have the tools needed to properly handle and curate data.

The amount of data available today is exploding as organizations continue to accumulate information at an exponential rate. This growth complicates our ability to understand what data exists, where it exists, who can use it, and how it can be used to generate better outcomes.

It’s critical that your organization find and develop the technologies, implement appropriate policies, and help train employees to adapt to the changing data-centric environment.

## Data’s Role in AIOps

An AI project’s success depends on data readiness (e.g., is the data cleansed in a standard format? is incomplete data removed/filled? etc.) both for training AI models and operationalizing AI capabilities. Data readiness for AI can be measured through the key drivers of availability, accessibility,¹ timeliness, accuracy,² usability,³ form, quality,⁴ provenance,⁵ and security.⁶

### Accuracy

The degree to which data correctly describes the “real world” object or event being described.

### Usability

A measure of how reliable the data is. Data that is messy, is missing substantial records, is difficult to ingest, or is otherwise prohibitive would be less usable.

### Provenance

Provenance of a resource is a record that describes entities and processes involved in producing and delivering or otherwise influencing that resource. Provenance provides a critical foundation for assessing authenticity, enabling trust, and allowing reproducibility. Provenance assertions are a form of contextual metadata and can themselves become important records with their own provenance.

### Security

How safe the data is from loss or unwanted manipulation.

Managing data as an asset can help your organization address these challenges and support AI needs through a multilayered approach to data readiness. A holistic approach will help drive data readiness across all dimensions of data as follows:

- A data strategy aligns data capabilities with AI needs and establishes effective data management and data usage practices tailored to AI needs.
- DataOps processes optimize workflows for automated data ingest, processing, and storage to help keep up with data needs at scale.





- 标准化方法使数据摄取、转换、验证和准备成为可能。
- 数据治理加速价值实现，涵盖与数据相关的合规、风险和监管问题（包括隐私、安全和访问控制）。
- 负责任的数据政策规定了访问权限、“查看权”授权、伦理原则，以及组织内数据使用的可接受应用。

## 数据战略

从试点转向运营化 AI 需要一个战略来帮助将数据能力与 AI 需求保持一致。这是实现可扩展且可重复的 AI 模型开发流程并运营化 AI 的第一步。

数据战略对于实现 AI 目标并通过以下重点领域实现价值创造的规模化至关重要：

- 识别 AI 数据需求和差距，包括数据使用的实际或法律限制
- 识别获取/收集数据以填补差距的方式，并识别非传统数据源以获得额外洞察，并将此数据整合到现有数据环境中
- 创建集中化的愿景以使数据能力与 AI 项目的需求保持一致
- 使数据管理、转换、质量和治理职能保持一致，以建立和扩展 AI 能力
- 实现对数据相关能力的一致性视图，以便它们能够随 AI 需求而演进
- 定义具体行动、权衡和指标以帮助评估和准备数据能力
- 跟踪数据血缘、来源和质量以帮助 AI 的透明度和可解释性

你的数据战略需要在 AI 语境下回答以下问题：

- AI 可能为你的组织解决哪些传统分析已无法解决的挑战？
- 该战略将如何使组织走上实现目标的道路？
- 哪些具体步骤可以帮助实施该战略？
- 可以将哪些实际影响与该战略联系起来？
- 如何通过将资源与战略保持一致来支持使命并实现价值创造的规模化？

虽然数据在各阶段之间的路径对每个组织都不同，但表 4-1 中的数据战略开发方法将数据战略开发和实施组织为四个阶段，可作为大多数组织的模板。

## 表 4-1. 建议的数据战略开发阶段

| 阶段 | 行动 |
|-------|---------|
| 识别 | - 识别你的组织创建、收集或以其他方式能够访问以训练 AI 模型并扩展 AI 需求的数据。<br>- 识别数据如何在企业内流动以及它如何与 AI 数据需求保持一致。<br>- 识别数据可用性的差距以及获取/收集数据的潜在方式。<br>- 识别非传统数据源，它们可以提供间接洞察。 |
| 评估 | 评估你当前的数据能力并评估数据差距：<br>- 评估数据差距、填补差距的来源，以及在数据获取或使用存在可用性或实际和/或法律限制的情况下的替代方案。<br>- 对当前数据能力进行记分卡评估，与 AI 数据就绪的关键驱动因素进行对比：可用性、可访问性、及时性、准确性、可用性、形式、质量、来源和安全性。<br>- 利用记分卡中的评估标准来衡量组织在人员、流程、工具和技术以及所需文化方面的表现。 |
| 对齐 | 定义数据战略以帮助你的组织解决 AI 需求并从数据中获得最大价值：<br>- 审查记分卡评估结果以确定组织优先需求。<br>- 定义组织就数据使用和管理达成一致的数据原则。<br>- 将其与分析/AI 机会保持一致以优先考虑目标并定义后续目标。 |
| 实施 | 实施数据战略和支持计划以帮助你从数据中获得最大价值：<br>- 制定实施计划、分配资源、定义 KPI 并制定变革管理计划。<br>- 执行由数据战略驱动的举措，并定期评估成果。 |

作为战略开发的一部分，AI 决策者应努力确保数据所有者、业务利益相关者、首席信息官和首席数据官的积极参与。这将有助于战略反映出关于数据及其在组织内适用用例的整体观点。

## DataOps：运营化你的数据战略

DataOps 是运营化你的数据战略以大规模交付企业 AI 需求所需数据的方法论。Gartner 将 DataOps 定义为一种协作式数据管理实践，专注于改善组织内数据管理者和数据消费者之间的数据流的沟通、集成和自动化。⁷

以规模支持 AI 数据需求需要高级管理层的关注。领导者可以确保战略执行成功并防止数据在各个业务单元内“孤岛化”。表 4-2 列出了你在开始构建数据战略时应纳入组织的关键角色和团队。

## 表 4-2. DataOps 中的关键角色

| 关键角色 | DataOps 角色 |
|---|---|
| 首席数据官 | 首席数据官的角色部分是数据战略家和顾问，部分是提高数据质量的管家，部分是数据共享的倡导者，部分是技术人员，部分是新数据产品的开发者。ᵃ |
| 企业信息管理（EIM）组 | 开发沟通、共享并强化组织内数据价值的机制。建立使用可重复流程和框架来治理和管理数据资产的政策和程序。确保数据被视为战略资产。 |
| 跨职能团队 | DataOps 从业者，他们通过各种行业领先的技术和工具实现跨职能所有权文化、可重复流程和数据能力的持续交付。 |
| 业务单元负责人 | 运营单元或业务职能的领导或指定代表，他们创建/来源和管理其数据。 |

> a “Open Data Policy—Managing Information as an Asset”，Project Open Data，访问于 2021 年 7 月 20 日。

首席数据官为对齐提供了锚点。该人员还负责组建企业信息管理组，以在整个组织中标准化并扩展数据管理。

这些团队共同开发机制以在组织内沟通、共享并强化数据的价值。目标是改变员工行为，使数据被视为战略资产。这些数据倡导者与跨职能 DataOps 团队和业务单元负责人一起合作，成为实现数据驱动型组织的必不可少催化剂。

领导层和关键角色可以通过以下几种方式支持数据战略：

- 按比例与业务和使命目标整合数据源。
- 构建可重现的数据环境以支持 AI 透明度和可解释性。
- 监控 DataOps 健康状况以提供有价值的方向并调整运营。

数据工具和管道支持你组织 AI 增长及相关能力。以下原则提供了跟上不断变化需求的实用方法：

### 目标化演进式数据架构

数据架构必须以灵活性和适应性为设计目标，以跟上创新的快速步伐。保持适当的设计抽象允许组件可替换性，这对于跟上 AI 的快速演进是必要的。

### 为规模和灵活性设计数据管道

与整体数据架构类似，在项目开始时为规模和灵活性设计数据管道，远比在后续增长阶段尝试扩展更高效。

### 将 API 接口作为数据共享的构建块

应用程序编程接口（API）定义了数据交换期间如何推送或拉取数据。API 有助于弥合数据孤岛并将其分解为更易用的部分。

## 数据准备活动

将数据战略从概念变为现实需要数据准备活动来为你的 AI 应用提供适当的信息。本节概述了重要考虑因素，并分享了数据摄取、转换和验证的实用技巧。

## 摄取

摄取使来自多个来源的原始数据能够合并为 AI 模型数据管道的单一来源。数据摄取可以支持结构化



and nonstructured data sources, such as databases, documents, web services, and electronic data interchanges. While it is preferable to ingest raw data in as close to its original form as possible, actual decisions are a function of cost, storage, and organizational maturity. This is especially true of applications in genomics, signal data, and finance, where ingesting data in raw form may quickly overwhelm processing capacity. When data is subsequently analyzed and transformed further down the pipeline, it’s important to know what the original data looked like to better understand a model’s results.

There are two data ingestion designs to choose from: batch and streaming. While this is a decidedly technical issue, the key factor in choosing between the two types is “data freshness,” or latency, and whether there is a need to process the most recent data available in near real time.

A *batch architecture* is designed to periodically ingest discrete batches of data and then push them through a series of data action components as part of a one-time processing step. These results are then available for use in your application or ML layer. Alternatively, it can serve as the basis for another data flow pipeline.

Using a batch architecture is the simplest and most common approach to ingesting large amounts of data and is sufficient for most use cases where quantity is more important than freshness and timely delivery of data. Batch architectures are also better suited for training data models, which tend to be compute intensive and rely on a training dataset that is relatively stable without much variation in the short term. As an example, batch data ingestion is useful in training a tumor detection model where a large volume of annotated medical imaging data is fed through a complex algorithm to detect tumor patterns.

A *streaming architecture*, on the other hand, is designed to ingest a live data stream as it occurs. Unlike batch architectures, streaming architectures are primarily focused on providing near real-time updates about the data being streamed in real time through the system (e.g., notifying a user that their bank transaction has posted). Streaming architectures are also more common in inference where operational data is fed through trained models for insights. Streaming architectures are useful in cases where the most recent data needs to be analyzed and acted upon in a timely manner. As an example, streaming is useful in cybersecurity applications due to the need for quick identification of a threat, minimizing the damage that can be caused in a short amount of time.

It should be noted that it is common for an organization to utilize a batch architecture in training AI models while adopting a streaming architecture for operationalizing trained models. Hybrid approaches, such as Lambda architectures, incorporate both batch and stream processing methods into a single deployment. A Kappa architecture pattern, on the other hand, seeks to eliminate the batch layer altogether to reduce complexity. In a Kappa architecture, all data is processed through stream processing methods.⁸

## Transformation

To prepare and deliver ingested data to AI applications, it needs to go through varying degrees of preparation to arrive at the application’s target format. Data transformation has additional value outside of just getting data into a preferred format for executing AI. Transforms can also be used to adapt data into formats accepted by a wide variety of other data capabilities with minimal effort. This minimizes extra work to recreate or transform data manually as your needs change.

To standardize and simplify transformation steps, it is useful to classify them into two types of transforms: static and dynamic.

Static transforms take an input and always produce the same output. These are often the most straightforward to implement and apply. Static transforms are especially useful when custom or one-off logic needs to be applied to inbound data. For instance, when migrating to a new data structure, it may be useful to add logic to transform information from the old to the new format.

Dynamic transforms, on the other hand, add flexibility to data transforms to account for potential changes in needs along the way. In this situation, a generic data transform option is used to align inputs with the desired output types. The dynamic transform then applies the appropriate transform. Multiple inputs can be fed into a single data transform to produce consistent output, simplifying the overarching data flow.

If teams subsequently encounter an issue, the dynamic transform approach can make corrections easier by allowing a configuration update, rather than a full release, to resolve the issue. Highly configurable, dynamic transforms can be widely applied and reused easily. They can also help discover a range of other potential data actions that can be reached by describing the potential output types for a given input type.

*AI-specific preprocessing*: Beyond standard data transformations, real-world applications of AI methods often involve overcoming significant challenges, such as limited data availability, to build and train models. Several AI-specific techniques are used to address some of these challenges. Examples of AI-specific preprocessing methods include data augmentation, which is an umbrella term for a suite of techniques to enhance the size and quality of training datasets so they can be used to build better models.⁹ Augmentation is commonly used in image classification or pattern detection applications when dealing with a limited dataset, such as in medical imaging. Commonly used augmentation techniques include basic image manipulations (flip, rotate, scale, crop, and translate).

## Validation

Although validation is another heavily technical concept, a broad understanding will help you create and implement your data strategy.

In short, your AI application’s machine learning algorithms require data validation steps to ensure data quality, usefulness, and accuracy. Validation increases confidence that the data being consumed is clean and ready for use by ensuring it meets minimum expectations.

Data validation can be broken down into three substages: (1) structural conformance, (2) expectation conformance, and (3) advanced conformance, where each substage is dependent upon the previous substage (Table 4-3). Teams should evaluate errors discovered during conformance checks at any stage in case there are downstream impacts that must be addressed before data is made available to support AI needs.

## Table 4-3. Data validation stages

|  | Structural conformance | Expectation conformance | Advanced conformance |
|---|---|---|---|
| Description | A primary check to ensure data is well formed before attempting to validate the meaning of the individual data elements/across data elements. Validating against the value of the data is not a part of data structure validations. | Ensure that values within the data fall within anticipated parameters. | Aggregated functions of the data can be considered covering two or more fields and are validated in relation to each other. |
| Examples | Fields in the dataset match a set of expected columns. Number of records meets the expected range. | **Type:** Each field or attribute may be validated against the expected data type. **Uniqueness:** Each field may have uniqueness. **Requiredness:** Whether a field or attribute must have a value. **Sets and ranges:** Each field may be validated against the sets and ranges of values. **Format matching:** Each field may be validated against an expected pattern. | **Cross validation:** Each field or attribute may be validated in relation to another field or attribute, typically in the same instance (e.g., row, object). **Aggregate functions:** When there is an expectation of a function of values for a field, rather than an expectation of values, it may be validated against that function. |

## AI Data Governance

Data governance provides oversight for organizations, processes, and procedures to facilitate successful data strategy implementation. AI requires


整体数据治理，不仅专注于创建结构以规范数据的存储、处理和负责任使用，还旨在促进最大化数据的整体可用性。

有效的治理流程确保主题专家——以及其他技术、业务和监管合规利益相关者——提供指导，以协调努力并实现贵组织全企业范围的战略目标。

数据治理优先事项包括：

- 创建针对支持企业规模 AI 数据需求量身定制的政策和程序
- 处理数据获取政策和程序，以纳入法律流程、协议以及数据经纪人/数据服务
- 授权关键利益相关者以促进并领导贵组织的数据治理
- 根据适当的监管框架制定数据安全计划和数据隐私政策（例如，《健康保险流通与责任法》[HIPAA]、《联邦信息安全现代化法》[FISMA]、欧盟《通用数据保护条例》[GDPR] 等）
- 优先考虑元数据管理以确保合规和故障排除支持

1 有关数据可用性和可访问性的定义，请参阅特拉华大学关于管理数据可用性的页面。

2 “准确性”术语定义来自 CDC 关于数据质量的报告。有关及时性的进一步定义也可在此报告中找到。

3 有关数据可用性对 AI 重要性的进一步讨论，请参阅 J. Li 和 X. Lui 的文章“An important aspect of big data: Data usability”。

4 有关质量的进一步定义，可在此 DataCleaner.org 的文章中找到。

5 “出处”术语定义来自 W2C Incubator Group Report，《Provenance XG Final Report》，2010 年 12 月 8 日。

6 有关数据安全的进一步信息，请参阅 [Gartner 的定义](https://www.gartner.com)。

7 [“DataOps”](https://www.gartner.com)，Gartner，访问日期 2021 年 7 月 20 日。

8 有关 Kappa 和 Lambda 架构的进一步阅读，请参阅 [Iman Samizadeh 在 TowardsDataScience 上的文章](https://towardsdatascience.com)。

9 Shorten, C., Khoshgoftaar, T.M. “A Survey on Image Data Augmentation for Deep Learning.” *J Big Data* 6, 60 (2019). https://doi.org/10.1186/s40537-019-0197-0
