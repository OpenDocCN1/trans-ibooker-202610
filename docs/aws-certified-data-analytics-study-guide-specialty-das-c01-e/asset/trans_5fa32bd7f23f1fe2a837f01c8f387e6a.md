
## 传输中的数据加密

EMR 提供了多种传输中数据加密机制，这些机制可能因 EMR 版本而异。通过安全配置页面，您可以启用以下特定于应用程序的加密功能：

1. **Hadoop**
   - **加密 Shuffle** – Shuffle 是 MapReduce 中的一个重要概念，需要跨多个节点传输数据。MapReduce 加密 Shuffle（bit.ly/2L5CGPm）允许使用 HTTPS 对 MapReduce Shuffle 进行加密。
   - **Hadoop 中的安全 RPC 调用** – 我们在本章前面已经介绍过。更多详细信息，请访问 Apache Hadoop 文档：bit.ly/3beq4jq。
   - **数据块传输的数据加密** – 我们在上一节中已经介绍过。更多详细信息，请访问 bit.ly/3cb88Yk。

2. **HBase**
   - 如果您正在设置一个已启用 Kerberos 的 EMR（从 EMR 5.10.0 版本起支持），集群内的通信将被加密。这由 `hbase.rpc.protection` 属性定义，对于已启用 Kerberos 的 EMR 集群，该属性设置为 privacy。有关 Amazon EMR 和 Kerberos 的信息，请访问：amzn.to/3bgj85m。

3. **Presto**
   - 我们在前面章节中讨论了 Presto 架构。Presto 节点之间的任何内部通信都使用 SSL/TLS（安全套接字层/传输层安全）——仅限 Amazon EMR 5.6.0 及以上版本。

4. **Tez**
   - 您可以使用 Tez 作为 Hive 查询的运行时，而不是 MapReduce。当 Tez 需要在 Hadoop 节点之间 Shuffle 数据时，Tez Shuffle 处理器使用 TLS。

5. **Spark**
   - 从 EMR 5.9.0 开始，Spark 组件之间的内部 RPC 通信使用 AES-256 加密。早期版本使用 SASL，并以 DIGEST-MD5 作为密码套件。
   - 当使用 Spark 历史服务器和支持 HTTPS 的文件服务器等接口时，所有 HTTP 通信都通过 SSL 加密。

## 身份验证

安全配置还允许您启用 Kerberos 进行身份验证。Kerberos 是一种由 MIT（麻省理工学院）创建的网络身份验证协议，它使用密钥加密技术提供强身份验证，因此密码或其他凭据不会以未加密的形式在网络上传送。

当您启用 Kerberos 身份验证时，Amazon EMR 会为安装在集群上的应用程序、组件和子系统配置 Kerberos，以便它们相互进行身份验证。

Amazon EMR 提供了两种选项：设置一个专用于密钥分发中心（KDC）的集群，或设置一个外部 KDC。在两种情况下，您都可以在控制台中配置票据有效期，默认设置为 24 小时。您还可以设置跨域信任（cross-realm trust），这意味着您允许来自不同 Kerberos 配置的主体（通常是用户）向 EMR 集群上的应用程序组件进行身份验证。设置跨域信任有一些注意事项，文档（amzn.to/2SNZTtw）中有更详细的说明。

## EMRFS 的 IAM 角色

EMRFS 是 EMR 集群推荐的持久存储。当您使用 `s3://mybucket/mydata` 格式请求存储在 Amazon S3 中的数据时，Amazon EMR 将使用 EMRFS 来满足您的请求。默认情况下，Amazon EMR 使用您在创建集群时附加到集群的服务角色中定义的权限策略。这意味着，无论哪个用户或组访问该数据，也无论 Amazon S3 上的位置如何，您都使用相同的角色。

在某些情况下，Amazon EMR 是一个共享集群，多个用户访问集群时需要对底层数据有不同级别的访问权限。您可以使用 EMRFS 的 IAM 角色实现这种细粒度的访问控制。

在安全配置中，您可以在 EMRFS 的 IAM 角色部分指定角色映射，每个角色映射对应一个标识符，该标识符用于确定通过 EMRFS 对 Amazon S3 的访问权限。这些标识符可以是用户、组或指示数据集位置的 Amazon S3 前缀。

## EMRFS 的 IAM 角色如何工作

当您设置了角色映射并使用 EMRFS 向 Amazon S3 发出请求时，如果请求匹配访问依据（用户、组、S3 位置），EMRFS 会让集群的 EC2 实例为特定请求代入相应角色。这实质上允许您从多个 IAM 角色中进行选择，EC2 实例在发出请求时可以代入这些角色。

当通过 EMRFS 发出请求时，EMRFS 会自上而下地评估角色映射，这意味着不仅顺序很重要，而且附加到角色的策略应该限制对 Amazon S3 的权限。如果找不到匹配项，EMRFS 会回退到您在创建集群时定义的原始服务角色。

