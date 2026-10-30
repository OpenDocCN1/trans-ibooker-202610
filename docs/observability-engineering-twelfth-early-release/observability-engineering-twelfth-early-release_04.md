# Chapter 3. Lessons from Scaling Without Observability

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 3rd chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at vwilson@oreilly.com.

So far, we’ve defined *observability* and how it differs from traditional monitoring. We’ve covered some of the limitations of traditional monitoring tools when managing modern distributed systems and how observability solves them. But an evolutionary gap remains between the traditional and modern world. What happens when trying to scale modern systems without observability?

In this chapter, we look at a real example of slamming into the limitations of traditional monitoring and architectures, along with why different approaches are needed when scaling applications. Coauthor Charity Majors shares her firsthand account on lessons learned from scaling without observability at her former company, Parse. This story is told from her perspective.

### An Introduction to Parse

Hello, dear reader. I’m Charity, and I’ve been on call since I was 17 years old. Back then, I was racking servers and writing shell scripts at the University of Idaho. I remember the birth and spread of many notable monitoring systems: Big Brother, Nagios, RRDtool and Cacti, Ganglia, Zabbix, and Prometheus. I’ve used most—not quite all—of them. They were incredibly useful in their time. Once I got a handle on TSDBs and their interfaces, every system problem suddenly looked like a nail for the time-series hammer: set thresholds, monitor, rinse, and repeat.

During my career, my niche has been coming in as the first infrastructure engineer (or one of the first) to join an existing team of software engineers in order to help mature their product to production readiness. I’ve made decisions about how best to understand what’s happening in production systems many, many times.

That’s what I did at Parse. Parse was a mobile-backend-as-a-service (MBaaS) platform, providing mobile-app developers a way to link their apps to backend cloud storage systems, and APIs to backend systems. The platform enabled features like user management, push notifications, and integration with social networking services. In 2012, when I joined the team, Parse was still in beta. At that time, the company was using a bit of Amazon CloudWatch and, somehow, was being alerted by five different systems. I switched us over to using Icinga/Nagios and Ganglia because those were the tools I knew best.

Parse was an interesting place to work because it was so ahead of its time in many ways (we would go on to be acquired by Facebook in 2013). We had a microservice architecture before we had the name “microservices” and long before that pattern became a movement. We were using MongoDB as our data store and very much growing up alongside it: when we started, it was version 2.0 with a single lock per replica set. We were developing with Ruby on Rails and we had to monkey-patch Rails to support multiple database shards.

We had complex multitenant architectures with shared tenancy pools. In the early stages, we were optimizing for development speed, full stop.




我在这里想强调，优化开发速度是*正确的选择*。基于这个决定，我们做出了许多后来不得不撤销并重做的早期选择。但大多数初创公司并不是因为工具选择错误而失败。让我们说清楚：大多数初创公司都会失败。它们失败是因为产品没有需求，或者找不到产品市场契合点，或者客户不喜欢他们构建的东西，或者其他各种原因，其中时间至关重要。选择使用 MongoDB 和 Ruby on Rails 的技术栈使我们能够足够快地上线，从而让客户感到愉悦，并且他们想要更多我们所销售的东西。

在 Facebook 收购 Parse 左右，我们托管了超过 60,000 个移动应用。两年半后，当我离开 Facebook 时，我们托管了超过一百万个移动应用。但即使在我 2012 年刚加入时，裂痕已经开始显现。

Parse 在我加入几个月后正式推出。我们的流量翻了一番，然后再次翻番，然后再次翻番。我们是 Hacker News 的宠儿，每当关于我们的帖子出现在那里时，我们就会迎来新用户注册的激增。

在 2012 年 8 月，我们托管的一个应用首次进入 iTunes Store 前 10 名。该应用在宣传一个来自挪威的死亡金属乐队。乐队用该应用进行直播广播。对乐队来说这是傍晚；对 Parse 来说却是凌晨。每当乐队直播时，Parse 就会在几秒钟内宕机。我们有一个扩展性问题。

## Scaling at Parse

