# 渐进式交付

过去十年左右，我们在向用户部署软件方面变得更聪明。出现了许多新技术，它们由不同的用例驱动，来自 IT 行业的许多不同部分，但主要它们都围绕着让推送新软件的行为风险更小而驱动。如果发布软件的风险变得更小，我们就可以更频繁地发布软件。

在将软件投入生产之前，我们会进行大量活动，以帮助我们在影响真实用户之前发现问题。生产前测试是其中的重要部分，尽管正如我们将在 \[链接待定\] 中讨论的那样，这只能做到一定程度。

在他们的书 Accelerate^(14) 中，作者通过广泛的研究清楚地表明，高绩效公司比低绩效公司部署更频繁，同时具有 *变化失败率低得多*。

“快速行动打破事物”的想法在软件交付方面似乎并不适用——频繁交付和较低的失败率是相辅相成的，意识到这一点的组织改变了他们对软件发布方式的思考。

这些组织使用诸如 Feature Toggles、Canary Releasing、Parallel Runs 等技术，我们将在本节中详细介绍。对发布功能方式的这种转变属于被称为 *Progressive Delivery* 的范畴。功能以受控的方式向客户推出——而不是大爆炸式部署，我们可以更明智地决定谁看到什么功能，以及新软件对客户可访问的速度。

从根本上说，所有这些技术都有一点共同的思维转变。也就是说，我们可以将部署的概念与发布的概念分开。

## 将部署与发布分离

Jez Humble，Continuous Delivery 的合著者，主张将这两个概念分开，并将其作为低风险软件发布的核心原则之一^(15)。

> 部署是指你将软件的某个版本安装到特定环境中（通常隐含生产环境）。发布是指你将系统或其一部分（例如一个功能）提供给用户使用。
>
> Jez Humble




# 渐进式交付

Jez 认为通过将这两个概念分开，我们可以确保我们的软件在生产环境中正常运行，而不会让用户看到故障。蓝绿部署是这一概念最简单的示例之一——你有一个正在运行的软件版本（蓝色），然后在生产环境中将新版本与旧版本并行部署（绿色）。你会检查新版本是否按预期工作，如果是，就将客户重定向到现在查看软件的新版本。如果在切换前发现问题，则不会影响任何客户。

虽然蓝绿部署是这一原则中最简单的示例之一——事实证明，当我们接受这一概念时，还有许多更复杂的技巧可以使用。

## 转向渐进式交付

James Governor，开发者导向的行业分析公司 RedMonk 的联合创始人，最早创造^(16)了“渐进式交付”这一术语，用来涵盖该领域使用的多种不同技术。他后来将渐进式交付描述为“对爆炸半径进行精细控制的持续交付”^(17)——因此是持续交付的扩展，但是一种使我们能够控制新发布软件潜在影响的技术。

承接这一主题，LaunchDarkly 的 Adam Zimman 描述了^(18)渐进式交付如何影响“业务”。从这个角度来看，我们需要转变对新功能如何到达客户的思考方式。它不再是一次性发布——现在可以是一个分阶段的活动。重要的是，渐进式交付可以赋能业务利益相关者，正如 Adam 所说“将功能的控制权委托给对结果负有最直接责任的所有者”。不过要实现这一点，相关利益相关者需要了解所使用的渐进式交付技术的机制。因此，这要求我们的产品负责人等在技术上更能理解如何最好地使用渐进式交付。

我们已经提到蓝绿部署是一种渐进式交付技术。让我们简要看看其他几种

## 功能开关

使用功能开关（也称为功能标志），我们将已部署的功能隐藏在开关后面，可用于打开或关闭功能。这最常作为主干分支开发的一部分使用，其中尚未完成的功能可以签入并部署，但仍对终端用户隐藏，但它在这些之外也有很多应用。这可以用于在指定时间打开功能，或关闭导致问题的功能。

你也可以以更细粒度的方式使用功能开关，也许可以根据发出请求的用户的性质让标志具有不同的状态。例如，你可以让一组客户看到功能已打开（也许是一个 Beta 测试组），而大多数人看到功能处于关闭状态——这可以帮助你实现金丝雀发布，我们接下来会讨论。用于管理功能开关的完全托管解决方案已存在，包括 LaunchDarkly^(19) 和 Split^(20)。尽管这些平台令人印象深刻，但我认为你可以用更简单的方式开始——一开始只需一个配置文件即可，然后在你开始推动想要如何使用开关时再考虑这些技术。

