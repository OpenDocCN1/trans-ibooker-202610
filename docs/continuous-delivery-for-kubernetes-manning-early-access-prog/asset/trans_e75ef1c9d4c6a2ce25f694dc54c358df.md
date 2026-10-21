

保持服务彼此独立，允许团队独立地对其进行演进。

你可以在自己的 Kubernetes 集群中使用 Knative Eventing 运行此场景，一步一步的教程说明如何实现可在找到这里：https://github.com/salaboy/from-monolith-to-k8s/blob/master/knative/knative-eventing-example.md

请注意，该示例可以使用内存型 Knative Broker 实现、Kafka 或 RabbitMQ Broker 进行安装，这使你能够根据需求测试不同的技术栈。

## USE CASE STEP BY STEP

如果你按照教程进行，会议平台将扩展新的功能，允许用户在平台管理员将门票设为可售时为会议活动购买门票。以下图表显示了在应用程序中为售票实现的新行为。

![](img/2ab98a5ff1ec073700d902ba95287fd6_172_0.png)

图 6.19 每位客户购买门票的步骤

正如你所见，这是一个非常简单的流程，遵循在线购买门票时非常知名且熟悉的流程，其中可能有大量人同时尝试购买。因此你面临的第一个步骤是一个队列。

以下截图显示了相同的流程，但实现为每个客户购买门票所需经过的一系列页面。

![](img/2ab98a5ff1ec073700d902ba95287fd6_173_0.png)

图 6.20 会议平台门票页面

所有售票功能都在新的门票部分下，可以在会议平台顶部菜单中找到。

![](img/2ab98a5ff1ec073700d902ba95287fd6_173_1.png)

图 6.21 会议平台门票后台选项

还有一个后台部分可用于模拟其他人排队并使门票对客户可用。

![](img/2ab98a5ff1ec073700d902ba95287fd6_174_0.png)

图 6.22 会议平台排队购买门票

作为客户，你需要等待轮到自己才能继续。如果刷新页面或转到不同部分，你需要重新加入队列，将你的位置留给队列中的下一个人。

![](img/2ab98a5ff1ec073700d902ba95287fd6_174_1.png)

图 6.23 在你完成付款前保留门票

一旦你离开队列，你将能够预订你感兴趣的门票类型和数量。此时，平台会为你保留门票，直到你

完成付款。如果花费太多时间或放弃页面，你预订的门票将被自动释放，允许其他客户购买它们。

图 6.24 使用信用卡为你的门票付款

预订门票后，你需要遵循非常标准的流程为其付款。在此案例中，模拟了信用卡付款，并与银行进行第三方/外部交互以授权付款。

图 6.25 付款将由外部实体处理，例如你的银行

一旦你的付款获得授权，平台将为你提供会议门票。

以下部分涵盖与实现此功能相关的一些挑战以及你需要使所有服务对正确事件做出反应的 Knative Eventing 资源。

## CHALLENGES & SOLUTIONS WHEN BUILDING EVENT-DRIVEN APPLICATIONS

- 用户界面交互和通过 WebSockets 路由 CloudEvents，这需要在 API 网关中完成，如何.. 暴露端点并打开 websocket 连接
- 接线发生在 Knative Eventing Triggers 中，需要过滤器以确保每个服务接收到它们感兴趣的事件
- 处理需求，扩展事件处理器
- 与不产生事件的第三方或遗留系统集成，构建桥梁
- 使用事件来监控或增强现有系统/服务

## 6.4 Summary

- Kubernetes 内置机制不考虑事件消费者、生产者或路由器。
- 事件驱动架构允许你构建更具弹性且解耦的服务，这些服务可以依赖消息队列提供的消息传递保证
- 借助 CloudEvents 规范（http://cloudevents.io），使用不同技术栈构建事件驱动应用程序是可能的，该规范为我们在不同系统之间共享事件提供标准格式。CloudEvents SDK 允许开发人员使用不同编程语言读取和写入 CloudEvents
- Knative Eventing 为 Kubernetes 带来事件生产者、消费者和路由器的概念。它允许开发人员构建其事件驱动服务和应用程序，而无需定义将用于路由事件的底层技术。流行的 broker（事件路由器）实现均受支持，例如用于开发的内存型、用于生产工作负载的 RabbitMQ 和 Kafka，以及受支持的云提供商特定托管服务，例如 Google Pub/Sub。

