

这将部署一个名为 `help_pubsub` 的函数，其中消息会到达 `mytopic` Pub/Sub 主题。

> **重要提示**
> 你可能有兴趣查看一些更高级的触发器，例如使用 Firebase 身份验证。查看 https://cloud.google.com/functions/docs/calling/ 以获取每种可能触发器的示例。

在下一节中，我们将回顾可用于 Cloud Functions 的 IAM 角色。

## IAM

对 Google Cloud Functions 的访问通过 IAM 保护。让我们看一下预定义角色的列表，以及每个角色的简短描述：

- **Cloud Function 管理员**：有权创建、更新和删除函数。可以设置 IAM 策略并查看源代码。
- **Cloud Functions 开发人员**：有权创建、更新和删除函数，以及查看源代码。不能设置 IAM 策略。
- **Cloud Functions 查看者**：有权查看函数。不能获取 IAM 策略，也不能查看源代码。
- **Cloud Function 调用者**：有权使用其公共 URL 调用 HTTP 函数。

请注意，要使 Cloud Functions 开发人员角色生效，您还必须将用户分配给 Cloud Functions 运行时服务账号上的 IAM 服务账号用户角色。

## 配额和限制

Google Cloud Functions 带有预定义的配额。这些默认配额可以通过汉堡菜单通过 **IAM & Admin | Quotas** 进行更改。从此菜单中，我们可以查看当前配额并请求提高这些限制。我们建议您熟悉每个服务的限制，因为这会影响您的可扩展性。对于 Cloud Functions，我们应注意以下三种类型的配额：

- **资源限制**：定义函数可以消耗的总资源量
- **时间限制**：定义事物可以运行的时间长度
- **速率限制**：定义您可以调用 Cloud Functions API 的速率

值的列表相当广泛。如果您想查看详细列表，请查看 *进一步阅读* 部分。

## 定价

Cloud Functions 的价格由多个因素组成。这些包括 **调用** 次数、**计算时间** 和网络速率（**联网**）。这些在以下图表中显示：

![](img/de2c97ab017f36b3750a5bade111edd7_287_0.png)

图 9.16 – Cloud Functions 定价

请记住，有一个每月免费使用层，您可以在不产生任何成本的情况下试用。截至本书撰写之时，它包括 200 万次调用、100 万秒的计算时间和 5 GB 的出站网络流量。尽情享受吧！

## 总结

在本章中，我们讨论了 Cloud Functions 以及它完美适用的多个用例。我们讨论了两种类型的函数，即 HTTP 和后台函数，并且还了解到函数可以通过特定事件或 HTTP 请求执行。最后，我们查看了如何使用 Google Cloud 控制台和 gcloud 命令部署函数。对于考试来说，了解 Cloud Functions 的用例以及何时使用它们对我们有利非常重要。

通过本章，我们已完成了 Google Compute 选项。在下一章中，我们将查看网络。

## 进一步阅读

有关本章所涵盖主题的更多信息，请查看以下资源：

- **幕后的 Cloud Functions**：https://cloud.google.com/functions/docs/concepts/exec
- **Cloud Functions 和 VPC**：https://cloud.google.com/functions/docs/connecting-vpc
- **本地模拟器**：https://cloud.google.com/functions/docs/emulator
- **冷启动：** https://cloud.google.com/functions/docs/bestpractices/tips
- **配额：** https://cloud.google.com/functions/quotas
- **定价：** https://cloud.google.com/functions/pricing-summary/
- **gcloud 命令：** https://cloud.google.com/sdk/gcloud/reference/functions/deploy
- **Cloud Run：** https://cloud.google.com/run/

# 10
## Networking Options
in GCP

我们已经知道计算是最基本的云功能。但即使我们能够在不连接到资源的情况下进行计算，也不会产生价值。网络涵盖了许多类型的连接。我们需要能够连接到 Google Cloud 控制台和 Google Cloud Platform (GCP) API。我们可能希望通过虚拟专用网络（VPN）或通过高带宽互连将本地数据中心连接到 GCP。最后，我们可能需要对虚拟机（VM）实例进行负载均衡连接。GCP 网络将帮助我们完成所有这些工作。

在本章中，我们将介绍我们需要了解的所有理论，以便了解工作负载如何在内部和外部与 GCP 通信。我们还将查看所有基本网络服务。

我们将涵盖以下主题：

- 探索 GCP 网络
- 了解虚拟私有云
- 负载均衡
- **网络地址转换（NAT）**
- 混合连接
- DNS
- 防火墙规则
- 私有访问

## 探索 GCP 网络

Google 网络与其他云有所不同。为了了解 Google 在网络方面投入的巨大资金，我们应该查看以下地图（https://cloud.google.com/about/locations/#network-tab）。该地图显示了数十万条光纤电缆在 85 个区域和 146 个 **接入点（PoP）** 之间运行。请记住，此视图来自 2021 年 10 月，并且仍在增长中：

![](img/de2c97ab017f36b3750a5bade111edd7_290_0.png)

图 10.1 – 网络位置（来源：https://cloud.google.com/about/locations/#network-tab。许可：https://creativecommons.org/licenses/by/4.0/legalcode）

拥有如此庞大的网络基础设施使您能够在非常接近您的 **互联网服务提供商（ISP）** 的位置连接 GCP。事实上，GCP 提供两种网络服务层级，如下所示：

- **Premium**：Premium 层提供高性能路由。除此之外，它还提供全球负载均衡和 **内容分发网络（CDN）** 服务。它面向需要全球存在且以最佳用户体验为目标的用例。此层附带 **服务等级协议（SLA）**。
- **Standard：** Standard 层是性能较低且没有 SLA 的网络。CDN 服务不可用，负载均衡器为区域级。它面向以成本为主要决定因素的用例。暴露给用户的 GCP 网络基于名为 **Andromeda** 的 **软件定义网络（SDN）**。该平台是 GCP 中所有网络服务的编排点。幸运的是，这对用户来说是抽象的，无需了解 Andromeda 本身的工作原理。

> **考试提示**
> 有趣的是，网络并不是考试的关键主题；然而，有一个 GCP 认证网络工程师考试将彻底测试您的 GCP 网络知识。不了解网络基础知识就不可能理解其他 GCP 服务。因此，在本章中，您将了解 Google 的全球网络和 GCP 的网络服务。

## 了解虚拟私有云

GCP 网络的基本概念是 **虚拟私有云（VPC）**，也简单称为 **网络**。正如您在 *第 2 章，入门 Google Cloud Platform* 中所学，GCP 可以划分为项目，以逻辑上隔离 Google Cloud 资源。在一个项目中，您可以创建多个 VPC。

默认情况下，每个项目最多可以创建五个网络（配额可通过联系支持扩展）。多个 VPC 使能够在网络层面上分离 GCP 资源，例如 VM、容器等。VPC 具有 **全局** 范围，并且可以跨所有 GCP 区域。允许驻留在不同 VPC 中的 VM 之间进行连接，您有两个选择：您可以创建共享 VPC 或对等 VPC。我们将在本章后面详细了解每个选项。

此外，VPC 被划分为区域子网，也称为 **子网**，它们具有关联的 IP 范围，用于为资源分配地址。

当您创建新项目时，会为您创建一个默认网络。每个区域都会创建子网，并分配不重叠的 CIDR 块。

> **重要提示**
> **无类域间路由（CIDR）** 是一种 IP 地址方案，取代了传统的 A、B、C 类系统。它基于可变长度子网掩码。在 CIDR 中，子网的前缀可以定义为任意数字，使网络掩码长度更灵活。这意味着组织可以更有效地利用 IP 地址方案。

