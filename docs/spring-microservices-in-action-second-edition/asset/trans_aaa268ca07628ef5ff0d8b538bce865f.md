

在开始我们的管道之前，让我们先在云中设置核心基础设施。

## 12.2 在云中设置 O-stock 的核心基础设施

在本书所有的代码示例中，我们都在单个虚拟机镜像内运行应用程序，每个单独的服务都作为 Docker 容器运行。我们现在将改变这一点，把数据库服务器（PostgreSQL）和缓存服务器（Redis）从 Docker 中分离出来，迁移到 Amazon 云中。所有其他服务仍将作为 Docker 容器在单节点 Amazon EKS 集群内运行。图 12.2 显示了 O-stock 服务部署到 Amazon 云的情况。

2. 数据库和 Redis 集群将被迁移到 Amazon 的服务中。
1. 所有核心 O-stock 服务将在单节点 EKS 集群内运行。
3. ECS 容器的安全组设置限制了所有入站端口流量，使得只有端口 8072 对公共流量开放。这意味着所有 O-stock 服务只能通过监听端口 8072 的 API Gateway 服务器访问。
5. 所有其他服务将只能从 EKS 容器内部访问。
4. 组织和许可服务由 OAuth2 身份验证服务保护。

![](img/3c6dc613426280ca3eafe43dea24391e_375_0.png)

图 12.2 使用 Docker，我们可以将所有服务部署到 Amazon EKS 等云提供商。

让我们逐步介绍图 12.2 中的细节。请注意，数字与图中的编号相对应：

- 1. 我们所有的 O-stock 服务（减去数据库和 Redis 集群）将作为 Docker 容器部署在单节点 EKS 集群内。EKS 配置并设置运行 Docker 集群所需的服务器。EKS 还可以监控 Docker 中运行的容器的健康状况，并在服务崩溃时重启我们的服务。
- 2. 部署到 Amazon 云后，我们将不再使用 PostgreSQL 数据库和 Redis 服务器，而是改用 Amazon 关系数据库服务（Amazon RDS）和 Amazon ElastiCache 服务。我们本可以继续在 Docker 中运行 Postgres 和 Redis 数据存储，但我们想突出展示从一种基础设施迁移到完全由云提供商管理的另一种基础设施是多么容易。
- 3. 与桌面部署不同，我们希望服务器的所有流量都通过我们的 API 网关。我们将使用 Amazon 安全组，仅允许部署的 EKS 集群上的端口 8072 可供外界访问。
- 4. 我们仍将使用 Spring 的 OAuth2 服务器来保护我们的服务。在可以访问组织和许可服务之前，用户需要使用我们的身份验证服务进行身份验证（有关此内容的详细信息请参见第 9 章），并在每次服务调用时出示有效的 OAuth2 令牌。
- 5. 我们所有的服务器，包括我们的 Kafka 服务器，都不会通过其公开的 Docker 端口对外部世界公开可访问。它们将只能从 EKS 容器内部访问。

## 使用 AWS 的一些先决条件

要设置您的 Amazon 基础设施，您需要以下内容：

- *您自己的 Amazon Web Services（AWS）账户*——您应该对 AWS 控制台以及在此环境中工作的概念有基本了解。
- *一个网络浏览器*
- *AWS CLI（命令行界面）*——此统一工具管理 AWS 服务。参见 https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html。
- *Kubectl*——此工具允许我们与 Kubernetes 集群通信和交互。参见 https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl。
- *IAM Authenticator*——此工具为我们的 Kubernetes 集群提供身份验证。参见 https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html。
- *Eksctl*——用于在我们的 AWS 账户中管理和创建 AWS EKS 集群的简单命令行实用程序。参见 https://docs.aws.amazon.com/eks/latest/userguide/getting-started-eksctl.html。

如果您完全是 AWS 新手，我们强烈建议您购买 Michael 和 Andreas Wittig 的书籍 *Amazon Web Services in Action*（Manning，2018）。本书的第一章在章节末尾包含了一篇写得很好的教程，介绍如何注册和配置您的 AWS 账户。该章节可在此处下载：

https://www.manning.com/books/amazon-web-services-in-action-second-edition

在本章中，我们尽可能使用了 Amazon 提供的免费套餐服务。唯一无法做到这一点的地方是在设置 EKS 集群时。我们使用了一台 M4 large 服务器，运行成本约为每小时 $0.10。如果您不想承担运行此服务器的高成本，请确保在完成后关闭您的服务。如果您想了解更多关于 AWS EC2 实例定价的信息，请访问以下链接的官方 Amazon 文档：https://aws.amazon.com/ec2/pricing/on-demand/。

最后，不能保证我们在本章中使用的 Amazon 资源（Postgres、Redis 和 EKS）在您想自行运行此代码时仍然可用。如果您要运行本章的代码，您需要设置自己的 GitHub 仓库（用于应用程序配置）、自己的 Jenkins 环境、Docker Hub（用于 Docker 镜像）和 Amazon 账户。然后您需要修改应用程序配置以指向您的账户和凭据。

### 12.2.1 使用 Amazon RDS 创建 PostgreSQL 数据库

首先，我们需要设置和配置我们的 Amazon AWS 账户。完成此操作后，我们将创建用于 O-stock 服务的 PostgreSQL 数据库。为此，我们首先需要登录 Amazon AWS 管理控制台（https://aws.amazon.com/console/）。您首次登录控制台时，会看到 Amazon Web 服务的列表。

- 1. 找到名为 RDS 的链接。点击该链接，它会带您进入 RDS 仪表板。
- 2. 在 RDS 仪表板上，点击大大的 Create Database 按钮。
- 3. 您应该会看到数据库列表。尽管 Amazon RDS 支持不同的数据库引擎，但请选择 PostgreSQL 并点击 Option。这会显示数据库字段。

您现在应该会看到一个包含三个模板选项的屏幕：生产数据库、开发/测试数据库和免费套餐。选择 Free Tier 选项。接下来，在 Settings 下，添加有关我们 PostgreSQL 数据库的必要信息，设置我们将用于登录数据库的主用户 ID 和密码。图 12.3 显示了此屏幕。

![](img/3c6dc613426280ca3eafe43dea24391e_378_0.png)

接下来，我们将保留这些部分的默认配置：DB Instance Size、Storage、Availability & Durability 和 Database Authentication。最后一步是设置以下选项。您可以在任何时候点击 Info 来获取有关您选择的帮助。图 12.4 显示了此屏幕。

- *Virtual Private Cloud（VPC）—*选择 Default VPC。
- *Publicly Accessible—*选择 Yes。
- *VPC Security Group—*选择 Create New，并为该组添加一个名称。例如，我们将安全组命名为 ost​ock-sg。
- *Database Port—*输入 5432 作为 TCP/IP 端口。

此时，我们的数据库创建过程开始（可能需要几分钟）。完成后，我们需要配置 O-stock 服务属性以使用数据库，

![](img/3c6dc613426280ca3eafe43dea24391e_379_0.png)

位于 Spring 配置服务器的服务配置文件中。为此，我们将返回 RDS 仪表板以查看有关我们数据库的信息。图 12.5 显示了此屏幕。

对于本章，您可以为每个需要访问基于 Amazon 的 PostgreSQL 数据库的微服务创建一个名为 aws-dev 的新应用程序配置文件。应用程序配置文件将包含 Amazon 数据库连接信息。

此时，我们的数据库已准备就绪（对于大约五次点击的设置来说相当不错）。让我们继续下一个应用程序基础设施部分，看看如何创建我们的 O-stock 许可服务将使用的 Redis 集群。

