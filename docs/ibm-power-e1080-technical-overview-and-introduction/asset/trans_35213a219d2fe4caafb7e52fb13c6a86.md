

IBM PowerVC 为 IBM Power Systems 客户提供以下优势：

- 它与 Power Systems 深度集成。
- 它提供虚拟化管理工具。
- 它简化了由 PowerVM 管理的服务器在自动化 IT 环境（如云）中的集成。
- 它是基于 Power Systems 的 IBM 基础设施即服务（IaaS）的构建模块。
- PowerVC 与其他云管理工具（如 Ansible、Terraform 或 OpenShift）集成，并可集成到编排工具中，如 IBM Cloud® Automation Manager、VMware vRealize 或 SAP Landscape Management（LaMa）。
- PowerVC 还提供私有云和公有云之间 VM 镜像的便捷交换。

IBM PowerVC 是 PowerVM 企业虚拟化技术集的补充，提供虚拟化管理。它基于开放标准，并将服务器管理与存储和网络管理集成。

由于 IBM PowerVC 基于 OpenStack 计划，Power Systems 可由兼容 OpenStack 标准的工具进行管理。当系统由 IBM PowerVC 控制时，可以通过以下三种方式进行管理：

- 系统管理员使用 IBM PowerVC 图形用户界面（GUI）
- 使用包含 IBM PowerVC 表示状态传输（REST）应用程序编程接口（API）的脚本的系统管理员
- 通过标准 OpenStack API 调用 IBM PowerVC 的更高层工具

以下 PowerVC 方案在 Power Systems 云可用解决方案中定位如下：

- IBM PowerVC：高级虚拟化管理
- IBM PowerVC for Private Cloud：基础云
- IBM Cloud Automation Manager：高级云
- VMware vRealize：高级云

PowerVC 提供企业客户所需的系统管理产品，以有效管理 IBM 高端硬件提供的先进功能。它减少资源使用并管理工作负载以实现性能和可用性。

## 3.3 使用 Ansible 进行系统自动化

企业花费大量宝贵的管理员时间执行重复性任务和使用手动流程。诸如更新、修补、合规检查、配置新的虚拟机或 LPAR，以及确保正确的安全更新到位等任务，占用了本可用于更有价值业务活动的宝贵时间。

使用 Ansible 实现自动化可为系统管理员赢回宝贵时间。

Red Hat Ansible Automation Platform for Power 已完全启用，因此企业可以自动化 AIX、IBM i 和 Linux 中的多个任务，包括部署应用程序。Ansible 还可以与 HMC、PowerVC 和 Power Virtual Server 结合，在任何需要的地方配置基础设施，包括来自其他 IBM 业务合作伙伴或基于 Power 处理器服务器的第三方提供商的云解决方案。

在新 LPAR 初始安装或设置后的首要任务是确保安装了正确的补丁。此外，还必须安装额外的软件（无论是开源软件、ISV 软件还是他们自己的企业软件）。Ansible 拥有一套推出新软件的能力，这使其在持续交付/持续集成（CD/CI）环境中很受欢迎。编排和自动化与安全产品的集成代表了 Ansible 在数据中心中应用的另一种方式。

尽管 AIX 和 IBM i 在许多不同业务领域被不同类型的客户广泛采用，但 Ansible 可以帮助将基于 Power 处理器的技术引入那些认为 AIX 和 IBM i 技能是稀缺商品、难以在市场上找到但又想利用硬件平台所有功能的客户。Ansible 体验在基于 Power 或 x86 处理器的技术上完全相同，并且相同的步骤可以在 IBM Cloud 中重复。

AIX 和 IBM i 技能客户也可以从 Ansible 提供的极端自动化解决方案中受益。

基于 Power 处理器的架构在工程团队紧密协作以确保无缝工作的集成堆栈方面，相比商品服务器平台（如 x86）具有独特的优势。处理器、主板、虚拟化和管理设备上的工程团队密切合作以确保集成堆栈无缝运行。这种方法与多供应商基于 x86 处理器的技术方法形成鲜明对比，在后者中，处理器、服务器、管理、虚拟化必须从不同的（有时是竞争的）供应商处购买。

Power 堆栈工程团队密切合作交付企业服务器平台，从而实现具有业界领先性能、可扩展性和安全性的 IT 架构（见图 3-3）。

![](img/7c8373945565ba432037f60bbf8458eb_160_0.png)

