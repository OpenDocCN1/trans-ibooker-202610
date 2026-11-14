
战略规划使 CISO 能够基于业务目标做出安全决策。然而，安全计划的其他特性可能并非由组织的战略目标驱动（例如，组织制造流程的某些方面可能对安全很重要，但未反映在战略计划中）。为了做出明智的决策，CISO 必须遵循一个流程，以建立组织业务目标与安全决策之间的联系。这种联系通常通过解读 `security business drivers` 来实现。

理解、解读和转化 `security business drivers` 使 CISO 能够就安全计划的各个方面做出重要决策，包括：

- 安全目标、目的和计划
- 安全策略、标准、程序和指南
- 安全架构（包括人员、流程和技术）
- 安全控制

`Security business drivers` 是对安全产生影响的业务特性。CISO 用于将安全业务驱动因素转化为安全计划实施的流程取决于组织。方法多种多样，从更多基于交谈或直觉的非正式方法，到利用框架或流程的正式方法。

一些组织采用基于非正式评估或“直觉”的 `informal approach`。这些活动可能只是随意的交谈或信息分享会议，可能不会产生展示决策是如何形成的具体工件。决策也可能表现为高级管理层关于优先事项或关注重点的授权或指令。CISO 也可能对组织进行非正式评估，考虑合规或财务等不同的组织视角。

其他组织遵循使用正式流程、行业框架或模型、风险管理练习或企业架构模型的 `structured approach`。这些严格的流程通常会产生用于证明关键决策合理性的工件，例如风险评估文档、电子表格、架构图和其他文档。这些工件可用于证明 CISO 尽到了应有的职责。

## 企业架构

将安全业务驱动因素转化为安全决策的一种方法是遵循框架以生成企业信息安全架构（EISA）。`Enterprise architecture`（`EA`）是绘制和记录企业架构的实践，旨在协助决策者将组织战略围绕业务的人员、流程和技术进行对齐。EISA 只是将 EA 方法应用于安全计划。CISO 可以使用 EA 模型来分解和记录信息安全计划架构，以及它如何从高层 `business architecture` 视角（如组织结构图）一直到低层 `technical architecture` 流程（如数据流图或网络架构图）与企业相关联。目标是确保这些组件（如安全软件、硬件、解决方案、技术、项目、流程和运营）与组织的企业架构、战略和业务驱动因素保持一致并集成。这允许围绕解决方案和技术进行主动决策，以实现效率和一致性。

组织的企业架构通常可以分为子架构，如业务、信息、应用和技术架构，信息安全组件和影响贯穿这些层，如 [Figure 5-2](#ch5.xhtml#fig5_2) 所示。EA 本质上是一个用于架构分析的概念框架，包含在企业不同层面发生的文档记录和绘图练习。

![Images](Images/F05-02.jpg)

**Figure 5-2** 通用企业架构层

- **Business architecture** 业务架构：驱动信息架构的组织业务战略、目标、治理和目的。
- **Information architecture** 信息架构：也称为数据架构，描述组织信息资产的结构、流程和存储架构。该层的需求规定了应用架构。
- **Application architecture** 应用架构：也称为信息系统架构，提供业务应用和服务的设计。这些由技术架构支持。
- **Technical architecture** 技术架构：支持业务应用和服务的 IT 基础设施，如硬件、软件和网络。

EA 有许多方法，组织可能会使用标准框架，如 Zachman 框架、联邦企业架构框架 (FEAF)、Sherwood 应用业务安全架构 (SABSA) 和 The Open Group 架构框架 (TOGAF)。

### Zachman

Zachman 框架是由 John Zachman 开发的用于企业架构开发的模型。它不是一种方法论，因为它不规定要使用的特定流程。相反，它提供了一个模式，可用于通过创建特定工件（如清单列表、设计文档、流程列表、数据流图和其他文档）来围绕特定功能、要素和流程定义和记录组织的企业架构。

该模型由分类名称（What、How、Where、Who、When 和 Why）与受众视角（Executive、Business Management、Architect、Engineer、Technician 和 Enterprise）相交形成的矩阵组成。这通过回答分类名称中概述的问题，允许记录和整合组织不同的企业观点和视角。例如，高管团队（业务背景）对企业的看法将不同于技术人员（业务组件实施者）。记录这些视角有助于实现企业架构的集成和整体方法。框架的一个简化实例如 Table 5-1 所示。请记住，EA 本质上是记录和绘制企业架构的练习。[Table 5-1](#ch5.xhtml#tab5_1) 中的 Zachman 模式本质上是所有应记录或绘制的工件的列表，以及它们如何与企业架构的各种视角相关联。

![Images](Images/p310.jpg)

**Table 5-1** Zachman 框架示例

虽然 Zachman 框架并非专门为信息安全而构建，但这些问题大多具有潜在的安全含义。此外，它是一个通用模型，可应用于多种类型的企业架构并进行定制，包括 EISA。

### FEAF

`Federal Enterprise Architecture Framework (FEAF)` 是由联邦 CIO 委员会开发的企业架构框架，旨在响应 1996 年 Clinger-Cohen 法案中概述的要求，该法案要求美国政府分支机构开发和维护企业架构框架。目标是帮助联邦机构利用通用语言分析其企业架构投资，并确定“良好”的标准。该框架由六个参考模型组成，如 [Figure 5-3](#ch5.xhtml#fig5_3) 中的 FEAF Version 2 Consolidated Reference Model (CRM) 所示，描述了这些架构域：

![Images](Images/F05-03.jpg)

**Figure 5-3** 联邦企业架构框架版本 2 综合参考模型  

(Source: [https://obamawhitehouse.archives.gov/sites/default/files/omb/assets/egov_docs/fea_v2.pdf](https://obamawhitehouse.archives.gov/sites/default/files/omb/assets/egov_docs/fea_v2.pdf))

- Performance（绩效）
- Business（业务）
- Data（数据）
- Application（应用）
