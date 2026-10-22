

在 Kubernetes 上，从勉强接受将数据基础设施用于开发和测试工作负载，到热情地倡导在生产环境中将数据库部署到 K8s 上。这段旅程是数据在 Kubernetes 社区中许多人的典型经历。到 2020 年中，Boris Kurktchiev 在他的文章《3 Reasons to Bring Stateful Applications to Kubernetes》中能够引用一个日益增长的共识，即在 Kubernetes 上管理有状态工作负载已达到可行性，甚至成熟的程度。

这种变化是如何发生的？在过去几年里，Kubernetes 社区已将重点转向添加支持以云原生方式在 Kubernetes 上管理状态的功能。我们在上一章中介绍的存储元素代表了这一转变的重要部分，包括 Kubernetes PersistentVolume 子系统以及对容器存储接口的采用。在本章中，我们将通过查看用于在此存储基础之上构建有状态应用程序的 Kubernetes 资源来完成这个故事的一部分。我们将特别关注一种特定的有状态应用程序：数据基础设施。

## The Hard Way

短语“doing it the hard way”已与避免简单选项而选择投入详细工作以实现具有持久意义的结果相关联。纵观历史，各种派别的先驱者以自豪地做出牺牲——流血、流汗、流泪——使后代的生活变得稍微更易忍受而闻名。这些前辈经常抱怨他们的门徒未能理解他们所经历的深度。

在技术世界中也一样。虽然 API 和“无代码”环境等新创新具有在全球范围内培养新一代开发者的巨大潜力，但要管理在全球范围内高度可用和安全的系统，仍然需要对底层技术有更深入的理解。当事情出错时，这种详细的知识就证明了其价值。这就是为什么我们许多人作为软件开发人员，在日常工作中从不接触物理服务器，却能从亲手焊接芯片和电路板组装自己的 PC 中获得如此多的收获。这也是我们充当朋友和家人非正式 IT 顾问时隐藏的好处之一。

对于 Kubernetes 社区来说，“the hard way”当然有更具体的含义。Google 工程师 Kelsey Hightower 的 [Kubernetes the Hard Way](https://) 已成为那些想要更深入了解构成 Kubernetes 集群元素的要素的人的一种入门仪式。这个流行的教程引导你下载、安装和配置构成 Kubernetes 控制平面的每个组件。结果是一个可工作的 Kubernetes 集群，虽然不适合部署生产工作负载，但对于开发和学习来说是完全可用的。这种方法的吸引力在于所有的指令都是手工输入的，而不是下载一堆为你完成一切的脚本，这样你就能理解每一步发生了什么。

在本章中，我们将效仿这种方法，自己一步步部署一些示例数据基础设施的硬方式。在此过程中，我们将更深入地实践你在第 2 章中学到的存储资源，并介绍用于管理计算和网络的额外 Kubernetes 资源类型，以完成我们在第 1 章中介绍的“计算、网络、存储”三元组。你准备好动手了吗？让我们开始吧！

## EXAMPLES ARE NOT PRODUCTION-GRADE

本章中我们提供的示例主要用于介绍 Kubernetes API 的新元素，并不打算代表我们在生产环境中推荐运行的部署。我们会确保突出显示其中的差距，以便在后续章节中演示如何填补它们。

## Prerequisites for running data infrastructure on Kubernetes

要跟随本章中的示例进行操作，你需要有一个可用的 Kubernetes 集群。如果你从未尝试过，也许你想使用 Kubernetes the Hard Way 指令构建一个集群，然后在同一个集群上以硬方式添加数据基础设施。你也可以使用简单的桌面 K8s，因为我们不会使用大量资源。如果你使用的是共享集群，你可能希望在自己的命名空间中安装这些示例，以将其与他人的工作隔离开来。

```
kubectl config set-context --current --namespace=<insert-namespace-name-here>
```

你还需要确保你的集群中有一个 StorageClass。如果你是从硬方式构建的集群开始的，你将没有一个。你可能希望按照 StorageClasses 部分中的说明安装一个简单的 StorageClass 和 provisioner 来公开本地存储（源代码）。

你希望使用支持 volumeBindingMode 为 WaitForFirstConsumer 的 StorageClass。这给了 Kubernetes 在我们需要时再延迟配置存储的灵活性。这种行为通常更受生产部署的青睐，所以你最好现在就养成习惯。

## Running MySQL on Kubernetes

首先，让我们从一个超级简单的示例开始。MySQL 由于其可靠性和易用性，是最广泛使用的关系型数据库之一。对于此示例，我们将在官方 Kubernetes 文档中的 MySQL 教程的基础上进行构建，并加入一些变化。你可以在 Deploying MySQL Example - Data on Kubernetes the Hard Way 找到本节使用的源代码。该教程包括两个 Kubernetes 部署：一个用于运行

MySQL pod，另一个用于运行示例客户端，在本例中为 Wordpress。此配置如图 3-1 所示。

![](img/cb55ccac39ceccbc2806f8db5c8fad69_78_0.png)

Figure 3-1. Sample Kubernetes Deployment of MySQL

在此示例中，我们看到每个 pod 都有一个 PersistentVolumeClaim。为了本示例的目的，我们假设这些声明由默认 StorageClass 提供的单个卷满足。你还会注意到每个 pod 都显示为 ReplicaSet 的一部分，并且为 MySQL 数据库公开了一个服务。让我们暂停一下并介绍这些概念。

## ReplicaSets

在 Kubernetes 上，生产应用程序部署通常不会部署单个 pod，因为当节点消失时，单个 pod 很容易丢失。相反，pod 通常在管理其生命周期的 Kubernetes 资源上下文中部署。ReplicaSet 是这些资源之一，另一个是 StatefulSet，我们将在本章后面讨论。

ReplicaSet（RS）的目的是确保在任何给定时间保持指定数量的给定 pod 副本运行。随着 pod 被销毁，其他 pod 会被创建以替换它们，以便满足所需的副本数。ReplicaSet 由 pod 模板、副本数和选择器定义。pod 模板定义了将由 ReplicaSet 管理的 pod 的规范，类似于我们在第 2 章示例中为单个 pod 创建时看到的。副本数可以为 0 或更多。选择器标识 ReplicaSet 的一部分的 pod。

让我们看一下图 3-1 中所示的 Wordpress 应用程序的 ReplicaSet 定义的一部分：

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  replicas: 1
  selector:
    matchLabels:
      app: wordpress
      tier: mysql
  template:
    metadata:
      labels:
        app: wordpress
        tier: mysql
    spec:
      containers:
      - image: mysql:5.6
        name: mysql
        ...
```

ReplicaSet 负责创建或删除 pod 以满足指定的副本数。你可以通过更改此值来放大或缩小 RS 的大小。pod 模板在创建新 pod 时使用。由 ReplicaSet 管理的 pod 在其 metadata.ownerReferences 字段中包含对 RS 的引用。ReplicaSet 实际上可以采用

