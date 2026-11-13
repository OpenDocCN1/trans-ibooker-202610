

2. 单击“Update”（更新），然后单击“Next step”（下一步），直到进入“Set Auto Scaling (optional)”（设置自动扩缩容（可选））屏幕，如下截图所示：

![](assets/05b8dc7d-0c16-4bb3-ab31-03a81bae4fa5.png)

3. 现在，单击第二个选项“Configure Service Auto Scaling”（配置服务自动扩缩容）以调整服务的期望任务数，并填写详细信息。您可以选择想要运行的任务的**最小数量**，该值必须大于或等于 0。您需要指定**期望任务数**和**最大任务数**。期望任务数是默认运行的数量。所有这些均可在下面的截图中看到：

![](assets/9cae55e3-a0b8-477e-aefc-72ca4cf3e148.png)

最小任务数始终要设置为大于 0 的值。

4. 现在，我们需要添加扩缩容策略。单击“Add scaling policy”（添加扩缩容策略）并填写详细信息，类似于下图所示：

![](assets/8d701a44-ac6c-4c97-990a-f87dc9f74cff.png)

看，我们之前在“Monitoring”（监控）和“Scaling”（扩缩容）章节学到的内容，现在是如何被 AWS 采用的？AWS 使用平均 CPU 利用率来进行扩容或缩容。此外，AWS 还提供了一些值，可以使该过程更平滑或更快。您可以设置扩容或缩容的冷却期。当您知道需要快速扩容，但希望在需要时较慢地缩容时，这非常有用。您可以根据需要发挥创意，设置尽可能复杂的扩缩容策略。

5. 单击“Save”（保存），然后单击“Update Service”（更新服务）。几秒钟后，您应该会看到以下结果：

![](assets/07b347f9-5de4-438e-9689-6b4afcb3ff2a.png)

