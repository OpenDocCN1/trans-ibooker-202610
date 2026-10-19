

![](assets/cover.png)

![](assets/cover.png)

# Canonical

[![“大规模开源 AI”宣传图，展示 Canonical、Microsoft 和 Intel 之间为通过 Ubuntu Pro 在由 Intel Xeon 驱动的 Azure 基础设施上增强 AI 模型而进行的协作。](assets/ad.png)](https://ubuntu.com/azure?utm_medium=organic&utm_source=whitepaper&utm_campaign=701N100000hbUhdIAE)

# 构建下一代 AI 基础设施

by Adrián González Sánchez

Copyright © 2026 O’Reilly Media, Inc. All rights reserved.

Published by O’Reilly Media, Inc., 141 Stony Circle, Suite 195, Santa Rosa, CA 95401.

O’Reilly books may be purchased for educational, business, or sales promotional use. Online editions are also available for most titles ([https://oreilly.com](https://oreilly.com)). For more information, contact our corporate/institutional sales department: 800-998-9938 or *corporate@oreilly.com*.

* Acquisitions Editor: Megan Laddusaw
* Development Editor: Gary O’Brien
* Production Editor: Aleeya Rahman
* Copyeditor: Penelope Perkins
* Cover Designer: Susan Thompson
* Interior Designer: David Futato
* Interior Illustrator: Kate Dullea
* May 2026: First Edition

# 第一版修订历史

* 2026-04-27: First Release

The O’Reilly logo is a registered trademark of O’Reilly Media, Inc. *Building the Next Generation of AI Infrastructure*, the cover image, and related trade dress are trademarks of O’Reilly Media, Inc.

The views expressed in this work are those of the author and do not represent the publisher’s views. While the publisher and the author have used good faith efforts to ensure that the information and instructions contained in this work are accurate, the publisher and the author disclaim all responsibility for errors or omissions, including without limitation responsibility for damages resulting from the use of or reliance on this work. Use of the information and instructions contained in this work is at your own risk. If any code samples or other technology this work contains or describes is subject to open source licenses or the intellectual property rights of others, it is your responsibility to ensure that your use thereof complies with such licenses and/or rights.

PROD: Uncomment the following sentence if appropriate and add it to the above para:This book is not intended as \[legal/medical/financial; use the appropriate reference\] advice. Please consult a qualified professional if you require \[legal/medical/financial\] advice.

This work is part of a collaboration between O’Reilly and Canonical. See our [statement of editorial independence](https://oreil.ly/editorial-independence).

979-8-341-67306-9

\[LSI\]

# 序言

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

# 第 1 章. AI 基础设施万岁

*AI 基础设施* 的概念并不新鲜，但随着世界各地的组织扩大其 AI 采用程度，它变得越来越相关。即使在预训练 GenAI 的时代，组织仍然需要坚实的平台和专用硬件。缺乏强大的架构基础是那些不拥有自己的裸机环境公司的重大瓶颈。此外，基础设施的性质会随着时间的推移而变化，因为 *训练* 新的机器学习 (`ML`) 或深度学习 (`DL`) 模型、*微调* 现有的大型语言模型 (`LLMs`)，或进入生产并 *推理* 最终的 AI 部署并不是同一回事。

而且 AI 基础设施的角色在从纯科学活动明显转向科学和工程的混合，以大规模提供 AI 赋能应用的时代仍然相关。这种混合保留了两者的优点，但也包括诸如 *实验* 成为新产品创建的常态，以及 *可扩展性*（根据你的背景，这意味着任何你想要的意思）以务实、高效和强大的方式连接现有资源等考虑。作为这种有机结果，日常活动的性质现在已经改变。以前是关于准备和挖掘数据源，今天——对于大多数组织而言——是一项技术管理和 `API`- 驱动的 AI 能力部署的练习，将注意力转移到经典的开发活动上。

