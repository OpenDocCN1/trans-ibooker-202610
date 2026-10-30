# Chapter 10. Applying Observability Practices in Your Team

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 10th chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at vwilson@oreilly.com.

Let’s switch gears to focus on the fundamentals of observability from a social and cultural practice perspective. In this chapter, we provide several tips to help you get started with observability practices. If you’re in a leadership role within your engineering team—such as a team lead, a manager, or maybe the resident observability fan/champion—the hardest thing to figure out (after getting management approval) in an observability implementation strategy is knowing where to start.

For us, this is a particularly tricky chapter to write. Having helped many teams start down this path, we know that no universal recipe for success exists. How and where you get started will always depend on many factors. As unsatisfying as “it depends” can be for an answer, the truth is that your journey with observability depends on particulars including the problems most pertinent to you and your team, the gaps in your existing tooling, the level of support and buy-in from the rest of your organization, the size of your team, and other such considerations.

Whatever approach works best for you is, by definition, not wrong. The advice in this chapter is not intended to suggest that this is the one true way to get started with observability (there is no singular path!). That said, we have seen a few emergent patterns and, if you are struggling with where to begin, some of these suggestions may be helpful to you. Feel free to pick and choose from any of the tips in this chapter.

## Join a Community Group

Observability is an emerging practice, and the approaches are still relatively young, with plenty of exploration left to do. Whenever the practices and technology behind our sociotechnical systems are rapidly evolving, one of the best ways to learn and improve is by participating in a community of people who are struggling with variations on the same themes as you. Community groups connect you with other professionals who can quickly become a helpful network of friends and acquaintances.

As you and your community face similar challenges, you’ll have an opportunity to learn so much very quickly just by hanging out in Slack groups and talking to other people who are banging against the same types of problems. Community groups allow you to connect with people beyond your normal circles from a variety of backgrounds. By actively participating and understanding how other teams handle some of the same challenges as you, you’ll make comparative observations and learn from the experiences of others.

Over time, you’ll also discover other community members with common similarities in tech stack, team size, organizational dynamics, and so forth. Those connections will give you someone to turn to as a sounding board, for background, or personal experiences with solutions or approaches you might also be considering. Having that type of shared context before you pull the trigger on new experiments can be invaluable. Actively



参与社区团体将为你节省大量时间和避免心碎。

参与社区还能让你及时发现可能错过的动态。不同的可观测性工具提供商会参与不同的社区，以更好地理解用户面临的挑战、收集对新想法的反馈，或只是大致了解正在发生的事情。参与你所选择的可观测性工具专属社区，也能让你实时了解正在发生的事情。

加入社区时，请记住社区关系是双向的。不要忘记做你该做的“砍柴挑水”：出现并先通过帮助他人开始贡献。成为好的社区公民意味着在提出重要求助之前，先参与并帮助群体一段时间。换句话说，不要只在需要别人帮助时才开口。社区的力量取决于你的贡献。

如果你需要一个起点，我们建议查看 CNCF 可观测性技术咨询小组（TAG）。在那里，你既可以找到 Slack 聊天群组，也有定期的在线会议。OpenTelemetry 社区页面也列出了有用的资源。更普遍地说，许多关于可观测性的对话都通过 Twitter 进行（搜索“observability”，你会找到值得关注的人和话题）。更具体地说，存在专注于产品的 Slack 群组（例如 Honeycomb 的 Pollinators Slack），你可以在那里找到通用信息和供应商特定信息的混合。我们还推荐 Michael Hausenblas 的新闻通讯 o11y news。

## 从最大的痛点开始

引入任何新技术都可能存在风险。因此，新技术计划往往以小型且不起眼的服务作为起点。与直觉相反，对于可观测性而言，从小处着手正是我们经常看到人们犯下的较大错误之一。

可观测性工具旨在帮助你快速找到难以捉摸的问题。从一个不起眼且相对不重要的服务开始，将产生与证明可观测性价值完全相反的效果。如果你从一个已经运行得相对较好的服务开始，你的团队将经历开始使用可观测性的所有工作，却得不到任何好处。

在带头推进一项新计划时，相对快速地取得成果非常重要。通过立即解决棘手或难以捉摸的问题来展示价值、激发好奇心并引起兴趣。是否有一个不稳定且时好时坏的服务已经连续几周把人唤醒，却没人能找到正确的修复方法？从那里开始。你是否一直遇到数据库持续拥塞的问题，却没人能查明原因？从那里开始。你是否正在运行一个服务，被尚无法识别的用户产生的莫名负载拖垮？那就是你最好的起点。

