# 第 2 章。 可观测性与监控之间的调试实践有何不同

### 给早期发布读者的说明

通过 Early Release 电子书，您可以在正式发布这些书名之前很久就利用这些技术——您可以在作者撰写时获得书籍的最原始形式——未经编辑的内容。

这将是最终书籍的第 2 章。

如果您对如何改进本书的内容和/或示例有任何评论，或者如果您注意到本章中缺少材料，请联系编辑 vwilson@oreilly.com。

在上一章中，我们介绍了用于调试的指标数据类型 的起源和常见用途。在本章中，我们将更仔细地审视与传统监控工具相关的特定调试实践，以及它们与可观测性工具相关的调试实践有何不同。

传统监控工具通过将系统条件与指示先前已知错误条件是否存在的已知阈值进行检查来工作。这是一种从根本上说是被动的 方法，因为它仅在识别先前遇到的故障模式时才有效。

相比之下，可观测性工具通过实现迭代探索性调查来工作，以系统地确定性能问题可能发生的位置和原因。可观测性使主动识别任何故障模式成为可能，无论是先前已知还是未知。

在本章中，我们重点了解基于监控的故障排除方法的局限性。首先，我们解析监控工具在生产环境中排查软件性能问题时的使用方式。然后我们检查那些基于监控的方法所制度化的行为。最后，我们展示可观测性实践如何使团队能够识别先前已知和未知的问题。

### 如何使用监控数据进行调试

*牛津英语词典* 将 *monitoring* 定义为在一段时间内观察和检查（某事）的进展或质量，以保持系统审查。传统的监控系统正是通过指标来实现这一点的：它们会随时间检查应用程序的性能；然后报告该时间间隔内的性能总体衡量。监控系统收集、聚合和分析指标，以筛选出指示是否存在令人担忧趋势的已知模式。

这种监控数据有两个主要消费者：机器和人类。机器使用监控数据来决定检测到的条件是否应触发警报或应宣布恢复。*metric* 是在记录时特定时间间隔内系统状态的数值表示。与查看物理仪表类似，我们可能只需瞥一眼指标，就能了解特定资源在特定时间点的利用率是过高还是不足。例如，CPU 利用率现在可能为 90%。

但这种行为正在改变吗？仪表上显示的测量值是在上升还是下降？指标通常在聚合时更有用。了解指标随时间推移的趋势值可提供影响软件性能的系统行为的洞察。监控系统收集、聚合和分析指标，以筛选出人类希望了解的趋势的已知模式。

如果 CPU 利用率在接下来的两分钟内继续保持在 90% 以上，有人可能会决定这是他们希望收到警报的条件。为清晰起见，值得注意的是，对机器而言，指标只是一个数字。

在指标世界中，系统状态非常二元。在某个数字和时间间隔以下，机器不会触发警报。在某个数字和时间间隔以上，机器会触发警报。阈值究竟在哪里，是人类的决定。

当监控系统检测到人类认为重要的趋势时，就会发送警报。同样，如果 CPU 利用率在预配置的时间段内降至 90% 以下，监控系统将确定触发警报的错误条件不再适用，因此宣布系统已恢复。这是一个原始的系统，然而我们如此多的故障排除能力都依赖于它。

人类使用相同数据调试问题的方式则更有趣。这些数值测量被输入到 TSDB 中，图形界面使用该数据库作为数据趋势图形表示的来源。这些图表可以被收集并组装成越来越复杂的组合，被称为 *dashboards*。

静态仪表板通常按每个服务组装一个，它们是工程师开始了解底层系统特定方面的有用起点。这是仪表板的最初意图：提供一组指标的跟踪概览并突出显示值得注意的趋势。然而，仪表板是发现调试新问题的糟糕选择。

当仪表板首次构建时，我们没有太多需要担心的系统指标。因此，相对容易构建一个仪表板，显示任何给定服务的任何人应该知道的关键数据。在现代，存储便宜，处理强大，我们可以收集的关于系统的 数据似乎几乎无限。现代服务通常收集如此多的指标，以至于不可能将它们全部放入同一个仪表板中。然而，这并没有阻止许多工程团队试图将它们全部放入单一视图中。毕竟，这就是仪表板的承诺！

