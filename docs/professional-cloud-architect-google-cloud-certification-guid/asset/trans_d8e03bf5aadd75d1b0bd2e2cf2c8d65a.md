

重要说明
在阅读案例研究时，建议您同时参考 GCP 解决方案页面：https://cloud.google.com/solutions。
此外，建议您参考 GCP 架构中心以获取参考架构：https://cloud.google.com/architecture/。

## 我们在案例研究中寻找什么？

让我们仔细看看 Google Cloud 提供的其中一个案例研究。我们选择了 Mountkirk Games。在首次通读该案例研究时，我们可以看到它被分解为六个不同的部分，让我们更详细地查看每个部分：

- 公司概览
- 解决方案概念
- 现有技术环境
- 业务需求
- 技术需求
- 执行声明

每个部分中的摘录都引用了案例研究的文本。需要提醒的是，完整文档可以在本章开头提到的网址中查看。

## 公司概览

让我们看看公司概览：

> “Mountkirk Games 为移动平台制作在线、基于会话的多人游戏。他们在成功将本地环境迁移到 Google Cloud 后，最近开始扩展到其他平台。
> 
> 他们最近的努力是创建一个复古风格的第一人称射击游戏（FPS），该游戏允许数百名同时玩家从多个平台和位置加入特定地理位置的数字竞技场。一个实时数字横幅将显示所有活跃竞技场中所有顶尖玩家的全球排行榜。”

来源：https://services.google.com/fh/files/blogs/master_case_study_mountkirk_games.pdf。

许可：https://creativecommons.org/licenses/by/4.0/legalcode。

那么，从这里我们可以提取哪些信息来帮助我们形成解决方案架构呢？嗯，我们可以看到这个概览引导我们思考移动平台，因此我们应该开始思考移动游戏架构的起点，以及前端架构应该是什么，后端架构应该是什么。我们可以看到我们需要能够应对多人游戏需求，这可能需要复杂的相互关联的表。我们还需要存储会话，因此我们需要研究对此所需的配置，以及其他一些关键要素，例如多个平台和位置。我们还需要一个排行榜，我们可以通过分解组件来实现。最后，我们应该考虑用户体验。查询到我们后端的速度会直接影响最终用户，以及我们游戏的整体成功。

第一段告知我们核心业务是面向移动平台的在线游戏。立即，我们应该意识到移动游戏平台会产生大量实时数据，并且有大量设备连接到它们，所有这些都将来自不同类型的连接（Wi-Fi、3G-5G 等）。这是一个相当独特的行业。

第二段开始暗示一些潜在的 GCP 服务。例如，它提到将有一个实时数字横幅，其中包含顶尖玩家的排行榜。我们可以尝试将其转化为适合此架构的 GCP 服务：

- 我们需要一个流式服务。我们知道哪项服务可以处理流式处理模式？如果你想到 **Pub/Sub**，那就正确了。
- 我们如何将这些事件转换为排行榜？我们可能需要一个 **Dataflow** 管道来清理数据并确保消息格式良好，然后将其转换为结构化数据。
- 该转换然后可以添加到 **BigQuery** 表格式中，将某人的用户名与 *kills* 数量匹配以列出顶尖玩家。
- **Data Studio** 也可用于创建交互式排行榜，从 BigQuery 消费并展示数据。

这只是为了让我们开始思考架构。也许有人会争辩说我们不需要 Dataflow，并且 BigQuery 可以直接从云存储桶查询数据，但这会是一个可扩展的设计吗？如果来自移动设备的事件引入了额外字段，我们是否需要重新审视这一点？

## 解决方案概念

现在，让我们看看解决方案概念：

> “Mountkirk Games 正在构建一款他们预计会非常受欢迎的多人游戏。他们计划将游戏后端部署在 Google Kubernetes Engine 上，以便能够快速扩展并使用 Google 的全球负载均衡器将玩家路由到最近的区域游戏竞技场。为了使全球排行榜保持同步，他们计划使用多区域 Spanner 集群。”

