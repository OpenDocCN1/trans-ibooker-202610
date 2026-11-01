
## 本地机器/EC2 实例设置：

-   您可以在 Python 虚拟环境中使用 `pip3 install poetry` 命令安装 Poetry。

-   从本地仓库执行 `build_wheel.sh` 脚本以构建 wheel 文件，如下图所示，wheel 文件将使用随 `awswrangler` 仓库提供的 `poetry.lock` 配置创建。

```
(awspandas) $poetry --version
Poetry (version 1.2.1)
(awspandas) $cd aws-sdk-pandas/building
(awspandas) $sh build-wheel.sh
+ pushd ..
~/Documents/Initiatives/DataWranglerbook/code/aws-sdk-pandas ~/Documents/Initiatives/DataWranglerbook/code/aws-sdk-pandas/building
+ rm -rf 'dist/*.whl'
+ poetry build -f wheel
Creating virtualenv awswrangler-Muv3bMem-py3.9 in /Users/ssmsun/Library/Caches/pypoetry/virtualenvs
Building awswrangler (2.17.0)
(awspandas) $ls ../dist
awswrangler-2.17.0-py3-none-any.whl
(awspandas) $
```

图 3.29：awswrangler Glue 自定义安装 – 先决条件

-   将 wheel 文件上传到 S3 存储桶，创建 Glue Python shell 作业（Python 版本 3.9），并提供 wheel 文件的 S3 位置。

-   粘贴以下代码以验证我们是否使用的是 `awswrangler` 的自定义版本：

```
import sys
import awswrangler as wr
print(wr.__version__)
print(wr.__description__)
print(wr.__message__)
```

-   执行作业并在 CloudWatch 日志中验证输出。

```
pymysql-1.0.2 pyp... python-dateutil-2.8.2 python-utils-3.3.3 pytz-2022.2.1 redshift-connector-2.0.909 requests-2.28
aws4auth-1.1.2 s3transfer-0.6.0 scramp-1.4.1 setuptools-65.4.0 six-1.16.0 soupsieve-2.3.2.post1 urllib3-1.26.12 yarl-1.8.1

[notice] A new release of pip available: 22.1.2 -> 22.2.2
[notice] To update, run: pip install --upgrade pip

Setup complete. Starting script execution:
----

2.17.0
Pandas on AWS. Updated for custom installation.
This was changed as a part of Learning exercise.
```

图 3.30：awswrangler Glue 自定义安装 – 验证 awswrangler 安装

现在您已探索了在 Python shell 中安装 `awswrangler` 的不同选项，您可以有效地将其用于所有与 Python 相关的活动。

## 在 Glue Spark 作业上标准安装 awswrangler

Glue Spark 作业可以使用 Glue 0.90（Spark 2.2）、Glue 1.0（Spark 2.4）、Glue 2.0（Spark 2.4）和 Glue 3.0（Spark 3.1）版本创建。`awswrangler` 仅支持 Glue 2.0 及之前的版本，因为它具有编译依赖项（C/C++）。默认值为 Glue 3.0，这是最新版本，您应为所有新作业使用最新的 Glue 3.0 版本，并在可能的情况下升级到它。这具有 Spark 和 AWS Glue 服务的最新功能，为运行作业提供更好、更安全、性能更强的环境。我们将使用以下步骤安装 awswrangler：

-   创建一个 Glue 3.0 作业并按如下方式提供作业参数：

```
- key: --additional-python-modules   values: pyarrow==2,awswrangler==<<version>>
```

![](img/ec1933a597ce63afa3bcf7496364f412_119_0.png)

图 3.31：awswrangler Glue 标准安装 – 作业参数配置

-   在脚本窗口中粘贴以下代码，保存作业，然后单击运行它：

```
import sys
from awsglue.transforms import *
from awsglue.utils import getResolvedOptions
from pyspark.context import SparkContext
from awsglue.context import GlueContext
from awsglue.job import Job
import awswrangler as wr
## @params:
args = getResolvedOptions(sys.argv, ['JOB_NAME'])
sc = SparkContext()
glueContext = GlueContext(sc)
spark = glueContext.spark_session
job = Job(glueContext)
job.init(args['JOB_NAME'], args)
print(wr.__version__)
job.commit()
```

-   作业执行完成后检查 CloudWatch 日志。您应该在那里成功看到 awswrangler 的版本被打印：

```
2022-09-30 16:06:07,219 main INFO Log4j appears to be running in a Servlet environment, but there's no log4j-web module available. If you support, please add the log4j-web JAR to your web archive or server lib directory.
2.17.0
```

图 3.32：awswrangler Glue 标准安装 – 验证输出

## 在 Glue Spark 作业上自定义安装 awswrangler

对于 awswrangler 的自定义安装，用户可以按照以下步骤从 wheel 文件安装自定义 awswrangler 构建，与 Python shell 作业类似：

