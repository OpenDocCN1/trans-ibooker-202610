

| 服务 | 描述 |
|---|---|
| Serverless for Apache Spark | 一种服务，允许工程师在无需配置和管理 Dataproc 集群的情况下运行 Spark 工作负载。^([b](#ch02.html#id412)) |
| Bigtable Data Boost | 一种无服务器计算服务，允许工程师使用与处理应用流量的集群隔离的无服务器计算，向 Bigtable 数据发送高吞吐量的读取作业和查询。这有助于确保这些集群的性能不会受到分析或批处理工作负载的影响。^([c](#ch02.html#id413)) |
| Datastream | 一种无服务器变更数据捕获和复制服务，允许工程师设置 ELT（提取、加载和转换）管道以实现低延迟数据复制。它还使工程师能够在不同应用和异构数据库之间同步数据流，同时将运营开销和延迟降至最低。 |
| Eventarc | 一种用于异步传递事件的服务，用于特定的无服务器架构。 |
| Pub/Sub | 一种异步且可扩展的消息传递服务，可解耦服务，并可用于无服务器应用。 |
| Workflows | 一种无服务器编排服务，使工程师能够构建复杂的工作流，并与 Google Cloud 上的其他无服务器服务无缝集成。 |
| Cloud Scheduler | 一种完全托管的 cron 服务，用于某些无服务器架构和实现。 |
| Serverless VPC Access | 一种服务，允许计算资源和应用通过 Google Cloud 完全管理的连接器，使用其 IP 地址访问 VPC 网络中的资源。 |
| Serverless network endpoint group（serverless NEG） | Google Cloud 中的单个端点，解析为 App Engine、Cloud Run 函数、API Gateway 或 Cloud Run 资源，允许这些无服务器资源用作负载均衡器的目标。 |

^([a](#ch02.html#id411-marker)) Google Cloud 在 2024 年 8 月的 [blog post](https://oreil.ly/hhd4Y) 中透露，Google Cloud Run functions 现在与 Cloud Run 一起运行在统一的无服务器平台上。使用 Cloud Functions（2nd gen）的开发者现在可以访问 Cloud Run 功能，包括但不限于多事件触发器管理、挂载 Cloud Storage 卷的能力，以及对使用 NVIDIA GPU 的推理函数的支持。^([b](#ch02.html#id412-marker)) Dataproc 是 Google Cloud 中用于运行开源大数据工具和框架的完全托管服务。你可以查看 [guide page](https://oreil.ly/ZdT2S) 以了解 Dataproc 与 Serverless for Apache Spark 之间的差异。^([c](#ch02.html#id413-marker)) Bigtable 是 Google Cloud 中用于大规模、低延迟工作负载的完全托管 NoSQL 列式数据库。

---

你经常会看到这些服务和功能在无服务器架构中协同工作，以应对各种挑战和需求。例如，如果你要在 Google Cloud 上构建一个无服务器、事件驱动的 HTML 到 PDF 转换服务，你可能会使用 Eventarc 在用户将 HTML 文件上传到指定的 Cloud Storage 存储桶时自动触发工作流。然后 Cloud Run 服务将下载并处理 HTML 文件，将其转换为 PDF，并将结果输出存储在单独的输出存储桶中。

### Note
你可以通过访问 Google Cloud 的 [“Go Serverless” page](https://oreil.ly/SZqQK) 找到 Google Cloud 无服务器服务、用途和其他资源的列表。

到目前为止，你应该熟悉为 AWS、Azure 和 Google Cloud 上的无服务器架构提供支持的云服务和功能。随着云服务提供商推出新服务，预计该列表会增长。

# Common Building Blocks, Patterns, and Solutions for Serverless Architectures

现在你已经了解了在 AWS、Azure 和 Google Cloud 上实现无服务器运营模式的各种服务、功能和能力，是时候更仔细地看看它们如何协同工作以形成无服务器解决方案了。在本节中，我将带你了解实现无服务器架构的各种构建块、模式和解决方案。我还将强调重要的运营和安全注意事项，以避免你在实际无服务器应用部署中可能遇到的挑战。

### Note
对无服务器应用的设计和实现有坚实的基础，将使你更容易掌握本书后续章节中讨论的无服务器安全概念和技术。

## Serverless Web Applications and APIs

无服务器 Web 应用和 API 利用各种云资源来响应 HTTP 请求等事件（[Figure 2-5](#ch02.html#fig_serverless_web_applications_and_apis)）。在无服务器架构中，你会看到的最常见实现之一涉及 API 网关调用无服务器函数，使用 AWS Lambda、Azure Functions 和 Google Cloud Run functions 等服务。

![](assets/lsse_0205.png)

此设置会根据传入流量自动扩展，并在空闲时可扩展为零。请求期间会发生什么？当 API 网关接收到 HTTP 请求时，会发生以下情况：

1. 调用云函数，并自动配置必要的计算基础设施。
2. 云函数处理事件或 HTTP 请求，并将响应返回给 API 网关。
3. 执行后，函数的计算环境可能会保持活动状态一段时间以处理额外请求，或在保持空闲时自动取消配置。

由于无服务器函数资源是动态创建和销毁的，它们理想情况下不会在本地存储状态。相反，状态通常保存在外部数据库和存储服务中，如 [Figure 2-6](#ch02.html#fig_serverless_web_applications_and_apis_with_storage) 所示。

![](assets/lsse_0206.png)

在此设置中，无服务器函数根据需要从数据库和存储服务检索和持久化记录和文件。这有助于确保数据在函数调用、扩展事件或非活动期间可靠存储且不会丢失。

假设你有一个简单的网站，用户可以在上面查看你撰写的博客文章并发表评论。使用我们刚刚讨论的无服务器架构，当用户在帖子上提交评论时会发生以下情况：

1. HTTP 请求触发事件，通过 API 网关调用无服务器函数。
2. 无服务器函数对其进行处理，将评论连同其他相关元数据（如时间戳、用户 ID 和博客文章 ID）存储在数据库中，并将新记录与关联的博客文章 ID 链接。
3. 作为评论一部分上传的图像和其他文件存储在云存储服务中。步骤 2 中创建的记录将包含对这些文件的引用，以便在渲染博客文章页面时可以轻松检索并与评论一起显示。
4. 处理完评论后，无服务器函数向 API 网关返回成功响应，API 网关随后将响应转发回用户的浏览器或应用。

评论连同任何相关文件和图像稍后会通过单独的过程检索并显示在博客文章页面上，该过程根据需要查询数据库和存储服务。

### Note
除了后端组件和资源之外，完整的无服务器应用设置通常还包括一个 Web 前端，该前端可以使用基于云的静态网站托管服务单独托管。前端通过公开的 API 端点与之通信，以执行创建、读取、更新和删除数据等操作。

