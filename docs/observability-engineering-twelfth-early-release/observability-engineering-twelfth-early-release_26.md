# Chapter 22. Where to Go from Here

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 22nd chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at [vwilson@oreilly.com](mailto:vwilson@oreilly.com)。

In this book, we’ve looked at observability for software systems from many angles. We’ve covered what observability is and how that concept operates when adapted for software systems—from its functional requirements, to functional outcomes, to sociotechnical practices that must change to support its adoption.

To review, this is how we defined *observability* at the start of this book:

> *Observability for software systems is a measure of how well you can understand and explain any state your system can get into, no matter how novel or bizarre. You must be able to comparatively debug that bizarre or novel state across all dimensions of system state data, and combinations of dimensions, in an ad hoc iterative investigation, without being required to define or predict those debugging needs in advance. If you can understand any bizarre or novel state without needing to ship new code, you have observability.*

Now that we’ve covered the many concepts and practices intertwined with observability in this book, we can tighten that definition a bit:

> *If you can understand any state of your software system, no matter how novel or bizarre, by arbitrarily slicing and dicing high-cardinality and high-dimensionality telemetry data into any view you need, and use the core analysis loop to comparatively debug and quickly isolate the correct source of issues, without being required to define or predict those debugging needs in advance, then you have observability.*

### Observability, Then Versus Now

We started writing this book almost three years ago. You might ask why on earth it has taken so long to get here.

First off, the state of observability has been a moving target. When we started writing this book, conversing with anyone about the topic required us to stop and first define “observability.” Nobody really understood what we meant whenever we talked about the cardinality of data or its dimensionality. We would frequently and passionately need to argue that the so-called *three pillars* view of observability was only about the data types, and that it completely ignores the analysis and practices needed to gain new insights.

As Cindy Sridharan states in the Foreword, the rise in prominence of the term “observability” has also led (inevitably and unfortunately) to it being used interchangeably with an adjacent concept: monitoring. We would frequently need to explain that “observability” is not a synonym for “monitoring,” or “telemetry,” or even “visibility.”

Back then, OpenTelemetry was in its infancy, and that was yet another thing to explain: how it was different from (or inherited from) OpenTracing and OpenCensus? Why would you use a new open standard that required a bit more setup work instead of your vendor’s more mature agent that worked right away? Why should anyone care?

Now, many people we speak to don’t need those explanations. There’s more agreement on how observability is different from monitoring. More people understand the basic concepts and that data misses the point without analysis. They also understand the benefits and the so-called promised land of observability, because they hear about the results from many of their peers. What many of the people we speak to today are looking for is more sophisticated analyses and low-level, specific guidance on how to get from where they are today to a place where they’re successfully practicing observability.

Second, this book initially started with a much shorter list of chapters. It had more basic material and a smaller scope. As we started to better understand which concerns were common and which had successful emergent patterns, we added more depth and detail. As we encountered more and more organizations using observability at massive scale, we were able to learn comparatively and incorporate those lessons by inviting direct participation in this book (we’re looking at you, Slack!).

Third, this book has been a collaborative effort with several reviewers, including those who work for our competitors. We’ve revised our takes, incorporated broader viewpoints, and revisited concepts throughout the authoring process to ensure that we’re reflecting an inclusive state of the art in the world of observability. Although we (the authors of this book) all work for Honeycomb, our goal has always been to write an objective and inclusive book detailing how observability works in practice, regardless of specific tool choices. We thank our reviewers for keeping us honest and helping us develop a stronger narrative.

Based on your feedback, we added more content around the sociotechnical challenges in adopting observability. Like any technological shift that also requires changing associated practices, you can’t just buy a tool and achieve observability. Adopting observability practices means changing the way you think about understanding your software’s behavior and, in turn, changing the relationship you have with your customers. Observability lets you empathize and align with your customers by letting you understand exactly how the changes you make impact their experience, day after day.

Illustrating the way that plays out in practice across multiple teams within an organization and sifting out useful advice for beginners has taken time as repeatable patterns have emerged (and as they continue to evolve).

So, where should you go from here? First, we’ll recommend additional resources to fill in essential topics that are outside the scope of this book. Then, we’ll make some predictions about what to expect in the world of observability.

### Additional Resources

The following are some resources we recommend:

