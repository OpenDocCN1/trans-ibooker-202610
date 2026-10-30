
## 6.1 仪表盘变量

让我们实际操作一下：首先，创建一个新的仪表盘（点击显示四个方块且标题为 `Dashboards` 的菜单项），然后点击 `New Dashboard`。接着，选择右上角图标行中带有标签 `Dashboard Settings` 的齿轮符号。你现在应该能看到仪表盘设置；在那里（左侧选项卡选择中），点击 `Variables`。在这里，你可以通过点击 `Add Variable` 按钮来创建一个新变量。我们将其名称设为 `Test`，类型为 `Custom`，标签字段中填写 `An Example Dropdown`。这使我们能够定义一个预定义值的下拉列表，供仪表盘用户从中选择一个或多个。对于 `Values Separated by Comma` 选项中的值，使用 `200,301,404`，结果将类似于图 6.2 所示。

![](OEBPS/Images/06-02.png)

图 6.2 预定义（固定）值的示例 Grafana 仪表盘变量定义

输入完成后，点击 `Update` 按钮。现在按 `ESC` 键或点击左上角的左箭头，你将返回到仪表盘，并应看到定义 `test` 变量的结果，如图 6.3 所示。

![](OEBPS/Images/06-03.png)

图 6.3 我们示例仪表盘变量 `test` 在仪表盘中的渲染效果

Tip 在 Grafana 9 版本中，可考虑使用支持的不同类型的变量（例如 `query`、`custom` 和 `data source`）作为实现关联的一种方式。从 Grafana 10 版本开始，提供了专门的关联 API，包括在关联中使用变量和转换的 UI 支持（[http://mng.bz/AoPz](http://mng.bz/AoPz)）。

现在你已经定义了变量 `test`，可以例如在仪表盘查询中使用它来进行参数化。我们就这样做。首先，我们需要一个容纳可视化的面板，因此点击 `Add a New Panel` 按钮创建它。由于我们将 `Prometheus` 定义为默认数据源，Grafana 会为我们选择合适的可视化（类型为 `time series`）。

那里已经有一个名为 `A` 的占位符（空）查询。接下来，请确保你处于 `Code`（而不是默认的 `Builder`）模式；这可以在查询右上角切换。现在，在 `Metrics Browser` 输入字段中输入以下参数化的 PromQL 查询：

```
rate(
  prometheus_http_requests_total{code="$test"}[5m]
)
```

此查询使用我们之前定义的 Grafana 仪表盘变量 `test` 来过滤标签值。换句话说，让仪表盘用户选择要查询的 HTTP 状态码（默认 HTTP 状态码为 `200`）。图 6.4 显示了仪表盘编辑器中参数化查询的结果（当你满意并想尝试仪表盘时，按右上角的 `Apply` 按钮）。

![](OEBPS/Images/06-04.png)

图 6.4 Grafana 仪表盘变量 `test` 在 PromQL 查询中的使用（面板编辑器）

图 6.5 显示了在我们的 PromQL 查询中使用仪表盘变量 `test` 的结果，允许你选择 HTTP 状态码。如果你选择不同的值，例如 `301`，面板很可能为空，这是预期的，因为不存在此类标签值（除非你对 `Prometheus` 做了额外操作或发生了异常情况）。

![](OEBPS/Images/06-05.png)

图 6.5 使用 Prometheus 作为数据源的 Grafana 仪表盘（带参数化 PromQL 查询）

尝试对 `test` 的值进行调整，或为 `handler` 标签添加一个新变量。在此上下文中，请注意，除了我们刚才使用的简单示例之外，还有多种变量类型，从数据源选择到自由文本输入再到查询变量。后者允许你针对特定数据源编写查询，返回一个值列表。例如，你可以编写一个 PromQL 查询，为新的 `handler` 变量填充下拉列表，然后在扩展的参数化 PromQL 查询 `prometheus_http_requests_total{code="$test",handler="$handler"}` 中使用它。另一个针对数据源查询以填充仪表盘变量值的示例是，考虑一个 Kubernetes 集群，你希望根据 PromQL 查询为仪表盘用户提供可供选择的可用命名空间列表。

Tip 与数据源一样，你可以直接（如前所示）或使用 `Terraform` ([http://mng.bz/Zq8P](http://mng.bz/Zq8P)) 为 Grafana 配置仪表盘。通过 Grafana 仪表盘注册表（[https://grafana.com/grafana/dashboards/](https://grafana.com/grafana/dashboards/)）可以获得许多优秀的社区维护仪表盘，你可以通过引用导入或通过下载 `JSON` 定义导入。

`Alerts` ([https://grafana.com/docs/grafana/latest/alerting/](https://grafana.com/docs/grafana/latest/alerting/)) 是第三个核心 Grafana 资源。统一告警的主题在 Grafana 9 版本中引入了根本性变化。我们在本章中不会讨论告警，而是在第 7 章深入探讨该主题。

你不必自行运行 Grafana。如果你更喜欢将升级、安全补丁和访问控制等繁重工作外包，（截至出版时）有许多选择：

*   Amazon Managed Grafana ([https://aws.amazon.com/grafana/](https://aws.amazon.com/grafana/)) 自 2020 年底以来就已存在，并于 2021 年 9 月进入通用可用性（GA）。它支持 AWS SSO 和 SAMLv2，使向组织中的任何用户授予仪表盘访问权限变得容易。

*   Azure Managed Grafana ([http://mng.bz/RxYK](http://mng.bz/RxYK)) 自 2022 年 8 月起进入 GA，支持 Grafana 9 版本功能。

*   你可以使用托管 Grafana 作为 Grafana Labs 的 Grafana Cloud ([https://grafana.com/products/cloud/](https://grafana.com/products/cloud/)) 方案的一部分，同时还包括一系列其他基于 OSS 的工具，包括用于日志的 Grafana Loki 和用于追踪的 Grafana Tempo。

*   阿里云提供 Grafana 托管服务 ([https://www.alibabacloud.com/product/grafana](https://www.alibabacloud.com/product/grafana))，提供与协作工具的集成，包括钉钉和企业微信。

好的，这很有趣。接下来是 Kibana（及其 OpenSearch 变体 Dashboards）。

### 6.1.2 Kibana 和 OpenSearch Dashboards

我们在第 5 章中讨论了 Elasticsearch 和 OpenSearch（ES/OS）作为后端。在此上下文中，你可以选择 Kibana ([https://www.elastic.co/kibana/](https://www.elastic.co/kibana/)) 用于 Elasticsearch，以及 OpenSearch Dashboards ([https://opensearch.org/docs/latest/dashboards/index/](https://opensearch.org/docs/latest/dashboards/index/)) 用于 OpenSearch 作为各自的前端。

由于 OpenSearch Dashboards 是 Kibana 的分支，因此不出所料，有许多共同的概念和功能，不考虑许可方面的差异。鉴于这两个项目及其基于的托管服务的性质和动态，我在这里不会详细介绍 Kibana 或 OpenSearch Dashboards，但我会指出一些起点，供你如想了解更多时参考：

*   在第 4 章中，我们使用 OpenSearch 和 OpenSearch Dashboards 作为使用 Fluent Bit 代理进行日志摄取和查询的后端和前端目标。

*   通过 Logz.io 博客，有一个优秀的 Kibana 教程（[https://logz.io/blog/kibana-tutorial/](https://logz.io/blog/kibana-tutorial/)）可供使用，或者如果你想投入更多时间，可以深入 Kibana 文档（[https://www.elastic.co/guide/en/kibana/current/get-started.html](https://www.elastic.co/guide/en/kibana/current/get-started.html)），它也提供了很好的入门指导。