> 在某些场景下，您需要应用程序用户之间的应用程序级隔离，而不是主机用户之间的主机级隔离。EMRFS 的 IAM 角色提供应用程序级隔离，因此任何可以访问集群的用户都可以绕过隔离来代入任何角色。

## AWS Lake Formation 集成

您还可以将 Amazon EMR 与 AWS Lake Formation 集成，以提供对 AWS Glue Data Catalog 中数据库和表的细粒度列级访问。Amazon Lake Formation 还允许您从符合 SAML 的企业身份系统启用对 EMR 和 Apache Zeppelin 笔记本的联合单点登录。在撰写本书时，该功能处于测试阶段，我们希望当您读到本书时，该功能已经正式发布（GA）。

在启用 Amazon EMR 和 Lake Formation 集成之前，必须满足以下要求：

1. 公司身份应在基于 SAML 的身份提供程序中管理，例如 Active Directory 联合身份验证服务（ADFS）。
2. 您的元数据存储应为 AWS Glue Data Catalog。
3. 您必须使用 Apache Zeppelin 或 EMR 笔记本来访问由 AWS Glue 和 AWS Lake Formation 管理的数据。
4. 您应在 AWS Lake Formation 中定义好权限。

Amazon EMR 和 AWS Lake Formation 支持以下应用程序：

- Apache Spark
- Apache Zeppelin
- Amazon EMR 笔记本

在撰写本书时，尚不支持其他应用程序。要了解更多关于 AWS Lake Formation 集成的信息，请访问 amzn.to/3cqaySV。

## 阻止公共访问

Amazon EMR 通常用作数据湖中数据的主要处理引擎。因此，它是一个关键资源，需要得到保护，以确保只允许相关用户访问。安全最关键的一个方面是阻止公共访问您的资源，这就是为什么 Amazon EMR 直接在控制台上提供此功能。

EMR 控制台允许您在开启（On）和关闭（Off）之间切换该设置，同时还提供为具有特定端口的集群设置端口例外的功能，这些例外将继续允许来自所有公共 IP 地址的入站流量。

当您将该设置切换为开启（On）时，如果某个 EMR 集群的安全设置允许来自所有公共地址（IPv4 0.0.0.0/0 或 IPv6::/0）的所有入站流量，则该集群将被阻止启动。此设置适用于当前区域中启动的所有集群。

## VPC 子网

您可以使用此选项为集群配置私有子网，方法是添加 Amazon S3 端点以访问 Amazon S3，或添加 NAT 实例以访问公共 AWS 服务端点。我们在前面章节中讨论了使用私有子网的详细信息。

**对账户中所有 IAM 用户可见的集群** 此选项允许您让集群对账户中的其他 IAM 用户可见。取消选择此选项意味着只有集群创建者才能在账户中看到该集群。

**权限** 此选项允许您选择 EMR 调用其他 AWS 服务所需的角色，并提供默认权限和自定义权限两种选项。

**默认权限**为 EMR 角色和 EC2 实例配置文件角色提供默认角色。我们在这里看到的新角色是 Auto Scaling 角色，它在快速选项中不可用。该角色允许自动缩放在触发自动缩放规则时添加或删除 EC2 实例。其默认角色是 EMR_AutoScaling_DefaultRole，它有一个名为 AmazonElasticMapReduceforAutoScalingRole 的默认策略，该策略基本上允许您描述 CloudWatch 告警，以及列出和修改实例组。

**自定义权限**为您提供选择自定义角色的选项。

**安全配置** 我们之前讨论了安全配置以及安全配置中可用的选项。默认值为“无”。但是，您可以选择要应用于此 EMR 集群的安全配置。

**EC2 安全组** 我们在本章前面讨论了子网和安全组的作用。提醒一下，安全组基本上是集群节点（主节点、核心节点、任务节点）的防火墙，帮助您控制进出集群的流量。您可以在安全选项的这一部分选择 EMR 托管的安全组和其他安全组。

图6.7展示了高级集群设置期间可用的安全选项。

## EMR 安全摘要

我们已经了解了各种 EMR 安全选项，包括在私有/公共子网中运行 EMR、从 EMR 访问其他服务、安全配置、阻止对服务的公共访问，以及由服务管理的集群。这不仅能让您为运行安全的 EMR 集群做好准备，还能帮助您应对考试中与 EMR 相关的安全问题。最后，我强烈建议您在参加考试之前实际操作这些安全选项并阅读以下博客。

- “保护 Amazon EMR 的最佳实践” – aws.amazon.com/blogs/big-data/best-practices-for-securing-amazon-emr/
- “使用 Block Public Access 配置保护您的集群” – aws.amazon.com/blogs/big-data/secure-your-amazon-emr-cluster-from-unintentional-network-exposure-with-block-public-access-configuration
- “使用 Apache Knox 在 Amazon EMR 中实现周边安全” – aws.amazon.com/blogs/big-data/implement-perimeter-security-in-emr-using-apache-knox

