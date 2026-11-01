
## 层版本配置

描述 - 可选

awswrangler Python 3.9 版本

上传 .zip 文件

从 Amazon S3 上传文件

上传

awswrangler-layer-2.17.0-py3.9.zip (45.7 MB)

对于大于 10 MB 的文件，请考虑使用 Amazon S3 上传。

兼容架构 - 可选 信息

为您的层选择兼容的指令集架构。

x86_64

arm64

兼容运行时 - 可选 信息

最多选择 15 个运行时。

运行时

Python 3.9

图 3.20：awswrangler 自定义 Lambda 安装 – 上传自定义 Lambda 层

4. 使用 Python 3.9 运行时创建一个 Lambda 函数并将以下代码粘贴到其中：

```
import json
import awswrangler as wr
def Lambda_handler(event, context):  # TODO
    implement
    print(wr.__version__)  print(wr.__description__)  print(wr.__message__)
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Lambda!')
    }
```

5. 将步骤 3 中创建的自定义 Lambda 层附加到 Lambda 函数。

version. 您还可以创建一个新层。

AWS 层
从 AWS 提供的层列表中选择一个层。

自定义层
从您 AWS 账户或组织创建的层列表中选择一个层。

指定 ARN
通过提供 ARN 指定层。

自定义层

由您的 AWS 账户或组织创建的、与您的函数运行时兼容的层。

awswrangler39

版本

1

图 3.21：awswrangler 自定义 Lambda 安装 – 将自定义层附加到 Lambda 函数

6. 执行 Lambda 函数并查看 **执行结果** 标签页。您应该能够看到一条消息，确认 `awswrangler` 包是从我们的自定义代码库构建的。

```
Response
{
  "statusCode": 200,
  "body": "\"Hello from Lambda!\""
}

Function Logs
OpenBLAS WARNING - could not determine the L2 cache size on this system, assuming 256k
START RequestId: 436fa52d-2b42-4ffb-96ad-fa7769981eb7 Version: $LATEST
2.17.0
Pandas on AWS. Updated for custom installation.
This was changed as a part of Learning exercise.
END RequestId: 436fa52d-2b42-4ffb-96ad-fa7769981eb7
```

图 3.22：awswrangler 自定义 Lambda 安装 – 验证 Lambda 输出

## AWS Glue 作业的标准和自定义安装

我们将查看 Project B 的 IT 人员希望在 Glue Python shell 上使用 pandas 的 AWS SDK 对源数据库中的数据执行数据整理活动的场景。Glue 作业有三种执行模式 – Python shell、Spark ETL 和 Spark Streaming ETL。Python shell 作业模式用于处理不需要 Apache Spark 环境的小规模文件处理。对于更大的数据量和分布式处理，您可以使用 Spark 作业模式，对于流式管道，您可以选择 Spark Streaming 作业模式。我们将了解如何为 Python shell 和 Spark 作业安装 `awswrangler`。

## Python shell 作业的 awswrangler 标准安装

Python shell 作业环境提供两个不同的 Python 版本，3.6 和 3.9（默认是 Python 3.6）。我们可以使用 wheel 文件将外部库附加到 Glue Python shell 作业。Wheel 文件是 Python 生态系统的一部分，有助于 Python 包安装过程，使其更快更稳定：

