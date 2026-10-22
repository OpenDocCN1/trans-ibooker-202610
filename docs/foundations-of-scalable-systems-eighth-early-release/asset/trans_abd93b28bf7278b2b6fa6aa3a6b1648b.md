

应该可以从可用的无服务器监控工具中直接获得

现在，我将进入真正有趣的部分——结果。

## Results

表 8-3 显示了每种测试配置的平均吞吐量。吞吐量最高的 6178 请求/秒 由 (CPU80, max10) 配置提供。该值比默认设置 (CPU60, max10) 提供的吞吐量高出 1.7%，并且比最低吞吐量 5605 请求/秒 高出约 9%。因此，结果显示从最低到最高吞吐量大约有 10% 的变化。相同的代码。相同的请求负载。不同的配置参数。

表 8-3. 每个测试的平均吞吐量

| Throughput | max10 | max35 | max60 | max80 |
|---|---|---|---|---|
| CPU60 | 6006 | 6067 | 5860 | 5636 |
| CPU70 | 6064 | 6121 | 5993 | 5793 |
| CPU80 | 6178 | 5988 | 5989 | 5605 |

现在我将计入成本。在表 8-4 中，我已将每次测试运行的成本按默认 GAE 配置 {CPU60, max10} 的成本进行了归一化。例如，{CPU70, max10} 配置的成本比默认配置高出 18%，而 {CPU80, max80} 配置的成本比默认配置低 45%。

表 8-4. 每个测试条件的平均成本

| Normalized Instance Hours | max10 | max35 | max60 | max80 |
|---|---|---|---|---|
| CPU60 | 100% | 72% | 63% | 63% |
| CPU70 | 118% | 82% | 63% | 55% |
| CPU80 | 100% | 72% | 82% | 55% |

从这些结果中我们可以得出几个相当有趣的观察：

- 默认设置 {CPU60, max10} 既不是性能最高也不是成本最低。这种配置让 Google 高兴，但也许对你的客户来说不一定如此。
- 我们使用 {CPU80, max10} 配置在与默认配置相同的成本下获得了 3% 更高性能。
- 与默认配置设置相比，使用 {CPU70, max35} 配置，我们获得了略微（大约 2%）更高的性能，同时成本降低了 18%。
- 使用 {CPU70, max80} 测试配置，我们获得了默认配置性能的 96%，成本为 55%。这对于略低的吞吐量来说是一个相当不错的成本节约。

有了这些信息，你可以选择最能平衡成本和性能需求的配置设置。对于多个相互依赖的配置参数，你不太可能通过直觉和专业知识找到“最佳”设置。存在太多相互交织的因素，无法实现这一点。参数研究让你能够快速、严格地探索一系列参数设置。使用两个或三个参数，每个参数有三个或四个值，你可以快速、廉价地探索参数空间。这使你能够看到值组合的效果，并就如何部署你的应用程序做出明智的决策。

## Summary and Further Reading

无服务器平台是构建可扩展应用程序的强大工具。它们消除了与管理更新显式分配的虚拟机集群相关的许多部署复杂性。部署与开发服务的代码并将其连同配置文件一起上传到平台一样简单。你使用的无服务器平台会处理其余工作。

至少在理论上是这样。

在实践中，当然，有一些重要的旋钮和控件可供你使用，以调整底层无服务器平台管理你的函数的方式。这些都是特定于平台的，但许多与性能和可扩展性有关，最终影响你支付的金额。本章中的案例研究说明了这种关系，并为你提供了一种方法，你可以利用它来找到那个难以捉摸的“最佳点”，该点在成本低于默认平台参数设置提供的情况下提供所需的性能。

利用无服务器计算的好处需要你购买云服务提供商。有很多可供选择，但所有选择都伴随着供应商锁定以及如果你决定迁移到新平台时随之而来的下游痛苦和折磨。

