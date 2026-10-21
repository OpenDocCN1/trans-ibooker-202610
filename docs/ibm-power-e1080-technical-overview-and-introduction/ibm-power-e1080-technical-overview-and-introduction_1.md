## 前言

本 IBM® Redpaper® 出版物提供了对 IBM Power® E1080 服务器（也称为 Power E1080）新架构的广泛理解，该服务器支持 IBM AIX®、IBM i 以及选定的 Linux 操作系统发行版。本文档的目的是介绍 Power E1080，即 IBM Power 产品组合中最强大和最可扩展的服务器，以及它的产品和相关功能：

- ► 设计支持多达四个系统节点和多达 240 个 IBM Power10™ 处理器内核

Power E1080 可按一、二、三或四节点配置订购，每个节点提供多达 60 个 Power10 处理器内核，最大可达四节点配置中的 240 个 Power10 处理器内核。

- ► 设计支持多达 64 TB 内存

Power E1080 最初可订购的总内存 RAM 容量最高达 8 TB，并在完整的四系统节点服务器中支持多达 64 TB。

- ► 设计支持在完整的四系统节点服务器中多达 32 个 Peripheral Component Interconnect® (PCIe) Gen 5 插槽，以及使用扩展 I/O 抽屉的多达 192 个 PCIe Gen 3 插槽

- ► 多达超过 4,000 个直接连接的串行连接 SCSI (SAS) 磁盘或固态驱动器 (SSD)

- ► 每台系统多达 1,000 个虚拟机 (VM) 具有逻辑分区 (LPAR)

- ► 系统控制单元，提供冗余系统主 Flexible Service Processor (FSP)

- ► 支持 IBM Power System Private Cloud Solution with Dynamic Capacity

本出版物面向希望更好地了解 Power 服务器的专业人士。预期读者包括以下角色：

- ► 客户
- ► 销售和营销专业人员
- ► 技术支持专业人员
- ► IBM 业务合作伙伴
- ► 独立软件供应商 (ISVs)

本文不替代当前的营销材料和配置工具。它旨在作为额外信息来源，结合现有资料，可用于增强您对 IBM 服务器解决方案的了解。

## 作者

本文由在 IBM Redbooks 奥斯汀中心的全球专家团队制作。

Giuliano Anselmi 是 IBM Digital Sales 都柏林的 IBM Power 数字销售技术顾问。他加入 IBM 后专注于基于 Power 处理器的技术。近 20 年来，他担任多个技术角色。他是其团队使命的重要资源，并为业务合作伙伴和客户提供参考。

Manish Arora 是在印度 IBM Techline 团队工作的 IBM Power 技术专家，持有印度 IETE 的计算机科学学士学位。他拥有 15 年 IBM Power 系统、AIX 和 Linux 技术的经验。他于 2007 年加入 IBM 担任 SSR，管理 X 系列、P 系列服务器和中端存储产品。他于 2016 年加入 IBM 担任技术专家。其专业领域包括企业 Power 系统、企业池、AIX、HMC 和 PowerVC。

Ivaylo Bozhinov 是 IBM 保加利亚的 Power Systems 主题专家 (SME)。他的主要专业领域是解决 IBM Power Systems 产品、IBM AIX、VIOS、HMC、IBM i、PowerVM® 和 Linux on Power Systems 服务器上的复杂硬件和软件问题。他自 2015 年起加入 IBM，提供响应式故障修复、主动、预防性和认知支持。

Dinil Das 在印度 IBM 的 Power Systems Lab Services 团队担任解决方案架构师。他持有 MG University 的技术与通信学位。Dinil 拥有超过 15 年与 IBM Power Systems、IBM System Storage 和高性能计算 (HPC) 工作的经验。专业领域包括系统性能、可用性和 HPC。