- *Site Reliability Engineering* by Betsy Beyer et al. (O’Reilly)

  We’ve referenced this book a few times within our own. Also known as “the Google SRE book,” this book details how Google implemented DevOps practices within its SRE teams. This book details several concepts and practices that are adjacent to using observability practices when managing production systems. It focuses on practices that make production software systems more scalable, reliable, and efficient. The book introduces SRE practices and details how they are different from conventional industry approaches. It explores both the theory and practice of building and operating large-scale distributed systems. It also covers management practices that can help guide your own SRE adoption initiatives. Many of the techniques described in this book are most valuable when managing distributed systems. If you haven’t started down the path of using SRE principles within your own organization, this book will help you establish practices that will be complemented by the information you’ve learned in our book.




### *实现服务级别目标* 作者：Alex Hidalgo（O’Reilly）

这本书深入探索了 SLO，而我们的书只是简要提及（见第 12 章和第 13 章）。Hidalgo 是一位网站可靠性工程师，是 SLO 相关事务的专家，也是 Honeycomb 的朋友。他在书中概述了更多与 SLO 领域相关的概念、哲学和定义，为你介绍进一步行动所需的基础知识。他用数学和统计模型非常详细地阐述了 SLO 的实现，这有助于进一步理解为什么可观测性数据如此独特地适合 SLO（第 13 章的基础）。他的书还涵盖了因采用 SLO 而必须转变的文化实践，并进一步阐释了我们书中引入的一些概念。

### *使用 OpenTelemetry 实现云原生可观测性* 作者：Alex Boten（Packt Publishing）

在撰写本书时，这本书仍在编写中，它比我们在本书中介绍的更深入、更详细地探讨了 OTel。Boten 的书详细介绍了 OTel 的核心组件（API、库、工具）及其基本概念和信号类型。如果你有兴趣使用管道来管理遥测数据，这本书会向你展示如何使用 OpenTelemetry Collector 来完成。虽然我们简单提及了 OpenTelemetry Collector，但这本书对其进行了更详细的介绍。如果你想更深入地了解 OTel 的核心概念，发现更多可能性，我们建议你留意这本书。

### *分布式追踪实践* 作者：Austin Parker 等人（O’Reilly）

这本书提供了关于如何进行应用插桩以进行追踪、收集插桩产生的数据并从中挖掘运维洞察的深入指南。虽然专注于追踪，但这本书涵盖了插桩最佳实践，以及如何选择能够产生有价值追踪数据的 span 特征。它由我们在 Lightstep 的朋友撰写，提供了关于分布式追踪未来发展方向的其他观点，既有信息量又实用。

### Honeycomb 的博客

在这里，你可以找到我们关于新兴可观测性实践不断变化的最新信息。这个博客有时专门介绍 Honeycomb 自己的可观测性工具。但更多时候，它探讨一般的可观测性概念、建议（参见我们的可观测性建议专栏“Ask Miss o11y”），以及 Honeycomb 工程团队的文章，这些文章通常展示可观测性如何塑造我们自身不断发展的实践。

此外，全书中的脚注和注释引用了许多有趣的相关信息来源和作者，这些来源和作者是我们所尊重并在形成自己的观点和意见时会参考的。

## 对可观测性未来走向的预测

将预测写进出版物是大胆之举，而回头检验这些预测是否经得起时间考验则更为大胆。但鉴于我们处于可观测性生态系统的中心位置，我们觉得自己有足够的依据对未来几年这个行业的发展方向做出一些有见识的预测。这些预测是在 2022 年 3 月提出的。

三年后，我们认为 OTel 和可观测性将成功交织在一起，甚至可能显得密不可分。我们已经看到，对开发和采用 OTel 感兴趣的人群与对进一步开发符合本书所概述的可观测性定义的工具类别感兴趣的人群之间存在大量重叠。OTel 作为应用插桩事实标准解决方案的势头和崛起，在很大程度上得益于其对追踪数据支持是最成熟格式这一事实。指标和日志数据仍处于早期开发阶段，但我们预计这些数据很快会达到同样的成熟度，为在各种环境中更广泛采用打开大门。我们还认为，仅需少量配置更改即可轻松切换不同后端解决方案的能力将比现在更加容易（现在已经相当简单了）。¹ 我们预测，更多详细介绍如何从一个供应商切换到另一个供应商的操作指南将会大量涌现，并成为热门内容。

本书大部分内容聚焦于调试后端应用程序和基础设施的示例。然而，我们预测可观测性也将渗透到更多前端应用中。目前理解和测试浏览器应用的前沿技术涉及真实用户监控（RUM）或合成监控。