为了使所有内容都适合仪表板，指标通常会被聚合和平均。这些聚合值可能传达特定条件——例如，集群中的 CPU 超过 90%。但这些聚合

以下场景对负责管理生产服务的工程师来说应该很熟悉。如果你是这样的工程师，请把自己置于这些鞋子里，并利用这种视角来审视你在自己工作环境中也做出的假设。如果你不是通常管理生产服务的工程师，请检查以下场景，以了解上一节中描述的局限性类型。

新的一天开始了，你的工作日刚刚开始。你走到办公桌前，你做的第一件事之一就是瞥一眼一堆随时显示的仪表板。你的仪表板系统力求成为“单一玻璃窗”，在它后面，你可以快速看到应用程序系统的各个方面、其各种组件及其健康状态。你还有几个仪表板充当高层次摘要，以传达重要的顶线业务指标——因此你可以查看，例如，你的应用程序是否正在打破新的流量记录，如果你的任何应用程序在夜间被从 App Store 中删除，以及各种其他需要立即关注的关键条件。

你瞥一眼这些仪表板，以寻找熟悉的条件并向自己保证，你可以开始新的一天，而不必分心于扑救生产紧急情况。仪表板显示了一组两到三打图表。你不知道其中许多图表实际上显示了什么。然而，随着时间的推移，你对这些图表赋予你的预测能力产生了信心。例如，如果屏幕底部这个图表变红，你应该立即放下手头的工作并开始调查，以免情况恶化。

也许你不知道所有图表实际上测量了什么，但它们相当可靠地帮助你预测生产服务中问题发生的位置，你对它非常熟悉。当图表以某种方式变化时，你几乎获得了预测的超能力。如果顶部图表的左角下探，而右下角图表稳步增长，你的消息队列存在问题。如果中央的方块每五分钟尖峰一次，背景比正常情况下红了几个色调，数据库查询出现了问题。




就在此时，当你浏览图表时，你注意到缓存层出现了问题。仪表盘上没有任何内容明确显示“你的主缓存服务器正在升温”。但你已经非常熟悉自己的系统，以至于通过解读屏幕上的模式，你可以立即采取行动，去做那些数据中完全没有明确说明的事情。你以前见过这类问题，根据过去的那些教训，你知道这种特定的指标组合表明存在缓存问题。

看到那个模式后，你迅速调出系统缓存组件的仪表盘来确认你的怀疑。你的怀疑得到了证实，于是你马上投入到修复问题的工作中。同样，你可以对不止少数几种模式这样做。随着时间的推移，你已经学会通过解读特定生产服务中的蛛丝马迹来洞悉问题的根源。

## 凭直觉排查的局限性

许多工程师对这种排查方法非常熟悉。问问自己，在调查问题的过程中穿梭于系统的各个组件时，你有多大程度依赖直觉？通常来说，作为行业惯例，我们重视这种直觉，多年来它也给我们带来了很多好处。现在再问问自己：如果把你放到同样的仪表盘工具面前，但换成一个完全不同的应用程序，用不同的语言编写，采用不同的架构，你还能洞悉那些相同的答案吗？当左下角变成蓝色时，你会知道自己该做什么，或者是否真的有必要采取行动吗？

显然，这个问题的答案是否定的。通过仪表盘看到的各类系统问题的表现方式，在不同的应用程序技术栈之间差别很大。然而，作为行业惯例，这是我们与系统交互的主要方式。从历史上看，工程师们依赖的是静态仪表盘，这些仪表盘上塞满了数据，而这些数据距离做出正确诊断所需的数据还隔着一到两层的解释。但当我们遇到新问题时，就会开始看到它们实用性的局限。让我们考虑几个例子。

## 示例1：相关性不足

一位工程师添加了一个索引，并想知道这是否达到了加快查询时间的目标。他们还想知道是否发生了其他意想不到的后果。他们可能想问以下问题：

- 某个特定的查询（已知是一个痛点）扫描的行数是否比以前少了？
- 查询规划器选择新索引的频率有多高，是针对哪些查询？
- 写延迟整体上是上升了，平均值或第95/99百分位数是多少？
- 使用这个索引时，查询比之前的查询计划更快还是更慢？
- 还有哪些其他索引会与新索引一起使用（假设存在索引交叉）？
- 这个索引是否让其他索引变得过时，这样我们就可以删除它们并回收一些写入容量？

