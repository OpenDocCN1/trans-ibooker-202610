## 第 2 章 设置你的环境

现在你已经跃跃欲试、摩拳擦掌，准备在你的 AWS 账户中进行实验和创建。我们将使用免费、公开可访问的示例代码，演示如何利用自动化的“魔力”创建或销毁对象或资源。

在本章中，我们将通过一些简单的命令设置你的本地环境。正如我们在第 1 章讨论的，AWS 将是本书中 Automatoonz 冒险之旅的首选云提供商。

当你完成运行以下命令后，你将拥有一个完全自动化的 Amazon Elastic Kubernetes Service (EKS) 集群和一个 AWS Lambda 容器镜像流水线，可在其上部署一个简单的 Python 应用程序。

### 提示

请记住设置并测试你的账户凭证，并测试你的访问权限。此外，别忘了销毁你在账户中创建的示例资源。

### 你需要什么

以下是你跟随本书练习和活动所需的工具列表：

- **AWS CLI**：一个开源工具，使你能够通过终端中的命令与指定账户中的 AWS 服务进行交互。更多信息请访问[官方网站](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.xhtml)。请使用最新版本。
- **AWS CloudFormation**：一种 AWS 领域特定语言 (DSL)，支持两种文件类型：YAML 和 JSON。这些文件用于部署和预置作为代码跟踪的基础设施资源。更多信息请访问[官方网站](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.xhtml)。
- **Docker (社区版)**：一个软件平台，或平台即服务 (PaaS)，将软件打包成称为容器的单元。这些容器允许你构建、测试和部署应用程序。更多信息请访问[官方网站](https://www.docker.com/)。请使用最新版本。
- **开放容器倡议 (OCI) 镜像**：一个 Linux 基金会项目，围绕容器格式和运行时的开放行业标准创建了官方认证计划。2015 年，Docker、CoreOS 和容器行业的其他领导者发起了 OCI。更多信息请访问[官方网站](https://opencontainers.org/about/overview/)。
- **Python**：一种开源、解释型、面向对象、具有动态语义的高级编程语言。更多信息请访问[官方网站](https://www.python.org/)。请使用 3.X 或更高版本。
- **Git 仓库**：一个开源、分布式版本控制系统，用于不同文件类型。更多信息请访问[官方网站](https://git-scm.com/)。请使用最新版本。
- **Kubectl**：一个开源 Kubernetes 命令行工具，允许用户与 Kubernetes 集群交互。更多信息请访问[官方网站](https://kubernetes.io/docs/tasks/tools/)。请使用最新版本。
- **Kubernetes**：一个用于自动化部署、扩展和管理容器化应用程序的开源系统。更多信息请访问[官方网站](https://kubernetes.io/)。请使用 1.21 或更高版本。

### 安装并验证你的设置

你可以使用包管理器下载工具的最新版本，或使用 `curl` 直接下载安装包。然后，解压安装文件并运行安装脚本。本节提供 Darwin、Linux 和 Windows 操作系统的命令。

#### 安装 AWS CLI

运行相应的命令以安装 AWS CLI，它允许你与 AWS 账户交互并管理 IaC。（详见 AWS 官方[文档](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started.xhtml)了解更多详情。）

**Darwin:**

```bash
brew install awscli
```

**Linux:**

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-<CPU_architecture>.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**Windows:**

```bash
choco install awscli
```

安装完成后，完成配置以关联你的 AWS 账户（见[文档](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.xhtml)）。这将允许你运行随本书附带的 [GitHub 仓库](https://github.com/bksarthak/devsecops-book) 中提供的示例代码。

#### 安装 Docker 引擎

运行相应的命令以安装 Docker 引擎（[文档](https://docs.docker.com/engine/install/centos/)）。Docker CLI 允许你与 Dockerfile 交互。

**Darwin:**

```bash
brew install docker
```

**Linux:**

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

**Windows:**

```bash
choco install docker-engine
```

安装完成后，测试 Docker CLI 是否有响应。

#### 检查你的 Python 版本

运行相应的命令以检查是否已安装 Python 3.X 或更高版本。Python 将在示例代码的 AWS Lambda 容器中使用。

**Darwin:**

```bash
python3 --version
```

**Linux:**

```bash
python3 --version
```

**Windows:**

```bash
python3 --version
```




### 创建您的第一个最简管道

#### 安装 Git

运行适当的命令以安装 Git，这是一个分布式版本控制系统。Git CLI 允许您与 Git 仓库交互。

Darwin：

```
brew install git
```

Linux：

```
sudo yum install git -y
```

Windows：

```
choco install git
```

安装完成后，完成配置以关联您的个人设置。然后，运行命令以测试 Git CLI 的响应。有关更多信息或故障排除，请访问[官方网站](https://git-scm.com/doc)。

#### 安装 Kubernetes

运行适当的命令以安装 Kubernetes CLI，它将允许您与 Git 仓库交互。

Darwin：

```
brew install kubectl
```

Linux：

```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Windows：

```
choco install kubernetes-cli
```

安装完成后，测试 Kubernetes CLI 的响应。有关更多信息或故障排除，请访问[官方网站](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux)。

现在您已安装并验证了工具，是时候创建管道了。

首先下载最新版本的示例代码，开始创建您的管道：

```
git clone <BOOK_SAMPLE_CODE_REPO_URL>
```

此命令将创建一个 AWS Service Catalog 组合，用于存放所有 AWS Service Catalog 基础设施和集群产品：

```
aws cloudformation create-stack \
       --template-body file://ServiceCatalogPortfolio.yml \
       --stack-name EKSPortfolio \
       --parameters file://parameter-files/ServiceCatalog.json \
       --capabilities CAPABILITY_NAMED_IAM \
       --region <INPUT_YOUR_REGION>
```

> 添加带有 KMS 密钥的 S3 存储桶创建

接下来，上传所有 AWS Service Catalog 文件和 AWS Lambda 函数代码：

```
aws s3 cp service_catalog_products/ \
s3://<s3_bucket_for_sc_products_lambda_function>/ \
--recursive \
--exclude "*" \
--include "*.yml" \
--include "*.zip" \
--region <INPUT_YOUR_REGION>
```

然后，预置 AWS Service Catalog 网络资源：

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSNetwork \
--parameters file://parameter-files/NetworkProduct.json \
--region <INPUT_YOUR_REGION>
```

以下命令将预置 AWS Identity Access Management (IAM) 产品角色资源，该资源与 AWS Service Catalog 集成：

```
aws cloudformation create-stack \
       --template-body file://ServiceCatalogProduct.yml \
       --stack-name EKSIAM \
       --parameters file://parameter-files/IAMProduct.json \
       --region <INPUT_YOUR_REGION>
```

预置 AWS Service Catalog EKS 节点组产品资源：

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSIAM \
--parameters file://parameter-files/IAMProduct.json \
--region <INPUT_YOUR_REGION>
```

预置 AWS Service Catalog EKS Lambda 产品资源：

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSLambda \
--parameters file://parameter-files/EKSLambdaProduct.json \
--region <INPUT_YOUR_REGION>
```

预置 AWS Service Catalog EKS 虚拟私有云 (VPC) 终端节点产品资源：

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSVPCENDP \
--parameters file://parameter-files/VPCEndpointProduct.json \
--region <INPUT_YOUR_REGION>
```

预置 AWS Service Catalog EKS 控制平面日志记录产品资源：

```
aws cloudformation create-stack \
--template-body file://ServiceCatalogProduct.yml \
--stack-name EKSLOG \
--parameters file://parameter-files/EKSLoggingProduct.json \
--region <INPUT_YOUR_REGION>
```

### 摘要

图 2-1 展示了您在 AWS 账户中部署的环境。

![](img/image-to-come.png)

让我们看看该管道的各个组成部分，对应 图 2-1 中的数字：

1.  您的笔记本电脑或终端，用于上传和下载代码
2.  AWS Service Catalog 管理逻辑产品和模块化代码
3.  AWS IAM 角色，允许访问 EKS 集群
4.  Amazon CloudWatch，用于日志记录和指标
5.  AWS Lambda 容器镜像和 Amazon ECR
6.  EKS 托管集群
7.  部署在 EKS 集群中的 Python 应用程序

花几分钟时间四处看看，熟悉您的环境。现在您已经设置好基础设施，准备开始保护它。第 3 章将帮助您入门。