6. 自动扩缩容现已设置完成。如果您愿意，现在可以使用 [https://loader.io/](https://loader.io/) 等工具对服务进行压力测试。对于重要的项目和服务，测试扩缩容以了解其运行方式是很有必要的。可能需要针对 CPU 和内存进行一些微调，但您的应用程序可以妥善及时地响应不断增长的流量。

测试服务器时，别忘了检查并处理数据库服务器。根据我的经验，数据库服务器往往比前端/API 服务器更容易成为瓶颈。

# 使用 Google Cloud 进行扩缩容和监控

Google Cloud 中的扩缩容在概念上与 AWS 相似。您拥有负载均衡器、实例以及可以对实例进行扩容或缩容的策略。就像 AWS 一样，Google Cloud 也提供了不止一种管理服务和自动扩缩容的方式：Kubernetes 可以直接在 Google Cloud 中进行管理（就像在 AWS `Elastic Kubernetes Service` (`EKS`) 中一样；详见[第 14 章](#e8c07878-f14b-46a3-8101-c541eef12c87.xhtml)“Docker 和云”）。

不过，假设我们像在[第 15 章](https://cdp.packtpub.com/hands_on_microservices_with_swift_5/wp-admin/post.php?post=37&action=edit)“在云中部署微服务”中那样，通过 App Engine 设置了我们的应用。在这种情况下，自动扩缩容默认已启用！我们无需做任何额外操作。Google 会自动检查 CPU、RAM 和其他指标，并根据您的需求进行扩容或缩容。

然而，如果您拥有更复杂的设置，即您自己设置了一些实例，则可以在设置托管实例组时配置自动扩缩容。

下面的截图显示了设置新托管实例组时的配置。请注意，您可以在此处添加自动扩缩容策略——这与 AWS 非常相似，如前所示：

![](assets/881bddc8-3158-4321-af16-276f35db710f.png)

就这样；App Engine 现在会为您完成其余工作。请注意，Google Cloud 和 AWS 之间的设置在原则上是如何相同的？原因在于，扩缩容在不同的提供商和解决方案中已经变得普遍相似。大家寻找的扩容或缩容指标都是相同的。

既然我们已经简要了解了 Google Cloud，接下来让我们看看 DigitalOcean 是如何做到这一点的。

# 使用 DigitalOcean 进行扩缩容和监控

当您使用 DigitalOcean 时，目前只有一种自动扩缩容选项：使用其托管 Kubernetes 服务。您需要先使用 Kubernetes 设置应用程序。Kubernetes 的使用和配置超出了本书的范围，但如果您确实在 DigitalOcean 上运行着 Kubernetes 设置，则可以轻松通过“Nodes”（节点）选项卡设置自动扩缩容，如下截图所示：

![](assets/9f61f769-79c6-4464-82b5-421adb793030.png)

当您单击上一个截图中看到的“Resize”（调整大小）或“Autoscale”（自动扩缩容）时，可以选择以下选项：

![](assets/ccc5ef13-d51f-43eb-a4ca-81371de3d910.png)

您可以指定想要作为最大值和最小值的 pod 数量（如果需要回顾什么是 pod，请再次查看[第 14 章](#e8c07878-f14b-46a3-8101-c541eef12c87.xhtml)“Docker 和云”）（最少一个）。

DigitalOcean 在成本和价格方面最为透明。对于预算敏感型项目，DigitalOcean 提供了清晰的成本预期。

Pod 本身的扩缩容是通过 Kubernetes `Horizontal Pod Autoscaler` (`HPA`) 完成的。您可以在官方文档 ([https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)) 中找到其工作原理的所有详细信息。其原则与我们之前讨论的相同。

DigitalOcean 方便地隐藏了通常伴随 Kubernetes 使用的大部分配置，如果您只是想开始使用 Kubernetes，这是一个简单的选择。

DigitalOcean 直到最近才开始实施任何自动扩缩容功能。直到 2019 年 9 月，Kubernetes 才成为一个选项。

我们现在已经了解了 DigitalOcean 及其自动扩缩容的实现方式。接下来，让我们在下一节中简要了解一下其他几个云提供商。

# 其他云提供商

除了 AWS、Google Cloud 和 DigitalOcean，在云提供商方面还有其他一些选择。让我们在这里看看它们：

- Heroku
- IBM Cloud
- Microsoft Azure
- Oracle Cloud

让我们从 Heroku 开始。

## Heroku

Heroku 与其他提到的提供商不同，因为它不提供自己的解决方案。其所有服务都使用 AWS `Elastic Compute Cloud` (`EC2`) 平台。

它最初作为一项专门面向开发人员的服务而推出，旨在减轻部署和运行应用程序的负担。Heroku 尚未正式支持服务端 Swift；但是，有几种方法可以在 Heroku 上运行 Vapor（参见 [https://grapeup.com/blog/read/server-side-swift-with-vapor-end-to-end-application-with-heroku-cloud-46](https://grapeup.com/blog/read/server-side-swift-with-vapor-end-to-end-application-with-heroku-cloud-46)）。

在扩缩容方面，Heroku 试图做到针对特定应用。它不像 AWS ECS 那样提供通用的自动扩缩容。因为它最初只支持 Ruby 项目，其工具和方法仍深受其影响。其关于自动扩缩容的建议之一是使用 Rails Autoscale，这仅适用于 Ruby on Rails。

总之，Heroku 是开发用途的一个不错选择。然而，任何生产应用都可能遇到问题，因为其自动扩缩容远不如其他提供商那样可定制。

## IBM Cloud

IBM Cloud 与 AWS 和 Google Cloud 高度可比。它提供大致相同的功能，且价格具有竞争力。对于中小企业和个人而言，IBM 进入云业务相对较新。它为企业公司运行基础设施已有数十年，但只是最近才开始向更广泛的受众提供这项服务。其自动扩缩容服务像其他提供商一样包含 Kubernetes，还有一种主要面向企业客户的更定制化的服务（参见 [https://www.ibm.com/cloud/auto-scaling](https://www.ibm.com/cloud/auto-scaling)）。

