## Cloud Data Lake Decision Framework Template

In this section, I’ll provide a template that you can use to plan your cloud data lake solution. You can customize this template as needed based on your specific scenarios or customers. I recommend that you plan at least over a one- to two-year horizon to ensure that your cloud data lake design is sustainable before you have to make drastic changes.

### Phase 1: Assess Framework

*Target 60%–70% accuracy and completeness*

The objective of this phase is to define and prioritize the requirements for the cloud data lake that will drive your architecture and implementation decisions. In this phase, the data engineering team needs to identify the requirements based on two key aspects: customer requirements and business drivers. I strongly recommend doing your due diligence in the assess phase, which will set you up for smooth planning and execution of the subsequent phases.

Use Table A-1 to record the findings from your stakeholder interviews.

### Table A-1. Inventory of problems and requirements

| Customer | Problem | Severity of problem | Helpfulness of data lake | How cloud data lake can help |
|---|---|---|---|---|
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |
|  |  | High/Medium/Low | High/Medium/Low |  |

Once you have defined your requirements, prioritize them against the business drivers, and use **Table A-2** to prioritize the requirements of your cloud data lake.

### Table A-2. Requirements for your cloud data lake

| Priority | Requirement |
|---|---|
| High/Medium/Low |  |
| High/Medium/Low |  |
| High/Medium/Low |  |

| Dataset to be ingested | Frequency | Data prep requirements | Data size |
|---|---|---|---|
|  |  |  |  |
|  |  |  |  |

In **Table A-5**, you can record the data processing requirements, which list the data preparation and data curation transformations and the finalized choices of what data processing technology (as an example, Spark or Hadoop) and tooling (for instance, run Spark on a VM, use a Databricks cluster) will be used.

### Table A-5. Data processing deliverables

| Input datasets | Output datasets | Data processing tool | Requirements (configurations, VMs required, etc.) |
|---|---|---|---|
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |



| 输入数据集 | 输出数据集 | 数据处理工具 | 要求（配置、所需虚拟机等） |

在表 A-6 中，你可以跟踪云数据湖将集成的各种系统。这实际上是你的数据湖的输出，你的客户将使用它。

## 表 A-6. 数据集成交付物

| 系统（仪表板、本地系统、其他云系统） | 数据集 | 数据集成工具 | 要求（配置、所需虚拟机等） | 已签约用于验证的试点客户 |

## 第三阶段：实施框架

*目标 100% 准确性和完整性*

你可以使用表 A-7 来记录和跟踪数据湖内的数据组织。

*表 A-7. 数据湖组织规划*

| 区域（原始/丰富/ curated/工作区） | 数据集 | 访问类型（读/写） | 受众 | 数据保留期 |

一旦你确定了数据资产，你可以使用表 A-8 来记录和跟踪与你的数据集交互的利益相关者，以及这些数据集如何被发现和管理。

*表 A-8. 数据治理跟踪*

| 数据生产者 | 数据集 | SLAs/SLOs | 是否已发布到目录（是/否/计划中） | 基于分类的特殊策略 | 数据保留期 | 数据共享策略 |

| 数据生产者 | 数据集 | SLAs/SLOs | 是否已发布到目录（是/否/计划中） | 基于分类的特殊策略 | 数据保留期 | 数据共享策略 |
|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |

在 **表 A-9** 中，我提供了一个框架，供你跟踪和了解数据湖的各种成本驱动因素，以及你如何优化成本。

## *表 A-9. 数据湖成本管理*

| 数据湖环境（开发/测试/预生产/生产） | SLAs 和 SLOs | 数据存储要求 | 数据计算要求 | 数据网络要求 |
|---|---|---|---|---|
|  |  | 冗余：<br><br>耐久性（版本控制等）：<br><br>分层策略：<br><br>默认保留策略： | 定时作业的集群配置：<br><br>临时作业的集群配置：<br><br>缓存：<br><br>软件许可： | 本地网络带宽：<br><br>跨区域数据访问要求：<br><br>跨区域数据出站要求： |

| 数据湖环境（开发/测试/预生产/生产） | SLAs 和 SLOs | 数据存储要求 | 数据计算要求 | 数据网络要求 |
|---|---|---|---|---|
|  |  | 冗余： | 定时作业的集群配置： | 本地网络带宽： |
|  |  | 耐久性（版本控制等）： | 临时作业的集群配置： | 跨区域数据访问要求： |
|  |  | 分层策略： | 缓存： | 跨区域数据出站要求： |
|  |  | 默认保留策略： | 软件许可： |  |
|  |  | 冗余： | 定时作业的集群配置： | 本地网络带宽： |
|  |  | 耐久性（版本控制等）： | 临时作业的集群配置： | 跨区域数据访问要求： |
|  |  | 分层策略： | 缓存： | 跨区域数据出站要求： |
|  |  | 默认保留策略： | 软件许可： |  |
|  |  | 冗余： | 定时作业的集群配置： | 本地网络带宽： |
|  |  | 耐久性（版本控制等）： | 临时作业的集群配置： | 跨区域数据访问要求： |
|  |  | 分层策略： | 缓存： | 跨区域数据出站要求： |
|  |  | 默认保留策略： | 软件许可： |  |
|  |  | 冗余： | 定时作业的集群配置： | 本地网络带宽： |
|  |  | 耐久性（版本控制等）： | 临时作业的集群配置： | 跨区域数据访问要求： |
|  |  | 分层策略： | 缓存： | 跨区域数据出站要求： |
|  |  | 默认保留策略： | 软件许可： |  |

| 数据湖环境（开发/测试/预生产/生产） | SLAs 和 SLOs | 数据存储要求 | 数据计算要求 | 数据网络要求 |
|---|---|---|---|---|
|  |  | 冗余：<br>耐久性（版本控制等）：<br>分层策略：<br>默认保留策略： | 定时作业的集群配置：<br>临时作业的集群配置：<br>缓存：<br>软件许可： | 本地网络带宽：<br>跨区域数据访问要求：<br>跨区域数据出站要求： |
|  |  | 冗余：<br>耐久性（版本控制等）：<br>分层策略：<br>默认保留策略： | 定时作业的集群配置：<br>临时作业的集群配置：<br>缓存：<br>软件许可： | 本地网络带宽：<br>跨区域数据访问要求：<br>跨区域数据出站要求： |
|  |  | 冗余：<br>耐久性（版本控制等）：<br>分层策略：<br>默认保留策略： | 定时作业的集群配置：<br>临时作业的集群配置：<br>缓存：<br>软件许可： | 本地网络带宽：<br>跨区域数据访问要求：<br>跨区域数据出站要求： |

最后，尽管我们已做好充分准备，但重要的是始终在事情出错时做好准备。你可以使用表 A-10 来了解出现事故时出了什么问题，如何缓解这些问题，以及如何计划改进以减少技术债务，从而最大限度地减少未来发生的可能性。

## 表 A-10. 事件跟踪

| 事件 | 问题类型（新数据/新功能/技术债务） | 受影响的客户 | 问题严重程度 | 待完成工作的描述 | 需求时间线 |
|---|---|---|---|---|---|
|  | 新数据/新功能/技术债务 |  | 高/中/低 |  |  |
|  | 新数据/新功能/技术债务 |  | 高/中/低 |  |  |
|  | 新数据/新功能/技术债务 |  | 高/中/低 |  |  |
|  | 新数据/新功能/技术债务 |  | 高/中/低 |  |  |
|  | 新数据/新功能/技术债务 |  | 高/中/低 |  |  |
|  | 新数据/新功能/技术债务 |  | 高/中/低 |  |  |