顾名思义，RUM 涉及从浏览器端测量和记录真实应用用户的体验。RUM 的重点是确定实际交付给用户的服务质量水平，检测应用错误或性能下降，并确定代码变更是否对用户体验产生了预期效果。RUM 的工作原理是在不影响代码性能的情况下收集和记录网络流量。在大多数情况下，JavaScript 片段被注入页面或应用内的原生代码中，以从浏览器或客户端提供反馈。为了使大量数据易于管理，RUM 工具通常使用采样或聚合进行整合。这种整合通常意味着你可以了解整体性能，但无法分解到详细了解任何一个特定用户性能水平的程度。

然而，尽管存在这些局限性，RUM 确实具有测量真实用户体验的优势。这意味着 RUM 工具可以捕获应用行为中各种意想不到的真实世界问题。RUM 可以帮助你发现各种问题，从只在你的开发团队从未听说过的冷门移动浏览器新版本中出现的回归，到地球另一端某个国家或地区特定 IP 地址的网络延迟。RUM 有助于识别和排查“最后一公里”问题。RUM 与合成监控的不同之处在于，它依赖真实用户点击页面来进行测量。

合成监控则是一种不同的方法，它依靠自动化测试按照一组给定的测试步骤进行测量。这些工具在受控环境中进行详细的应用程序性能和体验测量。创建行为脚本（或路径）来模拟客户可能在使用你的应用时采取的操作。然后按指定的时间间隔持续监控这些路径的性能。这有点像代码中的单元测试，这些路径通常由开发人员自己拥有和运行。这些路径——也就是用户使用前端应用时的典型行为模拟——需要开发和维护，这需要时间和精力。通常，只有使用频繁的路径或业务关键流程才会被监控性能。由于合成测试必须预先编写脚本，要对用户可能采取的导航路径的每一种排列组合进行性能测量是不可行的。

然而，虽然合成监控工具不会向你展示真实用户体验的性能，但它们确实有一些优点。它们允许针对你可能关心的各种已知条件（例如，特定设备类型或浏览器版本）进行主动测试。由于它们通常会产生一定程度可重复的结果，因此可以纳入自动化回归测试套件。这样就可以在代码部署给真实用户之前运行它们，从而在性能问题可能对真实用户产生影响之前将其捕获。

