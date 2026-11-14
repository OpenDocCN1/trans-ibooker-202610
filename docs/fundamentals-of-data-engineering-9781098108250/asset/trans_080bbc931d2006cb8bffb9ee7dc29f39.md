

## 数据管理

数据管理是一个广泛的领域，就技术而言，并不总是显而易见某项技术是否将数据管理作为主要关注点。例如，在幕后，第三方供应商可能会使用数据管理最佳实践——合规性、安全性、隐私、数据质量和治理——但会将这些细节隐藏在有限的 UI 层之后。在这种情况下，在评估产品时，询问公司其数据管理实践会有所帮助。以下是您应该询问的一些示例问题：

*   你们如何防止数据泄露，无论是来自外部还是内部？
*   你们的产品对 GDPR、CCPA 和其他数据隐私法规的合规性如何？
*   你们是否允许我托管自己的数据以符合这些法规？
*   你们如何确保数据质量，以及我是否在你们的解决方案中查看的是正确的数据？

还有许多其他问题可以问，这些只是考虑数据管理与选择正确技术关系时的几种思路。这些相同的问题也应适用于您正在考虑的开源软件（OSS）解决方案。

## DataOps

问题总会发生。这不可避免。服务器或数据库可能宕机，云厂商的某个区域可能发生中断，您可能部署了有 Bug 的代码，错误数据可能被引入您的数据仓库，以及其他不可预见的问题可能发生。

在评估新技术时，您对部署新代码有多少控制权？如果出现问题，您将如何收到警报？当出现问题时，您将如何响应？答案在很大程度上取决于您正在考虑的技术类型。如果该技术是开源软件（OSS），您很可能需要负责设置监控、托管和代码部署。您将如何处理问题？您的事件响应流程是什么？

如果您使用的是托管产品，大部分运维工作都不在您的控制范围内。请考虑供应商的 SLA、他们通知您问题的方式，以及他们是否透明地说明了如何处理该问题，包括提供修复的预计时间（ETA）。

## 数据架构

