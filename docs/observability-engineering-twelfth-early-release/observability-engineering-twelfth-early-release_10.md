# 第9章 可观测性与监控如何协同

### 致早期版本读者的一则说明

通过早期发布电子书，你可以获得书籍最初的形态——作者写作时的原始未编辑内容——从而在这些技术正式发布之前就能加以利用。

这将是最终成书的第9章。

如果您对如何改进本书的内容和/或示例有任何意见，或者发现本章存在缺失材料，请通过 vwilson@oreilly.com 联系编辑。

到目前为止，本书已经考察了可观测系统的差异化能力、实现可观测性所需的技术组件，以及可观测性如何融入技术版图。可观测性与监控在本质上是不同的，两者服务于不同的目的。在本章中，我们将研究它们如何相互配合，以及如何确定两者在组织内共存时的考量因素。

许多组织围绕其生产软件系统积累的指标数据和监控专业知识，已有数年甚至数十年的历史。如前面章节所述，传统监控方法足以应对传统系统。但在管理现代系统时，这是否意味着你应该抛弃所有既有资产，从零开始使用可观测性工具？这样做既草率又鲁莽。事实上，对大多数组织来说，应当如何处理这两种方法的共存，取决于他们所承担的职责。

本章通过考察可观测性与监控各自的优势、最适合的领域及互补方式，来探讨它们如何协同。每个组织各有不同，因此无法为可观测性与监控的共存开出一个放之四海而皆准的方子。不过，一条有用的指导原则是：可观测性最适合理解应用层的问题，而监控则最适合理解系统层的问题。通过审视你的工作负载，你可以找出两者最适合你的结合方式。

## 监控的用武之地

在第2章中，我们重点区分了可观测性与监控。那一章主要讨论了监控系统的不足以及可观测性如何弥合这些不足。但监控系统仍然能提供有价值的洞察。我们先来看看传统监控系统在哪些地方依然是对的工具。

传统监控理解系统状态的方式是一个成熟且完善的过程。数十年迭代改进，使监控工具从最初简单的指标和循环数据库（RRDs）的简陋起步，演进到TSDBs和精细的标签系统。提供这类服务的成熟选项也极其丰富——从开源软件解决方案、初创公司，到上市公司，不一而足。

监控实践广为人知，其理解范围超越了围绕特定工具形成的专家社区。在整个软件行业中，存在一些监控最佳实践，任何在生产环境中操作过软件的人大概都会认同。

例如，监控领域有一条被广泛接受的核心原则：人不必整天坐在那里盯着图表；当出现问题时，系统应主动告知其用户。如此看来，监控系统是反应式的。它们通过向人员发出问题正在发生的警报，来对已知故障状态做出反应。

监控系统和指标为此不断优化自身。它们自动报告已知故障条件是否正在发生或即将发生。它们针对在已知故障模式下报告未知情况进行优化（换言之，它们的设计目的是检测*已知的未知*）。

监控系统为查找已知的未知而进行优化，意味着它最适合理解你的系统状态——系统的变化频率远低于应用代码，且变化方式更容易预测。我们所说的*系统*，是指你的基础设施、运行时，或是能帮助你察觉何时即将触及运行约束的计数器。

如第1章所述，指标和监控是为检查硬件级性能而创造的。随着时间推移，它们被改造成涵盖更广泛的基础设施和系统级问题。本书的大多数读者在科技公司工作，应该认识到，底层系统并不是你业务的重点。归根结底，对你的业务而言，重要的是你所编写的应用在客户手中表现如何。你的业务关心这些底层系统的唯一原因是，它们可能对应用性能产生负面影响。

例如，你可能想知道一个存在噪声邻居的虚拟实例上的CPU是否被占满，因为这会告诉你，你看到的延迟问题并非出自代码内部。或者，如果你发现整个集群的物理内存即将耗尽，那说明一场迫在眉睫的灾难很可能源自你的代码。将系统约束与应用性能进行关联很重要，但系统性能主要作为警告信号或排除代码问题的依据。

