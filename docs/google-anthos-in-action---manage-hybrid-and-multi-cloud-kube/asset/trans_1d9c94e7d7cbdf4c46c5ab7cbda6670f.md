
公有云。Dremel 部署、运行并运营在 Google 管理的账户中由 Anthos 支持的 GKE 上，而该账户位于数据所存储的公有云中。借助 Anthos，Google Cloud 得以推出 BigQuery Omni，使 Google Cloud 能够将其分析能力扩展到驻留在其他云（如 AWS 和 Microsoft Azure）中的数据。请参阅[此链接](#)获取 BigQuery Omni 的演示。

需要特别指出的是，尽管 BigQuery Omni 利用了 Anthos GKE 的能力，但用户并不一定要成为 Anthos 客户。在这种情况下，BigQuery Omni 工程团队才是 Anthos 客户，而 BigQuery Omni 用户不会直接与 Anthos GKE 交互。

如图 3（BigQuery Omni 技术栈）所示，BigQuery Omni 引入了在 GCP 以及其他公有云（如 AWS 和 Microsoft Azure）上运行 BigQuery 查询引擎 Dremel 的能力。架构组件分布如下。在 GCP 中，BigQuery 技术栈保持不变。从下到上，它由通过拍比特网络连接到 Dremel 运行的 BigQuery 计算集群的 BigQuery 存储组成。为了临时存储和缓存中间查询结果（对于某些复杂分析，这些结果可能达到 TB 量级），Dremel 实现了分布式内存 shuffle 层。当运行在 GCP 以外的公有云（例如 AWS）上时，BigQuery Omni 不会将数据从该云迁移到 Google 中 BigQuery 的托管存储，而是直接对驻留在该云原生存储系统中的数据执行分析。

在 BigQuery Omni 中，当用户执行访问多云数据的查询时，BigQuery 路由器会使用安全连接将查询真正路由到运行在数据所在位置的 Dremel 引擎。Dremel 工作负载必须被授予访问驻留在公有云中的数据的权限。查询结果可以直接导出回数据存储，查询结果或查询所用数据不会发生跨云移动。BigQuery Omni 还支持将文件加载到“常规”BigQuery，以满足需要关联 BigQuery 中驻留数据或使用 Vertex AI 训练机器学习模型的用户。

## 图 15.3. BigQuery Omni 技术栈

![](img/fb3bebce7b5f593bca1a7f8a1371be06_570_0.png)

## 15.4.1 让 BigQuery 访问 AWS 中的数据

为了让 BigQuery Omni 对驻留在 AWS 上的数据执行查询，用户需要拥有 AWS 账户，并有权修改 S3 数据的*身份和访问管理 (IAM)* 策略，以授予 BigQuery 访问权限。该过程涉及多个步骤，如下列章节所述。

### 创建 AWS IAM 角色和读取策略

用户必须首先为其希望授予 BigQuery 访问权限的 S3 创建 AWS IAM 角色和读取策略，然后将角色和策略附加在一起。有关如何执行这些任务的最新说明，请参阅 Google Cloud BigQuery Omni [文档](#)或 AWS 文档。

### 定义外部表

创建连接后，用户需要为其存储在 S3 中的数据创建外部表定义。有关如何执行这些任务的最新说明，请参阅 Google Cloud BigQuery Omni [文档](documentation)或 AWS 文档。

## 15.4.2 充分利用 BigQuery 的存储设计优化

BigQuery 的可扩展性和高性能源于一系列相互叠加的设计选择。例如，为了优化查询分析，BigQuery 存储引擎以[列式格式](columnar format)存储数据，并对数据进行分区和聚簇，以减少查询的搜索空间。如果远程数据的存储和组织方式并非如此，相对于在 Google Cloud 上运行 BigQuery，这可能导致一定程度的性能下降。然而，能够使用 BigQuery 分析以现有格式和现有位置释放数据价值，且无需任何迁移工作，这种好处可能是一个可接受的权衡。

用户始终可以选择通过模拟 BigQuery 使用的一些技术方法来优化数据，使其适合 BigQuery Omni 分析，包括：

- *将数据转换为列式格式：* 允许逐列检索数据。在大多数用例中，分析查看的是列中的所有值，而不是对表中的行进行范围扫描。因此，像 [Apache Parquet](Apache Parquet) 这样的列式格式优化了列的检索，减少了分析期间访问的数据量，从而提高了性能并降低了成本。
- *压缩数据：* 允许从存储中检索更少的字节。对于以列式格式存储的数据，压缩可以利用列条目可能非常重复或遵循某种模式（例如连续数字）这一特点。Apache Parquet 使用 [Google Snappy](Google Snappy) 压缩列式数据。
- *数据分区：* 在搜索特定数据值时，可以实现更高效的数据扫描。BigQuery 使用 [Hive 分区](hive partitioning)。

## 15.4.3 BigQuery Omni 背后的差异化优势

BigQuery Omni 区别于其他分布式查询引擎（如 [Presto](Presto)）之处在于，它可以随着时间推移利用 Anthos 作为管理平台，不仅用于部署，还用于监控、运营、更新和管理查询的整个生命周期。本质上，BigQuery 希望为客户提供一个多云方案。BigQuery 需要一个类似 Google “borg” 的多云解决方案，使 Dremel 能够在 AWS 和 Microsoft Azure 上几乎像在 Google Cloud 上一样工作。

BigQuery 查询引擎也运行在 GKE 环境中，充分利用了云原生部署的所有现代特性。

需要特别指出的是，这可能是有史以来第一次，一家云厂商（此处指 Google）在竞争对手云环境的边界内部署了真正专有且差异化的代码 Dremel。有人可能会说，Google 已经在 AWS 和 Microsoft Azure 上运行 GKE，这也是专有代码。但此处的关键差异化在于，GKE 是管理容器的平台，这是如今所有云提供商都具备的能力；而 Dremel 掌握着大规模分析的关键，为 Google 用户带来了巨大价值。

最终，BigQuery Omni 为多云世界带来了一致的用户体验，通过单一管理面板对驻留在多个云中的数据进行分析，打破了此前围绕云边界形成的壁垒。借助 Anthos，BigQuery Omni 将其用作安全的多云基础设施平台，为驻留在其他云中的数据提供分析基础。而 BigQuery 在实现这一切时，无需任何跨云数据移动，也不会创建任何数据副本。

## 15.5 Anthos 混合 AI

Google 处于 AI 革命的前沿，并将其尖端的 AI 技术融入其众多产品中，为用户提供诸如自动-

![](img/fb3bebce7b5f593bca1a7f8a1371be06_574_0.png)

混合 AI 的前提是，用户已经有一个或多个 Anthos 本地集群注册到 GCP。使用 [GCP Marketplace](#)，用户可以搜索和发现 Google Cloud AI 解决方案，例如一种将语音转换为文本的解决方案，将在下一节中说明。用户可以选择一个解决方案，并点击部署到本地 Anthos 集群。这些 AI 解决方案已打包为 Anthos 应用，可以部署到任何 Anthos 集群，包括本地运行的集群。这些解决方案由 Anthos 完全管理，并受益于所有 Anthos 功能，包括自动缩放，这保证了本地计算资源仅在使用时分配，且只分配所需的资源。Anthos 会对这些 AI 解决方案进行计量，并根据使用量向客户收费。Anthos Hybrid AI 允许 AI 应用程序按需动态部署、运行和解除部署。

## 15.5.2 混合 AI 解决方案

Anthos Hybrid AI 的首个版本在本地提供以下 Google Cloud AI 服务：

- *光学字符识别 (OCR)*：将文档转换为数字格式的文本；
- *语音转文本*：将语音转换为文本，支持包括英语、法语、西班牙语、粤语和日语。[Google Cloud 版本](https://) 已支持超过一百种不同语言及其变体，并可根据需求轻松地将其他语言添加到本地版本中。

### 光学字符识别

企业有大量数据被非数字化媒体（如纸质文档）所束缚，或隐藏在无法轻松进行数字化分析的数字格式中。例如，发票或政府颁发的产权证的扫描数字图像可能分别作为交易记录和所有权证明，但提取其中可单独分析的各条信息仍然非常困难。交易涉及的实体名称、日期、地点、价值以及交换物品的具体信息可能仍深藏在数字图像中。以往通过人工输入数据将这些信息数字化的尝试已被证明容易出错、耗时、成本高昂且难以扩展。