起初，弄清楚我们的服务发生了什么很困难。我们的诊断具有挑战性，因为（得益于我们的共租模型）每当某件事变慢时，*一切*都会变慢，即使它与导致问题的原因无关。

解决这一系列问题需要大量的试错调整。我们不得不提升 MongoDB 数据库管理技能。然后，终于，经过大量工作，我们找到了问题的根源。为了在未来缓解同样的问题，我们编写了工具，可以选择性地限制特定应用或即时重写/限制结构不良的查询。我们还为这个挪威死亡金属乐队的用户 ID 生成了定制的 Ganglia 仪表板，以便将来能够迅速判断它是否导致了宕机。

到达那里很艰难。但我们掌握了它。我们都长舒了一口气，但这只是开始。

Parse 让移动应用开发者能够轻松启动新应用并快速将其发布到应用商店。我们的平台大获成功！开发者们喜欢这种体验。所以，日复一日，他们就是这么做的。很快，我们看到新的 Parse 托管应用每周多次飙升至 iTunes Store 或 Android Market 的榜首。那些应用会做诸如使用我们发送推送通知、保存游戏状态、执行复杂的地理位置查询等事情——工作负载完全不可预测。数百万台设备的通知会在一天中的任何时间被倾倒到 Parse 平台上，没有任何警告。

就是在那时，我们开始遇到所选架构、所用语言以及用于理解这些选择的工具中的许多根本性缺陷。我想重申：每个选择都是*正确的*。我们快速进入市场，我们找到了利基市场，我们交付了成果。现在，我们必须弄清楚如何成长到下一个阶段。

我在这里暂停一下，描述一下我们当时做出的一些决定，以及它们在这个规模下产生的影响。当传入的 API 请求到来时，它们会被负载均衡并交给一个 Ruby on Rails HTTP 工作进程池，即 Unicorns。我们的基础设施在 Amazon Web Services（AWS）中，我们所有的 Amazon Elastic Compute Cloud（EC2）实例都托管一个 Unicorn 主进程，该主进程会 fork 几十个 Unicorn 子进程；这些子进程随后自行处理 API 请求。Unicorns 被配置为保持与多个后端打开的套接字连接——

MySQL 用于内部用户数据，Redis 用于推送通知，MongoDB 用于用户定义的应用数据，Cloud Code 用于在容器中服务器端执行用户提供的代码，Apache Cassandra 用于 Parse 分析，等等。

这里需要注意的是，Ruby 不是线程语言。因此，那个 API 工作进程池是固定的。每当任何一个后端在履行请求时变得稍微慢一点，池就会迅速填满对该后端的待处理请求。每当一个后端变得非常慢（或完全无响应）时，池会在几秒钟内填满——整个 Parse 就会宕机。

起初，我们通过过度配置实例来攻击这个问题：我们的 Unicorns 在正常稳定状态下运行在 20% 的利用率。这种方法让我们能够度过一些较温和的减速。但与此同时，我们也做出了痛苦的决定，从 Ruby on Rails 完全重写为 Go。我们意识到，摆脱这个地狱的唯一方法就是采用原生线程语言。重写代码花了我们两年多，而写出这些代码只花了一年。在此期间，我们正处于体验传统运维方法与现代架构问题根本不兼容的各种方式的最前沿。

这对 Parse 来说是一个特别残酷的时期。我们有一支经验丰富的运维工程团队在做所有“正确”的事情。我们发现我们都知道的最佳实践，它们源于使用传统方法，根本不足以应对现代分布式微服务时代的问题。

在 Parse，我们完全投入基础设施即代码。我们有一套复杂的 Nagios 检查、PagerDuty 警报和 Ganglia 指标。我们有数万个 Ganglia 图表和指标。但这些工具对我们不起作用，因为它们只有在我们已经知道问题会是什么时才有价值——当我们知道哪些阈值是好的以及在哪里检查问题时。

例如，TSDB 图表在我们知道要精心策划和制作哪些仪表板时很有价值——如果我们能预测需要哪些自定义指标来诊断问题。我们的日志工具在我们对要寻找的内容有相当好的想法时很有价值——如果我们提前记住了记录正确的内容，并且知道要搜索的正确正则表达式。我们的应用性能管理（APM）工具在问题表现为他们正在寻找的十大糟糕查询、糟糕用户或糟糕端点之一时很棒。