这些只是一些示例问题，他们还可以问更多。然而，他们手头只有仪表盘和图表，显示CPU平均负载、内存使用、索引计数器，以及主机和运行中数据库的大量其他内部统计信息。他们无法按用户、查询、目标或源IP等维度进行切片切块或细分。他们所能做的只是凭肉眼观察大致变化，然后主要基于时间戳做出一个复杂的猜测。

## 示例2：无法下钻

一位工程师发现了一个无意中使数据过期的bug，并想知道它影响的是所有用户还是只是某些分片。然而，他们在仪表盘上所能看到的是，磁盘空间正在以可疑的速度快速下降……而且只发生在一个数据分片上。他们迅速断定问题仅限于该分片，然后继续前进，没有意识到另一个分片的磁盘空间之所以看起来保持稳定，是因为恰好有并发导入操作。

## 示例3：工具跳转

一位工程师在某个特定时间看到错误激增。他们开始翻看仪表盘，寻找同一时间其他指标中的峰值，确实找到了一些，但他们无法分辨哪些是错误的原因，哪些是结果。于是他们跳到日志工具中，开始搜索错误。一旦他们找到错误的请求ID，就转向链路追踪工具，将错误ID复制粘贴到追踪工具中。（如果该请求没有被追踪，他们会一遍又一遍地重复，直到捕获到被追踪的请求。）

随着时间的推移，这些监控工具可以更好地检测更细粒度的问题——如果你有一个稳健的传统，总是在宕机后进行事后分析，并在可能的情况下响应式地添加自定义指标。通常，这种情况的发生方式是，值班工程师弄清楚了问题，或者得出了一个合理的假设，同时弄清楚如果存在的话，究竟是哪些指标能回答这个问题。他们提交一个变更来创建该指标并开始收集它。当然，现在想看看你上一次的变更是否产生了你猜测的影响已经太迟了——你无法回到过去再捕获一次那个自定义指标，除非你能完整重放整个确切的场景——但故事是这样的：如果再次发生，下次你就能确定了。

前面示例中的工程师可能会回去为每个查询族、每个集合的过期率、每个分片的错误率等添加自定义指标。（他们可能会发疯，为每个查询族的锁使用情况、每个索引的命中率、执行时间的分桶等添加自定义指标——然后在下个计费周期发现监控预算翻了一倍。）

## 传统监控从根本上是被动的

上面的方法完全是反应式的，但许多团队将其视为正常的运维状态——排查问题就是这样做的。充其量，这是一种与关键遥测数据玩“打地鼠”游戏的方式，总是在事后疲于追赶。而且成本相当高，因为指标工具的定价方式往往使自定义指标随着每个指标线性扩展。许多团队热情地全力投入自定义指标，然后看到账单时大惊失色，最终不得不回头修剪掉其中大部分。

你的团队是那些团队之一吗？为了帮助确定这一点，请在一周内维护生产服务时留意以下指标：

- 当生产环境出现问题时，你是根据系统信息面包屑的实际可见痕迹来确定需要调查的地方吗？还是凭直觉去定位这些问题？你是在上次找到答案的那个地方查看吗？
- 你是否依赖自己对这套系统及其历史问题的专家级熟悉程度？当你使用排查工具调查问题时，你是在探索性地寻找线索吗？还是试图确认一个猜测？例如，如果延迟整体变慢，而你有几十个可能造成延迟的数据库和队列，你能用数据确定延迟来自哪里吗？还是像往常一样猜测一定是MySQL数据库，然后去查看MySQL图表来确认你的直觉？
- 你多久会凭直觉跳到一个解决方案，然后寻找证据来确认它是正确的并据此行事——但实际上错过了真正的问题，因为那个被确认的假设只是症状，或者是结果而非原因？
- 你的排查工具是在为你的问题提供精确答案，并引导你直接找到答案吗？还是在信息不足的情况下进行问题诊断？

问题，沿着任何一个维度或维度组合，而不需要先预测问题可能发生在哪里以及如何发生；这种模式以提问和理解为中心。

让我们从三个轴来比较监控和可观测性之间的差异：依赖机构知识、发现隐藏问题，以及对诊断生产问题的信心。我们将在接下来的章节中提供更多关于这些差异如何以及为何发生的深入示例。现在，我们将在高层次上做这些比较性差异。