正如[第 3 章](#ch03.html#designing_good_data_architecture)所讨论的，良好的数据架构意味着评估权衡并为工作选择最佳工具，同时保持决策的可逆性。随着数据版图以极快的速度演变，完成工作的*最佳工具*是一个移动的目标。主要目标是避免不必要的锁定，确保数据栈的互操作性，并产生高投资回报率（ROI）。请据此选择您的技术。

## 编排示例：Airflow

在本章的大部分内容中，我们一直主动避免过多讨论任何特定技术。我们为编排破例，因为该领域目前由一种开源技术主导，即 Apache Airflow。

Maxime Beauchemin 于 2014 年在 Airbnb 发起了 Airflow 项目。Airflow 从一开始就作为一个非商业开源项目进行开发。该框架迅速在 Airbnb 之外获得了显著的关注度，于 2016 年成为 Apache 孵化器项目，并于 2019 年成为 Apache 顶级项目。

Airflow 享有许多优势，主要归功于其在开源市场的主导地位。首先，Airflow 开源项目极其活跃，提交频率高，对 Bug 和安全问题的响应时间快，且项目最近发布了 Airflow 2，这是代码库的一次重大重构。其次，Airflow 拥有巨大的关注度。Airflow 在许多通讯平台上拥有充满活力、活跃的社区，包括 Slack、Stack Overflow 和 GitHub。用户可以轻松找到问题和难题的答案。第三，Airflow 可通过许多供应商（包括 GCP、AWS 和 Astronomer.io）作为托管服务或软件分发版进行商业获取。

Airflow 也有一些缺点。Airflow 依赖几个核心的不可扩展组件（调度器和后端数据库），这些组件可能成为性能、规模和可靠性的瓶颈；Airflow 可扩展的部分仍然遵循分布式单体模式。（参见[“单体与模块化”](#ch04.html#monolith_versus_modular)。）最后，Airflow 缺乏对许多数据原生构造的支持，例如模式管理、血缘和编目；并且开发和测试 Airflow 工作流具有挑战性。

我们在此不试图详尽讨论 Airflow 的替代方案，仅提及撰写时几个关键的编排竞争者。Prefect 和 Dagster 旨在通过重新思考 Airflow 架构的组件来解决前面讨论的一些问题。会有其他此处未讨论的编排框架和技术吗？请有此预期。

我们强烈建议任何选择编排技术的人都研究此处讨论的选项。他们还应熟悉该领域的动态，因为当您阅读此内容时，肯定会有新的发展发生。

## 软件工程

作为一名数据工程师，您应致力于跨数据栈进行简化和抽象。尽可能购买或使用预构建的开源解决方案。消除无差异的繁重工作应是您的主要目标。将您的资源——定制编码和工具——集中在能为您带来可靠竞争优势的领域。例如，在您的生产数据库和云数据仓库之间手工编写数据库连接代码，这对您来说是竞争优势吗？很可能不是。这是一个非常成熟的已解决问题。请选择现成的解决方案（开源或托管 SaaS）。世界不需要第 100 万零 1 个数据库到云数据仓库的连接器。

另一方面，客户为什么向您购买？您的业务在做事方式上可能有其独特之处。也许是驱动您金融科技平台的特定算法。通过抽象掉大量冗余的工作流和流程，您可以继续削减、完善和定制那些能为业务带来实质性进展的事物。

# 结论

选择正确的技术并非易事，尤其是当新技术和模式每天都在涌现时。今天可能是历史上评估和选择技术最令人困惑的时期。选择技术是在用例、成本、自建与购买、模块化之间的平衡。始终以对待架构的方式对待技术：评估权衡，力求决策可逆。

# 附加资源

*   [*Cloud FinOps*](https://oreil.ly/1qj1p) 作者：J. R. Storment 和 Mike Fuller（O’Reilly）
*   [“Cloud Infrastructure: The Definitive Guide for Beginners”](https://oreil.ly/jyJpz) 作者：Matthew Smith
*   [“The Cost of Cloud, a Trillion Dollar Paradox”](https://oreil.ly/Wjv0T) 作者：Sarah Wang 和 Martin Casado
*   FinOps Foundation 的 [“What Is FinOps” 网页](https://oreil.ly/TO0Oz)
*   [“Red Hot: The 2021 Machine Learning, AI and Data (MAD) Landscape”](https://oreil.ly/aAy5z) 作者：Matt Turck
*   Ternary Data 的 [“What’s Next for Analytical Databases? w/ Jordan Tigani (MotherDuck)” 视频](https://oreil.ly/8C4Gj)
*   [“The Unfulfilled Promise of Serverless”](https://oreil.ly/aF8zE) 作者：Corey Quinn
*   [“What Is the Modern Data Stack?”](https://oreil.ly/PL3Yx) 作者：Charles Wang

^[1](#ch04.html#ch01fn33-marker) 更多详情，请参阅 Joseph Reis 在 [*97 Things Every Data Engineer Should Know*](https://oreil.ly/J8PBq) (O’Reilly) 中的“Total Opportunity Cost of Ownership”。

^[2](#ch04.html#ch01fn34-marker) J. R. Storment 和 Mike Fuller, *Cloud FinOps* (Sebastopol, CA: O’Reilly, 2019), 6, [https://oreil.ly/RvRvX](https://oreil.ly/RvRvX)。

^[3](#ch04.html#ch01fn35-marker) 这是 Storment 和 Fuller 的 [*Cloud FinOps*](https://oreil.ly/JjV17) 中的一个重点强调。

^[4](#ch04.html#ch01fn36-marker) 示例包括 [Google Cloud Anthos](https://oreil.ly/eeu0s) 和 [AWS Outposts](https://oreil.ly/uaHAu)。