图 3-3 Power 堆栈

Power 堆栈中的每一层都经过优化，使基于 Power10 处理器的技术成为关键任务企业工作负载的首选平台。该堆栈包括接下来描述的 Ansible Automation Platform。

### 3.3.1 Ansible Automation Platform

Ansible Automation Platform 与基于 IBM Power 处理器的技术集成，该集成包含在 Red Hat Ansible 网站的 Certified Integrations 部分。

用于基于 IBM Power 处理器的技术的各种 Ansible 集合（截至本文撰写时已被客户下载超过 25,000 次）现已包含在 Red Hat Ansible Automation Platform 中。因此，这些模块由 Red Hat 的 24x7 企业支持团队提供支持，该团队与相应的基于 Power 处理器的技术开发团队协作。

### 3.3.2 Ansible 生态系统中的 Power Systems

一系列 Ansible 集合可用于基于 Power 处理器的平台。*集合*是一组模块、Playbooks 和角色。拥抱 Power 社区的 Ansible，我们的 AIX 和 IBM i 社区拥有合适的模块集。一些示例是开发用于管理 AIX 逻辑卷的工具，这些工具将模块接口置于安装密钥命令之上，或管理 AIX init tab 条目。

从 IBM i 角度来看，一个示例是能够对内置于 IBM i 平台的集成 IBM Db2® 数据库运行 SQL 查询、管理对象权限等。所有这些对 AIX 管理员或 IBM i 管理员都有意义。

我们的操作系统团队开发的模块会发送到开源社区（名为 Ansible Galaxy）。每个开发人员都可以将任何可以作为集合候选的对象发布到开放的 Ansible Galaxy 社区，并可能获得 IBM 的认证支持，通过订阅 Red Hat Ansible Automation Platform（见图 3-4）。

![](img/7c8373945565ba432037f60bbf8458eb_161_0.png)

### 3.3.3 用于 AIX 的 Ansible 模块

IBM Power Systems AIX 集合作为 Ansible Content for IBM Power Systems 的更广泛产品的一部分，可从 Ansible Galaxy 获得并提供社区支持。

该集合包括模块和示例 playbook，有助于自动化任务，并可从此网页开始使用。

### 3.3.4 用于 IBM i 的 Ansible 模块

Ansible Content for IBM Power Systems - IBM i 提供模块、操作插件、角色和示例 playbook，用于自动化 IBM i 上的任务，包括以下示例：

- 命令执行
- 系统和应用程序配置
- 工作管理
- 修复管理
- 应用程序部署

有关集合的更多信息，请参阅此网页。

### 3.3.5 用于 HMC 的 Ansible 模块

IBM Power Systems HMC 集合提供可用于管理 HMC 系统配置和部署的模块。该集合内容有助于通过 Ansible 生态系统将基于 Power 处理器平台的工作负载作为企业自动化战略的一部分纳入其中。

有关此集合的更多信息，请参阅此网页。

### 3.3.6 用于 VIOS 的 Ansible 模块

IBM Power Systems VIOS 集合提供可用于管理 Power VIOS 系统配置和部署的模块。该集合内容有助于通过 Ansible 系统将基于 Power 处理器平台的工作负载作为企业自动化战略的一部分纳入其中。

有关更多信息，请参阅此网页。

## 3.4 从核心到云保护信任

尽管强制执行数据加密策略是最大限度降低数据泄露风险的有效方法，进而最大限度降低成本，但截至本文撰写时，只有 17% 的受访企业表示他们使用加密保护了超过 50% 的云中敏感数据¹。全球范围内只有少数企业在整个组织中一致应用加密策略，主要是因为它增加了复杂性、成本，并对性能产生负面影响，这意味着对业务的 SLA 未能实现。

快速发展的网络威胁格局需要关注网络弹性。持久且端到端的保护是降低威胁暴露的唯一方法。

基于 Power 处理器的平台始终提供其类别中最安全和最可靠的服务器。Power E1080 进一步扩展了基于 Power 处理器平台的业界领先的安全性和可靠性，重点在于保护跨所有混合云环境中的应用程序和数据。它还引入了以下主要方面的重大创新：

- 先进的数据保护，提供简单易用且高效的能力，通过加密和多因素身份验证等机制保护敏感数据。

¹ source: Thales Data Threat Report - Global Edition: https://cpl.thalesgroup.com/resources/encryption/2021/data-threat-report

