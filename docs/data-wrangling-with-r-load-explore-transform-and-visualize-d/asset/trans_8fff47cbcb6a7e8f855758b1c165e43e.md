
考虑一种情况，我们需要在测试数据库和生产数据库上执行 Athena 查询。测试查询和生产查询之间的区域、账户和缓存因素各不相同。而不是在每次 API 调用中单独配置它们，我们可以使用全局配置一次性自定义它们。

这里的另一个优势是，您可以自定义配置变量以添加自己的变量，并进一步自定义 `awswrangler` 在您项目中的行为。

到目前为止，我们已经学习了多种安装和配置 `awswrangler` 包以满足您特定用例的方法。现在，我们将探讨各种 AWS 服务与 `awswrangler` 的集成，以及如何高效地使用这些服务。

## 使用不同 AWS 服务的 pandas AWS SDK 功能

我们现在将探索 `awswrangler` 包与 AWS 服务的集成。`awswrangler` 可与更多 AWS 服务配合使用，我们已解释了它与一些常用服务（如 Amazon S3、RDS 数据库、Amazon Redshift 和 Amazon Athena）的集成。

## Amazon S3

Amazon 的 **简单存储服务 (S3)** 是用于结构化、半结构化和非结构化数据的最大且性能最强的对象存储服务，也是构建数据湖的首选存储服务。Amazon S3 允许您以无限规模迁移、存储、管理和保护所有结构化和非结构化数据。通过 S3 数据湖，您可以打破数据孤岛、分析多样化的数据集、在一个集中位置管理数据访问并加速机器学习。Amazon S3 在设计上是安全的，按需可扩展，并且能够抵御整个 AWS 可用区故障。您可以使用 AWS 原生服务并与第三方服务提供商集成，在您的数据湖上运行应用程序。

以下是在 `awswrangler` 中与 Amazon S3 配合可用的主要功能：

- **从 S3 读取/下载数据**：我们有函数可从 S3 路径读取 JSON、CSV、Excel、Parquet 和定宽文件格式的数据到 pandas DataFrame 中。这些函数有助于将数据从 S3 位置加载到 DataFrame 中，以便使用 pandas 函数进行处理。它们还提供分区下推、加载在特定时间后修改/加载的对象（用于增量数据加载）、以可配置的块加载大文件数据以及提供数据多种格式化选项（pandas 或 PyArrow 参数）等额外功能。

要将数据从 S3 位置获取到本地路径/本地服务器，用户可以利用下载函数并在本地机器上对数据进行进一步处理。同样，如果用户想要查询 S3 对象中的数据以进行调试并了解用于目录更新的元数据，S3 Select 和 `read_parquet_metadata` 函数都会有所帮助。

- **向 S3 写入/上传数据：** 使用 `awswrangler` 库，可以将数据以 JSON、CSV、Parquet 和 Excel 等各种文件格式写入 S3。用户可以选择以各种格式在 S3 中存储数据集。这些函数还提供创建分区并更新 Glue 目录的选项，以便可以通过 Athena 和 Redshift Spectrum 查询数据。数据可以以多种模式保存，例如追加（添加新文件）、覆盖（替换目标路径下的所有文件）和覆盖分区（仅覆盖该分区中的文件）。用户可以配置 S3 函数以支持模式演进（允许缺失或新增列）并允许 Athena 的分区投影。

如果您想从本地/本地位置将数据导出到 S3，可以利用 `upload` 函数。`download` 和 `upload` 函数使用文件系统函数读取和写入数据，并使用多线程逻辑提高效率。

## Parquet 文件处理有何不同？

读取和写入其他文件格式时，`awswrangler` 使用 pandas 函数读取和写入文件。然而，对于 Parquet 文件的加载/写入，`awswrangler` 将利用 Arrow 库以实现更快的 数据加载并在内存中映射它们。请阅读介绍部分中有关使用 Arrow 库的好处以获取更多详细信息。

## 使用 awswrangler 的 Amazon S3 实用示例

让我们使用一些实际示例来进一步了解 `awswrangler` 读取/写入 S3 的概念。

我们将从最近的天气数据集中加载 CSV 数据并将其存储为 JSON 和 Parquet 格式。然后，我们将以每种格式加载数据并验证每种格式的加载时间：

- 1. 在笔记本中安装 `memory_profiler` 库以跟踪内存使用情况，以及加载和卸载数据所需的时间：

```
1]: pip install memory_profiler
Looking in indexes: https://pypi.org/simple, https://pip.repos.neuron.amazonaws.com
Collecting memory_profiler
  Downloading memory_profiler-0.60.0.tar.gz (38 kB)
Preparing metadata (setup.py) ... done
Requirement already satisfied: psutil in /home/ec2-user/anaconda3/envs/amazonei_pytorch_latest_p37/lib/python3
```

图 3.42：S3 集成 – 安装内存分析器

- 2. 我们将加载约 4 GB 数据到 pandas DataFrame 中，顺便说一下，这是一个用于执行数据加载基准测试的大型数据集。

