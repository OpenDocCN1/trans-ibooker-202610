

## Azure 无服务器服务

| Azure Durable Functions | Azure Functions 的一项功能，使开发人员能够在无服务器架构中编写有状态函数。 |
| Azure Container Apps | 用于基于容器的应用程序的无服务器平台。 |
| Azure Functions on Azure Container Apps | 一种容器托管选项，可让开发人员将函数与其它微服务、API 或任何容器化应用程序一起运行。 |
| Azure Kubernetes Service (AKS) virtual nodes | AKS 的扩展，为托管 Kubernetes 服务带来无服务器能力，用于在 Azure 上运行容器化应用程序。 |
| Azure Static Web Apps | 一种无服务器服务，使开发人员能够托管静态前端网站并使用无服务器后端 API，通过从代码存储库自动部署全栈 Web 应用程序。 |
| Azure Event Hubs | 一种数据流服务，每秒可处理数百万个事件，实现从各种来源实时收集和处理大量数据。 |
| Azure Service Bus | 一种完全托管的企业消息服务，使应用程序和服务之间能够进行通信。 |
| Azure Event Grid | 一种高度可扩展的无服务器事件代理，使用事件集成应用程序。 |
| Azure SignalR Service | 一种完全托管的服务，帮助工程师构建具有实时功能的应用程序。它支持一种无服务器服务模式，可与 Azure Functions 配合使用以提供实时消息传递功能。 |
| Azure API Management | 一种完全托管的服务，允许开发人员和工程师发布、保护、转换、维护和监控 API 资源。 |
| Azure Logic Apps | 一种无服务器工作流自动化服务，使团队更轻松地管理和扩展其应用程序。 |
| Azure DevOps | 一组基于云的服务，可用于 Azure 上的无服务器操作（例如软件开发和交付）。它允许创建可将代码部署到各种服务的 CI/CD 管道。 |
| Azure Functions Durable Task Scheduler | Azure Durable Functions 的完全托管后端，用于编排和管理有状态的无服务器工作流。 |
| Azure SQL Database serverless | Azure SQL Database 中数据库的计算层，可根据工作负载自动扩展。 |
| Azure Cosmos DB serverless | Azure Cosmos DB 中的一种容量模式，使用该服务时可实现无服务器按需消费模式。 |
| Azure Blob Storage | 一种对象存储解决方案，针对在 Azure 上实现的各种无服务器架构中常用的海量对象进行了优化。 |
| Azure Data Factory | 一种基于云的无服务器数据集成服务，允许用户创建、安排和管理数据管道和工作流。 |
| Azure Stream Analytics | 一种实时数据流和分析服务，每秒可处理数百万个事件。 |
| Azure Synapse Analytics | 一种企业分析服务，提供无服务器和专用资源，帮助用户大规模地摄取、转换、建模、分析和提供数据——包括但不限于无服务器 SQL 池和无服务器 Apache Spark 池。 |
| Azure Queue Storage | 一种用于存储大量消息的服务，使函数分离和组件解耦成为可能，以便在大型工作负载中扩展和管理任务。 |
| Azure Files | 一种完全托管的无服务器文件共享服务。 |
| Azure Storage Actions | 一种无服务器平台，通过使用户能够在多个存储账户中的数百万个对象上执行常见数据操作，从而简化存储对象的管理。 |
| Microsoft Foundry | 一个统一平台，旨在让开发人员构建生成式 AI 应用程序，并使用尖端的 AI 工具和 ML 模型进行探索、构建、测试和部署。它支持无服务器和非无服务器部署选项，允许在模型和应用程序的托管和扩展方式上具有灵活性。^([a](#ch02.html#id408)) |
| Azure Application Insights | 一种应用程序性能管理服务，允许开发人员深入了解其应用程序的性能和可用性。 |
| ^([a](#ch02.html#id408-marker)) Microsoft Foundry 支持的四种部署选项为 Azure OpenAI 服务、Azure AI 模型推理、无服务器 API 和托管计算。 |

在实践中，其中许多服务和功能通常在无服务器架构中一起使用，以满足各种业务和技术需求。例如，如果您在 Azure 上构建无服务器 Web 应用程序或 API，您可能会使用 Blob Storage、API Management 和依赖 Key Vault 在应用程序组件之间安全管理机密、密钥和凭据的 Azure Functions。

### Note
您可以通过访问 [Azure serverless page](https://oreil.ly/UetSe) 找到 Azure 无服务器服务的列表、用例和其他资源。

## Google Cloud 上的无服务器服务、功能和能力

[Table 2-3](#ch02.html#gcp_serverless_services) 是可在 Google Cloud 上用于无服务器架构的服务和功能的全面但非详尽的列表。让我们逐一了解它们如何融入更大的图景。

### Table 2-3. Services and capabilities that enable the serverless operational model on Google Cloud

| Service/feature/capability | Description |
| --- | --- |
| Cloud Run functions | 一种无服务器计算服务，允许开发人员编写和执行响应事件的代码。与 AWS Lambda 类似，Cloud Run functions 在其它 Google Cloud 服务之间充当粘合剂，使开发人员能够为不同用例构建各种无服务器应用程序。^([a](#ch02.html#id411)) |
| Cloud Run | 一种完全托管的无服务器容器计算平台。 |
| Cloud Build | 工程师团队的无服务器 CI/CD 平台。 |
| App Engine | 一种无服务器平台，允许开发人员在无需担心底层基础设施的情况下构建和部署 Web 应用程序。 |
| API Gateway | 一种完全托管的服务，让开发人员为无服务器和非无服务器应用程序设置、保护和监控 API。 |
| Vertex AI | Google Cloud 上的 ML 平台，允许工程师构建支持各种 ML 用例（包括但不限于生成式 AI）的无服务器和非无服务器应用程序。 |
| Vertex AI Pipelines | 用于构建和运行 ML 管道的无服务器编排器。它使数据科学家和 ML 工程师能够通过在 ML 生命周期中自动化和监控可重复的工作流来实现机器学习运营（MLOps）实践。 |
| Firestore | 一种完全托管的无服务器 NoSQL 文档数据库，用于构建丰富的移动、Web 和物联网应用程序。 |
| Cloud Storage | Google Cloud 上各种无服务器和非无服务器应用程序中使用的对象存储解决方案。 |
| Dataflow | 一种无服务器数据处理服务，可与 Google Cloud 上的其它无服务器服务无缝集成。 |
| BigQuery | 一种无服务器、高度可扩展且经济高效的数据仓库。 |
| Cloud SQL serverless exports | Google Cloud SQL 中的一项无服务器功能，使工程师能够在不影响生产工作负载性能或稳定性的情况下从数据库实例导出数据。此功能允许主数据库资源以通常的性能速率执行操作，因为会创建一个临时数据库实例来卸载导出操作。导出操作完成后，此临时实例会自动删除。 |
| Database Migration Service | 一种无服务器数据迁移服务，抽象了手动配置、管理和监控用于将数据库迁移到 Google Cloud 的数据库迁移资源的繁重工作，可实现最小停机时间。 |