*机构知识*是未成文的信息，可能组织内某些人知道，但其他人并不普遍了解。在基于监控的方法中，团队往往围绕“资历是知识的关键”这一观念来组织：在团队中呆得最久的工程师往往是团队中最好的调试者，也是最后求助的调试者。当调试源于个人解读先前已知模式的经验时，这种倾向就不足为奇了。

相反，实践可观测性的团队则倾向于一个截然不同的方向。使用可观测性工具时，团队中最好的调试者通常是最好奇的工程师。实践可观测性的工程师能够通过提出探索性问题来审问他们的系统，并利用发现的答案引导自己进行进一步的开放式询问（见第8章）。可观测性不重视对某一特定系统的深入了解来提供调查直觉，而是奖励那些能够跨不同系统迁移的娴熟调查能力。

这种转变的影响在处理深埋在复杂系统内部的问题时最为明显。基于反应式监控、奖励直觉和预感的方法，也容易因确认偏差而掩盖问题的真正根源。当发现问题时，它们是按照行为模式与先前已知问题的相似程度来诊断的。这可能导致只治疗症状


of a problem without ever getting to the actual source. Engineers guess at what might be occurring, jump to confirm that guess, and alleviate the symptom without ever fully investigating why it was happening in the first place. Worse, by introducing a fix for the symptom instead of its cause, teams will now have two problems to contend with instead of one.

Rather than leaning on expert foreknowledge, observability allows engineers to treat every investigation as new. When issues are detected, even if the triggering conditions appear similar to past problems, an engineer should be able to put one metaphorical foot in front of the other to follow the clues provided by breadcrumbs of system information. You can follow the data toward determining the correct answer—every time, step by step. That methodical approach means that any engineer can diagnose any issue without needing a vast level of system familiarity to reason about impossibly complex systems to intuitively divine a course of action. Further, the objectivity afforded by that methodology means engineers can get to the source of the specific problem they’re trying to solve rather than treating the symptoms of similar problems in the past.

The shift toward objective and methodological investigation also serves to increase the confidence of entire teams to diagnose production issues. In monitoring-based systems, humans are responsible for leaping from tool to tool and correlating observations between them, because the data is pre-aggregated and does not support flexible exploration. If they want to zoom in closer, or ask a new question, they must mentally carry the context when moving from looking at dashboards to reading logs. They must do so once more when moving from logs to looking at a trace, and back again. This context switching is error-prone, exhausting, and often impossible given the inherent incompatibilities and inconsistencies encountered when dealing with multiple sources of data and truth. For example, if you’re responsible for drawing correlations between units like TCP/IP packets and HTTP errors experienced by your app, or resource starvation errors and high memory-eviction rates, your investigation likely has such a high degree of conversion error built in that it might be equally effective to take a random guess.

Observability tools pull high-cardinality, high-dimensionality context from telemetry data into a single location where investigators can easily slice and dice to zoom in, zoom out, or follow breadcrumbs to find definitive answers. Engineers should be able to move through an investigation steadily and confidently, without the distraction of constant context switching. Further, by holding that context within one tool, implicit understandings that were often stitched together by experience and institutional knowledge instead become explicit data about your system. Observability allows critical knowledge to move out of the minds of the most experienced engineers and into a shared reality that can be explored by any engineer as needed. You will see how to unlock these benefits as we explore more detailed features of observability tools throughout this book.

## Conclusion

The monitoring-based debugging methods of using metrics and dashboards in tandem with expert knowledge to triage the source of issues in production is a prevalent practice in the software industry. In the previous era of elementary application architectures with limited data collection, an investigative practice that relies on the experience and intuition of humans to detect system issues made sense, given the simplicity of legacy systems. However, the complexity and scale of the systems underlying modern applications has quickly made that approach untenable.

Observability-based debugging methods offer a different approach. They are designed to enable engineers to investigate any system, no matter how complex, without leaning on experience or intimate system knowledge to generate a hunch. With observability tools, engineers can approach the investigation of any problem methodically and objectively. By interrogating their systems in an open-ended manner, engineers practicing observability can find the source of deeply hidden problems and confidently diagnose issues in production, regardless of their prior exposure to any given system.

Next, let’s look at a concrete experience that ties these concepts together by looking at past lessons learned from scaling an application without observability.