```
(base) ssm sun038f9d355c9cf aws-data-wrangler % aws s3 ls s3://noaa-ghcn-pds/csv/by_year/202 --human-readable --summarize
2022-10-05 13:59:29  1.5 GiB 2020.csv
2022-10-05 13:57:46  1.5 GiB 2021.csv
2022-10-05 13:57:37  1.1 GiB 2022.csv

Total Objects: 3
Total Size: 4.1 GiB
```

图 3.43：S3 集成 – CSV 文件列表以了解文件大小

以下屏幕截图显示了来自 Parquet 文件（Snappy 压缩）的类似数据量的存储大小，明显更小。这展示了以 Parquet 格式存储数据集的价值，因为它能高效存储数据集，并在运行分析查询时性能非常高。

```
2ebfc_0.snappy.parquet
2022-10-05 13:57:36   4.6 KiB parquet/by_year/YEAR=2022/ELEMENT=WT10/aba894e511414a32b2b4fed17f2
2ebfc_0.snappy.parquet
2022-10-05 13:57:36   15.0 KiB parquet/by_year/YEAR=2022/ELEMENT=WT11/aba894e511414a32b2b4fed17f2
2ebfc_0.snappy.parquet

Total Objects: 383
  Total Size: 286.6 MiB
```

图 3.44：S3 集成 – Parquet 文件列表以了解文件大小

- 3. 验证以 CSV 和 Parquet 文件格式加载数据所需的时间。

Parquet 文件加载指标如下所示：

```
%time
%memit
#Load compressed parquet file of size 286MB
weather_data=wr.s3.read_parquet("s3://noaa-ghcn-pds/parquet/by_year/YEAR=202")
```

峰值内存：26221.25 MiB，增量：26067.42 MiB
CPU 时间：用户 2分19秒，系统：28.6 秒，总计：2分48秒
墙钟时间：2分26秒

weather_data.info()

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 98619752 entries, 0 to 98619751
Data columns (total 7 columns):
  #   Column      Dtype
---  ---------   -----
 0   ID          string
 1   DATE        string
 2   DATA_VALUE  Int64
 3   M_FLAG      string
 4   Q_FLAG      string
 5   S_FLAG      string
 6   OBS_TIME    string
dtypes: Int64(1), string(6)
memory usage: 5.2 GB
```

图 3.45：S3 集成 – Parquet 文件加载指标

前述 Parquet 文件的指标取自 t3.xlarge 实例。类似的实例无法将 CSV 文件加载到内存中，抛出错误并卡住。由于加载 CSV 文件是内存密集型操作，我们将笔记本实例从 t3.xlarge 更改为 r5.2xlarge。更改后，CSV 文件能够加载得更快，如以下屏幕截图所示。

```
%time
%memit
#Load CSV file with 4.1GB of data
weather_data=wr.s3.read_csv("s3://noaa-ghcn-pds/csv/by_year/202")
```

峰值内存：28901.76 MiB，增量：28747.74 MiB
CPU 时间：用户 1分15秒，系统：32.1 秒，总计：1分47秒
墙钟时间：1分13秒

```
weather_data.info()
```

<class 'pandas.core.frame.DataFrame'>
RangeIndex: 98707563 entries, 0 to 98707562
Data columns (total 9 columns):
#  Column      Dtype
---  ------      -----
0  Unnamed: 0  int64
1  ID          object
2  DATE        int64
3  ELEMENT     object
4  DATA_VALUE  int64
5  M_FLAG      object
6  Q_FLAG      object
7  S_FLAG      object
8  OBS_TIME    float64
dtypes: float64(1), int64(3), object(5)
memory usage: 6.6+ GB

图 3.46：S3 集成 – CSV 文件加载指标

这本质上展示了为特定用例使用合适的计算容量以获得更好性能的优势。

前述练习还展示了以不同格式读取数据的便捷性，并突出了将数据以 Parquet 文件格式存储用于分析用例的好处。

## 其他 awswrangler S3 函数

以下是 `awswrangler` 可以执行的一些附加功能：

- **复制对象：** 我们使用 boto3 中的 `s3.copy` 函数一次复制一个对象。通常，用户会迭代函数以复制多个对象。`awswrangler` 使从一个 S3 前缀/文件夹轻松复制多个文件到另一个 S3 位置变得简单。`copy` 函数将源文件附加到新位置。合并对象函数可以根据配置参数覆盖目标位置中的文件或将文件附加到目标位置。
- **列出对象：** 此函数接受后缀和前缀参数以从 `s3` 路径过滤文件。它采用通配符字符格式进行文件列表。这是通过文件名匹配库实现的（https://docs.python.org/3/library/fnmatch.html）。在列出文件后，它还可以按时间戳过滤文件，这在从路径列出文件以进行增量数据处理时非常有利。

以下屏幕截图显示了从 S3 位置创建的在特定时间戳之后的文件列表。您可以看到该位置已有 CSV 文件，但当我们限制列表仅显示最近一分钟内更改/上传的文件时，列出对象不会列出任何文件。