对于更深入地了解功能开关的世界，我强烈推荐 Pete Hodgson 的文章“Feature Toggles (aka Feature Flags)”^(21)，其中详细介绍了如何实现它们以及它们可以使用的多种不同方式。

## 金丝雀发布

> 犯错是人类的本性，但要真正搞砸事情，你需要一台计算机^(22)。
>
> Paul Ehrlich

我们都会犯错，而计算机可以让我们比以往任何时候都更快、更大规模地犯错。鉴于错误是不可避免的（相信我，它们确实如此），那么做一些可以限制这些错误影响的事情是有意义的。金丝雀发布就是其中一种技术。

金丝雀发布的名字来源于矿工带入矿井的金丝雀，作为早期预警系统，警告他们危险气体的存在。在金丝雀发布中，想法是让我们客户的有限子集看到新功能。如果发布出现问题，则只有该部分客户受到影响。如果该功能对金丝雀组有效，则可以将其推广到更多客户，直到每个人都看到新版本。

对于微服务架构，可以在单个微服务级别配置开关，对来自外部世界或其他微服务的该功能请求打开（或关闭）功能。另一种技术是并排运行两个不同版本的微服务，并使用开关路由到旧版本或新版本。在这里，金丝雀实现必须位于路由/网络路径中的某个位置，而不是位于一个微服务中。

当我第一次进行金丝雀发布时，我们手动控制发布。我们可以配置看到新功能的流量百分比，并在一周的时间内逐渐增加，直到每个人都看到新功能。在一周内，我们会关注错误率、错误报告等。现在，更常见的是以自动化方式处理此过程。例如，像 Spinaker^(23) 这样的工具可以根据指标自动增加调用，例如如果错误率在可接受水平，则增加对新微服务版本的调用百分比。

## 并行运行

在金丝雀发布中，对某项功能的一次请求将由旧版本或新版本提供服务。但这并不能让我们直接比较两个不同的实现。如果您想确保新功能与已知基线完全相同的方式工作，这可能很重要。实现这一点的较简单方法之一是对旧实现和新实现都运行任何请求，并比较结果。

在并行运行中，你就是这样做的——你并排运行同一功能的两个不同实现，并向两个实现发送对该功能的请求。在微服务架构中，最明显的方法可能是将服务调用分派到同一服务的两个不同版本并比较结果。另一种选择是在同一服务内部共存该功能的两个实现，这通常可以使比较更容易。

在执行两个实现时，重要的是要意识到你可能只想要其中一次调用的结果。其中一个实现被认为是真实来源——这是你当前信任的实现，通常是现有实现。根据你使用并行运行比较的功能的性质，你可能需要仔细考虑这种细微差别——例如，你不希望向客户发送两次相同的订单更新，或两次支付同一张发票！



我将在我的书《从单体到微服务》第 3 章中更详细地探讨并行运行模式^(24)。在那里我探讨了它在帮助将单体系统中的功能迁移到微服务架构方面的用途，在这种架构中，我们希望确保新的微服务的行为与等效的单体功能相同。在另一个背景下，GitHub 在重构其代码库的核心部分时使用了这种模式，并发布了一个开源工具 `Scientist`^(25)来帮助他们完成此过程。在这里，并行运行是在单个进程内完成的，`Scientist` 帮助他们比较调用。

### 提示

有了蓝绿部署、功能开关、金丝雀发布和并行运行，我们才刚刚触及渐进交付领域的表面。这些想法可以很好地协同工作（例如，我们已经提到过如何使用功能开关来实现金丝雀发布），但你可能希望逐步上手。首先，只需记住区分部署和发布这两个概念。接下来，开始寻找能够帮助你更频繁地部署软件但又安全的方式。与产品负责人或其他业务利益相关者合作，了解这些技术如何帮助你更快地推进，同时也有助于减少故障。

# 总结

好吧，我们在这里涵盖了很多内容。在继续之前，让我们简要回顾一下。首先，让我们回顾一下我之前概述的部署原则：

### 隔离执行

以隔离的方式运行微服务实例，使其拥有自己的计算资源，并且其执行不会影响附近运行的其他微服务实例。

