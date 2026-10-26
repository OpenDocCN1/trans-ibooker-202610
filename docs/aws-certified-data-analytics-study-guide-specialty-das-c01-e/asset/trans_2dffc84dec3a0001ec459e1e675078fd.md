

## 使用 Amazon Kinesis Firehose 进行数据转换

虽然捕获数据至关重要，但您通常还需要对高速数据流执行数据转换。您可以使用 Amazon Kinesis Firehose 启用数据转换，它默认会缓冲最多 3 MB 的传入数据。Kinesis Firehose 会以同步方式调用指定的 Lambda 函数，每个缓冲批次使用 AWS Lambda 同步调用模式。任何 Lambda 调用的请求和响应都有 6 MB 的最大负载大小限制，因此您必须确保函数的缓冲大小小于或等于 6 MB。

您还可以将预构建的 Lambda 蓝图与 Kinesis Data Firehose 结合使用。以下 Lambda 蓝图可用于最常见的转换：

- 通用 Firehose 处理：用于任何自定义转换逻辑。
- Apache 日志转 JSON
- Apache 日志转 CSV
- Syslog 转 JSON
- Syslog 转 CSV
- Kinesis Data Firehose 将记录流作为源处理：一个基本 Lambda 函数，用于访问输入中的 Kinesis Data Stream 记录并返回处理状态。
- Kinesis Data Firehose CloudWatch Logs 处理器：从 CloudWatch Logs 订阅筛选器发送的记录中解析并提取每个单独的日志事件。

Kinesis Firehose 的每次 Lambda 函数调用最长可持续 5 分钟。

## 在 Amazon Kinesis 中转换输入记录格式

最常见的分析需求之一是将行格式转换为列格式，以优化分析查询。客户最常用的列格式是 Parquet 或 ORC，因为它们不仅可以压缩数据，还能为分析查询提供最佳性能。Amazon Kinesis Firehose 允许您将 JSON 数据转换为 Parquet 或 ORC。Amazon Kinesis Firehose 需要三个要素来转换数据的记录格式：

1. 用于读取输入数据 JSON 的反序列化器：您可以选择以下两种反序列化器之一：
   - Apache Hive JSON SerDe
   - OpenX JSON SerDe
2. 用于确定如何解释数据的模式：您可以使用 AWS Glue 从数据中推断模式，并将其存储在 Glue Data Catalog 中。然后，Kinesis Firehose 会引用该模式并使用它来解释您的数据。

您还可以为 Kinesis Data Analytics 应用程序使用 Apache Flink 接口。本质上，您可以在自己熟悉的集成开发环境（IDE）中创建 Flink 应用程序，然后将应用程序代码上传到 Kinesis Data Analytics。这样，您就可以在完全托管的弹性服务中运行应用程序。然后，您可以使用 AWS 生态系统来管理应用程序。例如，您可以通过 Amazon CloudWatch 配置各种指标（如应用程序、任务、操作符和并行度），还可以在 AWS Management Console 中显示作业图。

Kinesis 提供 Kinesis 处理单元（KPU）作为运行代码的资源。在撰写本文时，每个 KPU 包含 1 个 vCPU、4 GB 内存和 50 GB 存储。应用程序所需的 KPU 数量通过 Parallelism 和 ParallelismPerKPU 属性计算：

- Parallelism – 此属性用于设置默认的 Apache Flink 应用程序并行度。除非在应用程序中显式覆盖，否则所有操作符、接收器和来源都将使用此并行度。默认值为 1，最大值为 64。

- ParallelismPerKPU – 此属性用于设置应用程序每个 KPU 可调度的并行任务数。默认值为 1，最大值为 8。

请注意，这些限制是软限制，可以通过请求提高限制来增加。

您可以使用 Kinesis 应用程序的自动扩缩容功能，根据需求和所需吞吐量上下调整资源（图 2.19）。

### 图 2.19 Kinesis Data Analytics - Flink 接口

![](img/fc6a7083d65b0716921d7bd4a5298460_100_0.png)

您可以在 AWS 控制台上配置 Kinesis Data Analytics 应用程序时，配置并行度和每个 KPU 的并行度，同时启用自动扩缩容（图 2.20）。

### 图 2.20 使用 Kinesis 扩展您的 Flink 应用程序

![](img/fc6a7083d65b0716921d7bd4a5298460_101_0.png)

## Amazon Kinesis Video Streams

随着 IoT 和集成设备的出现，您可以捕获其他测量数据，现在还可以捕获视频数据，这些数据需要以实时方式进行存储、分析和响应。视频对于智能家居、智慧城市、视频监控安全、工业自动化和计算机视觉等许多应用至关重要。虽然实时传输、处理和分析视频的需求不断增长，但构建这样的视频摄取系统至少可以说是相当复杂的。事实证明，您需要集成许多不同的设备和操作系统，而为各种设备和开发环境编写代码非常复杂。此外，随着设备数量的增长，您需要扩展流式传输和分析应用程序，同时支持流上的延迟、节奏和抖动。需要提供 API 来检索视频、处理视频并重放视频。

视频流分析是媒体客户最常提出的需求之一，亚马逊在 2017 年的 re:Invent 大会上发布了 Amazon Kinesis Video Streams，其主要目标是传输视频和时间编码数据以供分析。Amazon Kinesis Video Streams 是一项完全托管的可扩展服务，允许您从数百万个来源传输视频，包括智能手机、安全摄像头、网络摄像头、无人机、汽车内置摄像头等。您还可以发送非视频的时间序列化数据，例如音频数据、热成像、深度数据、RADAR（无线电探测和测距）数据等。当来自这些来源的实时视频流进入 Kinesis 视频流时，您可以构建应用程序，逐帧实时访问数据以进行低延迟处理。Amazon Kinesis Video Streams 与来源无关；您可以使用 GStreamer 库从计算机的网络摄像头传输视频，或使用 RTSP 从网络上的摄像头传输视频。

Amazon Kinesis Video Streams 不仅允许您捕获这些数据，还可以轻松构建支持视觉的应用程序。它提供持久存储，您可以在其中配置保留期限、加密静态数据，并根据生产者时间戳和摄取时间戳为数据添加时间戳（图 2.21）。存储的数据可搜索，并允许您与各种下游服务集成，例如 Amazon AI 服务、Apache MXNet、TensorFlow 和其他第三方提供商。Kinesis Video Streams 为客户带来的主要优势如下：

- 连接并从数百万台设备传输流。
- 以持久方式存储和索引数据。
- 专注于业务案例，而不是基础设施管理。
- 按需付费的定价模式。
- 以安全方式传输数据。
- 构建实时和批处理应用程序。

关键概念如下：

- **生产者** – 生产者是向 Kinesis Video Streams 提供数据的任何应用程序、设备或来源。生产者可以是生成视频的设备，甚至可以发送非视频数据，如音频流、图像或 RADAR 数据。

- **Kinesis Video Producer Libraries** – 这些库应安装在您的设备上，允许您连接到 Kinesis 流，并实时或通过缓冲将视频传输到流中。

- **Kinesis Video Streams** – 生产者将视频放入 Kinesis 视频流，这基本上是一种资源，使您能够传输实时视频数据，可选地存储该数据，并让数据可供实时消费以及批量或临时消费。您可以从 AWS 控制台或使用 AWS SDK 创建 Kinesis 视频流（见图 2.22）。