快速展示价值将赢得反对者的支持，创造更多支持，并进一步推动可观测性的采用。不要选择一个容易的问题作为开始。选择一个可观测性旨在攻克的难关。从那个服务开始。对代码进行插桩，部署到生产环境，带着强烈的好奇心去探索，弄清楚如何找到答案，然后分享这一成功。在每周团队会议上展示你的解决方案。写下你的发现和方法论，然后与公司分享。确保负责该服务的值班人员知道如何找到该解决方案。

推动采用的最快方式是解决负责管理生产服务的团队最大的痛点。瞄准这些痛点。抵制从小处着手的冲动。

## 购买而非构建

与从最大的痛点开始类似，决定是构建自己的可观测性工具还是购买商业现成解决方案，最终归结为尽快证明投资回报率（ROI）。我们将在第15章更详细地讨论这一论点。现在，我们先在最初就做出决策，倾向于用最少的努力证明最大的价值。首先构建完整解决方案是可能的最大努力，而且实现价值的时间也最长。

请准备好尝试多种可观测性解决方案，看看它们是否满足[第1章](#)中提出的功能要求。请记住，可观测性允许你理解和解释系统可能进入的任何状态，无论它多么新奇或怪异。你必须能够以即席的方式，跨系统状态数据的所有维度及其组合维度，对该怪异或新奇的状态进行比较性调试，而无需预先定义或预测这些调试需求。

不幸的是，在撰写本文时，能够满足这些要求的工具寥寥无几。虽然各家厂商的市场部门热衷于将可观测性标签贴在其产品套件中的工具上，但很少有工具能够实现本书所概述的工作流程和收益。因此，作为用户，你应该准备好尝试许多工具，以分辨哪些真正有效，哪些只是将多年来销售的相同传统监控工具重新包装。

做到这一点的最佳方式是使用 OpenTelemetry 对应用程序进行插桩（参见[第7章](#)）。使用 OTel 可能不像其他厂商的专有代理或库那样快速和简单，但也不应该缓慢或难以使用。从一开始就正确完成这一过程所需的前期时间投入，将在你之后决定尝试多种解决方案以确定哪种最符合需求时，带来极大的回报。

这些错误信息和不确定性是当今供应商生态系统的一个不幸现实。因此，人们很容易倾向于通过构建自己的解决方案来绕过整个混乱局面。然而，这种选择也充斥着一些不幸的现实（参见[第15章](#)）。

首先，即使你想自己运行，也很少有现成的开源可观测性工具可供使用。你会遇到的三个主要候选者是 Prometheus、ELK 技术栈（Elasticsearch、Logstash 和 Kibana）以及 Jaeger。虽然每个都提供了特定且有价值的解决方案，但没有一个能提供完整的解决方案，能够实现本书所述的可观测性功能要求。

*Prometheus* 是用于指标监控的 TSDB。虽然 Prometheus 可以说是一个拥有活跃开发社区的最先进的指标监控系统之一，但它仍然仅在基于指标的监控解决方案领域内运作。它带有使用粗略度量来发现更细粒度问题的固有限制（参见[第2章](#)）。

*ELK 技术栈* 专注于提供日志存储和查询解决方案。日志存储后端针对纯文本搜索进行了优化，牺牲了其他类型的搜索和聚合。虽然在搜索已知错误时很有用，但在搜索诸如“谁看到了这个问题，他们何时看到的？”这类复合问题的答案时，纯文本搜索就变得不切实际了。在完全基于日志的解决方案中，分析和识别对可观测性至关重要的海量纯文本日志中的相关模式是很有挑战性的（参见[第8章](#)）。

*Jaeger* 是一个基于事件的分布式追踪工具。Jaeger 可以说是当今可用的最先进的开源分布式追踪工具之一。正如在[第6章](#)中所讨论的，追踪事件是可观测系统的基本构建块。然而，一个必要的组成部分还在于分析能力，以确定在调查过程中哪些追踪事件值得关注。Jaeger 对过滤某些数据有一些支持，但缺乏分析和细分所有追踪数据的复杂能力（参见[第8章](#)）。

这些工具各自提供了系统视图的不同部分，可用于实现可观测性。今天使用它们的挑战在于，要么运行分散的系统，将上下文在它们之间传递的负担放在操作者的脑海中，要么构建自己的定制解决方案来将这些独立组件粘合在一起。如今，不存在一个开源工具能够在一个开箱即用的解决方案中提供可观测性能力。希望这种情况不会永远如此。但这就是今天的现实。




最后，无论你决定朝哪个方向走，确保最终结果确实能为你提供可观测性。对解决方案进行压力测试。再次强调，要抵制从小处着手的冲动。去解决大的、困难的问题。可观测性要求能够跨所有高基数字段，具有高维度，以交互式和高性能的实时探索方式调试系统状态。这个解决方案能否提供前面章节中描述的那种分析和迭代调查？如果答案是肯定的，恭喜你！你找到了一个很棒的可观测性解决方案。如果答案是否定的，那么你正在使用的产品可能被误导性地标记为可观测性解决方案，而实际上并不是。

开始使用可观测性的关键是快速行动，并在过程早期展示价值。选择购买解决方案将使你的团队专注于使用可观测性工具解决问题，而不是自己构建工具。如果你用OTel为应用程序添加插桩，就可以避免供应商锁定的陷阱，并将遥测数据发送给你最终决定使用的任何工具。

## 逐步充实你的插桩

正确地为应用程序添加插桩需要时间。像OTel这样的项目中包含的自动插桩是一个很好的起点。但最有价值的插桩将特定于你的应用程序的需求。一开始要尽可能多地添加有用的插桩，但要做好后续开发的计划。

> **注意**
>
> 关于自动插桩与手动插桩的更多示例，我们推荐Mike Goldsmith的博客[“什么是自动插桩？”](https://www.mikesdotblog.com/what-is-auto-instrumentation/)

在整个组织推广可观测性的最佳策略之一是，在刚开始时先为一个或两个令人痛苦的服务添加插桩。将该插桩实践作为试点团队其余成员的参考点，并从中学习。一旦试点团队熟悉了新工具，就可以利用任何新的调试情况来引入更多有用的插桩。

每当值班工程师收到生产环境问题的分页通知时，他们应该做的第一件事就是使用新工具为应用程序的问题区域添加插桩。利用新的插桩来确定问题发生的位置。当人们采用这种方法第二或第三次后，通常会意识到通过先引入插桩来调试问题要容易得多，也省时得多。从插桩入手进行调试可以让你看到真正发生了什么。

一旦试点团队成员掌握了方法，他们就可以帮助其他人学习。他们可以提供关于如何创建有用插桩的指导、建议有用的查询，或向其他人指出更多有用的故障排查模式示例。每一次新的调试问题都可以用来构建你需要的插桩。你不需要拥有一个完整开发的插桩集才能从可观测性中获得即时价值。

## 关于插桩约定的说明

当你开始进行插桩时，重点应该放在用你选择的可观测性工具尽可能多地证明价值上。你迭代地充实插桩这一事实比你怎么做更重要。

然而，请记住，随着你的插桩增长以及在团队中的普及，你应该为生成的自定义遥测数据引入命名约定。关于组织范围插桩标准的示例，请参见第14章。

## 寻找机会利用现有工作

采用任何新技术最大的障碍之一是*沉没成本谬误*。个人和组织在继续某种行为或努力时，会因为之前投入的时间、金钱或精力而陷入沉没成本谬误。¹ 你的组织已经在传统方法上投入了多少时间、金钱和精力，而这些方法已不再满足你的需求？

当觉得资源被浪费的感觉悄然来袭时，对根本性变革的抵制往往会遇到阻碍。那些年投入在理解和插桩旧解决方案上的时间呢？虽然沉没成本谬误并不合乎逻辑，但其背后的感受是真实的，如果你不采取任何措施，它们会扼杀你的努力。

要时刻留意并抓住任何机会，将其他工作“叉车式”地纳入你的可观测性计划。例如，如果你有一个现有的数据流，可以将其分流到次要目标，或者有可以用另一种方式查看的关键数据，要抓住机会将这些数据送入你的可观测性解决方案。你可以利用的情况示例如下：

- 如果你正在使用ELK技术栈——或者仅仅是其中的Logstash部分——添加一小段代码将源流的输出分叉到次要目标是很简单的。将该流发送到你的可观测性工具。邀请用户比较体验。
- 如果你已经在使用结构化日志，你所需要做的就是在日志事件在整个技术栈中传播时为其添加唯一ID。你可以将这些日志保留在现有的日志分析工具中，同时将它们作为跟踪事件发送到你的可观测性工具。
- 尝试在现有APM解决方案旁边运行客户端可观测性插桩（例如，Honeycomb的Beelines或OTel）。邀请用户对比和对照体验。
- 如果你使用Ganglia，你可以通过解析它放入`/var/tmp`的可扩展标记语言（XML）转储来利用该数据，使用每分钟运行一次的cron任务将数据作为事件送入你的可观测性工具。这虽然不是可观测性的最优用法，但肯定能让Ganglia用户感到熟悉。
- 将旧监控仪表板中最有用的部分，在新可观测性工具中重新创建为易于引用的查询。虽然仪表板肯定有缺点（参见[第2章](#)），但这为新用户提供了一个落脚点，让他们能一眼了解关心的系统性能，并且给了他们探索和了解更多内容的机会。

任何能融合两个世界的方法都将有助于降低采用障碍。其他人需要理解他们关心的内容如何映射到新解决方案中。帮助他们在你为他们创造的新世界中看到他们当前的世界。这种初探并不完美也没关系。即使体验相当粗糙，你的目标是熟悉感。在这种方法中使用新工具可能会很糟糕，但如果事物的名称是熟悉的，数据是他们所知道的，它仍然会比一个完全从零开始的数据集更能吸引人们与之互动。

## 为最艰难的最后一搏做准备

使用上述策略先解决最大的痛点并采用迭代方法，可以帮助你在开始阶段快速取得进展。但这些策略并没有考虑到实施可观测性解决方案最难的部分之一：冲刺到终点线。既然你已经有了一些势头，你还需要一个策略来完成剩余的工作。

根据工作范围和团队规模，将新的插桩迭代式地纳入你的值班流程，通常可以使大多数团队完成他们打算将可观测性引入技术栈的每个部分所需工作的大约一半到三分之二。不可避免的是，大多数团队会发现他们技术栈的某些部分比其他部分缺乏活跃的开发。对于那些很少触及的技术栈部分，你需要一个扎实的完成计划，否则你的实施努力可能会滞后。

即使有最好的项目管理意图，随着推动可观测性采用的一些痛苦开始缓解，完成实施工作的紧迫性也可能随之减弱。大多数团队所处的现实是，工程周期稀缺，需求总是在竞争，在解决眼前问题后，另一个痛点总在拐角处等待。

完整实施的目标是构建一个可靠的、优先选择的调试解决方案，当任何问题发生时，可以用它来全面了解生产应用程序的状态。在达到那种最终状态之前，你很可能会有各种最适合解决不同问题的工具。在实施阶段，这种脱节是可以容忍的，因为你正在为一个更具凝聚力的未来而努力。但从长远来看，不完成实施可能会消耗团队的精力、认知能力和注意力。

这时你需要制定一个时间表，快速推进剩余工作。你的目标里程碑应该是完成必要的剩余插桩工作，以便你的团队可以使用你的可观测性工具作为生产环境调试的首选方案。考虑设立一个特别的冲刺来达到终点线，比如一个以派对和奖品结束的黑客周（或类似有趣而愚蠢的活动），将项目推向终点，完成它。

在这个阶段，值得注意的是，你的团队应尽可能多地尝试将插桩通用化，以便它可以在其他应用程序或组织内的其他团队中重用。这里一个常见的策略是避免重复初始实施工作，创建通用的可观测性库，允许你替换底层解决方案而不涉及代码内部，类似于OTel所采用的方法（参见[第7章](#)）。

## 结论





准确了解从何处以及如何开始你的可观测性之旅取决于团队的具体情况。希望这些通用建议能帮助你找到入手点。积极参与同行社区对你来说可能是最有价值的切入点。在起步阶段，专注于解决最大的痛点，而不是从那些已经运行得足够顺畅的地方开始。在整个实施过程中，记住要保持快速推进、展示高价值和投资回报率，并以迭代方式开展工作的倾向。寻找机会让组织尽可能多的部门参与进来。并且不要忘记计划在一次大的最后冲刺中完成工作，以使你的实施项目顺利完成。

本章中的提示可以帮助你完成开始使用可观测性所需的工作。一旦这项工作完成，日常使用可观测性将默认解锁其他新的工作方式。本书的这一部分其余内容将详细探讨这些内容。在下一章中，我们将研究可观测性驱动开发如何彻底改变你对新代码在生产环境中行为的理解。

> 1 Hal R. Arkes and Catherine Blumer, “The Psychology of Sunk Costs,” *Organizational Behavior and Human Decision Processes* 35 (1985): 124–140.