随着时间推移，指标也被逐渐用于监控应用层面的问题。但正如你在第一部分中看到的，这些聚合度量过于粗糙，因为它们无法拆解到能显示服务中单个请求性能的程度。作为警告信号，像指标这样的聚合度量效果很好。但指标并不是、也从来不是判断你编写的代码在单个用户手中表现如何的好方法。

## 可观测性的用武之地

与监控相比，可观测性有着不同的原则和用例。如第2章所述，可观测性更具主动性。它的实践要求工程师在代码部署时始终关注自己的代码，并且每天花时间在生产环境中探索代码、观察用户如何使用代码，并留意异常点和值得追踪的可疑轨迹。

如第8章所述，核心分析循环支持从第一性原理进行调试，专为发现此前未知的故障模式而设计（换言之，它旨在检测*未知的未知*）。可观测性为发现未知的未知而进行的优化，意味着它最适合理解你编写的代码的状态——代码的变化频率远高于系统（通常每天都会变），且变化方式更难预测。

监控工具和可观测性工具有着不同的最佳实践和不同的实现方式，它们服务于不同的目的。

## 系统与软件的考量

在更传统的情境中，系统与软件的区分很清晰：裸机基础设施是系统，而在该系统内运行的一切都是软件。现代系统及其众多高阶抽象让这种区分变得有些模糊。我们先给出一些定义。

在此语境下，*软件*指你正在积极开发的、作为面向客户交付价值的生产服务运行的代码。软件是你的业务为了解决市场问题而*想要*运行的东西。

*系统*是一个统称，涵盖运行该服务所必需的基础设施和运行时等其他一切。系统是你的业务为了支撑其*想要*运行的软件而*需要*运行的东西。按此定义，系统（或基础设施，此处这两个术语可互换使用）包括从数据库（例如MySQL或MongoDB）到计算与存储（例如容器或虚拟机），再到你部署和运行软件之前必须预置和设置的任何其他一切。

云计算世界让这些定义变得有些难以确定，因此我们进一步深入探讨。假设要运行你的软件，你需要运行像Apache Kafka、Postfix、HAProxy、Memcached，甚至Jira之类的底层组件。如果你以服务形式购买这些组件的访问权，那么按照该定义，它们并不算是基础设施；你本质上是在付费请别人替你运行它们。然而，如果你的团队负责安装、配置、定期升级并解决这些组件的性能问题，那么它们就是你需要操心的基础设施。

与软件相比，系统层的一切都是一种商品：变化不频繁、面向不同的用户群体，并提供不同的价值。软件——你为客户使用而编写的代码——是业务的核心差异化因素：它正是贵公司今天存在的原因。因此，软件在应如何管理方面有一系列不同的考量。表9-1提供了一个对比。




表 9-1. 系统间变化的因素

| 因素 | 你的系统 | 你的软件 |
|---|---|---|
| 变化速率 | 包更新（每月） | 仓库提交（每日） |
| 可预测性 | 高（稳定） | 低（新功能众多） |
| 对业务的价值 | 低（成本中心） | 高（收入来源） |
| 用户数量 | 少数（内部团队） | 众多（你的客户） |
| 核心关注点 | 系统或服务是否健康？ | 每个请求能否及时可靠地获取其端到端执行所需的资源？ |
| 评估视角 | 系统 | 你的客户 |
| 评估标准 | 低级内核和硬件设备驱动 | 变量和 API 端点 |
| 功能职责 | 基础设施运维 | 软件开发 |
| 理解方法 | 监控 | 可观测性 |

正如系统和软件是互补的，用于理解各自行为的方法也是互补的。监控最有助于工程师理解系统级关注点。可观测性最有助于工程师理解应用级关注点。评估你自己的组织需求意味着要理解哪些关注点对你的业务最关键。