-   这些步骤与 Python shell 作业自定义安装中的步骤类似。执行 Python shell 作业自定义安装中的步骤 1–3 以创建 wheel 文件。将您的 wheel 文件上传到 S3 位置。

-   通过将 wheel 文件位置作为作业参数的一部分提供，将 wheel 文件附加到 Glue Spark 作业，如以下屏幕截图所示。

![](img/ec1933a597ce63afa3bcf7496364f412_119_1.png)

图 3.33：awswrangler Glue Spark 自定义安装 – 将 wheel 文件附加到作业

-   复制并粘贴来自之前的 Spark 程序的代码并执行 Spark 作业。验证输出日志以查看自定义库是否已安装在 Glue Spark 作业中。

```
2022-10-02 11:59:56,721 main INFO Log4j appears to be running in a Servlet environment, but there's no log4j-web module available. If you support better web container support, please add the log4j-web JAR to your web archive or server lib directory.
2.17.0
Pandas on AWS. Updated for custom installation.
This was changed as a part of Learning exercise.
```

图 3.34：awswrangler Glue Spark 自定义安装 – 验证输出

此外，您可以单击错误日志并验证日志内容，显示从 wheel 文件安装 **awswrangler**。

```
INFO 2022-10-02 11:59:54,819 26668 com.amazonaws.services.glue.PythonModuleInstaller [main] Processing ./awswrangler-2.17.1-py3-none-any.whl
Collecting boto3<2.0.0,>=1.24.11 Downloading boto3-1.24.84-py3-none-any.whl (132 kB)
132.5/132.5 kB 4.6 MB/s eta 0:00:00
Collecting progressbar2<5.0.0,>=4.0.0 Downloading progressbar2-4.0.0-py2.py3-none-any.whl (26 kB)
Collecting backoff<3.0.0,>=1.1.1 Downloading backoff-2.1.2-py3-none-any.whl (14 kB)
Requirement already satisfied: pymysql<2.0.0,>=1.0.0 in /home/spark /home/local/lib/python3.7/site-packages (from awswrangler==2.17.1) (1.0.2)
Collecting requests-aws4auth<2.0.0,>=1.1.1 Downloading requests-aws4auth-1.1.1-py2.py3-none-any.whl (1.1.2)
Collecting redshift-connector<2.1.0,>=2.0.889 Downloading redshift_connector-2.0.909-py3-none-any.whl (112 kB)
112.1/112.1 kB 3.0 MB/s eta 0:00:00
Collecting opensearch-py<3,>=1 Downloading opensearch-py-2.0.0-py2.py3-none-any.whl (204 kB)
204.6/204.6 kB 5.7 MB/s eta 0:00:00
Requirement already satisfied: pandas<2.0.0,>=1.2.0 in /home/spark/.local/lib/python3.7/site-packages (from awswrangler==2.17.1) (1.3.2)
Collecting numpy<2.0.0,>=1.21.0
```

图 3.35：awswrangler Glue Spark 自定义安装 – 在错误日志中验证输出

## 在 Amazon SageMaker 笔记本上标准和自定义安装

Amazon SageMaker 笔记本可用于交互式开发 Python 和 PySpark 应用程序的代码。

对于 SageMaker python3 笔记本，**awswrangler** 可以通过运行命令安装，如下所示。用户可以利用 Conda 和 pip 包管理系统在 Notebook 实例中安装外部库（https://docs.aws.amazon.com/SageMaker/latest/dg/nbi-add-external.html）：

```
!pip install awswrangler==<version>
```

![](img/ec1933a597ce63afa3bcf7496364f412_120_0.png)

图 3.36：awswrangler SageMaker 标准安装

此外，请注意，安装仅在会话期间持久保存，如果支持 SageMaker 笔记本的 EC2 实例重新启动，您必须再次运行安装步骤。用户可以利用 SageMaker 生命周期策略在实例启动阶段在 SageMaker 上安装所需的库（https://docs.aws.amazon.com/SageMaker/latest/dg/nbi-lifecycle-config-install.html）。当开发通过 SageMaker 实例进行时，我们可以设置 SageMaker 笔记本生命周期策略以在所有兼容内核上安装 `awswrangler`。我们可以将 `pip install` 语句作为生命周期策略的一部分添加，以便在实例创建时 `awswrangler` 可以附加到内核，用户无需担心安装所需的包。

### 自定义安装

要使用自定义 `awswrangler` 包，请上传我们之前为 Lambda 函数生成的 ZIP 文件，并为简单起见将包上传到与脚本相同的目录中。如果自定义库上传到不同路径，我们需要将 `awswrangler` 源代码可用的路径包含到 `sys.path` 中。

![](img/ec1933a597ce63afa3bcf7496364f412_121_0.png)

图 3.37：awswrangler SageMaker 自定义安装 – 上传自定义库

从自定义路径导入 `awswrangler`，而不是通过 `pip` 命令安装它，您现在可以验证更改是否可在笔记本中使用。
