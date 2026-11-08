# SQL Server 2019 Always On：持续运行支持 24x7 应用

*本书献给我的灵感之源——我的孩子们 Reuben、Iris 和 Finola。*

# 前言

## 引言

*SQL Server 2019 AlwaysOn* 旨在成为 DBA 的快速入门指南，他们需要为其 SQL Server 工作负载实施高可用性和灾难恢复。

本书首先讨论高可用性概念和方法，然后对 HA/DR 技术进行高层次概述。随后进入细节。我们将讨论如何创建 Windows 集群，然后介绍如何创建 SQL Server AlwaysOn 故障转移群集实例。

本书的重点随后转向 AlwaysOn 可用性组。在这里，本书讨论如何在 Windows 和 Linux 操作系统上为高可用性和灾难恢复创建和配置可用性组。

我们随后将探讨 AlwaysOn 可用性组的非典型用例，例如无群集和域无关的可用性组，以及在 Azure IaaS 上配置可用性组和 SQL Server 标准版的基础可用性组。

最后，本书关注 AlwaysOn 的运维方面，其中章节涵盖该技术的管理、监控和故障排除。

## 致谢

我想向令人惊叹的 Chris Dent 表示感谢，他再次帮助我完成了本书的网络配置。

我还要感谢 Ian Stirk，他的技术审阅始终提升了我书籍的质量，本书也不例外。

## 关于作者 Peter A. Carter

是拥有超过 15 年经验的 SQL Server 专家，致力于开发、管理和构建 SQL Server 平台和数据层应用程序。Peter 于 2011 年获得微软颁发的 MCC，并在其从 2005 年版本开始的 SQL Server MCTS、MCITP、MCSA 和 MCSE 认证系列中占据一席之地。他对 SQL Server 的热情体现在他所做的一切中，他的目标是让自己对该技术的热情激励他人。

## 关于技术审阅者 Ian Stirk

是伦敦的一名自由 SQL Server 顾问。除本职工作外，他还是作者、软件实用工具的创建者以及技术审阅者，定期为 [www.i-programmer.info](http://www.i-programmer.info) 撰写书籍评论。

他涵盖 SQL Server 的各个方面，并在性能和可扩展性方面具有专业兴趣。如果您需要帮助处理 SQL Server 系统，请随时通过 ian_stirk@yahoo.com 或 [www.linkedin.com/in/ian-stirk-bb9a31/](http://www.linkedin.com/in/ian-stirk-bb9a31/) 联系他。

Ian 想感谢 Peter Carter、Jonathan Gennick 和 Jill Balzano，让他的这次书籍体验变得更轻松。

我们没有人能独自站立，基于此，Ian 想感谢这些特别的人：Mr. Courts、Graham Howes、Dave Starkey、Florence Dedeng、Meaza Yohannes、Feven Yohannes 和 Adhanet Mehretab。

Ian 为本书工作所获得的费用已捐赠给 World Child Cancer（[www.worldchildcancer.org/](http://www.worldchildcancer.org/)）。
