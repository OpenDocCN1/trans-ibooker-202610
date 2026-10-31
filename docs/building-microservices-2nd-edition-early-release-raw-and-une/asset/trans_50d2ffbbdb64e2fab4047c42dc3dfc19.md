
# 微服务耦合模式

## 跳板耦合

![](Images/tramp_coupling_shipping_direct.png)

对于这个具体示例，我可能会考虑一个更简单（尽管更细微）的变更，即完全隐藏 `Order Processor` 对 `Shipping Manifest` 的需求。将管理库存和安排包裹派送的工作都委托给我们的 `Warehouse` 服务的想法是有意义的，但我们不喜欢的是我们泄露了一些低层实现，即 `Shipping` 微服务需要一个 `Shipping Manifest`。隐藏这一细节的一种方法是让 `Warehouse` 在其契约中接收所需的信息，然后在本地构建 `Shipping Manifest`。现在，这意味着如果 `Shipping` 服务更改其服务契约，只要所需数据由 `Warehouse` 收集，那么从 `Order Processor` 的角度来看，这一变更将是不可见的。

![](Images/tramp_coupling_hiding.png)

虽然这有助于保护 `Warehouse` 微服务免受 `Shipping` 的某些变更影响，但仍有一些情况需要所有各方进行变更。让我们考虑我们想要开始国际运输的想法。作为其中的一部分，`Shipping` 服务需要在 `Shipping Manifest` 中包含一个 `Customs Declaration`。如果这是一个可选参数，那么我们可以毫无问题地部署 `Shipping` 微服务的新版本。但如果这是一个必需参数，则 `Warehouse` 需要创建它。它可能能够利用其已有的（或被提供的）信息来完成此操作，但如果需要额外信息，则可能需要 `Order Processor` 向其传递额外信息。

尽管在这种情况下我们并未消除需要在全部三个微服务中进行变更的需求，但我们获得了关于何时以及如何进行这些变更的更大控制权。如果我们采用初始示例中那种紧密的（直通）耦合，添加这个新的必需 `Customs Declaration` 可能需要对全部三个微服务进行同步发布。至少通过隐藏这一细节，我们可以更轻松地分阶段部署。

一种最终的方法可以帮助减少直通耦合，即让 `Order Processor` 仍然通过 `Warehouse` 将 `Shipping Manifest` 发送给 `Shipping` 微服务，但让 `Warehouse` 完全不知道 `Shipping Manifest` 本身的结构。`Order Processor` 将清单作为订单请求的一部分发送，但 `Warehouse` 并不会尝试查看或处理该字段——它只是将其视为数据块而不关心其内容。它只是原样转发。`Shipping Manifest` 格式的变更仍然需要对 `Order Processor` 和 `Shipping` 微服务进行变更，但由于 `Warehouse` 不关心清单本身实际包含的内容，因此无需变更。

## 共同耦合

共同耦合发生在两个或多个微服务使用同一组数据时。这种耦合形式的一个简单且常见的例子是多个微服务使用同一个共享数据库，但这也可能表现为使用共享内存或共享文件系统。

共同耦合的主要问题是数据结构的变更可能同时影响多个微服务。请考虑 [图 2-7](#ch02.xhtml#common_coupling_reference_data) 中 MusicCorp 的一些服务的示例。正如我们之前讨论的，MusicCorp 在全球运营，因此需要有关其运营所在国家的各种信息。在这里，多个服务都在从共享数据库读取静态参考数据。如果该数据库的模式以向后不兼容的方式更改，则需要对数据库的每个消费者进行更改。实际上，由于这个原因，像这样的共享数据往往很难更改。

![](Images/common_coupling_reference_data.png)

[图 2-7](#ch02.xhtml#common_coupling_reference_data) 中的示例相对来说相当温和。这是由于静态参考数据本质上并不经常更改，而且该数据也是只读的——因此我倾向于对这种方式共享静态参考数据感到宽松。然而，如果公共数据的结构更改得更频繁，或者多个微服务对同一数据进行读写，则共同耦合会变得更成问题。

[图 2-8](#ch02.xhtml#common_coupling_order_update1) 向我们展示了一种情况，其中 `Order Processor` 和 `Warehouse` 服务都在从共享的 `Order` 表中读写，以帮助管理向 MusicCorp 客户派送 CD 的流程。两个微服务都在更新 `STATUS` 列。`Order Processor` 可以设置 `PLACED`、`PAID` 和 `COMPLETED` 状态，而 `Warehouse` 将应用 `PICKING` 或 `SHIPPED` 状态。

![](Images/common_coupling_order_update.png)

尽管你可能会认为 [图 2-8](#ch02.xhtml#common_coupling_order_update1) 是一个有些人为的例子，但这个简单直接的共同耦合示例有助于说明一个核心问题。从概念上讲，我们有 `Order Processor` 和 `Warehouse` 微服务管理订单生命周期的不同方面。在对 `Order Processor` 进行更改时，我能否确定我不会以破坏 `Warehouse` 对世界的看法的方式更改订单数据，反之亦然？

确保某物的状态以正确方式更改的一种方法是创建有限状态机。状态机可用于管理某个实体从一个状态到另一个状态的转换，确保禁止无效的状态转换。在 [图 2-9](#ch02.xhtml#state_machine_example) 中，可查看 MusicCorp 中订单的状态允许转换。订单可以从 `PLACED` 转到 `PAID`，但不能直接从 `PLACED` 转到 `PICKING`（这个状态机可能不足以满足完整端到端购买和货物运输所涉及的现实业务流程，但我想给出一个简单的示例来说明这个概念）。

![](Images/state_machine_example.png)

在这个特定示例中的问题是 `Warehouse` 和 `Order Processor` 都共享管理此状态机的责任。我们如何确保它们对允许的转换达成一致？有方法可以在微服务边界之间管理此类流程，我们将在讨论 Sagas 时在 [链接待定] 中再次回到这个话题。

这里的一个潜在解决方案是确保单个微服务管理订单状态。在 [图 2-10](#ch02.xhtml#common_coupling_order_service) 中，`Warehouse` 或 `Order Processor` 都可以向 `Order` 服务发送状态更新请求。在这里，`Order` 微服务是任何给定订单的事实来源。在这种情况下，真正重要的是我们将来自 `Warehouse` 和 `Order Processor` 的请求视为请求——*请求*。在这种情况下，`Order` 服务的工作是管理与订单聚合相关的可接受状态转换。因此，如果它收到来自 `Order Processor` 的将状态从 `PLACED` 直接更改为 `COMPLETED` 的请求，如果该更改无效，它可以自由拒绝该请求。

### 提示

确保你将发送到微服务的请求视为下游微服务在无效时可以拒绝的东西。
