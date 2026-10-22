

## 第4章 使用 Cloud Dataflow 构建应用数据管道

## 早期发布读者的说明

使用 Early Release 电子书，您可以在这些书籍正式发布之前就获得书籍的最早形式——作者在写作时的原始未编辑内容——从而能够在这些技术正式发布之前就利用它们。

这将是最终书籍的第4章。请注意，GitHub 仓库将在稍后激活。

如果您对如何改进本书的内容和/或示例有任何意见，或者如果您注意到本章中缺少材料，请联系编辑 mcronin@oreilly.com。

在本章中，您将了解 Cloud Dataflow，它在框架内用作完全托管的数据处理管道。在第5章中，您将部署云原生应用的第二部分，即在 Cloud Dataflow 上运行的管道。供参考，您目前处于框架的流程阶段，您将在此阶段将 Google Cloud 服务组合起来将音频文件转换为文本、丰富它们，并开始使用 Cloud Dataflow 将数据流式传输到 BigQuery（见图4-1）。

![](img/eac8762c24ec77948aa0c3277f8e6eb3_112_0.png)

图4-1. 语音分析框架

## Cloud Dataflow 概述

Cloud Dataflow 是用于运行 Apache Beam 管道的完全托管服务，可让您执行各种数据处理任务。Dataflow 可以从不同来源移动数据：它可以转换数据，并且可以实时执行诸如

检测异常之类的数据任务。在框架内，Dataflow 将从 Pub/Sub 读取数据，调用 Speech-to-Text 和 Natural Language API，丰富数据，最后将其写入 BigQuery。Google 于2016年将 Google Cloud Dataflow SDK 捐赠给 Apache 软件基金会，其他组织贡献了运行器和 IO 以将 Beam 运行器与现有数据库集成，这使得该项目在功能和社区支持方面不断发展。

Apache Beam 是一个编程模型，用于定义和执行定义的管道。管道可以是批处理和流式处理，它们对不同的运行器公开如下：

- Google Cloud Dataflow
- Apache Spark
- Apache Flink
- Apache Apex
- DirectRunner（用于测试的本地运行器）

其思想是相同的管道可以在不同的运行器上执行，从而允许对使用哪个运行器进行概率和选择。对于框架，您将使用 Google Cloud Dataflow 运行器。选择 Cloud Dataflow 作为运行器可以让您专注于管道，并允许 Cloud Dataflow 按需管理资源并自动扩展以实现最小延迟和高资源利用率。在 Google Cloud 上运行管道时，运行器会将可执行代码和依赖项上传到 Cloud Storage，然后创建一个执行管道的 Dataflow 作业。您可以通过编程方式设置运行器，或使用命令行指定它。以下示例显示了如何通过以编程方式将运行器设置为 Dataflow 来构建管道。

```
options = PipelineOptions(
    flags=argv,
    runner='DataflowRunner',
    project='my-project-id',
    job_name='unique-job-name',
```

```
temp_location='gs://my-bucket/temp',
region='us-central1')
```

要将管道在 Apache Spark 上执行，您只需将运行器更改为 SparkRunner，以下代码是设置管道运行器为 Apache Spark 的示例。

```
options = PipelineOptions(
    flags=argv,
    runner='SparkRunner',
    project='my-project-id',
    job_name='unique-job-name',
    temp_location='gs://my-bucket/temp',
    region='us-central1')
```

选择 Apache Beam 并以 Cloud Dataflow 作为运行器可以让您专注于管道以及所需的任何转换，而不是支持基础设施和管道。

## Apache Beam 概念

在本节中，您将进一步阅读 Apache Beam 概念，这将使您能够理解设计和构建数据处理管道所需的各个组件。

### 管道

管道定义了运行器将在您的数据上执行的步骤。简单来说，管道是输入数据、数据上的转换以及数据输出的表示。

图4-2 是管道的简单表示。

![](img/eac8762c24ec77948aa0c3277f8e6eb3_114_0.png)

*图4-2. 简单的管道表示*

图4-3 是您将使用 Dataflow 作为运行器部署的框架管道的示例。

![](img/eac8762c24ec77948aa0c3277f8e6eb3_115_0.png)