来源：https://services.google.com/fh/files/blogs/master_case_study_mountkirk_games.pdf。

许可：https://creativecommons.org/licenses/by/4.0/legalcode。

解决方案概念只为我们提供了几行信息，但仍然提供了关键信息。他们告诉我们设计需要使用某些服务，但为什么他们希望使用这些服务？此外，我们应该思考哪些服务应设计为典型游戏基础设施的前端和后端组件。

以前端为例。他们希望使用 Google Kubernetes Engine（GKE），但他们可以使用其他各种选项。他们可以使用 App Engine 或 Cloud Run，这两种也提供与云负载均衡的完全集成。然而，这两者都是完全托管的，而 GKE 为开发人员提供了更多控制权，并且历史上游戏已在 GKE 上以大规模部署，使用专用游戏服务器方法：

![](img/de2c97ab017f36b3750a5bade111edd7_589_0.png)

图 18.1 – 游戏架构（转自 https://cloud.google.com/architecture/mobile-gaming-analysis-telemetry，许可：https://creativecommons.org/licenses/by/4.0/legalcode）

## 现有技术环境

现在，让我们回顾现有环境和需求：

> “现有环境最近已迁移到 Google Cloud，并且有五款游戏通过 lift-and-shift 虚拟机迁移带了过来，只有少数例外。
> 
> 每个新游戏都存在于一个隔离的 Google Cloud 项目中，该项目嵌套在维护大部分权限和网络策略的文件夹下方。低流量的旧游戏已合并到一个项目中。开发和测试也有单独的环境。”

来源：https://services.google.com/fh/files/blogs/master_case_study_mountkirk_games.pdf。

许可：https://creativecommons.org/licenses/by/4.0/legalcode。

## 业务需求

现在，让我们看看业务需求：

> “支持多种游戏平台。
> 
> 支持多个区域。
> 
> 支持游戏功能的快速迭代。最小化延迟。
> 
> 针对动态扩展进行优化。
> 
> 使用托管服务和资源池。
> 
> 最小化成本。”

来源：https://services.google.com/fh/files/blogs/master_case_study_mountkirk_games.pdf。

许可：https://creativecommons.org/licenses/by/4.0/legalcode。

## 技术需求

我们现在有几个技术需求：

> “根据游戏活动动态扩展。
> 
> 在接近实时的全球排行榜上发布计分数据。
> 
> 将游戏活动日志存储为结构化文件以供将来分析。
> 
> 使用 GPU 处理在服务器端渲染图形以支持多平台。
> 
> 支持旧游戏最终迁移到此新平台。”

**来源：** https://services.google.com/fh/files/blogs/master_case_study_mountkirk_games.pdf。

**许可：** https://creativecommons.org/licenses/by/4.0/legalcode。

当你阅读这些需求时，会想到哪些服务？几乎每一行都有一些关键词或短语：动态扩展、日志、迁移和延迟等。我们应该将这些映射到 GCP 服务。例如，如果我们看到根据游戏活动扩展的需求，我们可能会开始思考能够满足此需求的服务。例如，Pub/Sub 能够从移动设备摄取事件并按需扩展。

## 执行摘要

我们案例研究的最后一部分是执行摘要：

> “我们的上一款游戏是第一次使用 Google Cloud，结果非常成功。我们能够以以前从未有过的方式分析玩家行为和游戏遥测数据。这种成功使我们能够押注于完全迁移到云端，并开始使用云原生设计原则构建全新的游戏。我们的新游戏迄今为止是最雄心勃勃的，将为我们打开支持除移动之外更多游戏平台的大门。延迟是我们最优先考虑的问题，尽管成本管理是下一个最重要的挑战。与我们的第一款基于云的游戏一样，我们已经习惯于云能够实现高级分析功能，以便我们可以快速迭代部署错误修复和新功能。”

**来源：** https://services.google.com/fh/files/blogs/master_case_study_mountkirk_games.pdf。

**许可：** https://creativecommons.org/licenses/by/4.0/legalcode。

