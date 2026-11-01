
```
$python3 --version
Python 3.9.12
$python3 -m venv awspandas
$ls
awspandas
$source awspandas/bin/activate
(awspandas) $pip3 list --disable-pip-version-check
Package      Version
---------    -------
pip          22.0.4
setuptools   58.1.0
(awspandas) $deactivate
$pip3 list
Package            Version
------------------  -------
aenum              3.1.11
aiohttp            3.8.1
aiosignal          1.2.0
asn1crypto         1.5.1
async-timeout      4.0.2
attrs              22.1.0
awscli             1.25.50
```

图 3.4：Python 虚拟环境

## 自定义安装

开源库的一个主要优势是可以根据项目需要进行自定义并添加额外的代码段。你可以按照以下步骤执行 AWS SDK for pandas 的自定义安装，添加特定于项目需求的模块。

作为先决条件，请激活虚拟环境，并在笔记本电脑/EC2 机器上安装 `python3` 和 `git`：

- 在 Mac 上安装 **git**，可以使用 `brew` 命令
- 在 EC2 实例上安装 **git**，可以使用 `sudo yum install git`:

1. 将代码库下载或复制到本地机器或 EC2 实例：
   1. 从 `aws-sdk-pandas` 仓库复制 git 仓库 URL（https://github.com/aws/aws-sdk-pandas）。
   2. 使用该链接，将整个 git 仓库克隆到本地机器/EC2 以进行进一步开发：

```
(awspandas) $git --version
git version 2.37.3
(awspandas) $git clone https://github.com/aws/aws-sdk-pandas
Cloning into 'aws-sdk-pandas'...
remote: Enumerating objects: 13000, done.
remote: Counting objects: 100% (565/565), done.
remote: Compressing objects: 100% (264/264), done.
remote: Total 13000 (delta 388), reused 431 (delta 299), pack-reused 12435
Receiving objects: 100% (13000/13000), 8.00 MiB | 7.91 MiB/s, done.
Resolving deltas: 100% (9773/9773), done.
(awspandas) $
```

图 3.5：awswrangler git 复制

2. 进入代码库并探索代码。你可以对现有代码进行更改并为项目需求添加额外的模块：

- 在 awswrangler 的安装路径内导航并探索不同的文件夹。你应该会在名为 awswrangler 的文件夹下看到代码，并且有针对不同服务的单独文件夹。
- 为了本次学习，让我们修改 __init__.py 文件，该文件将在 Python 代码中导入 awswrangler 时执行。探索为各种 AWS 服务/awswrangler 功能导入的模块。你可以看到一条从 __metadata__.py 文件获取变量的导入语句。按如下所示在导入语句中追加一个新变量 __message__，并保存文件。

```
from awswrangler.__metadata__ import __description__, __license__, __title__, __version__,__message__
_ # noqa
from awswrangler._config import config  # noqa
```

图 3.6：awswrangler __init__.py 文件

3. 打开 __metadata__.py 文件并更改 __description__ 字段，如下所示。此外，添加另一个变量 __message__，该变量已在上一步中导入到 __init__.py 模块中，并保存文件。

```
__title__: str = "awswrangler"
__description__: str = "Pandas on AWS. Updated for custom installation."
__version__: str = "2.17.0"
__license__: str = "Apache License 2.0"
__message__: str = "This was changed as a part of Learning exercise."
```

图 3.7：awswrangler __metadata__.py 文件

3. 从本地机器或 EC2 实例上修改过的自定义代码安装 awswrangler：

- 导航到 awswrangler 代码可用的顶级文件夹。
- 执行 pip3 install . 命令，如下所示。这将使用具有自定义更改的本地代码库在你的机器上安装 awswrangler。