## 7

## Functions for Kubernetes

## This chapter covers

- 如何通过使用函数即服务（faas）方法提高开发人员生产力和体验
- 函数如何加快开发团队速度，同时提供简单的编程模型。我们将研究一个名为 Knative Functions（`func`）的项目。
- 如何利用 Kubernetes 的多语言特性，使用相同的工具、概念和标准在不同语言中编写函数以共享信息
- 如何连接多个函数以构建我们行走骨架的复杂场景

我们一直在讨论 Kubernetes 和可在 Kubernetes 之上构建应用程序时解决特定挑战的工具，但在本章中，我们将关注开发人员。为了使开发人员能够专注于编写业务所需的应用程序，我们需要确保他们拥有一种编程模型，该模型不会迫使他们学习事物如何在 Kubernetes 之上构建、容器化和部署。

在本章中，我们将研究一个名为 `func`（http://github.com/knative-sandbox/kn-plugin-func）的 Knative 工具，它使用我们在本书迄今为止学到的所有内容，提供简化的 Kubernetes-less 和 Dockerfile-less 多语言体验。`func` 通过提供开发人员体验来实现这一点，使开发人员能够专注于其应用程序/函数代码，而不是编写 YAML 或担心构建容器。

本章分为三个主要部分

- Function or Service? What’s the difference?
- 使用 `func` CLI 或 IDE 插件更接近开发人员
  - 让我们创建我们的第一个函数，预期内容和函数生命周期
- 为我们的会议活动主题演讲构建一个高度可扩展的游戏

让我们从第一个问题开始：Functions or Services？

## 7.1 Functions or Services? What’s the difference?

到目前为止，我们一直在谈论服务来描述我们在 Kubernetes 内部运行的应用程序。这些服务中的每一个通常映射到单个容器，可以通过同时运行同一容器的多个副本来扩展。我们还看到 Kubernetes 使用相同的命名法，因为 Kubernetes Service 资源帮助我们为运行的任意数量的容器副本提供单一入口点。

我们还看到了将 Kubernetes Ingress、Kubernetes Service、Kubernetes Deployment 以及容器镜像构建并准备好在容器注册表中，以便最终在用户或其他服务可以与其交互的环境中使服务运行所需的步骤数量。

Kubernetes 与流行的 Serverless 方案相比如何？你需要在 Kubernetes 和 AWS Lambdas 等 Serverless 平台之间做出选择吗？

在本节中，我们将讨论 Function as a Service（FaaS）平台以及它们与我们迄今为止介绍的方法的比较。我们将比较它们的优点和缺点与 Kubernetes 方法，并在第 7.2 节中了解如何在 Kubernetes 之上提供 FaaS 体验以获得两者兼得。

### 7.1.1 Functions, and Functions as a Service

Functions as a Service（FaaS）平台，也称为 Serverless 平台，旨在消除处理基础设施甚至构建容器的所有麻烦。在本章中，术语 Serverless 将与 FaaS 互换使用。Functions-as-a-Service 平台（FaaS）建立在函数是执行单一任务、有一个输入并产生输出的简单构造的想法之上。让我们从快速介绍函数是什么开始，以了解基于函数的平台 supposed to work 的工作原理。

## WHAT IS A FUNCTION?

函数是非常有范围的，通常是小型应用程序，负责执行单一任务。为了执行此任务，函数接收输入并创建可供另一个函数使用的输出。输入和输出通常以事件的形式出现。意味着函数对发生的事件做出反应，并在完成时产生事件。