可观测性将帮助你深入理解你开发和发布的软件在为客户提供服务时的表现。经过良好仪器化以实现可观测性的代码使你能够回答关于用户性能的复杂问题，看到软件在生产环境中的内部运作，并识别和迅速修复那些仅检查聚合性能时容易被忽略的问题。

如果你的公司将编写和发布软件作为其核心业务战略的一部分，你就需要一个可观测性解决方案。如果除了提供可接受的整体聚合性能之外，你的业务战略还依赖于为特定的一批高知名度客户提供优质服务，那么你对可观测性的需求就更加突出。

监控将帮助你了解你在支持该软件运行的系统的工作表现如何。基于指标的监控工具及其关联的告警可帮助你看到底层系统的容量限制或已知错误条件何时被达到。

如果你的公司将向客户提供基础设施作为其核心业务战略的一部分（例如，基础设施即服务，即 IaaS 提供商），你将需要大量的监控——低级域名系统（DNS）计数器、磁盘统计等。对于这些组织而言，底层系统是业务关键的，它们需要成为向客户提供的这些低级系统的专家。然而，如果提供基础设施不是你业务的核心差异化因素，监控的重要性就会降低。你可能只需要监控大部分的高级服务和端到端检查即可。确定你的业务需要多少更少的监控需要考虑多个因素。

运行大量自有基础设施的公司需要更多的监控。无论是在本地运行系统还是使用云提供商，这一考虑更多地与基础设施所在位置无关，而与运营责任有关。无论你是在云中配置虚拟机，还是在本地管理自己的数据库，关键因素在于你的团队是否承担了确保基础设施可用性和性能的负担。

承担运行自有裸机系统责任的组织需要监控来检查低级硬件性能。他们需要监控来检查以太网端口的计数器、硬盘性能的统计数据以及系统固件的版本。那些将硬件级操作外包给 IaaS 提供商的组织将不需要在该级别执行指标和聚合。

就这样，越往上层堆栈发展。随着更多运营责任转移给第三方，基础设施监控关注点也随之转移。

将大部分基础设施外包给更高级别的平台即服务（PaaS）提供商的公司可能几乎不需要任何传统监控解决方案。Heroku、AWS Lambda 等基本上让你付费让他们完成确保你的业务所需基础设施的可用性和性能的工作，以便你转而专注于你想要运行的软件。

今天，你的体验可能因云提供商的健壮性而有所不同。 presumably，抽象层足够干净且足够高层，以至于摆脱对基础设施监控的依赖不会令人极度沮丧。但从理论上讲，所有提供商都在向一个能够实现这种转变的模式发展。

## 例外：不可忽视的基础设施监控

监控系统与可观测性软件之间这种整洁的划分线有几个例外。如前所述，确定你的软件性能如何的评估视角是客户体验。如果你的软件运行缓慢，你的客户就会体验不佳。因此，评估客户体验的主要关注点是理解可能导致性能瓶颈的任何因素。对这种整洁划分线的例外是任何直接指示你的软件如何与其底层基础设施交互的指标。

从软件角度来看，看到每个常见监控工具从 /proc 文件系统发现的数千个变量的图表几乎没有任何价值。关于电源管理和内核驱动程序的指标可能有助于理解低级基础设施细节，但会被软件开发人员常规且欣然地忽略（正如它们应该的那样），因为它们几乎不提供关于软件性能影响的有用信息。

然而，像 CPU 使用率、内存消耗和磁盘活动这样的高阶基础设施指标是物理性能限制的指示。作为软件工程师，你应该密切关注这些指标，因为它们可能是由你的代码触发的问题的早期预警信号。例如，你想知道你刚刚推送的部署是否导致驻留内存使用量在几分钟内增加三倍。能够看到新功能引入后 CPU 消耗突然翻倍或磁盘写入活动激增等突发变化，可以迅速提醒你存在有问题的代码更改。

高阶基础设施指标是否可用，取决于你的底层基础设施抽象程度如何。但如果它们可用，你肯定希望将其作为可观测性方法的一部分进行捕获。

