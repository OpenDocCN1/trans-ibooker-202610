

3. 部署后，作业将通过 Cloud Scheduler 可见。在 Google Cloud Console 中，在 App Engine | Cron jobs 下，点击 GO TO CLOUD SCHEDULER 按钮：

![](img/de2c97ab017f36b3750a5bade111edd7_149_0.png)

图 5.21 – Cron jobs

点击 APP ENGINE CRON JOBS 选项卡：

![](img/de2c97ab017f36b3750a5bade111edd7_149_1.png)

图 5.22 – Cron job summary

至此，你新建的作业已创建完成，并将每 24 小时运行一次。请注意，如果你之前未使用过，可能需要启用 Cloud Scheduler API。

> Exam Tip
> 请记住，cron 作业是一种周期性触发作业的方式，是使用 Compute Engine 搭配原生第三方工具的替代方案。

## Memcache

缓存是加快我们访问频繁使用数据的常见方式。这包括会话数据、用户偏好设置，或任何不经常变化的其他查询。如果多个查询访问相同的数据，则是缓存的完美候选。Memcache 是内置于 App Engine 的服务。它允许你在内存中保存键值对，其访问速度远快于查询数据库。Memcache 有两种服务级别，如下所示：

- **Shared**：这是 Memcache 的默认且免费版本。请注意，该服务按尽力而为的方式提供缓存容量。资源与 App Engine 平台内的多个应用程序共享。
- **Dedicated**：这是 Memcache 的可选付费版本。它提供专用于你的应用程序的固定缓存容量。按每 GB/小时计费。这应用于需要可预测性能的应用程序。

你可以通过访问 **App Engine | Memcache** 菜单来添加要缓存的值：

![](img/de2c97ab017f36b3750a5bade111edd7_150_0.png)

图 5.23 – Memcache

> **Exam Tip**
> 确保你理解标准缓存是按尽力而为方式提供的，可能不适合所有用例。如果想要可预测的性能，请使用专用缓存。

接下来，我们将查看 App Engine 所需的 IAM 角色。

## IAM

访问 Google App Engine 由 IAM 保护。让我们看一下预定义角色的列表，以及每个角色的简短说明：

- **App Engine Admin**：有权读取、写入和修改所有应用程序配置和设置的访问权限。
- **App Engine Creator**：有权为项目创建 App Engine 资源。
- **App Engine Service Admin**：对应用程序配置和设置具有只读权限。它还允许写入服务和版本设置，但不允许我们部署应用程序版本。
- **App Engine Deployer**：提供对应用程序配置和设置的只读访问。它允许我们部署和创建新版本并删除旧版本。然而，我们无法修改现有版本的流量配置。
- **App Engine Viewer**：提供对应用程序配置和设置的只读访问。
- **App Engine Code Viewer**：提供对应用程序配置、设置和已部署代码的只读访问。

对于更粗粒度的访问，你也可以使用原始角色，如 **Owner**、**Editor** 和 **Viewer**。

### Quotas and limits

Google App Engine 带有预定义的配额。这些默认配额可以通过汉堡菜单，在 **IAM & Admin | Quotas** 下进行更改。从此菜单中，我们可以查看当前配额并请求提高这些限制。你应该了解每个服务的限制，因为这会影响你的可扩展性。

对于 App Engine，我们应注意以下配额：

- **Free quotas**：对于每个应用程序，你被允许免费使用一定数量的资源。此配额仅可对付费应用程序超出。
- **Spending limits**：此配额用于控制你的支出限制。它可以由 **project owner** 或 **billing administrator** 设置。
- **Safety limits**：此配额由 Google 设置以保护 App Engine。

## Pricing

App Engine 的价格由多个因素组成，且标准环境和灵活环境不同。对于标准环境实例，手动和基础扩缩服务按运行时间按小时计费。对于灵活环境，应用程序部署在虚拟机实例中，按每秒计费，最低使用时间为 1 分钟。

除此之外，你还需要包含应用程序使用的额外资源的成本。要获取最新价格，请查看以下链接：
https://cloud.google.com/appengine/pricing.

## Summary

在本章中，我们查看了 App Engine，这是一个 PaaS 产品。我们区分了两种环境类型，即标准和灵活。然后，我们查看了如何部署 App Engine 应用程序以及如何将其更新到新版本。我们还学习了如何在版本之间拆分和迁移流量。最后，我们了解了 cron 作业，它允许我们运行计划作业。

在下一章中，我们将查看 Cloud Functions，这是一个 Function-as-a-Service（FaaS）产品。

## Further reading

要了解更多关于 App Engine 的信息，请查看以下链接：

- **app.yaml reference:** https://cloud.google.com/appengine/docs/standard/python/config/appref
- **Environments:** https://cloud.google.com/appengine/docs/the-appengine-environments
- **Memcache:** https://cloud.google.com/appengine/docs/standard/python/memcache/
- **Firewall:** https://cloud.google.com/appengine/docs/standard/python/creating-firewalls
- **Cron Jobs for Python:** https://cloud.google.com/appengine/docs/standard/python/config/cron
- **Cron Jobs for Java**: https://cloud.google.com/appengine/docs/standard/java/config/cron
- **Cron Jobs for Go**: https://cloud.google.com/appengine/docs/standard/go/config/cron
- **Custom domains**: https://cloud.google.com/appengine/docs/standard/python/mapping-custom-domains
- **SSL certificates**: https://cloud.google.com/appengine/docs/standard/python/securing-custom-domains-with-ssl
- **App Engine scaling**: https://cloud.google.com/appengine/docs/standard/python/how-instances-are-managed

## 6
使用 Google Kubernetes Engine 管理 Kubernetes 集群

在上一章中，我们深入探讨了 Google Compute Engine，它提供 Infrastructure-as-a-Service（IaaS）。在本章中，我们将查看一个 Container-as-a-Service（CaaS）产品。Google Kubernetes Engine（GKE）允许我们按需创建托管的 Kubernetes 集群。在开始讨论 GKE 之前，我们需要了解一些概念，例如微服务、容器和 Kubernetes 本身。

> **Exam Tips**
> GKE 在云架构师考试中被重点考察。确保你理解容器、微服务、Kubernetes 以及 GKE 特定主题的基本概念。特别注意 *Kubernetes* 部分。很多问题与 Kubernetes 相关，而非 GKE 特定。确保你理解 Kubernetes 架构和资源，特别注意网络和服务，以便了解应用程序如何被访问，了解如何使用 IAM 和 RBAC 保护集群，以及最后，了解如何管理、扩缩和升级 GKE 集群。

如果你从未听说过这些，不用担心——我们将带你了解所有基本概念。本章将涵盖以下主题：

- 微服务的介绍
- 容器
- Kubernetes
- Google Kubernetes Engine

## An introduction to microservices

让我们先回顾一下微服务的概念。在传统世界中，应用程序以单体架构交付。这意味着多个服务托管在单个节点上。在微服务架构中，应用程序被划分为多个微服务，每个微服务托管在单独的节点上，如下所示：

![](img/de2c97ab017f36b3750a5bade111edd7_156_0.png)