```
(awspandas) $cd aws-sdk-pandas
(awspandas) $ls
CODE_OF_CONDUCT.md          awswrangler               test_infra
CONTRIBUTING.md            building                  tests
CONTRIBUTING_COMMON_ERRORS.md  docs                  tox.ini
LICENSE.txt                fix.sh                    tutorials
NOTICE.txt                 poetry.lock               validate.sh
README.md                  pyproject.toml
THIRD_PARTY.txt            test.sh
(awspandas) $pip3 install .
Processing /Users/ssmsun/Documents/Initiatives/DataWranglerbook/code/aws-sdk-pandas
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Collecting botocore<2.0.0,>=1.27.11
  Downloading botocore-1.27.82-py3-none-any.whl (9.2 MB)
     ---------------------------------------- 9.2/9.2 MB 25.5 MB/s eta 0:00:00
```

图 3.8：在本地机器上安装 awswrangler

4. 验证并开始使用 **awswrangler** 自定义安装：

- 打开一个 Python shell 并导入 **awswrangler**。
- 打印 `__message__` 和 `__description__` 变量，以查看我们是否已从本地机器/EC2 实例安装了 **awswrangler** 包，并应用了之前步骤中进行的更改。

```
(awspandas) $python3
Python 3.9.12 (main, Apr  5 2022, 01:53:17)
[Clang 12.0.0 ] :: Anaconda, Inc. on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import awswrangler as wr
>>> print(wr.__description__)
Pandas on AWS. Updated for custom installation.
>>> print(wr.__message__)
This was changed as a part of Learning exercise.
>>>
```

图 3.9：验证带有自定义更改的 awswrangler 本地安装

5. 作为最后一步，你可以通过退出虚拟环境并使用 `pip3 install awswrangler` 命令安装 **awswrangler** 包来验证我们所做的更改仅限于我们的虚拟环境（这将从 **pypi** 远程仓库安装包，而不包含我们的自定义代码更改）：

```
(awspandas) $deactivate
$pip3 install awswrangler
Requirement already satisfied: awswrangler in /Users/ssmsun/miniconda3/lib/python3.9/site-packages (2.16.1)
```

```
$python3
Python 3.9.12 (main, Apr  5 2022, 01:53:17)
[Clang 12.0.0 ] :: Anaconda, Inc. on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import awswrangler as wr
>>> print(wr.__description__)
Pandas on AWS.
>>> print(wr.__message__)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: module 'awswrangler' has no attribute '__message__'
>>>
```

图 3.10：使用 Python 虚拟环境的多个 awswrangler 版本

我们看到了如何仅使用 `pip` 命令在本地机器/EC2 实例上安装 `awswrangler` 包。我们还从 `awswrangler` git 仓库下载了代码，对项目所需的更改进行了修改，并安装了更改后的 `awswrangler` 版本。我们还学习了如何在虚拟环境中安装包，以便为一个项目所做的更改不会与其他项目冲突。

恭喜完成此步骤！你可以在这里花一些时间玩弄该包以了解更多。一旦探索完毕，让我们进入下一节，在那里我们需要帮助一名 IT 人员使用 Lambda 函数安装 `awswrangler`。

## 使用 Lambda 函数的标准和自定义安装

对于大多数用例，包括像项目 A 的 IT 人员处理小量数据的情况，我们希望在无服务器环境中部署和执行数据集成代码。AWS Lambda 是一种无服务器的事件驱动计算服务，让你无需配置或管理服务器即可为几乎任何类型的应用程序或后端服务运行代码。Lambda 预装了各种重要的库，但用户必须在将代码部署到 Lambda 函数时，将默认环境中不可用的库与代码一起打包。

对于较小的库，用户可以将依赖项与代码一起打包并上传到 Lambda 函数。我们将看到如何创建包含 Lambda 函数依赖库的 ZIP 归档（https://docs.aws.amazon.com/Lambda/latest/dg/python-package.html#python-package-create-package-with-dependency）。为此，创建一个新的 Python 虚拟环境并在虚拟环境中安装新包。这将把依赖包安装在 `pip`
