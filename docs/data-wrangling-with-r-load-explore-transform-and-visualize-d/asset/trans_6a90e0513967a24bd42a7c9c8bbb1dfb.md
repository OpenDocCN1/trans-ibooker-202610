
```python
from platform import python_version
print(python_version())
```

3.8.12

```python
from python38_custom import awswrangler as wr1
```

```python
print(wr1.__version__)
print(wr1.__description__)
print(wr1.__message__)
```

2.17.0
Pandas on AWS。已为自定义安装更新。
这是作为学习练习的一部分而更改的

图 3.38：awswrangler SageMaker 自定义安装 – 检查安装状态

## 使用 EMR 安装 pandas 的 AWS SDK

我们可以通过配置 bootstrap 脚本在新的 EMR 集群上安装 awswrangler。创建一个包含以下命令的 bootstrap 脚本并将其上传到 S3 存储桶中：

```bash
#!/usr/bin/env bash
set -ex
sudo pip3 install pyarrow==2 awswrangler==2.17.0
```

创建一个包含项目所需软件的 EMR 集群，如果您想使用 Jupyter notebook 交互式开发脚本，还可以启用 JupyterHub 和 Livy。我们将启动一个与此集群关联的 Jupyter notebook。您可以通过打印版本号和描述来验证 awswrangler，正如我们之前所做的那样。

```python
import awswrangler as wr
print(wr.__version__)
print(wr.__description__)
```

最后执行于 2022-10-02 21:16:49，耗时 71ms

2.17.0
Pandas on AWS。

图 3.39：awswrangler EMR 安装 – 检查安装状态

您还可以使用 sc.list_packages() 命令来列出 sparkContext 对象中的软件包，从而验证 awswrangler 的安装。

```python
[2]: sc.list_packages()
Last executed at 2022-10-02 21:14:26 in 1.31s
```

```
aenum (3.1.11)
aiohttp (3.8.1)
aiosignal (1.2.0)
asn1crypto (1.5.1)
async-timeout (4.0.2)
asynctest (0.13.0)
attrs (22.1.0)
aws-cfn-bootstrap (2.0)
awswrangler (2.17.0)
backoff (2.1.2)
beautifulsoup4 (4.9.3)
```

图 3.40：awswrangler EMR 安装 – 检查已安装的软件包

您可以在 EMR 集群上上传自定义的 `awswrangler` 软件包并安装以在 EMR 集群中使用。参考 EMR 文档 (https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-jupyterhub-install-kernels-libs.html) 安装额外的软件包和内核。

我们花费了大量时间在不同环境中安装 `awswrangler`，以便贵组织中的不同角色能够使用 `awswrangler` 库。此外，我们还展示了如何对 `awswrangler` 进行更改并在所有环境中部署，以更适合贵组织的需求。AWS SDK for pandas 库已提供大量功能，大多数客户可以在其项目中使用官方库，因此我们提供了针对不同角色的使用方法。如果您的项目需要根据需求自定义 `awswrangler` 库，我们也展示了不同的实现方式。现在，我们将探讨如何设置配置选项，使 `awswrangler` 更符合您项目特定需求。

## AWS SDK for pandas 的配置选项

在 `awswrangler` 中配置全局变量的选项可以帮助我们使 `awswrangler` 库更符合我们的特定用例。我们将深入了解如何设置配置来自定义 `awswrangler` 的工作方式。

### 设置全局变量

`awswrangler` 中的全局配置变量可以通过两种不同的机制进行设置：

- **环境变量**：在 `awswrangler` 软件包将被执行的 operating system/kernel 环境中创建环境变量。例如，在 Jupyter notebook 中，使用 `%env` 命令为环境变量设置值。这是在 `awswrangler` 对象实例化时加载的。
- **Wrangler 配置变量**：您也可以直接使用 `wrangler` 对象为环境变量设置值。使用 `awswrangler.config.<<variable name>>` 语法为该配置分配值。

### 配置如何应用？

Python 有一个有趣的特性叫做装饰器，用于为现有代码添加功能。这也称为元编程，因为程序的一部分试图在编译时修改程序的另一部分。装饰器确保在函数被调用之前应用配置，使用 `@decorator_func` 语法在 `awswrangler` 源代码中针对不同服务。