但我们有一系列问题是这些解决方案无法帮助我们解决的。在这种情况下，上一代工具已不足以应对：

- 每隔一天，就有一个全新的用户飙升进入移动应用商店的前 10 名列表。
- 来自我们任何前 10 名列表中识别出的用户的负载并不是导致网站宕机的原因。
- 慢查询列表都只是问题的症状而不是原因（例如，读查询变慢是因为大量微小的写操作，集体饱和了锁，而单独返回几乎是即时的）。
- 我们需要帮助看到我们整体网站可靠性为 99.9%，但这 0.1% 并不是均匀分布在所有用户中。这 0.1% 意味着只有一个分片 100% 宕机，而它恰好保存了所有属于一家拥有老鼠吉祥物的著名数十亿美元娱乐公司的数据。
- 每天，可能会出现一个新的机器人账户并做一些事情，从而饱和我们 MySQL 主库的锁百分比。

这些类型的问题与上一代问题在范畴上完全不同：正是为了解决那些问题才构建了那套工具。那些工具是为可预测性占主导的世界而构建的。在那些日子里，生产软件系统有“应用”——所有功能和复杂性都包含在一个地方——和“数据库”。但现在，扩展意味着我们把那些单体应用炸开成许多服务



被许多不同的租户使用。我们把数据库拆分成了多样化的多种存储系统。

在许多公司，包括 Parse，我们的商业模式将我们的产品转变为平台。我们邀请用户在我们的托管服务上运行他们认为合适的任何代码。我们邀请他们对我们的托管数据库运行任何他们想运行的查询。而在这样做的过程中，我们对系统的所有控制权在一阵可以赢得的市场美元的烟雾中蒸发了。

在这个服务即平台的时代，我们的客户喜欢我们赋予他们的强大能力。这种驱动力彻底革新了软件行业。而对于我们这些运行为这些平台提供动力的底层系统的人来说，这意味着一切变得极其——而且是指数级地——难以不仅是运营和管理，而且是*理解*。

我们是如何走到这一步的，行业又是在什么时候似乎一夜之间发生了变化？让我们来看看那些造成如此地震式转变的各种小迭代。

## 向现代系统演进

在开始的时候——回到互联网泡沫时代的早期——有“应用”和“数据库”。它们很简单易懂。它们要么是*正常*的，要么是*宕机*的。它们要么是慢的，要么是不慢的。我们需要监控它们的存活状态和可接受的响应阈值。

这项任务并不总是简单。但从运营角度来看，它是直接的。相当长一段时间里，我们甚至围绕着架构简洁性的巅峰。最流行的架构模式是 LAMP 堆栈：Linux、Apache、MySQL，以及 PHP 或 Python。你可能以前听过，我觉得有义务现在再说一遍：*如果你可以用 LAMP 堆栈（或等价物）解决你的问题，你可能应该这么做。*

在设计服务时，第一条规则是不要*增加不必要的复杂性*。你应该仔细识别需要解决的问题，并只解决*这些问题*。仔细思考需要保持开放的选项，并保持这些选项开放。因此，大多数情况下，你应该选择无聊的技术。不要把无聊与糟糕混为一谈。无聊的技术仅仅意味着其边缘情况和故障场景已被许多人很好地理解。

然而，面对当今用户的需求，我们中的更多人发现，许多我们需要解决的问题无法通过谦逊而强大的 LAMP 堆栈来解决。这可能是出于多种原因。你可能有更高的可靠性需求，需要 LAMP 堆栈无法提供的弹性保证。也许你有太多数据或数据过于复杂，无法很好地由 LAMP 堆栈服务。通常，我们在服务规模、可靠性或速度方面达到那种简单架构模型的边缘——这驱使我们对堆栈进行分片、分区或复制。

我们管理代码的方式也很重要。在更大或更复杂的环境中，单体仓库会产生组织压力，从而推动技术变革。拆分代码库可以在允许组织比所有人都为 The One Big App 做贡献时更迅速和自主的方式下，创造更清晰的所有权区域。