RUM 和合成监控服务于具体而不同的用例。可观测性的用例与 RUM 类似，是测量生产环境中的真实用户体验，但具有更高的保真度，使你能够调试单个客户的体验。正如[第 14 章](#)中所述，许多团队在其 CI/CD 构建流水线（或测试套件）中使用可观测性数据。这意味着你可以运行端到端测试脚本，在系统中演练用户路径，并通过简单地在遥测数据中将发起的测试请求标记为测试请求来监控其性能。我们预测，几年之内，你将不必在前端应用中选择 RUM 或合成监控，而只需使用可观测性来满足这两种用例。

我们还预测，在三年内，OTel 的自动插桩将迎头赶上，并与各种供应商特定库和代理中提供的非 OTel 自动插桩包相媲美。如今，对大多数团队来说，使用 OTel 仍然是一个选择，因为（取决于你选择的语言）OTel 自带的自动插桩可能不如特定供应商专有产品所提供的插桩。OTel 的开源性质，加上其极其活跃的开发者生态系统，意味着这种情况最终将不复存在。OTel 的自动插桩将至少与那些带有供应商锁定陷阱的替代插桩一样丰富。假以时日，使用 OTel 将成为无需犹豫的选择，也是任何应用可观测性计划的事实标准起点（这种情况今天已经开始发生）。

你不应将其理解为自动插桩将成为为可观测性工具生成有用遥测数据的唯一需要。自定义插桩（参见[第 7 章](#)）在调试与驱动业务逻辑的代码最相关的问题时，仍将是绝对必要的。我们预测，就像没有注释的代码是不可想象的一样，没有自定义插桩的代码也将是不可想象的。作为工程师，我们都将习惯于在编写新代码时考虑需要的插桩。

三年后，构建流水线将大大加快，反馈循环将更短，更多团队将在其 CI/CD 流水线的最后阶段自动部署变更到生产环境（而且他们将真正开始


practicing the D part of the CI/CD acronym). 持续部署可能是一件棘手的事情，但将功能发布与功能部署解耦的做法将使其对大多数组织变得可实现。功能标志将继续获得更多采用，像渐进式交付这样的部署模式也将变得更常见。

我们个人将密切关注的领域是开发者工作流。作为一个行业，我们需要在尽可能早的阶段将可观测性与编写和发布代码的行为连接起来（参见第 11 章）。作为一个行业，我们需要继续缩小输入（编写代码）与输出（运行代码）之间的差距。今天很少有开发者拥有在每次部署后快速获得其代码行为变化反馈的工具。开发者需要密切了解代码更改如何影响每次迭代中生产环境中的用户。能够做到这一点的开发者微乎其微。但是，对于那些能够做到的人来说，体验上的差异是变革性的。

从轶事来看，我们听这些开发者讲述，他们的体验是如此基础且必不可少，以至于他们无法想象再次没有这种能力工作。定量来看，我们开始看到切实的益处显现：能够更快地推进、浪费更少时间、犯更少的错误，并在错误发生时更迅速地捕获这些错误（参见第 19 章）。简而言之，学习如何使用可观测性有助于他们成为更好的软件工程师。我们相信行业需要在生产环境中拥有从根本上更好的开发者工作流，并且我们预测可观测性将成为许多人实现这一目标的途径。

可观测性能否降低实现这种变革性体验的门槛，让更多开发者获得？每次在生产环境中解决一个之前无法解决的问题时，那种感觉像个变态巫师的多巴胺冲击，是否足以维持可观测性采用的势头？我们作为一个行业，能否让可观测性对每个工程团队都更易于访问？

时间会告诉我们。关注这一领域。并请告知你们的进展情况。你们随时可以在 Twitter 上给我们留言：@mipsytipsy、@lizthegrey 和 @gmiranda23（分别）。

*Charity, Liz, and George*

1. 例如 Vera Reynolds 提供了教程“OpenTelemetry (OTel) Is Key to Avoiding Vendor Lock-in”，介绍如何使用 OTel 将跟踪数据发送到 Honeycomb 和 New Relic。

## 关于作者

**Charity Majors** 是 Honeycomb.io 的联合创始人和工程师，Honeycomb.io 是一家将时序的速度与丰富事件的原始能力相结合的初创公司，为复杂系统提供交互式、迭代式调试。她曾在 Facebook、Parse 和 Linden Lab 等公司担任系统工程师和工程经理，但似乎总是负责数据库。

**Liz Fong-Jones** 是一名开发者倡导者、劳工和伦理组织者，以及拥有超过 15 年经验的站点可靠性工程师（SRE）。她是 Honeycomb.io 的倡导者，为 SRE 和可观测性社区发声，此前曾担任 SRE，负责的产品范围从 Google Cloud 负载均衡器到 Google Flights。

**George Miranda** 是 Honeycomb.io 的前工程师转型产品营销人员。他在金融和视频游戏行业构建大规模分布式系统超过 15 年。他发现了自己讲故事的天赋，现在致力于塑造工具、实践和文化，以帮助改善负责管理生产系统的人员的生活。

## 序言

*Observability Engineering* 封面上的动物是鬃狼（*Chrysocyon brachyurus*）。鬃狼是南美洲最大的犬科动物，可在阿根廷、巴西、巴拉圭、玻利维亚和秘鲁部分地区找到。它们的栖息地包括塞拉多生物群落，由湿润和干燥森林、草原、稀树草原、沼泽和湿地组成。尽管名字如此，鬃狼实际上并不是狼，而是一个完全不同的物种。

鬃狼身体细长，耳朵大，长而黑色的腿使它们在奔跑时能够看过高高的草丛。它们肩高近三英尺，重约 50 磅。它们身体的大部分是黑色的，除了它们特有的鬃毛，由长长的金红色毛发组成，危险临近时会直立起来。与灰狼和大多数其他形成族群的犬科动物不同，鬃狼是独居的，通常单独狩猎。它们是杂食性和薄明性，在黄昏和黎明时外出捕食小型哺乳动物、兔子、鸟类和昆虫。它们也会寻找水果和蔬菜，如 lobeira，一种小番茄状浆果，其名字意为“狼之果”。

鬃狼在繁殖季节以一夫一妻制配对，共享 10 英里的领地，野外繁殖季节从四月持续到六月。在圈养中，鬃狼会产下 1 到 5 只幼崽。两只亲本在圈养中都已知会梳理、喂养和保护幼崽，但在野外很少与幼崽一起出现。幼崽发育迅速：第 9 天即可视听，一个月后开始吃反刍食物，15 周断奶。幼鬃狼通常在一年后被认为完全长大并准备离开父母的领地。鬃狼被 IUCN 归类为“近危”，主要是由于栖息地丧失。O’Reilly 封面上的许多动物都濒临灭绝；它们对世界都很重要。

封面插图由 Karen Montgomery 绘制，基于 *Brockhaus Lexicon* 中的黑白版画。封面字体为 Gilroy Semibold 和 Guardian Sans。正文字体为 Adobe Minion Pro；标题字体为 Adobe Myriad Condensed；代码字体为 Dalton Maag 的 Ubuntu Mono。