图4-3. 框架管道

以下示例代码显示了管道如何读取和写入用于运行作业的参数中定义的相关文件。

```
with beam.Pipeline(argv=beam_args) as pipeline:
    lines = pipeline | 'Read files' >>
beam.io.ReadFromText(args.input)
    lines | 'Write files' >>
beam.io.WriteToText(args.output)
```

您可以按如下方式为上述管道传递运行作业的参数：

```
python3 dataflow.py --input [FILE_NAME] --output [FILE_NAME] --runner DataFlowRunner --project [PROJECT_ID] --temp_location gs://[BUCKET]/temp --staging_location gs://[BUCKET]/stag --region us-central1 --job_name myjob
```

### PCollection

PCollection 定义了管道将操作的数据。数据可以来自固定源，如文件，也可以来自持续更新的源，如 Cloud Pub/Sub。在框架中，数据将来自 Cloud Pub/Sub 的持续更新源，是无界的。它将通过从 Cloud Pub/Sub 读取数据创建初始 PCollection，并且从那里它将为管道中的每个步骤创建 PCollection 输出以将数据写入 BigQuery。

![](img/eac8762c24ec77948aa0c3277f8e6eb3_116_0.png)

图4-4. Apache Beam 管道

### 转换

转换是您定义并在数据上执行的功能。您定义的功能将在数据的每个元素上执行您定义的操作并产生一个或多个 PCollection 作为输出。您的功能可以对您的数据执行几乎任何操作，包括使用外部库的能力。作为示例：在此框架中，您正在丰富数据以收集用户说话时间的指标，您还使用数据丢失防护库在将元素发送到下一个转换之前从元素中删除 PII 数据。但如果您还想从原始音频文件中删除 PII 语音段，您可以使用转换并使用您定义的功能导入 ffmpeg 包，这将允许您静音找到的 PII 信息。转换的输出元素可以是丰富数据的连续流，并且还可以将删除后的数据写入 Google Cloud Storage。

您正在使用的框架执行以下转换：

1. 从 Cloud Pub/Sub 读取消息

```
decode_messages = messages | 'DecodePubSubMessages'
>> beam.Map(lambda x: x.decode('utf-8'))
```
2. 使用异步语音识别从 Speech-To-Text API 检索长音频文件的数据

```
stt_output = decode_messages | 'SpeechToTextOutput'
>> beam.Map(stt_output_response)
```
3. 解析并丰富 Speech-To-Text API 数据

```
parse_stt_output = stt_output | 'ParseSpeechToText'
>> beam.Map(stt_parse_response)
```
4. 从 Natural Language API 获取 ParseSpeechToText 转换中元素的情感和实体响应

```
stt_nlp_output = parse_stt_output |
'NaturalLanguageOutput' >> beam.Map(get_nlp_output)
```
5. 如果管道参数的执行包含 dlp=true，则处理来自 NaturalLanguageOutput 转换的元素以

# 使用 Google Cloud DLP API 进行删除

```
dlp_output = stt_nlp_output | 'RedactTextOptional'
>> beam.Map(lambda j: redact_text(j, project_id))
```

框架还使用更高级别的 Map 转换，如下面的代码片段所示，图4-5 表示 Dataflow 界面中的可视化转换，图4-6 和 4-7 表示转换的输入和输出元素。

```
# function to redact sensitive data if dlp key value is set to true
def redact_text(data, project):
    info_types = []
    deidentify_config = {
        "info_type_transformations": {
            "transformations": [
                {
                    "primitive_transformation": {
                        "replace_config": {
                            "new_value": {
                                "string_value": '#',
                            }
                        }
                    }
                }
            ]
        }
    }
    if data['dlp'] == 'true' or data['dlp'] == 'True':
        dlp = google.cloud.dlp_v2.DlpServiceClient()
        parent = dlp.project_path(project)
        response = dlp.list_info_types('en-US')
        for info_type in response.info_types:
            info_types.append({'name': info_type.name})
        inspect_config = { "info_types": info_types}
        item = {"value": data['transcript']}
        response = dlp.deidentify_content(
            parent,
            inspect_config=inspect_config,
            deidentify_config=deidentify_config,