这些类型需求——以及更多——是推动现代系统架构清晰而显著转变的一部分。在技术层面，这些转变包括几个主要影响：

- 一切的分解，从一个到多个
- 对多种数据存储的需求，从一个数据库到多种存储系统
- 从大型单体应用向许多更小的微服务迁移
- 多种基础设施类型，从“大型机”服务器转向容器、函数、无服务器和其他临时、弹性资源

这些技术变化也在人类和组织层面产生了强大的涟漪效应：我们的系统是*社会技术*的。这些转变在社会层面引入的复杂性（及其相关的反馈循环）推动了系统及其思考方式的进一步变化。

考虑一下我们在 LAMP 堆栈流行高峰期的计算时代所认为的一些普遍的社会特征。运营和开发团队之间存在一道高耸的组织壁垒，代码有时会被抛过那堵墙。运营团队 notoriously 抵制向生产系统引入更改。部署更改会导致不可预测的行为，往往会导致停机。因此他们会阻止开发人员直接触碰生产环境——密切关注其正常运行时间。在这种“玻璃城堡”方法中，部署冻结是保持生产服务稳定的常用方法。

在 LAMP 堆栈时代，这种对生产的保护性方法也并非完全错误。那时，生产系统中的许多混乱确实是在引入坏代码时造成的。当新代码部署时，其效果几乎是二元的：网站是正常运行还是宕机（也许，有时如果你运气不好，它有第三种状态：莫名其妙地变慢）。

当时几乎不存在控制处理坏代码的副作用的架构构造（而这些在今天很常见）。根本没有服务优雅降级之类的东西。如果开发人员向一个看似微不足道的小系统组件（例如，导出功能）引入了一个错误，它可能会在有人使用它时每次都导致整个系统崩溃。没有机制可以暂时禁用那些原来并非那么微不足道的子组件。不可能在修复那个损坏的子系统时让服务的其余部分不受影响地运行。一些团队会尝试向其单体应用添加这种细粒度控制。但这条路如此充满风险和困难，以至于大多数人甚至没有尝试。

无论如何，许多这些单体系统继续变得更大。随着越来越多的人试图在同一路径上协作，锁竞争、成本上限和动态资源需求等问题成为阻碍。从一个到多个的地震式转变对这些团队来说变得必要。单体应用被迅速分解为分布式系统，随着这种转变而来的还有二阶效应：

- 服务可用性的二元简单性从正常或宕机转变为复杂的可用性启发式，以表示任何数量的部分故障或降级可用性。
- 部署到生产的代码，其部署方案相对简单，转变为渐进式交付。

> **NOTE**
> 
> *渐进式交付*，一词由 RedMonk 联合创始人 James Governor 提出，指的是一系列技能和技术，关注对代码或对生产所做的更改的受控、部分部署（例如，金丝雀发布、功能标志、蓝绿部署和滚动部署）。

- 部署立即更改代码路径并上线的情况转变为通过功能标志将代码部署与功能发布解耦的推广方案。
- 应用在生产中运行一个当前版本的情况转变为在任何给定时间通常有多个版本在生产中烘焙。
- 拥有内部运营团队运行你的基础设施的情况转变为许多关键基础设施组件由其他公司其他团队运行并通过 API 抽象出来，在那里它们甚至可能对你这个开发者不可访问。
- 过去用于告警和排查先前遇到过的已知故障的监控系统在分布式世界中变得捉襟见肘，在那里从未遇到过（也可能永远不会再遇到）的未知故障成为新的普遍问题类型。

管理像 LAMP 堆栈这样的单体系统的工具和技术对于运行现代系统来说是极其无效的。以“一揽子”发布部署应用的系统与微服务的管理方式截然不同。对于微服务，应用通常是分块推出，代码部署不一定会发布功能，因为功能标志现在可以在无需部署的情况下启用或禁用代码路径。

同样，在分布式世界中，预发布系统变得比过去更有用或更可靠。即使在单体世界中，复制生产环境到预发布环境也一直很困难。现在，在分布式世界中，这几乎是不可能的。这意味着调试和检查在预发布环境中变得无效，我们已转向一种需要在生产环境本身完成这些任务的模型。

