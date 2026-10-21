# Chapter 1. Demystifying AI

A majority of analytics exist to take operational data (e.g., past/present stock prices) and provide focused insights (e.g., predicted stock prices) that inform decision making. This essential objective is the same for conventional business analytics and AI analytics and includes a range of functions (e.g., automation, augmentation, conversational AI for consumers, etc.).

The key difference is how developers create the code that transforms operational data into insights. For conventional business analytics, this is a static process where the developer manually defines each logical operation the computer must take.

AI analytics, via machine learning (ML), attempts to derive the necessary operations directly from the data, reducing the onus on the developer to create and update the model over time (but not eliminating the developer) and making it possible to address otherwise prohibitively sophisticated use cases (e.g., computer vision).¹

## Machine Learning (ML)

A subset of AI, machine learning is the study of computer algorithms that improve automatically through experience and by use of data. ML algorithms build a model based on sample data, known as “training data” (defined in Chapter 5), to make predictions or decisions without being explicitly programmed to do so.

Beyond the initial challenge of the ML algorithm teaching an AI analytic to complete a basic task, we must ensure that it does not learn additional, undesirable behaviors that may impact its long-term sustainability (reliability, security, etc.). The ability to holistically understand the learned



行为 of an AI analytic is called explainability and will be explored in detail in the following chapters.

随着机器学习，计算机模型使用经验/历史数据通过识别数据中的模式来做出决策。这些经验有多种形式。例如，它们可以通过回顾历史过程数据或观察当前流程来收集，或者可以使用合成数据生成。然而，在许多情况下，实践者必须在这些模式被使用之前手动提取它们。模式的复杂程度和由此产生的操作可以根据所选算法、使用学习参数以及训练数据被处理到算法中的方式而有很大差异。

同样，AI（更具体地说——深度学习子领域）使用模型，例如神经网络，来学习各种数据类型中的高度复杂模式。概括地说，AI 使计算机能够感知、从数据中学习、抽象并对数据采取行动，同时自动识别提供的数据集中的模式。²

AI 可用于各种用例——其中一些你可能已经熟悉。AI 可部署来识别模式的几个常见示例包括：

- 1. 检测数据集中的异常条目（例如，识别欺诈性与合法的信用卡购买）
- 2. 将图像中的一组像素分类为熟悉或不熟悉（例如，建议你手机拍摄的照片中可能包含你的哪些朋友）
- 3. 根据你的历史提供新的娱乐选择建议（例如，Netflix、Spotify、Amazon）

我们也可以描述 AI 不是什么——至少目前不是。例如，一些老科幻电影描绘了机器人具有与人类进行复杂、即兴和流畅对话的能力，或在意外情况下执行复杂行动和决策的能力，就像人类一样。事实上，

我们还没有达到那种 AI 复杂程度；要达到那里，需要大量持续的投资来推进当前 AI 能力。

目前，AI 的运营实例代表了所谓的 *狭窄智能*，或在受控情况下补充人类判断以做出单一决策的能力。人工 *通用智能*，即机器能够匹配人类在不受控情况下执行多个决策的能力，目前并不存在。虽然最近已有进展朝向通用智能方向发展，³ 但我们距离这种类型的 AI 在任何有意义的规模上出现仍相去甚远。图 1-1 提供了 AI 在今天能做什么和不能做什么的高层次概述。

## What AI can do well today

- 执行一些简单、定义明确的任务，其表现与人类一样好或更好
- 发现并利用数据中的模式——包括人类看不到的模式
- 在提供大量标记、组织良好的数据供其学习时，对给定任务的执行能力会更好

## What AI cannot do well today

- 执行任何整项工作比人类更好
- 解释其在信息中寻找模式的机制或这些模式的含义（超出为其贴标签）
- 理解围绕给定任务的上下文
- 从无组织、无标签或少量数据中学习
- 执行需要创造力、同理心或复杂判断的任务

Figure 1-1. AI limitations

## AI Pilot-to-Production Challenges

成熟的 AI 能力不会一夜之间出现。相反，它们需要数月到数年的持续、协作、跨组织的努力才能实现。在组织内部，变革推动者创建和维持跨利益相关者（例如，战略领导层、终端用户和风险管理人员）的认同是一个关键且必不可少的挑战。

