
^([7](#ch06.xhtml#idm46534869951576-marker)) [*https://help.github.com/en/github/creating-cloning-and-archiving-repositories/about-code-owners*](https://help.github.com/en/github/creating-cloning-and-archiving-repositories/about-code-owners)

^([8](#ch06.xhtml#idm46534869942104-marker)) [*https://cacm.acm.org/magazines/2016/7/204032-why-google-stores-billions-of-lines-of-code-in-a-single-repository/fulltext*](https://cacm.acm.org/magazines/2016/7/204032-why-google-stores-billions-of-lines-of-code-in-a-single-repository/fulltext)

^([9](#ch06.xhtml#idm46534869939336-marker)) 参见 Git 虚拟文件系统设计历史 [*https://docs.microsoft.com/en-us/azure/devops/learn/git/gvfs-design-history*](https://docs.microsoft.com/en-us/azure/devops/learn/git/gvfs-design-history)

^([10](#ch06.xhtml#idm46534869935832-marker)) [*https://medium.com/@maoberlehner/monorepos-in-the-wild-33c6eb246cb9*](https://medium.com/@maoberlehner/monorepos-in-the-wild-33c6eb246cb9)

^([11](#ch06.xhtml#idm46534869934248-marker)) [*https://lerna.js.org/*](https://lerna.js.org/)

# 第 7 章 部署

部署单体应用程序是一个相当直接的过程。微服务由于其相互依赖性以及丰富的技术选项，完全是另一回事。当我撰写本书第一版时，本章已有很多内容可谈论你可用的各种选择。自那以后，Kubernetes 逐渐成为主流，而 Function As A Service 平台（FAAS）为我们提供了更多思考如何真正交付软件的方式。

尽管过去十年技术可能已经改变，但我认为与构建软件相关的许多核心原则并未改变。事实上，我认为确保我们彻底理解这些基础思想更为重要，因为它们可以帮助我们理解如何在这种新技术的混乱环境中导航。考虑到这一点，本章将确保突出一些与部署相关的核心原则，这些原则值得牢记，同时也展示你可用的不同工具在将这些原则付诸实践方面可能如何提供帮助（或造成阻碍）。

不过首先，让我们稍微掀开幕布，看看当我们从系统架构的逻辑视图转向真实的物理部署拓扑时会发生什么。

## 从逻辑到物理

到目前为止，当我们谈论微服务时，我们是以逻辑意义而非物理意义谈论它们的。我们可以谈论我们的 `Invoice` 微服务如何与 `Order` 微服务通信，如 [图 7-1](#ch07.xhtml#logical-view) 所示，而无需实际查看这些服务部署的物理拓扑。架构的逻辑视图通常会抽象掉底层物理部署问题——在本章范围内，这一概念需要改变。

![](Images/logical-view.png)

这种对微服务的逻辑视图在实际在真实基础设施上运行它们时可能会隐藏大量复杂性。让我们看看此类图表可能会隐藏哪些细节。

### 多个实例

当我们思考这两个微服务的部署拓扑时，其并不像一件事与另一件事通信那样简单。首先，似乎很可能我们会有每个服务的多个实例。

拥有服务的多个实例可以让你处理更多负载，并且可以提高系统的健壮性，因为你可以更容易地容忍单个实例的故障。因此，我们可能有 `Invoice` 的一个或多个实例与 `Order` 的一个或多个实例通信。这些实例之间通信的确切处理方式将取决于通信机制的性质，但如果我们假设在这种情况下使用某种基于 HTTP 的 API，负载均衡器就足以处理将请求路由到不同实例，如 [图 7-2](#ch07.xhtml#multiple-instances) 所示。

![](Images/06-multiple-instances.png)

你想要的实例数量将取决于应用程序的性质——你需要评估所需的冗余度、预期负载水平等，以得出可行的数量。你可能还需要考虑这些实例将在何处运行。如果出于健壮性原因拥有服务的多个实例，你可能希望确保这些实例不会全部位于同一底层硬件上。进一步说，这可能要求你将不同的实例不仅分布在多台机器上，而且分布在不同的数据中心，以防止整个数据中心变得不可用。这可能导致像 [图 7-3](#ch07.xhtml#multiple-instances-multiple-dcs) 中那样的部署拓扑。

![](Images/06-multiple-instances-multiple-dcs.png)

这可能看起来过于谨慎——整个数据中心不可用的几率有多大？好吧，我无法对每种情况回答这个问题，但至少在处理主要云服务提供商时，这绝对是你必须考虑的事情。对于托管虚拟机之类的东西，AWS、Azure 或 Google 都不会为单台机器提供 SLA，也不会为单个可用区（对于这些提供商而言，这是数据中心的最接近等价物）提供 SLA。实际上，这意味着你部署的任何解决方案都应分布在多个可用区中。

### 数据库

进一步说，还有一个我们到目前为止忽略的重要组件——数据库。正如我已经讨论过的，我们希望微服务隐藏其内部状态管理，因此微服务用于管理其状态的任何数据库都被认为隐藏在微服务内部。

这导致了经常提到的格言“不要共享数据库”，我希望到现在为止其理由已经足够充分。

但当我们考虑到我有多个微服务实例这一事实时，这如何运作？每个微服务实例是否应该有自己的数据库？一句话，不。在大多数情况下，如果我访问我的 `Order` 服务的任何实例，我希望能够获取有关同一订单的信息。因此，我们需要在同一逻辑服务的不同实例之间实现一定程度的共享状态。这在 [图 7-4](#ch07.xhtml#multiple-instances-one-db) 中显示。

![](Images/06-multiple-instances-one-db.png)

但这是否违反了我们的“不要共享数据库”规则？并不真的。我们主要担心之一是，当在多个不同的微服务之间共享数据库时，与访问和操作该状态相关的逻辑现在分散在不同的微服务中。但在这里，数据是由不同实例的 **same** 微服务共享的。访问和操作状态的逻辑仍然保留在单个逻辑微服务内。

#### 数据库部署与扩展

与我们的微服务一样，到目前为止我们主要以逻辑意义谈论数据库。在上面的 [图 7-3](#ch07.xhtml#multiple-instances-multiple-dcs) 中，我们忽略了底层数据库的冗余或扩展需求的任何问题。我们还回避了你将使用的大多数数据库的一个重要概念——你可以在同一数据库基础设施上管理多个逻辑隔离的数据库。