这里监控与可观测性之间的联系是相关性。当性能问题发生时，你可以使用监控来快速排除或确认系统级问题。因此，将系统级指标数据与你的应用级可观测性数据并排查看是有用的。一些可观测性工具（例如 Honeycomb）以共享上下文呈现这些数据，尽管其他工具可能需要你使用不同的工具或视图来进行这些关联。

## 实际案例

虽然可观测性仍是一个新兴领域，但监控与可观测性共存的几种模式正在出现。本节中引用的示例代表了我们在客户中或更广泛的可观测性社区中常见看到的模式，但绝非详尽或权威。这些方法旨在说明本章所述概念在现实世界中的应用。

我们的第一个示例客户拥有丰富的工具生态系统来理解其生产系统的行为。在转向可观测性之前，团队使用 Prometheus 进行传统监控、Jaeger 进行分布式追踪以及传统的 APM 工具的组合。他们希望通过简化现有的多工具方法来改善事件响应时间，该方法需要在三个不同的系统中捕获的数据之间进行关联。

转向基于可观测性的方法意味着他们能够整合需求并将足迹减少到共存的监控系统和可观测性系统。该组织中的软件工程团队主要使用可观测性来理解和调试生产环境中的软件。中央运营团队仍使用 Prometheus 来监控基础设施。然而，软件工程师报告说，当他们对代码的资源使用影响有疑问时，仍可以参考 Prometheus。他们还报告说这种需求很少见，并且他们很少需要使用 Prometheus 来排查应用瓶颈。

我们的第二个示例客户是一家相对较新的公司，能够构建一个从零开始的应用栈。他们的生产服务主要利用无服务器函数和 SaaS 平台来驱动其应用，并且几乎没有运行自己的基础设施。最初就没有真正的基础设施，因此他们从未开始



The path of trying to make monitoring solutions work for their environment. They rely on application instrumentation and observability to understand and debug their software in production. They also export some of that data for longer-term aggregation and warehousing.

Lastly, our third example customer is a mature financial services company undergoing a digital transformation initiative. They have a large heterogeneous mix of legacy infrastructure and applications as well as greenfield applications that are managed across a variety of business units and engineering teams. Many of the older applications are still operating, but the teams that originally built and maintained them have long since disbanded or been reorganized into other parts of the company. Many applications are managed with a mix of metrics-based monitoring paired with dashboarding capabilities (provided by an all-in-one commercial vendor), along with various logging tools to search their unstructured logs.

The business would not realize much, if any, value from ripping out, rearchitecting, and replacing monitoring approaches that work well for stable and working services. Instead, greenfield applications are being developed for observability instead of using the former approach requiring a mix of monitoring, dashboards, and logging. When new applications use company infrastructure, software engineering teams also have access to infrastructure metrics to monitor resource usage impacts. However, some software engineering teams are starting to capture infrastructure metrics in their events in order to reduce their need to use a different system to correlate resource usage with application issues.

## Conclusion

The guiding principle for determining how observability and monitoring coexist within your organization should be dictated by the software and infrastructure responsibilities adopted within its walls. Monitoring is best suited to evaluating the health of your systems. Observability is best suited to evaluating the health of your software. Exactly how much of each solution will be necessary in any given organization depends on how much management of that underlying infrastructure has been outsourced to third-party (aka cloud) providers.

The most notable exceptions to that neat dividing line are higher-order infrastructure metrics on physical devices that directly impact software performance, like CPU, memory, and disk. Metrics that indicate consumption of these physical infrastructure constraints are critical for understanding the boundaries imposed by underlying infrastructure. If these metrics are available from your cloud infrastructure provider, they should be included as part of your approach to observability.

By illustrating a few common approaches to balancing monitoring and observability in complementary ways, you can see how the considerations outlined throughout this chapter are implemented in the real world by different teams. Now that we’ve covered the fundamentals of observability in depth, the next part of this book goes beyond technology considerations to also explore the cultural changes necessary for successfully adopting observability practices and driving that adoption across teams.