请参考以下截图，其中装饰器函数在 Athena `read_sql_query` 函数之前应用，这将在代码中设置与 Athena 相关的全局变量。`apply_configs` 函数在 `config.py` 脚本中定义，如果需要更深入地了解，可以阅读它。

```python
@apply_configs
def read_sql_query(
    sql: str,
    database: str,
    ctas_approach: bool = True,
    unload_approach: bool = False,
    unload_parameters: Optional[Dict[str, Any]] = None,
    categories: Optional[List[str]] = None,
    chunksize: Optional[Union[int, bool]] = None,
    s3_output: Optional[str] = None,
    workaround: Optional[str] = None,
```

图 3.41：awswrangler – 一个 config.py 代码片段

`awswrangler` 软件包中提供以下类别的变量。然而，用户可以根据特定项目需求向代码添加配置变量：

- **Endpoint URL**：为 `awswrangler` 用于连接 AWS 环境的各种服务提供端点 URL。要以编程方式连接到 AWS 服务，您使用端点，它是 AWS Web 服务的入口点。AWS SDK 和 AWS **命令行界面 (CLI)** 会自动为 AWS 区域中每个服务的默认端点使用，但您可以使用此变量为 API 请求指定替代端点。一个示例是连接到特定区域或使用 FIPS 端点以使用 TLS 连接增强安全性。
- **botocore 配置变量**：这些是 boto 环境的配置参数。有关可以传递给 botocore 配置的参数的更多详细信息，请参阅 boto3 文档。boto3 配置中参数的一些示例包括区域名称、输出格式、连接超时和重试次数。有关可以配置的变量的更多详细信息，请参阅 botocore 文档 (https://botocore.amazonaws.com/v1/documentation/api/latest/reference/config.html)。
- **通用配置变量**：这些是用于影响各种服务行为的通用配置 - 例如，我们可以配置块大小参数以从 S3 读取数据。

## 与 Athena 缓存行为相关的参数

**awswrangler** 可以检索在特定时间段内已执行的 Athena 查询结果。我们可以使用配置变量来配置此行为，以在 *X* 秒或 *X* 个查询内返回预执行的数据，以便用户更快地获取有效数据。以下变量有助于配置 Athena 缓存行为：

- **MAX_CACHE_SECONDS**：**awswrangler** 可以在 Athena 的历史记录中检查此查询是否之前已运行。如果是，并且其完成时间距离当前查询小于 **max_cache_seconds**，**awswrangler** 将跳过查询执行并仅返回上次的相同结果。
- **MAX_CACHE_QUERY_INSPECTIONS**：这是将从历史记录中检查以尝试找到一些可重用结果的最大查询数。它仅在 **max_cache_seconds > 0** 时生效。
- **MAX_REMOTE_CACHE_ENTRIES**：这是将从 AWS 检索用于缓存检查的最大查询数。它仅在 **max_cache_seconds > 0** 时生效，默认值是 **50**。
- **MAX_LOCAL_CACHE_ENTRIES**：这是将本地缓存元数据的最大查询数。这将降低延迟，并使缓存中可用的查询数超过 **max_remote_cache_entries**。此值不应小于 **max_remote_cache_entries**。它仅在 **max_cache_seconds > 0** 时生效，默认值是 **100**。

## 跨不同服务使用的参数

使用 **awswrangler** 软件包与 Amazon S3 交互时使用以下参数。它们将有助于提高读写操作的性能：

- **CONCURRENT_PARTITIONING**：如果为 **True**，这将在写入分区时增加并行度（使用线程）级别。它将减少写入时间并增加内存使用量。
- **S3_BLOCK_SIZE**：此配置用于从 S3 读取文件，要么按指定的块大小读取，要么如果此变量的值设置为 **-1** 则一次性完全读取。

## 配置的常见用例

我们可以配置 **awswrangler**，使相同的设置在多个命令中重复使用，这意味着我们不必每次都输入它们。此外，当我们在多个环境中执行程序时，全局配置可以帮助我们设置默认值，从而根据特定环境自定义 **awswrangler** 执行。