Turgut Genc 是英国 IBM Systems Lab Services 的高级顾问，持有伊斯坦布尔 Yıldız 技术大学计算机科学工程硕士学位。他是一位多面手，在 Power Systems 和 AIX 方面拥有 18 年的专业经验。他是 EMEA Power to Cloud Rewards 团队的负责人，负责 IBM PowerVS、企业池、性能、迁移研讨会和 Lab Services 工具。

Bartlomiej Grabowski 是 IBM Champion 和 DHL IT Services 的首席系统专家，支持全球 IBM Power Systems。他拥有超过 17 年的企业解决方案经验。他持有波兰 Bielsko-Biala 计算机科学与管理学院的计算机科学学士学位。其专业领域包括 IBM Power Systems、IBM i、IBM PowerHA®、IBM PowerVM 和存储解决方案。Bartlomiej 还运营技术博客 www.theibmi.org。他是铂金 IBM Redbooks® 作者。

Madison Lee 是哥伦比亚大学三年级计算机科学学生，今年夏天在 IBM 旧金山办公室担任技术解决方案专家实习生。她加入 IBM Redbooks 团队，使她能够参与多个驻地项目。在实习期间，Madison 共同撰写了两份官方 IBM 文档。

Armin Röll 在德国担任 IBM Power IT 专家。他持有德国汉堡大学实验物理学位。Armin 拥有 25 年 IBM Power 和 AIX 预销售技术支持经验。他撰写了多本关于 AIX 和 IBM Power 系统硬件的 IBM Redbooks 出版物，是铂金 Redbooks 作者。

该项目的管理者为：

Scott Vetter
PMP, IBM Austin, US

感谢以下人员对本项目做出的贡献：

Ryan Achilles, Brian Allison, Ron Arroyo, John Banchy, Joanna Bartz, Bart Blaner,
Gareth Coates, Arnold Flores, Austin Fowler, George Gaylord, Douglas Gibbs, Nigel Griffiths,
Daniel Henderson, Stephanie Jensen, Kyle Keaty, Rajaram B Krishnamurthy,
Fredrik Lundholm, Charles Marino, Michael Mueller, Vincent Mulligan,
Hariganesh Muralidharan, Kaveh Naderi, Mark Nellen, Brandon Pederson,
Michael Quaranta, Ian Robinson, Todd Rosedahl, Bruno Spruth, Bill Starke,

## 现在您也可以成为出版作者！

这是一个展示您的技能、发展职业生涯并同时成为出版作者的机会！加入 IBM Redbooks 驻地项目，并在您专长的领域共同撰写一本书，同时磨练您使用领先技术的经验。您的努力将有助于提高产品接受度和客户满意度，同时扩展您的技术联系和关系网络。驻地项目时长为两到六周，您可以亲自参与或作为远程驻地人员在家工作。

了解有关驻地计划的更多信息，浏览驻地索引，并在以下网址在线申请：

```
ibm.com/redbooks/residencies.html
```

## 欢迎评论

您的评论对我们很重要！

我们希望我们的论文尽可能有用。请通过以下方式之一向我们发送对本文或其它 IBM Redbooks 出版物的评论：

- 使用以下网址的在线 Contact us review Redbooks 表单：
- ibm.com/redbooks
- 发送电子邮件至：
- redbooks@us.ibm.com
- 邮寄您的评论至：
- IBM Corporation, IBM Redbooks
- Dept. HYTD Mail Station P099
- 2455 South Road
- Poughkeepsie, NY 12601-5400

## 保持与 IBM Redbooks 的联系

- 在 LinkedIn 上找到我们：
- http://www.linkedin.com/groups?home=&gid=2130806
- 通过 IBM Redbooks 每周通讯探索新的 Redbooks 出版物、驻地和研讨会：
- https://www.redbooks.ibm.com/Redbooks.nsf/subscribe?OpenForm
- 通过 RSS Feed 了解最新的 Redbooks 出版物：
- http://www.redbooks.ibm.com/rss.html





![](img/7c8373945565ba432037f60bbf8458eb_14_0.png)
