# 下一代 AI 基础设施构建指南 (for . .)













## 第一版修订历史

* 2026-04-27: First Release



PROD: Uncomment the following sentence if appropriate and add it to the above para:This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice.

This work is part of a collaboration between O’Reilly and Canonical. See our [statement of editorial independence](https://oreil.ly/editorial-independence).

979-8-341-67306-9

\[LSI\]

## 序言

我仍然记得，带着一丝怀旧，年轻时无休止的夜晚，我花在父母在我苦苦哀求多年后才为我们买的个人电脑上。那时，技术并不一定容易获取，而这笔购买相当于投入了一个月的工资。其他时光，但都是美好的时光……

我会在那些夜晚分析电脑上的每一块硬件和软件——一颗 Intel Pentium III 500 MHz 处理器。它推动了我最初的电子游戏，以及长时间的功能分析和测试，以了解操作系统的所有细节。出于某种原因，我在 Linux 发行版中尝试复制我在 Microsoft Windows 98 中看到的一些操作系统功能时，获得了很大的乐趣。这是一个技术挑战，激发了我的好奇心，并帮助我仅凭试错，自己找到技巧，手边只有手册和书籍，而 DSL 连接要到几年后才出现。

但我认为最浪漫的事情之一是能够通过邮件获取 Ubuntu 和 Kubuntu 发行版。那时，Canonical 免费向全球发送光盘，以促进开源的大规模采用。他们的 `ShipIt` 项目于 2011 年停止（你可以从他们的网站下载镜像），但我们生活中的这一段经历有力地提醒了我们，轻松获取技术是多么重要。

现在一切都不同了，但我仍然发现了一些相似之处。以前的 Pentium，如今是 Intel Core 或 Intel Xeon 处理器。取而代之的是等待数周的物理磁盘，我在笔记本上安装 `ISO` 镜像。当我从事专业项目时，我会通过 Azure 门户从 Linux 虚拟机 (`VMs`) 部署这些 Ubuntu 镜像。我显然有偏见，因为我整天都在使用云平台，但能够在几次点击之间访问那些配备专用硬件和我经典发行版的资源，在这些对高效且强大的 AI 创新需求极高的时代，感觉有点神奇。

虽然我始终欣赏生成式 AI (`GenAI`) 时代的持续进步，拥有更广泛、更高效的语言模型（尤其是我可以在笔记本上运行的那种小型语言模型 \[SLMs\]），但在我的日常 AI 架构和产品管理工作中，我真的需要推动模型训练和大规模推理的极限。幸运的是，我的父母不再需要帮我获取技术了。而对我的合作伙伴和客户来说更好的是，他们不需要花费无尽的夜晚或投入公司整个月的收入来实现他们的新 AI 应用。

我个人认为这很美。大小公司都能获得同等水平的<|message|>。建筑师、开发者、科学家和工程师能够获得实施其项目的工具，无论其物理位置如何。进入门槛更低，获得创新的机会更多。

在一个渴望好消息和积极影响的世界里，这很重要。当我比较我早期的日子和现代的日子时，虽然我珍惜复古时代的美好回忆，但我很幸运能够成为一代人中的一员，能够凭借强大的技术栈创造更多有趣的东西，有趣的是，它结合了与我小时候使用的一模一样的组件：计算能力、操作系统和技能。

Adrián González Sánchez

O’Reilly author, AI professional, and university lecturer

P.S. Thanks mum and dad, you made it happen.
