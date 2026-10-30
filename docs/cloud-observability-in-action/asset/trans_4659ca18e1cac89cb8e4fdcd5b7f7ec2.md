
阅读主题相关的通用资料，例如博客文章和文章，并不会有什么坏处——例如，参阅“Logstash、Fluentd、Fluent Bit 或 Vector？如何选择合适的开源日志收集器” ([http://mng.bz/4DrR](http://mng.bz/4DrR)) 或“Who Is the Winner: Comparing Vector, Fluent Bit, Fluentd Performance ([https://medium.com/ibm-cloud/log-collectors-performance-benchmarking-8c5218a08fea](https://medium.com/ibm-cloud/log-collectors-performance-benchmarking-8c5218a08fea))；然而，你应该将它们用作灵感或起点，而不是“购买指南”。换句话说，你很可能需要根据自己的需求和偏好自行评估，才能正确选择一个代理。按任意顺序，我为你列出可作为评估代理并挑选合适方案的起点标准。

### 4.5.1 Security for and of the agent

我所在的 AWS，安全是最高优先级，所以我从这里开始。重要的是同时考虑代理的安全*的*和安全*为*，我的意思是：

*   对于代理的安全*为*，请自问和/或向厂商询问以下问题：

    *   谁负责处理 CVE 和安全补丁？

    *   代理是否经过渗透测试，如果有，是在何时进行（例如，一次性、定期，或在主要组件变更时）？

    *   你能否获得可复现的构建产物，例如代理的容器镜像？

    *   代理的供应链是否已知和/或有文档记录？

*   对于代理的安全*的*，请尝试确定以下内容：

    *   代理是否支持 TLS/HTTPS 以及等效的加密安全流量？

    *   你能否在代理集群中应用策略（例如凭据、访问和过滤器）？

    *   是否已建立机制来处理敏感数据，例如个人身份信息（PII），以满足某些立法（如欧洲通过《通用数据保护条例》（GDPR））的要求？

理想情况下，你应通过提供代理的开源项目、ISV 或云服务商的清单，提前获得这些信息。

### 4.5.2 Agent performance and resource usage

代理性能——即信号的处理速度，通常通过吞吐量以及所用资源来衡量——是关键的评估标准。以下是此类性能相关报告的一些示例：

*   上游“OpenTelemetry Collector Performance”文档 ([http://mng.bz/QP96](http://mng.bz/QP96))。

*   AWS Distro for OpenTelemetry Collector（ADOT）收集器性能报告 ([http://mng.bz/N2J1](http://mng.bz/N2J1))。

聚焦性能时，一个重要标准简而言之与代理的资源占用有关。例如，一个标准可能是代理在各个阶段（例如空闲、平均或峰值）用于收集、转换和摄取信号时消耗多少 RAM 和/或 CPU 秒。

除此之外，你希望能够按信号类型进行分解，以防是多信号代理，如 OpenTelemetry 收集器。图 4.11 显示了 Grafana 仪表板 ([https://ref.otel.help/otel-collector-ops/otel-collector-dashboard.json](https://ref.otel.help/otel-collector-ops/otel-collector-dashboard.json)) 的示例屏幕截图，捕获了 OpenTelemetry 收集器随时间的资源使用情况。

![](OEBPS/Images/04-11.png)

图 4.11 OpenTelemetry 收集器资源使用情况的 Grafana 仪表板

在资源使用方面，你要确保代理使用的资源相对于你的整体占用来说很少。不要忘记累积效应——也就是说，虽然单个代理可能只使用几 MB，但在拥有数千个代理的整个集群中，这可能会累积到 TB 级别。如今大多数代理都有控制资源使用的机制（例如，在 OpenTelemetry 收集器的情况下，你可以使用内存限制处理器来防止内存不足（OOM）情况）。

### 4.5.3 Agent nonfunctional requirements

除了功能方面外，还应自问以下问题：

*   代理是开源的、可扩展的，还是由单一厂商或社区支持？

*   谁来监控监控者，或者换句话说，代理遥测怎么办？在 2023 年及以后，你应该期望不仅能够访问代理日志，还能获得指标（理想情况下为 Prometheus 格式）以及来自代理的追踪和剖析。

*   代理是否能够处理一种或多种信号类型？信号特定的代理意味着你需要多个代理才能实现 360 度覆盖。

非功能性需求远不止我们讨论的内容，但再次强调，这些问题构成了基础，在选择代理时你可以根据组织需求或偏好对其进行扩展。

## Summary

*   有两种类型的代理：厂商特定的代理，如 Datadog 代理，以及信号特定的代理（通常为开源），如用于日志的 Fluent Bit。

*   在本书中，我们将厂商特定和信号特定的代理视为传统代理，与开源的、包罗万象的 OpenTelemetry 收集器形成对比。

*   OpenTelemetry 是一组规范、SDK、协议（OTLP）和代理。

*   OpenTelemetry 允许你以厂商中立的方式收集、处理和摄取日志、指标、追踪以及（未来）剖析。

*   使用 OpenTelemetry，你可以获得有关信号来源（资源属性）的丰富元数据，以及关于遥测信号本身的元数据。

*   基于语义约定，OpenTelemetry 使你能够在后端实现有效的关联。

*   自动插桩是无需更改应用程序代码即可自动生成遥测的重要基线方法。

*   如果某个厂商或编程语言（在 OpenTelemetry 中）支持自动插桩，请使用它。

*   当自动插桩无法满足你的需求（业务逻辑）时，你需要使用 OpenTelemetry SDK 手动为代码插桩。

*   使用 OpenTelemetry 收集器从任何来源收集和路由追踪和指标，最终还有日志，到任何目标，形成一个通用遥测代理。

*   代理需要安全、高性能且精简（资源使用最少）。

*   选择代理可能需要时间和精力，但从长远来看是值得的投资。

# 5 Backend destinations

本章涵盖

*   我们所说的后端目标是什么以及存在哪些类型的目标

*   处理日志、指标和追踪的后端目标选项

*   什么是时序数据存储以及可以用它们做什么

*   介绍列式数据存储和格式及其用例

*   选择后端目标的注意事项

到目前为止，你应该知道存在哪些主要信号类型（参见第 2 章），它们来自哪里（参见第 3 章），以及收集和预处理这些信号有哪些选项（参见第 4 章）。在本章中，我们将深入探讨后端目标，即代理的目标或接收器。后端目标，或简称为后端，是你可观测性问题的事实来源。它们允许你存储和查询信号，并使其可用于前端目标（参见第 6 章）。

为什么我要区分后端和前端？嗯，有一些例子完全属于其中一类。例如，Grafana 是一个纯前端目标（有时也称为*展示阶段*），它查询后端目标以可视化或对信号发出警报。另一方面，Cloud Native Computing Foundation（CNCF）项目 Cortex 是一个纯后端目标（除了一个简陋的管理 Web UI，你只有用于摄取和查询指标的 API）。
