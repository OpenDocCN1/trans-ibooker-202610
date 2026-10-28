

_dmarc.paypal.com.

你将看到以下输出：

```
"v=DMARC1; p=reject; rua=mailto:d@rua.agari.com; ruf=mailto:d@ruf.agari.com"
```

现在试试这个：

```
_dmarc.linkedin.com.
```

你将看到以下输出：

```
"v=DMARC1; p=reject; rua=mailto:d@rua.agari.com; ruf=mailto:d@ruf.agari.com; pct=100"
```

字段及其值如下：

**v=** DMARC 版本（始终为 1）。

**p=** 当 DMARC 身份验证失败时，接收邮件服务器应应用的策略。适用于主域和所有子域，除非包含可选的 sp 标签。三种可能的策略值是 none、quarantine 和 reject。

**rua=** 告诉接收邮件服务器将聚合报告发送到何处，这些报告以 XML（可扩展标记语言）格式提供，有助于深入了解你的电子邮件基础设施，通过识别身份验证可能存在的问题甚至恶意活动。

**ruf=** 告诉接收邮件服务器将具有 DMARC 违规的电子邮件的删节取证副本发送到何处。与显示电子邮件流量的 RUA 报告不同，RUF 报告包含电子邮件的片段。由于隐私问题，删节并不理想，因此这种报告已不再常用。医疗、金融和政府行业不会要求此项，以免将来因删节不够完善而承担责任。

**pct=** 应用 DMARC 策略的消息百分比。这允许公司逐步实施和监控策略及其可能产生的影响。取值范围为 1 到 100，100 为默认值。

以下是其他一些 DMARC 字段和值：

**fo=** 告诉接收邮件服务器向你发送电子邮件样本以

## 第 18 章 云计算

### 实验练习

- 18.01 Microsoft Azure 培训
- 18.02 探索 Microsoft Azure
- 18.03 AWS Educate 和 AWS 培训与认证
- 18.04 探索 AWS

### 实验分析

### 关键术语测验

在 https://csrc.nist.gov/publications/detail/sp/800-145/final， 国家标准与技术研究所（NIST）对云计算的定义如下：

云计算是一种模型，可实现无处不在、便捷、按需的网络访问共享的、可配置的计算资源池（例如网络、服务器、存储、应用程序和服务），这些资源可以快速配置并在最少的管理工作或服务提供商交互的情况下释放。此云模型由五个基本特征、三种服务模型和四种部署模型组成。

云计算在 NIST 特刊 800-145 中进一步说明（可在 https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-145.pdf 获取）：

- i. 你将看到以下消息：“您的账户已全部设置好。请完成反馈以继续。” 在“还有其他想让我们知道的吗？” 框中，输入任何内容并单击提交按钮。你将被带到 Microsoft Azure 快速入门中心，如图 18-1 所示。

**图 18-1 快速入门中心**

### 步骤 2 现在你已全部设置好，将探索 Microsoft Azure 快速入门中心。

- a. 如果你是从上一步继续，请单击 Quickstart Center 上方的 Home 链接。如果不是从上一步继续，请前往 https://portal.azure.com/，如图 18-2 所示。

![](img/6e921d997751f0a0f87e5170ac9657a1_655_0.png)

**图 18-2 Azure 主页**

你会注意到一个通知，说明你有 200.00 美元的 Azure 服务额度。在探索各种资源时请记住这一点，这些资源都与相关成本相关联。

- b. 单击 Azure 服务下的第一个选项“创建资源”。

- c. 单击左侧边栏菜单上 Azure Marketplace 的每个类别，如图 18-3 所示。这些类别包括以下内容：AI + 机器学习、分析、区块链、计算、容器、数据库、开发工具、DevOps、身份、集成、物联网、IT 与管理工具、媒体、迁移、混合现实、监控与诊断、网络、安全、软件即服务（SaaS）、存储和 Web。

```text
Home >
New
Search the Marketplace
Azure Marketplace
See all
- Get started
- Recently created
- AI + Machine Learning
- Analytics
- Blockchain
- Compute
- Containers
- Databases
- Developer Tools
- DevOps
- Identity
- Integration
- Internet of Things
- IT & Management Tools
- Media
- Migration
- Mixed Reality
- Monitoring & Diagnostics
- Networking
- Security
- Software as a Service (SaaS)
- Storage
- Web

Popular
- Windows Server 2016 Datacenter
  - Quickstarts + tutorials
- Ubuntu Server 18.04 LTS
  - Learn more
- Web App
  - Quickstarts + tutorials
- SQL Database
  - Quickstarts + tutorials
- Function App
  - Quickstarts + tutorials
- Azure Cosmos DB
  - Quickstarts + tutorials
- Kubernetes Service
  - Quickstarts + tutorials
- DevOps Starter
  - Quickstarts + tutorials
- Storage account
  - Quickstarts + tutorials
```

**图 18-3 Azure Marketplace**

某些类别中的项目具有“快速入门 + 教程”链接，而其他项目具有“了解更多”链接。探索所有类别中的所有项目。

- d. 从五个不同类别中选择任意五个资源并解释为什么通过 Microsoft Azure 使用它们是有优势的。

- e. 可选地，使用你的 200 美元额度或花费更多资金，部署一个或多个资源。

- f. 可选地，在主屏幕上（你可以通过单击顶部蓝色栏左侧的 Microsoft Azure 返回），单击工具部分中的 Microsoft Learn。 在顶部，你可以单击“帮助我们自定义你的路径”链接并按照向导操作以查看相关选择。 在“我们认为你可能会喜欢这些”部分，你可以单击“探索其他热门路径”链接，清除所有筛选器后将显示 2,000 条结果！

**8–10 小时，大约**

### 实验练习 18.03：AWS Educate 和 AWS 培训与认证

来自 https://aws.amazon.com/what-is-aws/：

Amazon Web Services（AWS）是全球最全面、最广泛采用的云平台，提供来自全球数据中心的 175 多项功能齐全的服务。数百万客户——包括增长最快的初创公司、最大型企业和领先的政府机构——都在使用 AWS 以降低成本、提高敏捷性并更快创新。

来自 https://aws.amazon.com/education/awseducate/：

AWS Educate 已在 200 多个国家和地区使用。它连接了 2,400 所机构、超过 10,000 名教育工作者和数百名

在此直接登录。

你可以在此处访问 AWS Educate 学生门户的视频演示。

### 免费 AWS 基础培训

要免费访问我们的基础 AWS 云从业者基础在线学习课程并找到其他自定进度实验，你必须拥有 AWS 账户或 Amazon ID。

- 如果你拥有 AWS 账户，请登录并单击此处以获得这些福利。
- 如果你没有 AWS 账户，请单击此处并按照说明创建 Amazon ID 以访问这些福利。

一旦访问培训与认证门户，请单击“学习库”并搜索“AWS Cloud Practitioner Essentials”以轻松找到并注册 AWS Cloud Practitioner Essentials 在线培训。设置账户后，你可以随时通过单击此处访问 AWS 培训。

再次感谢你参与 AWS Educate，我们希望你喜欢这个项目！

祝你学习顺利，

AWS Educate 团队

- e. 登录 AWS Educate 门户 https://www.awseducate.com/signin/SiteLogin。

- f. 在云计算 101 部分，单击开始按钮，保持英语为默认语言（或根据需要更改），然后单击开始按钮。

- g. 你将进入云计算 101 课程的主页。单击左侧菜单中的模块链接以查看课程的三个模块的具体内容。

**模块 1：简介**

AWS 云计算基础

**模块 2：AWS 云计算服务**