## 向现代实践演进

我们系统的技术和社会方面是相互关联的。鉴于这些社会技术系统，将我们的技术向不同性能模型转变的重点也需要转变定义团队绩效方式的模型。

此外，这些转变是如此相互关联，以至于不可能在它们之间划出清晰的界限。前一节中描述的许多技术转变影响了团队如何必须重新组织和改变实践以支持它们。

然而，在一个领域，社会行为的激进转变是绝对清晰的。向现代分布式系统的演进也带来了改变工程师必须与生产环境的关系的第三阶效应：


-   用户体验不能再被一概而论为对所有服务用户都一样。在新模式下，同一服务的不同用户可能会以不同方式在系统中被路由，使用不同的组件，从而获得可能差异很大的体验。
-   用于发现生产环境中系统条件超出已知阈值的边缘情况的监控告警，会产生大量误报、漏报和无意义的噪声。告警已转向一种只关注直接影响用户体验的症状、从而触发更少告警的模式。
-   调试器不能再附加到某个特定的运行时上。满足服务请求现在需要跨网络跳跃，跨越多个运行时，通常单个请求就要跳转多次。
-   需要人工修复且可在运行手册中定义的已知重复故障已不再是常态。服务故障已从这种模式转向已知重复故障可以自动恢复的模式。无法自动恢复因而触发告警的故障，很可能意味着响应工程师将面临一个前所未见的问题。

这些第三级信号意味着，关注的焦点正在发生巨大的引力式转变：从预生产环境的重要性，转向对生产环境了如指掌的重要性。传统上在代码进入生产环境*之前*强化代码、确保其安全性的做法，已经开始被视为具有局限性，甚至有些徒劳。测试驱动开发和针对预演环境运行测试仍然有用。但它们永远无法复制代码在生产环境中被使用时那种狂野而不可预测的特性。

作为开发者，我们能够投入到完成工作中的精力周期是固定的。传统方法的局限性在于，它首先注重预生产环境的加固。即便还有剩余的零星注意力，也会被用于关注生产系统。如果我们想在生产环境中构建可靠的服务，就必须颠倒这个顺序。

在现代系统中，我们*必须*把大部分工程注意力和工具放在生产系统上，这是第一要务。剩余的注意力周期应该用于预演系统和预生产系统。预演系统是有价值的，但其本质是次要的。

预演系统不是生产环境。它们永远无法复现生产环境中正在发生的事情。预生产系统那种无菌实验室般的环境，永远无法模拟真实付费用户在实际世界中测试代码时所处的条件。然而，许多团队仍然把生产环境视为一座玻璃城堡。

工程团队必须重新排列生产环境的价值优先级，并相应地改变自己的实践。如果不把生产系统作为首要关注点，这些团队将只能使用次等的工具、可见性和可观测性，在生产系统上辛苦地摆弄。他们会继续把生产环境视为一个不敢轻易涉足的脆弱环境——一看到潜在问题的丝毫迹象，便本能而安心地回滚部署——因为他们缺少足够的手段来进行微小的调整、调优设置、优雅地降级服务，或者针对自己了然于胸的问题渐进式地部署变更。

向现代系统演进的技术变革也带来了社会变革，意味着工程团队必须改变他们与生产环境的关系。不改变这种关系的团队，将更真切地承受不改变所带来的痛苦。在现代系统中，生产环境越早不再是玻璃城堡，负责这些系统的团队的生活——以及使用这些系统的客户的体验——就会越早得到改善。

## Parse 的实践转变

在 Parse，我们在快速扩张的过程中不得不颇为痛苦地经历这些变化。但这些变化并非一夜之间发生。我们只知道传统的实践方式。就我个人而言，我一直遵循着那条我在其他公司使用过多次的、众所周知的生产系统管理路径。

当全新问题出现时，比如那次挪威死亡金属乐队事件，我会深入其中进行大量调查，直到发现问题根源。我的团队会进行事后剖析来解剖问题，编写运行手册以指导未来的我们如何处理它，制作一个（或两个）自定义仪表盘，以便下次该问题能立刻浮现出来，然后继续前进，并认为问题已经解决。