有开源无服务器平台，如 Apache OpenWhisk (https://openwhisk.apache.org/)，可以部署到本地硬件或云提供的虚拟资源上。还有诸如 Serverless Framework (https://www.serverless.com/) 之类的解决方案，它们是提供商无关的。这使得将编写为无服务器的应用程序部署到多个主流云提供商成为可能，包括所有通常的嫌疑人。这提供了代码可移植性，但并不能使系统免受不同提供商部署环境复杂性的影响。不可避免地，在新平台上实现所需的性能、可扩展性和安全性不会是一件轻而易举的事。

关于无服务器计算的一个很好的信息来源是 Jason Katzer 的 *Learning Serverless* (2020, O’Reilly Media)。我还推荐两篇极其有趣的文章，讨论了无服务器计算的当前技术水平和未来可能性。这些是：

- D. Taibi, J. Spillner and K. Wawruch, “Serverless Computing-Where Are We Now, and Where Are We Heading?,” in IEEE Software, vol. 38, no. 1, pp. 25-31, Jan.-Feb. 2021, doi: 10.1109/MS.2020.3028708.
- J. Schleier-Smith et al, 2021. What serverless computing is and should become: the next phase of cloud computing. Commun. ACM 64, 5 (May 2021), 76–84.

最后，无服务器平台是实现 *microservices architectures* 的常见技术。微服务是一种将应用程序分解为多个独立部署和可扩展的“部分”的架构模式。这种设计方法非常适合无服务器实现，而且方便的是，它们是我们下一章要介绍的主题。

1 可扩展服务的简要概述可以在此链接找到 https://medium.com/@i.gorton/six-rules-of-thumb-for-scaling-software-architectures-a831960414f9
2 云账单的调查是一篇有趣的阅读 - https://www.prnewswire.com/news-releases/survey-enterprises-overspending-by-millions-on-cloud-bills-300968100.html
3 意外的云账单示例见 https://www.infotech.com/software-reviews/research/unexpected-cloud-costs-are-rocking-the-enterprise
4 此链接详细说明了受支持的运行时环境 - https://cloud.google.com/appengine/docs/the-appengine-environments
5 GAE 语言比较说明了加载时间 - 见 https://medium.com/dev-genius/scalability-and-cost-analysis-for-cloud-based-software-systems-part-1-472012435b26
6 请参阅此链接获取文档 - https://cloud.google.com/appengine/docs/standard/java/config/appref#automatic_scaling_max_concurrent_requests
7 还有一个可选的 min-pending-latency 参数，默认值为零。如果你够勇敢，最小值和最大值如何协同工作在这里有解释。 https://cloud.google.com/appengine/docs/standard/python/config/appref#scaling_elements
8 截至 2021 年，Lambda 还支持使用 Docker 容器构建的服务。这使开发人员在创建容器镜像时可以选择语言运行时。
9 关于冷启动行为的良好描述见此链接 https://medium.com/hackernoon/im-afraid-you-re-thinking-about-aws-lambda-cold-starts-all-wrong-7d907f278a4f
10 此实验描述了空闲函数在冷启动之前保留多长时间 - https://acloudguru.com/blog/engineering/how-long-does-aws-lambda-keep-your-idle-functions-around-before-a-cold-start?utm_source=medium_blog&utm_medium=redirect&utm_campaign=medium_blog
11 https://aws.amazon.com/lambda/pricing/
12 “在 1,769 MB 时，一个函数相当于一个 vCPU（每秒一个 vCPU-秒的积分）。” 来自 https://docs.aws.amazon.com/lambda/latest/dg/configuration-function-common.html
13 已建立的客户可以与 AWS 谈判以提高这些限制。
14 或者，如果 lambda 使用跨不同应用程序，可以将其分离到不同的账户。AWS 账户设计和使用超出了本书的范围。
15 实际上，该函数的最大预留并发量为 (Burst Limit -100)。AWS 为所有未关联显式并发限制的函数保留 100 个并发实例。这确保所有函数都有访问一些备用容量以执行。

