
### Digma
Digma ([https://digma.ai/](https://digma.ai/)) 被定位为 *连续反馈管道*（这似乎是一个营销术语，因为我没有找到正式定义），具有分析后端和 IDE 插件。其目标是持续分析可观测性来源并向您作为开发人员提供反馈。插件在您编辑代码时提供与性能、错误和用法相关的代码级洞察。Digma 从您的 `OpenTelemetry` 追踪和指标生成这些洞察，这些数据由 Digma 后端收集和分析。图 9.7（左侧）展示了插件在 `VS Code` 中的运行情况。

![](OEBPS/Images/09-07.png)

Figure 9.7 Digma 的屏幕截图

### Sprkl
Sprkl ([https://sprkl.dev/](https://sprkl.dev/)) 自称是“个人可观测性平台”，在 IDE 中编码时提供对您代码更改的个性化反馈。它使用 `OpenTelemetry` 对代码进行检测，并允许您在执行时探索代码更改。您可获得代码级追踪、对代码更改在系统其他实体中如何交互的洞察，以及代码性能报告。

目前，Sprkl 专注于 Node.js 代码，通过 Kubernetes 对 Java、C# 和 Python 提供部分支持。Sprkl 本地需要 Docker，并以 `VS Code`（在那里作为扩展提供）作为 IDE。设置好 `VS Code` 中的 Sprkl 并运行 Docker 后，探索其功能的良好起点是示例应用 `sprkl-dev/use-sprkl` ([https://github.com/sprkl-dev/use-sprkl](https://github.com/sprkl-dev/use-sprkl))。

### Tracetest
Tracetest ([https://tracetest.io/](https://tracetest.io/)) 与前两种工具略有不同。它专注于让您能够构建由 `OpenTelemetry` 追踪驱动的集成和端到端测试。这意味着您需要一个分布式追踪后端并将 Tracetest 指向它，或者 alternatively 直接将 spans 发送到 Tracetest，例如使用 `OpenTelemetry` collector。您可以将其用于沿请求路径对服务定义测试和断言，并对响应和追踪数据同时定义断言。此外，您可以手动运行测试或通过 CI 构建作业运行。图 9.8 显示了 Tracetest 测试结果，以瀑布方式呈现请求。

![](OEBPS/Images/09-08.png)

Figure 9.8 Tracetest UI（结果视图）的屏幕截图

您可以通过 `Docker Compose` 或 `Helm` chart 安装 Tracetest ([http://mng.bz/nWG5](http://mng.bz/nWG5))，并提供多个示例集成，例如与 AWS X-Ray ([http://mng.bz/vnEp](http://mng.bz/vnEp)) 或 Grafana Tempo ([http://mng.bz/4DwR](http://mng.bz/4DwR))。

### Rookout
Rookout ([https://www.rookout.com/](https://www.rookout.com/)) 自称是“以开发者为先的可观测性工具”。它在您的技术栈中捕获实时调试数据，支持 Java、.NET、Node.js、Python、Ruby 和 Go。Rookout 允许您在多种环境中进行实时调试，并可在可用断点旁查看追踪。

### Autometrics
Autometrics ([https://autometrics.dev/](https://autometrics.dev/)) 建立在 `Prometheus` 和 `OpenTelemetry` 客户端库之上。它使在代码中对函数进行检测变得容易（目前支持 Rust、Go、Python、Typescript 和 C#），使用通用指标，包括请求率、错误率和延迟，并为您生成 `PromQL` 查询。

至此，我们已完成工具评述。现在，让我们讨论在该领域采用工具时应考虑的问题。

## 9.3 工具考量
我们在本章中研究了持续剖析以及更广泛的开发者生产力工具，到现在您可能已兴奋地想要深入其中。在您走得太远之前，我想在以下小节中分享一些想法和谨慎之词。虽然我个人非常看好并支持，但我们必须认识到其中的棱角并正确设定期望。

开发者可观测性是一个相对年轻的领域，许多竞争者上市时间不足一年。以下是专门针对 CP 工具所面临挑战的讨论。

### 9.3.1 符号化
通过 *符号化*，我们指的是将机器地址翻译为人类可读（符号）名称的过程。例如，如果您查看一个剖析文件，其中包含堆栈帧标签，如 `0x8c6fff910`，您就不会真正知道该内存地址代表什么。另一方面，如果您看到类似 `/some/path/ file.go:42` 或 `calculateSummary()` 的内容，那就具有意义。

现在的挑战是确定符号信息的所在位置。对于编译型语言，如 C/C++ 或 Go，符号表（包含所用符号）通常是二进制文件的一部分，而对于解释型语言，包括 Python、Ruby 或 JavaScript，符号通常是程序的一部分。

有关此主题的进一步阅读，请参阅以下博客文章：
*   “Fantastic Symbols and Where to Find Them” 作者 Kemal Akkoyun ([http://mng.bz/QPB6](http://mng.bz/QPB6))
*   “BPF binaries: BTF, CO-RE, and the Future of BPF Perf tools” 作者 Brendan Gregg ([http://mng.bz/XNR9](http://mng.bz/XNR9))
*   “BPF CO-RE (Compile Once – Run Everywhere)” 作者 Andrii Nakryiko ([https://nakryiko.com/posts/bpf-portability-and-co-re/](https://nakryiko.com/posts/bpf-portability-and-co-re/))

一旦我们获得了符号，我们的工作还未完成；我们仍需要存储剖析文件。但如何在高效且经济的方式下做到这一点呢？

### 9.3.2 存储剖析文件
在内存中存储剖析文件对于某些用例是足够的，当您检查当前发生的情况时。然而，当您想要比较更长的时间范围或服务的不同版本时，您需要能够处理剖析文件的量并以持久的方式将其存储在某处。

在持续剖析工具中发现的两种常见技术是
*   *为了高效存储和查询剖析文件，使用了列式存储（另请参阅第 5 章，其中我们讨论了列式数据存储的设计和属性）。* 例如，在 `Parca` 的背景下，团队决定开发 `FrostDB` ([https://github.com/polarsignals/frostdb](https://github.com/polarsignals/frostdb))，这是一个专用的剖析列式数据库，用 Go 编写。您可以嵌入 `FrostDB`，它具有半结构化模式（其中一些或全部模式字段和/或类型未知或未定义），使用 `Apache Parquet` 进行存储，使用 `Apache Arrow` 进行查询。
*   *为了最小化时间戳的存储占用，持续剖析后端通常使用 XOR 压缩。* 可以将其视为时间戳的 delta-of-delta 编码。这种技术最初由 Facebook 工程师团队在 “Gorilla: A Fast, Scalable, In-Memory Time Series Database” 中描述，由 Tuomas Pelkonen 等人撰写（2015 年，Proceedings of the VLDB Endowment；[http://www.vldb.org/pvldb/vol8/p1816-teller.pdf](http://www.vldb.org/pvldb/vol8/p1816-teller.pdf)）。如果您想了解该领域压缩的更多信息，请查看 Joshua Lockerman 和 Ajay Kulkarni 的 “Time-Series Compression Algorithms, Explained” ([https://www.timescale.com/blog/time-series-compression-algorithms-explained/](https://www.timescale.com/blog/time-series-compression-algorithms-explained/))。

一旦高效存储，您将希望查询剖析文件。那么这里的挑战是什么？

### 9.3.3 查询剖析文件
理想情况下，我们能够支持对剖析文件的表达性查询。目前，主要存在简单的基于标签的查询语言，例如 `Parca` ([https://www.parca.dev/docs/querying-parca](https://www.parca.dev/docs/querying-parca)) 中的以及 `Pyroscope` 的 `FlameQL` ([https://pyroscope.io/docs/flameql/](https://pyroscope.io/docs/flameql/))。这本身并不是坏事，因为 `Prometheus` 广泛使用且流行的查询语言 `PromQL` 也是基于标签的查询语言。然而，这里的问题是（开放）标准是否以及何时会出现，以及它将是与 SQL 相关还是与 DSL 相关的格式。