- AWS Glue Python 3.6 环境未预安装 `awswrangler`。在添加额外库之前，请参阅 Python Glue 作业环境中已安装的库 (https://docs.aws.amazon.com/glue/latest/dg/add-job-python.html#python-shell-supported-library)。我们必须为 Glue 作业中要使用的 `awswrangler` 版本附加 `wheel` 文件并将其附加，如以下步骤所述：
- 1. 对于 Python 3.6 作业，您可以使用以下语法从公共工件存储桶下载 wheel 文件：
  `s3://aws-data-wrangler-public-artifacts/releases/<version>/ awswrangler-<version>-py3-none-any.whl`.

```
(awspandas) $aws s3 cp s3://aws-data-wrangler-public-artifacts/releases/2.14.0/awswrangler-2.14.0-py3-none-any.whl .
download: s3://aws-data-wrangler-public-artifacts/releases/2.14.0/awswrangler-2.14.0-py3-none-any.whl to ./awswrangler-2.14.0-py3-none-any.whl
```

图 3.23：awswrangler glue 安装 – 下载 wheel 文件

### 注意

*awswrangler 从版本 2.15 开始不再支持 Python 3.6，因此如果您使用 Python 3.6 使用更高版本，将会抛出错误。*

- 2. 将 wheel 文件上传到 S3 存储桶并复制 S3 URL。创建一个 Python 版本为 3.6 的 Glue 作业，并在作业详细信息页面下的 **高级属性** 中的 Python 库路径部分提供 wheel 文件。

![](img/ec1933a597ce63afa3bcf7496364f412_116_0.png)

图 3.24：awswrangler glue 安装 – 上传 wheel 文件

3. 将以下代码粘贴到脚本窗口并保存作业：

```
import sys
import awswrangler as wr
print(wr.__version__)
```

- 4. 执行代码并在 CloudWatch Logs 中验证输出，检查是否 **awswrangler** 已成功加载到 Glue Python shell 作业中。

```
Successfully installed asn1crypto-1.5.1 awswrangler-2.14.0 beautifulsoup4-4.11.1 boto3-1.23.10 botocore-1.26.10 certifi-2022.9.24 normalizer-2.0.12 decorator-5.1.1 et-xmlfile-1.1.0 idna-3.4 jmespath-0.10.0 jsonpath-ng-1.5.3 lxml-4.9.1 numpy-1.18.5 openpyxl-3.0 py-1.1.0 packaging-21.3 pandas-1.1.5 pyarrow-1.22.1 ply-3.11 progressbar2-3.55.0 pyarrow-6.0.1 pymysql-1.0.2 pyparsing-3.0.9 python-utils-3.3.3 pytz-2022.2.1 redshift-connector-2.0.909 requests-2.27.1 requests-aws4auth-1.1.2 s3transfer-0.5.2 scram-1.4.1 59.6.0 six-1.16.0 soupsieve-2.3.2.post1 urllib3-1.26.12 xlsx-2.0.1 xlwt-1.3.0
```

2.14.0

图 3.25：awswrangler glue 安装 – 检查 awswrangler 是否已加载到 Glue 环境中

- Python 3.9 已预安装 **awswrangler** 版本 2.15.1。创建作业时，启用 **加载常用分析库（推荐）** 复选框。如果您想使用不同版本的 **awswrangler**，则必须像在 Python 3.6 环境中那样附加 wheel 文件。但是，如果您想使用预安装的 **awswrangler** 2.15.1 版本，请按照以下步骤操作：
- 1. 创建一个 Python 版本为 3.9 的作业并选择 **加载常用分析库（推荐）** 复选框。

### 类型

ETL 作业的类型。这会根据您选择的源类型自动设置。

Python Shell

### Python 版本

Python 3.9

加载常用分析库（推荐）

从 pypi.org 包含常用的 Python 库，如 pandas、numpy 和 s3fs。如果您加载自己的库并希望避免版本冲突，请取消选中。

图 3.26：awswrangler glue 安装 – 创建 Python 3.9 shell 作业

- 2. 粘贴在 Python 3.6 作业中用于打印 `awswrangler` 版本的示例代码。保存并执行作业。输出将显示 Python shell 作业中使用的 `awswrangler` 版本。您可以通过单击 Glue Python shell 作业中的输出日志链接来查看此信息。

```
Setup complete. Starting script execution:
----

2.15.1

No newer events at this moment. Auto retry paused. Resume
```

图 3.27：awswrangler glue 安装 – 验证 awswrangler 版本

为了使用更新版本的 `awswrangler`，请按照与 Python 3.6 shell 类似的步骤添加新的 wheel 文件。以下屏幕截图显示了将 `awswrangler` 版本 2.17 上传到 Glue Python shell 的输出，这与 `awswrangler` 2.15.1 相比是一个更新的版本。

```
pyparsing-3.0.9 python-dateutil-2.8.2 python-utils-3.3.3 pytz-2022.2.1 redshift-connector-2.0.909 requests-2.28 s3transfer-0.6.0 scramp-1.4.1 setuptools-65.4.0 six-1.16.0 soupsieve-2.3.2.post1 urllib3-1.26.12 yarl-1.8.1
```

```
[notice] A new release of pip available: 22.1.2 -> 22.2.2
[notice] To update, run: pip install --upgrade pip
```

```
Setup complete. Starting script execution:
----

2.17.0
```

图 3.28：awswrangler glue 安装 – 验证 awswrangler 版本（自定义版本安装）

## Python shell 作业的 awswrangler 自定义安装

我们将利用 `poetry` 库从我们修改过的 `awswrangler` 仓库创建自定义 wheel 文件。我们将利用我们用于
