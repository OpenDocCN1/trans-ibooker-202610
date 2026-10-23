# Innovative SAP SuccessFactors Recruiting：自定义集成和自动化创建指南

*献给 Kim Lessley。*

![](img/879ce8537ecb7481ad77093e6fcbe574_10_0.png)

**Anand ‘Andy’ Athanur** has over 25 years of HR systems experience and currently works at SAP enabling partners to innovate their solutions with SAP SuccessFactors.

![](img/879ce8537ecb7481ad77093e6fcbe574_10_1.png)

**Mark Ingram** has extensive knowledge of recruiting systems and integrations across his 25-year career as a developer, product manager, and consultant. There isn’t a recruiting problem that he doesn’t like!

![](img/879ce8537ecb7481ad77093e6fcbe574_10_2.png)

**Michael A. Wellens, M.S.**, is a certified SAP SuccessFactors consultant with over 15 years of human resources information systems implementation experience. He has successfully launched a variety of core HR and talent management solutions across a number of Fortune 500 companies around the world and has written on diverse SAP, HR, and IT topics. You can follow him on LinkedIn or on Twitter.





## 关于技术审阅人

![](img/879ce8537ecb7481ad77093e6fcbe574_11_0.png)

**Rinky Karthik** 是 SAP 全球解决方案架构团队的高级 HXM 云架构师。她是一名认证的 SuccessFactors 专业人员，在多个全球实施项目中涉及各种 SuccessFactors 模块，并拥有丰富的 SAP 本地化（On-Premise）实施经验。作为 SAP HXM 解决方案架构师，她在为拥有复杂业务和创新业务模式的全球组织提供 SuccessFactors 解决方案架构咨询服务方面发挥了重要作用，成功推动了云转型，并考虑了最佳实践指导。

Rinky 合著了 SAP Press 出版的书籍 *Integrating SAP SuccessFactors*，并撰写了 SAPinsider 出版的书籍 *SAP SuccessFactors Recruiting: The Complete Integration Guide*。

## 致谢

感谢 David Ludlow 开启招聘路径的 Markdown 之旅。

感谢 SuccessFactors 招聘产品团队多年来持续的支持。

#### 引言

人才获取是任何组织的关键业务流程。没有关键资源，组织根本无法运转。随着世界继续面临与全球大流行病相关的经济挑战，人们最近更是深刻感受到了这一点。当各公司竞相争夺熟练资源时，它们通常会寻找招聘系统来帮助获得竞争优势。随着许多客户订阅 SAP SuccessFactors 等云软件，关键差异化因素更难寻找。云客户可能会问：“如果我们中的许多人都使用相同的软件，我们如何能在社区中脱颖而出？” 此外，根据我们的经验，许多供应商和客户在使用 SAP SuccessFactors 时，需要资源来弥合产品文档和实际案例之间的差距。

在本书中，我们将探讨如何创新您的 SAP SuccessFactors 招聘解决方案。我们将逐步介绍端到端的招聘业务流程，并突出 SAP SuccessFactors 允许进行自动化和集成的关键领域。在每一步中，我们将引导您使用一些工具，以您自己的创新方式增强流程。在第 1 章中，我们将对 ODATA 进行基本概述，使您熟悉可以在招聘过程中使用的协议。在第 2 章中，我们将了解如何使用非 SAP 系统自动化职位申请创建。第 3 章查看了使用中间件执行相同流程。第 4 章研究了如何增强招聘发布流程。然后，在第 5 章中，我们查看了增强候选人互动的不同方式。第 6 章将机器人流程自动化视为自动化各种招聘流程的一种方式。同样，第 7 章展示了如何使用集成中心自动化这些流程。随着我们进入第 8 章，我们将看到系统如何使用评估集成框架与评估供应商集成。在第 9 章中，我们专注于 ODATA 集成。然后，在第 10 章中，我们将看到 SAP SuccessFactors 如何使用背景调查集成框架与背景调查供应商集成。第 11 章引导我们了解如何使用 SAP SuccessFactors 业务规则功能来自动化候选人录用流程。然后，在第 12 章中，我们查看了智能服务功能如何帮助自动化流程。在第 13 章中，我们查看了如何使用非 SAP 系统自动化入职流程。最后，在第 14 章中，我们以如何实现业务价值和得出结论作为结尾。读完本书后，您应该对每个步骤中可用于增强招聘系统的工具有扎实的了解。此外，您将了解来自作者实际经验中的实用示例。