### 专注于自动化

选择允许高度自动化的技术，并将自动化作为文化核心的一部分。

### 基础设施即代码

表示基础设施的配置以简化自动化并促进信息共享。将此代码存储在源代码控制中，以便可以重建环境。

### 目标为零停机部署

进一步推进独立部署能力，并确保部署微服务的新版本可以在不影响服务用户（无论是人类还是其他微服务）停机的情况下完成。

### 期望状态管理

使用一个维护微服务处于定义状态的平台，在发生故障或流量增加时根据需要启动新实例。

也很重要理解**你的**需求。`Kubernetes` 可能非常适合你，但也许更简单的方案同样有效。不要为选择更简单的解决方案而感到羞愧，也不要太担心将工作外包给他人——如果我可以将工作推到公有云上，那么我就会这样做，因为这让我能够专注于自己的工作。

最重要的是，这个领域正在经历大量的变动。我希望我能让你对该领域的关键技术有所了解，同时也分享了一些原则，这些原则很可能会比当前流行的热门技术更持久。无论接下来是什么，希望你能更好地从容应对。

在下一章中，我们将更深入地探讨一个我们在此简要提及的话题：测试我们的微服务以确保它们确实能正常工作。

^(1) Morris, Kief.*“基础设施即代码*。O’Reilly, 2015。撰写本文时，Kief 也正在着手他的书的第二版

^(2) [*https://nomadproject.io*](https://nomadproject.io)

^(3) [*https://www.weave.works/oss/flux/*](https://www.weave.works/oss/flux/)

^(4) [*https://www.docker.com/products/docker-desktop*](https://www.docker.com/products/docker-desktop)

^(5) [*https://read.acloud.guru/why-the-future-of-software-and-apps-is-serverless-reprinted-from-10-15-2012-b92ea572b2ef*](https://read.acloud.guru/why-the-future-of-software-and-apps-is-serverless-reprinted-from-10-15-2012-b92ea572b2ef)

^(6) [*https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp*](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp)

^(7) [*https://www.youtube.com/watch?v=94dS3kWDswk*](https://www.youtube.com/watch?v=94dS3kWDswk)

^(8) 我可能不是这个规则的提出者

^(9) [*http://terraform.io*](http://terraform.io)

^(10) [*http://pulumi.com*](http://pulumi.com)

^(11) [*https://queue.acm.org/detail.cfm?id=2898444*](https://queue.acm.org/detail.cfm?id=2898444)

^(12) [*https://knative.dev/*](https://knative.dev/)

^(13) [*https://www.katacoda.com*](https://www.katacoda.com)

^(14) Nicole Foresgren, Jez Humble and Gene Kim, *加速：构建和扩展高绩效技术组织的科学* (T Revolution Press, 2018)

^(15) [*http://www.informit.com/articles/article.aspx?p=1833567&seqNum=2*](http://www.informit.com/articles/article.aspx?p=1833567&seqNum=2)

^(16) [*https://redmonk.com/jgovernor/2018/08/06/towards-progressive-delivery/*](https://redmonk.com/jgovernor/2018/08/06/towards-progressive-delivery/)

^(17) [*https://thenewstack.io/the-rise-of-progressive-delivery-for-systems-resilience/*](https://thenewstack.io/the-rise-of-progressive-delivery-for-systems-resilience/)

^(18) [*https://launchdarkly.com/blog/progressive-delivery-a-history-condensed/*](https://launchdarkly.com/blog/progressive-delivery-a-history-condensed/)

^(19) [*http://launchdarkly.com*](http://launchdarkly.com)

^(20) [*https://www.split.io*](https://www.split.io)

^(21) [*https://martinfowler.com/articles/feature-toggles.html*](https://martinfowler.com/articles/feature-toggles.html)

^(22) 这句话常被归因于生物学家 Paul Ehrlich，但其实际出处尚不明确 [*https://quoteinvestigator.com/2010/12/07/foul-computer/*](https://quoteinvestigator.com/2010/12/07/foul-computer/)

^(23) [*https://www.spinnaker.io*](https://www.spinnaker.io)

^(24) Newman, Sam. *从单体到微服务*. O’Reilly, 2019.

^(25) [*https://github.com/github/scientist*](https://github.com/github/scientist)
