

我们强烈建议在购买任何安全工具之前，先回答一个简单的问题：*你试图解决什么安全问题？* 像“让事情变安全”和“提升我的安全”这样的回答模糊至极。试着将其浓缩为一句话。如果这不能给你一个明确的答案，请考虑编写一份风险声明。风险声明大致如下：

“缺乏 X 导致 Y 损失，因为 Z。”

X = 一个安全功能，如访问控制或日志记录

Y = 如果未实施安全控制造成的影响（即，什么处于风险之中？）

Z = 你的安全控制如何防止安全事件

我们不会深入探讨采购细节，但在购买工具之前，了解该工具旨在解决什么安全问题至关重要。许多企业级工具提供免费试用或演示。如果你的团队具有构建者心态，你可能想探索开源云安全工具的广阔天地，其中许多已在行业中崭露头角并持续变得更好。

一旦你选定了工具，谁来维护它？是否需要定期更新？你的工具在运维侧应有明确的负责人。假设你获得了一个云安全态势管理工具，安全团队已集成每个云账户以供摄入。安全团队从账户接收告警，但不采取任何行动，因为他们预期云账户所有者会处理这些告警。但云账户所有者认为*安全*团队正在处理。如果没有处理告警的明确流程，团队就没有行动计划。简单的工具如责任分配矩阵（或 RACI 图表）和运行手册可以解决这个问题。我们将在第 7 章更详细地讨论这些工具。

# 总结

现在我们已经介绍了 DevSecOps 并阐明了本书的范围，让我们开始在流水线中实施 DevSecOps 吧！

后续章节将从为 Automatoonz 搭建基础设施开始，并迭代地将安全能力构建到应用程序开发过程中。我们将主要关注技术和工具，但会根据我们的经验指出任何人员和流程方面的必备要素。

本章关键要点快速回顾：

*   DevSecOps 是 DevOps 的一个变体，它在其迭代 DevOps 模型中包含了安全。
*   如果你能防止错误配置的资源被创建，就能建立安全卫生并教育开发人员。
*   自动化安全检查需要幂等性、基线和建议。第 3 章和第 4 章将更详细地讨论此主题。
*   仅自动化安全检查无法解决安全问题。建立人员和流程来拥有和运营自动化。
*   在推出安全工具之前，先确定*为什么*要实施它。

# 第 2 章 设置你的环境

现在你已经跃跃欲试、摩拳擦掌，准备在你的 AWS 账户中进行实验和创建。我们将使用免费、公开可访问的示例代码，演示如何利用自动化的“魔力”创建或销毁对象或资源。

在本章中，我们将通过一些简单的命令设置你的本地环境。正如我们在第 1 章讨论的，AWS 将是本书中 Automatoonz 冒险之旅的首选云提供商。

当你完成运行以下命令后，你将拥有一个完全自动化的 Amazon Elastic Kubernetes Service (EKS) 集群和一个 AWS Lambda 容器镜像流水线，可在其上部署一个简单的 Python 应用程序。

### 提示

请记住设置并测试你的账户凭证，并测试你的访问权限。此外，别忘了销毁你在账户中创建的示例资源。

## 你需要什么

以下是你跟随本书练习和活动所需的工具列表：

- **AWS CLI**：一个开源工具，使你能够通过终端中的命令与指定账户中的 AWS 服务进行交互。更多信息请访问[官方网站](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.xhtml)。请使用最新版本。
- **AWS CloudFormation**：一种 AWS 领域特定语言 (DSL)，支持两种文件类型：YAML 和 JSON。这些文件用于部署和预置作为代码跟踪的基础设施资源。更多信息请访问[官方网站](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.xhtml)。
- **Docker (社区版)**：一个软件平台，或平台即服务 (PaaS)，将软件打包成称为容器的单元。这些容器允许你构建、测试和部署应用程序。更多信息请访问[官方网站](https://www.docker.com/)。请使用最新版本。
- **开放容器倡议 (OCI) 镜像**：一个 Linux 基金会项目，围绕容器格式和运行时的开放行业标准创建了官方认证计划。2015 年，Docker、CoreOS 和容器行业的其他领导者发起了 OCI。更多信息请访问[官方网站](https://opencontainers.org/about/overview/)。
- **Python**：一种开源、解释型、面向对象、具有动态语义的高级编程语言。更多信息请访问[官方网站](https://www.python.org/)。请使用 3.X 或更高版本。
- **Git 仓库**：一个开源、分布式版本控制系统，用于不同文件类型。更多信息请访问[官方网站](https://git-scm.com/)。请使用最新版本。
- **Kubectl**：一个开源 Kubernetes 命令行工具，允许用户与 Kubernetes 集群交互。更多信息请访问[官方网站](https://kubernetes.io/docs/tasks/tools/)。请使用最新版本。
- **Kubernetes**：一个用于自动化部署、扩展和管理容器化应用程序的开源系统。更多信息请访问[官方网站](https://kubernetes.io/)。请使用 1.21 或更高版本。

## 安装并验证你的设置

你可以使用包管理器下载工具的最新版本，或使用 `curl` 直接下载安装包。然后，解压安装文件并运行安装脚本。本节提供 Darwin、Linux 和 Windows 操作系统的命令。

### 安装 AWS CLI

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

### 安装 Docker 引擎

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

### 检查你的 Python 版本

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

