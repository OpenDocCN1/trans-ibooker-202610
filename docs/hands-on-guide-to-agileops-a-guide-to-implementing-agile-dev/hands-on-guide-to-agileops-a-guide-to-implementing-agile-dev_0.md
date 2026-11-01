# AGILEOPS 实用指南：云...运维的敏捷、DevOps 和 SRE 实现指南







## 关于作者

![](img/f561fe6de202c361a03741b184bee8ea_8_0.png)

Navin Sabharwal 拥有超过 23 年的行业经验。他是敏捷、云计算、DevSecOps、AIOps、FinOps、人工智能和 SaaS 产品工程等领域的思想领袖，也是战略与开发负责人。他在人工智能和机器学习领域持有七项专利。他著有 15 多本关于云容量管理、可观测性、DevOps、云自动化、容器化、Google AutoML、BERT 和 NLP 的书籍。他可以通过 Navinsabharwal@gmail.com 和 www.linkedin.com/in/navinsabharwal 联系。

![](img/f561fe6de202c361a03741b184bee8ea_8_1.png)

Raminder Rathore 是一名拥有二十年 IT 经验的 DevOps 从业者。目前她在 HCL Technologies 担任实践负责人，帮助客户推动敏捷性和韧性方面的转型项目。她在推动和完成企业范围的 DevOps 和敏捷项目方面取得了成功，包括制定转型、流程优化和自动化工具合理化（包括开源和 COTS）的组织路线图；规划和架构 CI/CD 流水线；为云平台设计服务目录；构建端到端产品可追溯性加速器；以及指导团队采用敏捷工作方式。她可以通过 raminder.rathore@gmail.com 和 https://www.linkedin.com/in/raminder-devops-practitioner/ 联系。

## 关于译者

![](img/f561fe6de202c361a03741b184bee8ea_9_0.png)

**Udita Agrawal** 是一名拥有超过 17 年 IT 经验的 DevOps 从业者。她在开发、测试、数据分析和基础设施运营方面的敏捷和 DevOps 实施方面拥有丰富的经验。目前她在 HCL Technologies 担任高级 DevOps 顾问，与不同客户合作以实现和改进敏捷和 DevOps 方面的实践和流程。她还对为团队提供相关领域的培训感兴趣。她可以通过 udita.agrawal@gmail.com 和 [www.linkedin.com/in/udita-agrawal-DevOps-Consultant](www.linkedin.com/in/udita-agrawal-DevOps-Consultant) 联系。

## 关于技术审校

**Afzaal Ahmad Zeeshan** 是 Adyen 的开发者倡导者，喜欢使用 .NET Core 和 Node.js 进行日常开发。他是云、移动和 API 开发方面的专家。Afzaal 拥有 Azure 平台的经验，并喜欢使用 .NET Core 构建跨平台库/软件。他为 Apress 撰写了 *面向 .NET Core 的 DevSecOps*，以及许多在线技术文章。

Afzaal 是阿里云 MVP；他因在软件开发方面的思想领导力两次获得 Microsoft MVP 称号，因技术写作和指导五次获得 CodeProject MVP 称号，并在同一领域四次获得 C# Corner MVP 称号。Afzaal 是 GitHub 和 GitLab 上的活跃开源贡献者。你可以在 `afzaal-ahmad-zeeshan` 和 `@afzaalvirgoboy` 找到他。

## 致谢

致我的家人 Shweta 和 Soumil，感谢你们始终在我身边，为我的智力和精神追求牺牲时间。没有你们的爱和支持，我的这些以及其他成就都不可能实现。

致我的妈妈和姐姐：感谢你们一如既往的爱与支持；没有你们的祝福，一切都不可能。

致我的合著者 Raminder 和 Udita：感谢你们的辛勤工作和快速交付本书。这是一次充实的经历，我期待很快再次与你们合作。

致我的 HCL 团队，你们以辛勤工作、引人入胜的技术对话和技术深度成为我的灵感来源：Raminder、Udita、Piyush、Amit、Sarvesh、Subramani、Praveen、Sagar、Amit Dwivedi、Sandeep、Shakuntala、Vasand、Punith、Nitin、Vivek、Gaurav Bali、Arvind Gautam 和 Inderpal。

感谢 Celestine、Aditee 以及 Apress 整个团队将我们的想法变为现实。与你们合作撰写书籍是一次美妙的经历，多年来决策速度和编辑支持都非常出色。

感谢女神 Saraswati 引导我们走向知识和精神的道路。

असतो मा साद गमय, तमसो मा ज्योतीर् गमय, मृत्योर् मा अमृतम् गमय
(Asato Ma Sad Gamaya, Tamaso Ma Jyotir Gamaya, Mrityor Ma Amritam Gamaya)
引导我们从无知走向真理，从黑暗走向光明，从死亡走向不朽。



## 前言
Agile 改变了当今软件开发的方式。数字化组织和初创企业已拥抱敏捷和 DevOps 并从中受益。然而，基础设施和云运营领域在很大程度上仍未受到敏捷运动的影响。基础设施和运营领域在很大程度上仍继续使用 IT 服务管理（ITSM）和 ITIL 来运行大规模运营。

对于运营而言，有许多可采用的最佳实践，即敏捷、DevOps、站点可靠性工程（SRE）、精益、ITIL、IT4IT；然而，关于如何在 IT 基础设施领域使用这些流程，目前尚无指导。同样，运行这些流程的工具是可用的，但关于如何使用这些工具来运行运营，目前也缺乏指导。

随着云计算的兴起，了解 IaaS、PaaS 或 SaaS 对企业的好处变得同样重要。为了提高环境和应用程序的可用性和正常运行时间，架构已从单体转向微服务。

存在众多敏捷方法，如 Scrum、Kanban 和 Scrumban，以及不同的规模化敏捷框架，如 SAFe®、Spotify、LeSS、Nexus 和 DAD。企业在决定采用哪种敏捷框架和方法时感到困难。它们可以轻松地应用于本地和云环境的运营。

本书是一本实践指南，介绍如何采用敏捷、DevOps 和 SRE 实践；如何为其制定路线图；以及如何在运行云和基础设施运营的同时，为组织选择最合适的流程，以实现更高的可靠性、敏捷性和更低的成本。

具体而言，本书详细涵盖以下内容：
- 软件开发与运营中的敏捷
- ITIL、IT4IT 和精益及其在敏捷运营中的相关性
- Scrum、Kanban 和 Scrumban 敏捷方法
- 敏捷运营的规模化敏捷框架
- 站点可靠性工程及其与敏捷和 DevOps 的集成
- DevOps 及其在敏捷运营中的适用性
- 基础设施即代码及其与流水线的集成
- 运营的敏捷故事和示例
- 创建敏捷运营的团队结构
- 为运营制定敏捷采用路线图
- 敏捷运营案例研究

我们欢迎您踏上这段激动人心的旅程，通过利用各种流程、方法和技术，发现运行 IT 运营的新方式，以创建高效、经济且敏捷的模式，满足现代数字企业的需求。

我们还将持续创建内容和最佳实践，您可以在配套网站 www.AgileInfraOps.com 上查看。