这种模式在单体系统中效果很好，因为真正全新的问题很少见。在 Parse 的最初日子里，它运作得也很好。但面对现代系统，这种模式完全无效，因为真正全新的问题很可能占据所遇到问题的大多数。一旦我们开始每天遭遇一连串不同类别的问题，这种方法徒劳无功便变得不容否认。在这种情况下，我们花在事后剖析、编写运行手册和制作自定义仪表盘上的时间几乎等于是浪费时间。我们再也遇不到那些相同的问题了。

这种传统方法的现实是，其中很大一部分都在猜测。在其他系统中，我已经非常擅长猜测可能出错的地方，以至于几乎不费吹灰之力。我凭直觉就能与自己的系统保持同步。我可以扫一眼一组复杂的仪表盘，然后自信地对团队说：“问题出在 Redis”，哪怕屏幕上根本没有 Redis 的踪影。我为这种技术直觉感到自豪。这很有趣！我感觉自己像个英雄。

我想强调软件行业英雄文化的这一面。在采用 LAMP 栈式运维的单体系统中，最后的调试手段通常是那个人：待得最久、从零构建系统的人。资历最深的工程师是最终的升级点。他们拥有最多的“伤疤”，脑海中存储着最大的事故目录，并且总是不可避免地飞身救场。

因此，这些英雄也永远无法真正休假。我记得在夏威夷度蜜月时，凌晨 3 点被传呼，因为 MongoDB 不知何故搞垮了 Parse 的 API 服务。我的老板，CTO，万分抱歉。但网站已经宕机了。那曾经是

时间分析可以沿着任意高基数维度任意切片和切块，那个时间降到了几分钟——甚至几秒。

我们可以通过从症状入手，沿着一条“面包屑路径”来调查一个全新问题，无论它通向哪里，无论我们是否是第一次遇到这个问题。我们不必熟悉问题就能解决它。相反，我们现在拥有了一种分析性和可重复的方法：提出问题，得到答案，再利用这些答案提出下一个问题，直到我们找到根源。在生产环境中排查问题，意味着从数据开始，坚持不懈地一步接着一步有条不紊地推进，直到得出解决方案。

这种新获得的能力不可逆转地改变了我们的实践。我们不再依赖复杂知识和锁在单个工程师脑海中的事故目录——队友无法触及——而是拥有了数据和一套方法论，它们在共享工具中可见，且人人都能访问。对团队而言，这意味着我们可以跟随彼此的足迹，追溯彼此走过的路径，理解他人在想什么。这种能力将我们从依赖用于模式匹配的工具的陷阱中解放出来。

借助指标和传统监控工具，我们可以轻松看到性能尖峰，或注意到问题可能正在发生。但这些工具不允许我们任意切片、切块并沿着技术栈一路挖掘，去识别问题根源，或发现我们没有任何其他途径能发现的错误关联。这些工具还要求我们在调查全新问题之前，预先预测哪些数据可能有价值（就好像我们能在全新问题出现之前就预知需要提出哪些问题）。同样，使用日志工具时，我们必须提前记得记录任何有用的内容——并且在与调查相关时，确切地知道要搜索什么。使用 APM 工具，我们可以快速看到工具供应商预测为最值得了解的问题，但当全新问题成为常态时，这些工具几乎无济于事。




这个故事源自我在 Parse 时期的经历，说明了组织为何以及如何从传统的工具和监控方法转向利用现代分布式系统和可观测性来扩展其实践。我于 2015 年离开 Facebook，时间点在它宣布即将关闭 Parse 托管服务之前。从那以后，我的团队和我在使用现代分布式系统时所面临的许多问题，随着软件行业转向采用类似技术，已变得愈发普遍。

Liz、George 和我认为，这一转变解释了可观测性背后的热情以及其流行度的上升。可观测性是对在扩展现代系统时变得普遍的问题的解决方案。在后续章节中，我们将探讨可观测性所带来的诸多方面、影响和益处。