在实验室条件下执行的 AI 分析试点（精选用例、 curated 数据、受控环境）是在适度成本下创造初始认同的最佳方式之一。然而，大多数分析用例需要组织将这些试点从实验室环境推广到生产环境，才能在这些选定用例中充分解决分析挑战。

许多组织常犯的错误是低估了在这些环境之间过渡的挑战，并且未能相应地成熟其开发能力。几个主要挑战包括可扩展性、可持续性和协调性。

## Scalability

在试点期间，AI 开发团队在角色和流程方面可以很小很简单，因为它们只解决单一用例。随着 AI 能力成熟并从试点迁移到生产，项目量通常会比可用人员更快地大幅上升。特别是，已在生产中的分析将开始与新部署竞争资源（由第 2 项中的可持续性挑战放大）。这要求开发团队、流程和工具的演进，以允许个人和集体在多个团队和项目之间分配劳动。此外，开发中涉及的数据量和速度将增加，要求越来越强大、高效和复杂的训练管道（在第 5 章中广泛讨论）。

## Sustainability

出于设计目的，实验室环境限制了对分析可持续性的威胁，以帮助试点团队专注于功能。一旦进入生产，分析将受到多种问题的困扰，包括运营（例如，负载变化、数据漂移、用户错误）、安全、法律和伦理。确保可持续性不会损害可扩展性需要开发的演进，以在发布前预见并解决这些问题。
可持续性也受益于协调（见第 3 项），以允许关键利益相关者参与其中（见第 3 章）。

## Coordination

在实验室环境中，试点团队出于设计目的仅与有限数量的利益相关者互动。随着这些试点进入生产，利益相关者数量会急剧增加，你的团队必须准备好在数据所有者、终端用户、运营人员、风险管理人员和其他人之间激发和促进协调。协调也有助于确保组织内劳动的公平和高效分配。

除了我们刚刚讨论的三个之外，表 1-1 提供了在将 AI 解决方案从试点推广到生产时你可能面临的更完整的挑战列表。

## Table 1-1. AI pilots versus AI in production

| Challenges to Operationalizing AI | |
|---|---|
| AI Pilots | AI in Production |
| Simplified, static use case | Multistakeholder, dynamic use case |
| High-performance laboratory environment | Distributed legacy systems with dynamic fallback options |
| Openly accessible, low latency, data remains consistent | Access controlled; latency restricted; high-velocity data |
| Complete responsibility and control over data | Data mostly controlled by upstream stakeholders |
| No change or widely anticipated changes to data | Rapid, unexpected data drift |
| One-time, manual explanation for algorithm’s results | Real-time, automated explanation |
| AI developer does not reexamine model after pilot | AI developer continues to monitor model |
| No-cost shut-down, refactor | Costly shut-down, refactoring |
| One-off development against a single use case | Reproducible development against multiple use cases |
| Small team with well-defined focus and requisite skill sets | Continual training for a wide mix of experience, skill levels, and specialties |
| Informal, research-oriented project management | Hybrid research/development project management |

未能使 AI 能力成熟以满足这些挑战，会威胁到 AI 采用的长期可行性，因为组织将难以以生产能力实施人工智能。AI 开发将放缓，现有分析将难以维持，领导层将因缺乏持久的任务影响而感到幻灭，终端用户将失去信心，并将引发艰难的对话。在设计阶段主动解决这些挑战，会使组织大幅提高 AI 计划的采用速度和影响。⁴

在接下来的章节中，我们将介绍一个框架（AIOps）来解决这些挑战，并使你的组织能够在整个企业范围内最大化 AI 的影响。





1 Aurélien Géron, *Hands-On Machine Learning with Scikit-Learn and TensorFlow* (O’Reilly Media, 2017)。

2 虽然人工智能存在许多、且往往相互竞争的技术定义，但我们希望为本报告提供一个宽泛、高层次的定义。我们的 AI 定义摘自 [国家安全委员会关于人工智能](https://)。你可以在其网站上查看他们 2021 年的报告，其中在第 20 页对 AI 进行了定义。

3 关于通用人工智能领域持续快速发展的当前研究和思考的一个例子是 David Silver、Satinder Singh、Doina Precup 和 Richard S. Sutton 的 “Reward Is Enough”，*Artificial Intelligence* 299 (2021 年 10 月)。

4 Mike Loukides, *AI Adoption in the Enterprise* (O’Reilly Media, 2021)。
