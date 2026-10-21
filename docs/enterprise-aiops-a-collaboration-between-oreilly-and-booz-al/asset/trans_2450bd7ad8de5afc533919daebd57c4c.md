

在技术基础设施上实施 DevSecOps³ 作为更广泛的 AIOps 框架的组成部分）。

## DevSecOps

基于 DevOps（一套致力于提高开发人员与运维人员之间沟通和协调效率的流程和技术），DevSecOps 在此基础上进一步扩展，将安全纳入其中，并需要使系统对有动机对手的蓄意干扰具有鲁棒性。

每个组织的参考架构实现都将不同，但基本组件保持不变（图 6-3）。参考架构可作为黄金标准，使您的团队能够快速部署自己的 AI 模型和应用程序。通过建立参考架构，您的组织可以从抽象框架中产生现实世界解决方案（解决方案基线）。这些解决方案基线对应于您组织的具体用例，并形成一个可重用组件的通用框架，符合通用架构，从而提高企业 AI 能力的效率和可扩展性。

## AIOps RA capabilities

### Data delivery

### Machine learning

### Observability & transparency

AIOps RA components

Data flow

Data repository

Data ingest

Data transform

Data access

Data enrichment

Data versioning

Data validation

Data preparation

Exploratory analysis

Feature engineering

Model training

Feature selection

Model evaluation

Model versioning

Model validation

Drift detection

Retrain detection

Model perf detection

Alerting

Artifact repository (model, applications, source code, docker, etc.)

Ethics, explainability, bias

Configured/custom software (i.e., BI, notebooks, custom, microservices)

Environment monitoring

Provenance collection and reporting

Metadata collection and reporting

Security (auth'n, auth'z, environment)

DataOps

MLOps

*Ops

## Path to production

## Development

## DevSecOps

## Staging & production

Key

Capability

Component

Repository

Environment

Automation

Figure 6-3. Reference architecture

要支持全面的参考架构，您还必须考虑数字架构决策，包括基础设施和开发流程，以将您的 AI 能力从原型演进为已部署的解决方案。您的数字架构选择应支持参考架构和 AIOps 框架的所有组件，包括与终端用户、数据平台和分析技术的接口。考虑到这些相互关联的元素，以下是围绕这些数字架构组件的一些高层考虑，以开发协调、可扩展和可持续的 AI 能力。

## Technical Infrastructure

从本质上讲，AI 模型训练在计算上可能非常密集，通常需要大量算力。用于训练简单/中等复杂度的深度学习模型的类型硬件是专用实例或由图形处理器单元（GPU）或 CPU 与 GPU 组合驱动的小型/中型集群。高性能计算（HPC）环境可针对涉及高维数据集的更复杂模型进行优化，并配备 GPU 和并行分布式文件系统。

## Development Processes

DevSecOps 提供标准化流程，包含组织开发工作的框架，包括跨技术团队的协作以及在其整个生命周期内对能力的统一文档记录。在开发期间，该流程及其相关工具对于使跨职能团队能够高效地开发和测试 AI 模型至关重要。部署后，我们还考虑 AI 能力的长期可持续性，例如能力的持续集成、交付以及对其对组织影响的评估（基于定义的绩效结果）。

## AIOps Integration

现在我们了解了来自 AI 采用蓝图的整体关注点，包括建立目标和绩效衡量，是时候考虑整合使 AI 运营化的要素的方法和流程了。为了实现这些期望的结果，我们必须考虑 AIOps 的组件流程——具体来说是 DataOps、MLOps 和 DevSecOps——包括它们在人员、流程和技术方面的相互关系和依赖性。首先，我们将概述这些考虑因素及其实施，然后从更技术性的角度深入探讨这些流程的整合。

## Operational Components

AI 能力旨在在部署以支持业务流程时提供价值。为此，团队必须将 AI 能力集成到历史上支持业务流程的系统中。鉴于系统复杂性的指数级增长，扩展和维持 AI 需要对流程和依赖关系进行整体视图——以及结构化的工程方法——以将 AI 模型组合转化为已部署的运营能力。图 6-4 将此视图定义为应包含并在组织内集成的核心组件，以实现成功的 AI 部署。

## AI process

## AI integration

Mission and system engineering

+ Pipeline utilities
+ Blueprints
+ Registries
+ Repositories
+ Standards
+ Services/tools

## AI pipelines

## DataOps

+ - Processing
+ - Versioning and validating
+ - Storage

## MLOps

+ - Model training
+ - Analytics
+ - Diagnostics and monitoring

## DevSecOps

+ - Programming
+ - Packaging
+ - Orchestration and deployment

Processes

Federated & distributed learning

## Enterprise IT infrastructure

End users

AI tools

Server/hub

Edge devices

Inference and diagnostic returns

Services

AI/ML application services

Configuration management and orchestration

Enabling components

Data management systems

Infrastructure and workspace environments

Figure 6-4. AIOps components

如图 6-4 所示，DataOps、MLOps 和 DevSecOps 构成了应根据您组织特定用例定制的 AIOps 核心运营组件。将其视为 AIOps 整体，这些方法的整合侧重于通过以下组合将组织需求转化为数据和分析流程：

+ - 开发集成自动化管道，跨越从数据摄取和处理到模型训练再到后续 AI 模型部署
+ - 根据需要更新集成到系统和业务流程中的 AI 模型，以确保它们保持最新和相关，而非静态
+ - 在数据、模型和开发结构之间持续集成和部署流程，并纳入反馈循环以推动流程改进
+ - 在所有阶段支持模型可解释性的元数据收集流程

在考虑我们将这些运营框架纳入的方法时，我们应重申 DataOps、MLOps 和 DevSecOps 在更广泛的 AIOps 框架中的一致性。我们将在下一节描述组件整合时使用这些工作定义。AIOps 整合的整体方法包含以下流程：

+ 1. 利用迭代的 DataOps 流程创建数据管道、普及访问权限，并监控和自动化流程以满足战略数据需求，从而实现 AI 能力。
+ 2. 结合 MLOps 以调整模型实现，更好地确定最适合给定运营需求的方法；MLOps 专注于 ML 模型的自动化训练、部署、监控和管理。

+ 3. 实施 *DevSecOps* 以协调更广泛的软件应用程序开发，支持 ML 模型的快速集成并部署到目标托管环境。如前所述，DevSecOps 在已建立的 DevOps 流程和实践中增加了关键的安全要素。

从企业角度来看，AIOps 组件作为支持组织目标的更大使命关键系统生态系统的一部分存在。鉴于使命关键系统的复杂性和快速演变性质，重要的是要遵循将 AI 能力集成到特定组织需求的最佳实践。与其他使命关键系统一样，AI 系统需要通过全面的企业安全方法进行保护，所有安全最佳实践均适用。

总体而言，该方法允许使用标准流程以可扩展和可重复的方式集成和迭代开发解决方案，同时在整个流程中保持完全可见性，这得益于元数据的帮助，元数据在集中式数据平台中收集。在以下部分，我们将应用 AI 采用蓝图来解决 AIOps 框架这些基本组件的整合问题。

## Component Integration

设计和开发企业 AI 能力需要对各个 AIOps 组件进行开发，以及上游和下游的协调，以使这些组件能够协同支持彼此。如本报告和 AI 采用蓝图所述，我们在通往整体 AIOps 整合的道路上考虑了几个相互关联的元素：负责任的 AI 以指导员工的旅程，数据工程以驱动分析流程；分析框架以从数据中获取价值和洞察；通过数字架构满足技术要求和持续部署。

