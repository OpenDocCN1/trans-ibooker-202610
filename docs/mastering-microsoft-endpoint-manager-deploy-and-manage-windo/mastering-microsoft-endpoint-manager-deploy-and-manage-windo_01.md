## Foreword

### The history behind Microsoft Endpoint Manager

I started at Microsoft as a developer right out of college in the early 90s. My first coding assignment was on **Project Hermes**, building an inventory and application delivery tool for the new operating system that Microsoft was about to release – Windows NT. Project Hermes later became **Systems Management Server (SMS)** 1.0. I stayed on the project and eventually became the product and engineering owner responsible for building and releasing Configuration Manager 2007 and 2012. Right after releasing Configuration Manager 2012, I went to lead the engineering team for Intune/MEM and re-architected Intune to become the industry-leading cloud-based device management service. At the start of 2020, I was asked whether I was interested in leading the product and engineering effort for a new startup project called Cloud PC; I remembered saying *sign me up* because I saw it as the last piece of the puzzle to complete the full circle for device management.

When I joined Microsoft, device management as an industry didn't exist. With the introduction of SMS 1.0 and through Configuration Manager 2012, Microsoft created a new IT function – device management. With Intune/MEM, we expanded device management from Windows devices to all devices and we brought device management to the cloud. **Virtual Desktop Infrastructure (VDI)** has existed for decades, but it has always been costly to set up and run and is difficult to adapt to changing business needs. With Windows 365 Cloud PC, we set out to democratize VDI. We want cloud PCs to be easy to provision and manage, just like how IT manages their physical devices today, and in addition, we wanted to remove much of the complexity and cost associated with VDI.

In the last year and a half, we built a brand-new Azure-based service to enable every MEM admin to provision and manage cloud PCs as easily as they can with their physical Windows PCs, or iOS/Android devices. With the full elasticity of Azure, the predictable pricing, and the ability to instantly purchase, provision, and resize cloud PCs, we are creating many new use cases for running a Windows PC in the cloud that cannot be easily achieved by traditional VDI or physical PCs.

On behalf of the hundreds of engineers and product managers that worked tirelessly in the middle of the COVID pandemic building the Windows 365 service, and to all the IT heroes everywhere managing and securing devices, we want to introduce to you a new category of PC – Cloud PC. This book will help you to adopt Windows 365 quickly and gain new superpowers in device management.

Enjoy your newfound power.

Ken Pan

CVP Windows 365 & MMD at Microsoft

### The history behind Cloud PC

I joined Microsoft in 2001 as a Hardware Technical Evangelist and Lead Program Manager for Networking. I worked with hardware partners to improve user experiences and expand the market for consumer networking products. During this time of my career, I created several patents around wireless setup, network discovery, and media streaming technologies. The culmination of this work led to the development and standardization of Wi-Fi Protected Setup and the adoption of a number of Microsoft networking technologies in common industry products. Working with partners to bring new value to customers was ingrained in my DNA and is something I have carried forward in my career.

During the Windows 7 era, I transitioned to media streaming and led the product team implementing video in HTML5, DRM, Media CODECs, and media streaming. My team delivered a number of new end user experiences including PlayTo and remote media access to Windows Media Player. From there I moved to the Xbox team to deliver SmartGlass for Xbox One.

About 10 years into my career at Microsoft, my interests led me to virtualization, which was a culmination of all the technologies I had worked on, such as home networking, audio and video encoding, DRM, media streaming, and device redirection. At that time, Microsoft's first-party virtualization offering (RDS) had a very limited feature set and scalability. Though the technology was heavily used by the tech community for accessing personal desktops, adoption was relatively small in the enterprise space (this was largely led by Citrix).

The RDS team was ripe with talented engineers who had focused their entire careers working on virtualization and the partner ecosystem was clamoring for opportunities to engage with Microsoft to deliver value-added services. This combination of a large talent pool, ready and willing partners, and continued growth of virtualization use cases created just the right environment to develop a new virtualization platform that could reach a broader audience.

- Create an end user experience that is consistent with physical Windows devices.
- Provide a seamless experience to scale up and out as the needs of our customers evolve.
- Provide fixed monthly pricing options that enable customers to manage their budgets.
- No global admin required; administration is properly distributed to the right admins in familiar portals.

Right around the time we finalized our principles, the world was hit with a global pandemic that would forever change the way we work. The team was only a few months old when COVID hit, and we had just started building out the new team when the world went into lockdown. Seeing as we were developing a new service that would empower users to be productive anywhere in the world, we took on the challenge of putting together a globally distributed team and developing the service using Teams, SharePoint, Azure DevOps, and other tools to keep the team in sync. 8 months later we started our private preview of the service and 8 months after that we went live.

The world will forever be changed by the effects of COVID. Organizations are creating more opportunities for remote workers, and they are migrating their workloads to the cloud to expand their reach to provide corporate resources to a globally distributed workforce. Windows 365 was cast from this new environment and is just the right product to empower organizations to provide secure and elastic Windows Desktops to their globally distributed workforce.

Scott Manchester

Director of Product Management at Microsoft





## Contributors
## 关于作者

**Christiaan Brinkhoff** 担任微软 Windows 365 工程团队的主管项目经理和社区负责人，推动诸如 Windows 11 集成等新功能以及大量不同的新社区计划，同时发挥其专业专长帮助客户构想新的虚拟化体验。Christiaan 于 2018 年作为 FSLogix 收购的一部分加入微软。

他还获得了微软 MVP、Citrix CTP 和 VMware vExpert 社区成就。

> *写书需要巨大的奉献和持续的精力，尤其是在搬到美国雷德蒙德后，家庭也在成长。我要感谢我的妻子一直支持我，并允许我投入大量私人时间来完成这本书！*

**Per Larsen** 担任微软 Endpoint Manager - 客户加速团队 - **商业管理体验 (CMX)** 工程的高级项目经理，他将来自微软最大和最具战略意义客户的经验带回工程团队，以推动服务的改进，使客户拥有持续改进的产品体验。他还帮助部署和采用微软 Endpoint Manager - Microsoft Intune。Per 主要专注于 Windows 和特殊设备（如 HoloLens 2、Surface Hub 和 Microsoft Teams Room System）的管理。

Per 在 2016 年 7 月 1 日至 2018 年 4 月 1 日加入微软时期间也是企业移动性 MVP。

> *在疫情期间在家全天候写书需要奉献和持续的精力。我要感谢我的孩子们一直支持我，并允许我投入大量私人时间来撰写这本书！*

## 关于审阅者

**Seif Bassem** 是微软全球技术团队的高级客户工程师，专注于 Azure 应用和基础设施。他还在微软 FastTrack 工作过，帮助客户使用 Microsoft Endpoint Manager 现代化其设备供应、管理和保护方式。

在加入微软之前，Seif 在 IT 行业拥有 10 年经验，领导一支工程师团队，负责在金融服务领域管理和部署各种微软解决方案和项目。

他是认证的 Azure 解决方案架构师、管理员、安全工程师和 Microsoft 365 认证现代桌面管理员。他还作为 CompTIA 主题专家参与了 IT 基础认证考试准备。

**Peter Cashen** 是英国的科技顾问，通过自己的公司 Kloud 365 Ltd 工作，自 1990 年代末以来专注于终端用户计算。他参与微软技术超过 20 年，包括 SMS、System Center Configuration Manager、Endpoint Manager 和其他相关工具。Peter 的客户来自多个行业，包括零售、银行、紧急服务、安全政府、制药、法律和工程，并曾在微软工作过一段时间。

Peter 目前正在帮助全球客户进行大型 Azure Active Directory 仅实现，以帮助他们减少（或消除）本地部署。

> 这很辛苦也很具挑战性——但我喜欢挑战！

**Paul Winstanley** 是五次企业移动性 MVP，拥有超过 25 年的 IT 经验。他在过去十年专注于通过 Microsoft Endpoint Manager、Configuration Manager 和 Intune 进行终端管理。

Paul 是一名独立顾问，拥有自己的终端管理公司 SCCM Solutions，该公司成立于 2013 年。

他在自己的 SCCMentor 网站上撰写博客，分享他对 Configuration Manager、Intune、Windows 10 和 MDM 的知识，并在 Twitter @sccmentor 上活跃。

他原籍英格兰北部的约克郡，在过去 25 年里与妻子、四个孩子和妹夫一起生活在伦敦。

> 我想感谢 Sheila，我的妻子，所有孩子 Joseph、Miles、James 和 Beth，以及我的妹夫 Paul，让我能够投入时间做我所做的事情。还要感谢社区分享想法和解决方案，并以友好方式交流，这有助于建立伟大的友谊，解决棘手问题，并在日常生活中让我的生活更轻松。

**Neil McLoughlin** 位于英国曼彻斯特。他在 IT 行业工作超过 20 年，涉足多个不同的行业和角色。他大约有 10 年为大型企业客户提供 Citrix 咨询的经验。

大约 5 年前，Neil 发现了云和 DaaS，此后专注于基于云的桌面解决方案，主要是 Azure Virtual Desktop 和 Microsoft 365。

Neil 热衷于社区工作，运营着英国 Citrix Azure Virtual Desktop 用户组和 WVD 社区，这是一个对 Azure Virtual Desktop 感兴趣的世界性社区。

他目前受雇于 Nerdio 担任英国现场 CTO，但之前曾在 New Signature、Computacenter 和 Cap Gemini 担任高级顾问。

> 我想感谢我的妻子给我时间让我在许多晚上和周末待在办公室审阅这本书，还要感谢 Christiaan Brinkhoff，他在需要时提供了很大的帮助。谢谢你，Christiaan，真的很感激你的所有帮助和建议！





# 第3节：精通Microsoft Endpoint Manager

5

## Microsoft Endpoint Manager的要求
自助服务功能 – IT管理员 87
重新配置云电脑 87
本地管理员 88
VM SKU升级（预览功能） 88
映像管理 – 创建自定义映像（可选） 89
支持的端点 95
信息工作者门户（IWP） 97
Azure AD – MyApps统一（工作区）门户 101
多因素认证与条件访问 102
云电脑的安全基线 105
通过Microsoft Endpoint Manager – Intune将远程桌面客户端分发到物理端点 106
在远程桌面客户端中自动订阅用户 117
Autopilot与云电脑 – 轻量瘦客户端（信息亭） 120
监控与分析 122
使用快速助手支持用户 123
Windows 11 124
Microsoft Managed Desktop 125
总结 127
问题 127
答案 127
延伸阅读 127

## 端点场景
Microsoft Intune的身份角色与权限 132
合规管理员 132
合规数据管理员 133
Intune管理员 133
消息中心读者 133
安全管理员 133
安全操作员 133
安全读者 134
Windows 365云电脑的身份角色与权限 134
Azure订阅所有者 134
Intune管理员 134
域管理员 135
Universal Print的身份角色与权限 135
打印机管理员 135
打印机技术员 135
许可要求 135
支持的操作系统 136
所需的Web浏览器版本 137
Windows 11要求 137

7

## 管理Windows Autopilot
技术要求 212
Windows Autopilot概述 212
将硬件ID上传到Windows Autopilot 216
针对现有设备的Windows Autopilot 226
开箱即用体验（OOBE）期间的Windows更新 229
在Intune中自动分配Windows Autopilot配置文件 230
登录到Graph Explorer 232
注册状态页（ESP） 235
ESP实现Windows CSP 236
Autopilot报告与诊断 237
公司门户 240
为Autopilot设备配置自动BitLocker加密 240
云配置场景 246
部署用户访问工作或学校资源可能需要的必备内容 253

## Edge信息亭自助部署场景
为Edge信息亭创建特定的ESP 253
创建Windows Autopilot配置文件 254
自助部署（预览） 255
Autopilot重置 260
擦除和重置设备 262
全新开始 263
Windows恢复环境 264
总结 264
问题 265
答案 265
延伸阅读 265

8

## 应用程序管理与交付
通过Microsoft Endpoint Manager进行应用程序交付 268
可以部署的不同应用程序类型 268
LOB应用程序 270
取代模式 288
社区工具 – Win32App迁移工具 292
部署Microsoft 365应用 293
更新频道 295
Office自定义工具 296
Microsoft 365应用管理中心 298
Microsoft 365应用 – 自定义 306
部署Microsoft Teams 307
OneDrive 308
部署Microsoft Edge 310

11

## Office策略管理
Office云策略服务 400
使用OCP服务创建策略配置 402
配置策略 405
OCP服务中的提示与技巧 408
Office云策略如何应用？ 412
安全策略顾问 415
总结 418
问题 419
答案 419
延伸阅读 419

12

## 用户配置文件管理
Windows配置文件 422
现代配置文件管理 423
企业状态漫游 423
Microsoft Office的漫游设置 425
Outlook的签名云设置 427
OneDrive for Business已知文件夹移动 428
Windows 10存储感知 430
OneDrive与存储感知 431
Microsoft Edge 432
ESR + OneDrive + Edge + Office 434
从传统配置文件管理迁移到现代配置文件管理 435
总结 436
问题 436
答案 437
延伸阅读 437

13

## 身份与安全管理
Microsoft Identity 440
AAD 441
AAD用户 441
AAD访客用户 442
AAD组类型 442
AAD成员类型 443
混合AAD 446
条件访问 450
用户和组 451
云应用 452
条件 454
问题 396
答案 397
延伸阅读 397

14
监控与端点分析

## 监控与分析
监控物理和虚拟云端点 496
端点分析 – 高级监控 499
启动性能 – 登录持续时间 499
性能得分明细 500
影响启动的前10大进程 506
操作系统重启历史 507
资源性能 508
洞察与建议 – 得分趋势 508
应用程序可靠性 509
Windows 365特定指标 510
洞察与建议 512
Configuration Manager数据收集 513
自定义基线 514
主动修复 515
Azure Monitor集成 517
生产力得分 518
服务运行状况 519
总结 521
问题 521
答案 521
延伸阅读 522

15

## Universal Print
- 什么是Universal Print？ — 523
- Universal Print – 架构解析 — 524
- 打印连接器 — 526
- 我的打印数据会去哪里？ — 526
- 打印机默认设置 — 526
- Universal Print – 服务要求 — 527
- 网络要求 — 528
- 学习如何部署Universal Print — 528
- 委派打印机访问权限 – 自定义角色 — 529
- 将现有打印机连接到Universal Print — 530
- 配置Universal Print — 531
- 启用混合AD配置 – 通过Universal Print连接器 — 536
- 使用Universal Print注册自定义打印机 — 538
- 与用户共享打印机 — 539
- 分配使用打印机的权限 — 540
- 测试已连接Universal Print的打印机 — 541
- 使用Microsoft Endpoint Manager分配和部署云打印机 — 546
- 总结 — 556
- 问题 — 556
- 答案 — 556
- 延伸阅读 — 557

## 第4节：来自实战的技巧与提示

16

## Microsoft Endpoint Manager故障排除
- MEM故障排除 — 562
- 服务运行状况与消息中心 — 564
- MEM中的故障排除边栏 — 566
- Windows 10 MEM注册故障排除 — 570
- BitLocker故障 — 574
- Windows 10设备诊断 — 575
- 客户端要求 — 575
- 应用程序交付故障排除 — 579
- Win32 — 579
- LOB — 581
- Microsoft Store应用 — 581
- Autopilot故障排除 — 582
- Windows 11 Autopilot诊断页面 — 584
- 定位Windows设备的故障排除 — 588

## 目录
- 故障排除
- Microsoft Edge — 592
- 总结 — 594
- 问题 — 594
- 答案 — 594
- 延伸阅读 — 594

17
## Windows 365故障排除
- 自行排查和联系Microsoft支持 — 596
- Windows 365预配错误 — 602
- 云电脑 – 基于设备的筛选 — 611
- 总结 — 616
- 问题 — 616
- 延伸阅读 — 616

18
## 社区帮助
- 加入新的W365社区！ — 617
- Microsoft Tech Community和MS Learn — 618
- 其他社区博客、Microsoft MVP等... — 618
- 总结 — 620

## 你可能还喜欢的其他书籍

## 索引

## 前言
现代工作场所解决方案旨在简化环境的管理层，但其采用缓慢的主要原因之一是对产品缺乏理解和认识。通过本书，你将学到将迁移到现代工作场所所需的一切知识，支持运行Windows 10、Windows 11或Windows 365。

*精通Microsoft Endpoint Manager*详细解释了各种概念，让你清楚地规划如何使用**Microsoft Endpoint Manager (MEM)**，并提前消除潜在的迁移挑战。你将掌握使用Windows 365云电脑、Windows Autopilot、配置文件管理、监控与分析、Universal Print等新服务。本书将带你了解最新功能和新Microsoft云服务，帮助你掌握MEM的基础知识，并了解可以管理哪些服务。无论是物理端点还是云端点，本书都涵盖。

读完本书后，你将能够设置MEM并高效地使用它运行Windows 10、Windows 11和Windows 365。

你将学到什么：
- 使用Windows 365云电脑简化Windows在云中的部署。
- 在MEM中配置高级策略管理。
- 探索物理和云电脑的现代配置文件管理和迁移选项。
- 通过基线设置和其他安全最佳实践强化安全性。
- 查找MEM、Windows 365云电脑等的故障排除提示和技巧。
- 发现物理和云管理端点的部署最佳实践。
- 跟上Microsoft社区的步伐，并发现值得关注的MVP名单。





# 本书适合谁

如果你是一名 IT 专业人员、企业移动性管理员、架构师或顾问，希望通过 MEM 学习如何在物理和云端点上管理 Windows 以实现远程工作，那么本书适合你。

# 本书涵盖内容

*Chapter 1, Introduction to Microsoft 365,* 教你如何在利用 Microsoft 365 更广泛产品套件中其他服务的同时保持资源安全。理解产品的 fundamentals 是成功部署最重要的因素。

*Chapter 2, What Is Unified Endpoint Management?,* 承认现代管理的 basics 有时难以理解，因此你将学习关于现代管理和零信任的概念以及 MEM（Intune）的历史和架构概念，以便清晰地了解物理、虚拟和移动的所有设备如何在同一个管理控制台中汇聚。

*Chapter 3, Introducing Windows 365,* 教你一切开始使用这项新 Microsoft 云服务所需的内容，该服务可简化部署以及使用 MEM 进行云 PC 维护。

*Chapter 4, Deploying Windows 365,* 教你关于如何部署 Windows 365 的一切所需知识，所需的要求是什么，以及技巧和窍门。

*Chapter 5, Requirements for Microsoft Endpoint Manager,* 提供对 MEM 不同要求的清晰理解，从操作系统版本和 URL 防火墙允许列表到所需的许可证和权限。

*Chapter 6, Windows Deployment and Management,* 教你关于使用 MEM – Intune 部署 Windows 10 Enterprise 的内容。

*Chapter 7, Manager Windows Autopilot,* 教你何时以及如何使用 Autopilot 在物理端点设备上注册 Windows 10。推荐的方法是什么以及事先需要做出哪些决定？你将在本章中了解所有这些内容。

*Chapter 8, Application Management and Delivery,* 教授部署和管理 Windows 10 端点上的 Microsoft 365 和业务线应用程序的最佳实践。

*Chapter 9, Understanding Policy Management,* 教你关于不同策略类型，现代策略管理意味着什么，以及它在 Windows 10/11 客户端上与组策略相比的工作方式。

Chapter 10, Advanced Policy Management, 教你关于不同的策略选项，以自定义和保护环境中的 Windows 10 Enterprise 桌面。

Chapter 11, Office Policy Management, 教你关于不同的策略选项，以自定义和保护环境中的 Windows 10 Enterprise 桌面。

Chapter 12, User Profile Management, 讨论配置文件管理是确保良好用户体验的非常重要的因素。你将了解不同的 Windows 配置文件类型以及服务之间的差异，以便在不同的端点设备上提供类似的体验，例如使用 Enterprise State Roaming 和 Microsoft Edge 的物理和云端点。

Chapter 13, Identity and Security Management, 教你如何以最安全的方式为终端用户和 IT 部门配置 Azure Active Directory。你将学习启用 Azure MFA 的不同选项，关于 BitLocker，以及如何使用 MEM – Intune 中的端到端安全级别集成配置 Microsoft Defender for Endpoint。

Chapter 14, Monitoring and Endpoint Analytics, 看看在部署桌面之后，确保 Windows 和应用程序的性能、登录持续时间细分和质量水平是多么重要。你将在本章中学习如何使用 Endpoint Analytics、Productivity Score 和 MEM 的其他监控功能来实现这一点。

Chapter 15, Universal Print, 看看 Universal Print 以及尽管企业越来越多地以数字方式做事，打印在物理纸上仍然很重要。Universal Print 是 Azure 上相对较新的平台服务，可以简化整个打印配置和维护过程，与传统的打印服务器环境相比。

Chapter 16, Troubleshooting Microsoft Endpoint Manager, 教授部署 Windows 10 Enterprise 和其他技巧和窍门以使部署顺利进行的最常见原因和修复方法。两位作者在多种形式的 Windows 部署方面拥有超过 20 年的现场经验，他们将在本节中分享这些经验。

Chapter 17, Troubleshooting Windows 365, 教你关于 Windows 365 Cloud PC 的所有不同故障排除错误，以便在环境中部署云 PC 时能够主动应对可能发生的任何错误。

Chapter 18, Community Help, 分享作者拥有强大的社区背景的一些最佳社区博客；有些由初学者撰写，而有些则由 Microsoft MVP 撰写。

# 如何充分利用本书

为了充分利用本书，最好对 MEM、Azure、Microsoft 365 云服务等有基础水平的了解。这不是必需的，因为你将在本书中学到所需的一切！

# 下载彩色图片

我们还提供了一份 PDF 文件，其中包含本书中使用的屏幕截图和图表的彩色图片。你可以在此处下载：https://static.packt-cdn.com/downloads/9781801078993_ColorImages.pdf。

# 使用的约定

本书中使用了多种文本约定。

Code in text: 表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 账号。以下是示例："Enter Device type restriction - HR as the name."

代码块如下所示：

```
<?xml version="1.0"?>
<HardwareReport>
    <HardwareInventory>
        <p n="ToolVersion" v="3" />
        <p n="HardwareInventoryVersion" v="131" />
```

当我们希望引起你对代码块中特定部分的注意时，相关行或项目以粗体显示：

```
msiexec /i " RemoteDesktop_1.2.1755.0_x64.msi" /qn ALLUSERS=2 MSIINSTALLPERUSER=1
```

Bold: 表示新术语、重要单词或你在屏幕上看到的单词。例如，菜单或对话框中的单词以粗体显示。以下是示例："Go to Tenant admin | Roles | Administrator Licensing."

> Tips or important notes
> Appear like this.

# 联系我们

我们始终欢迎读者的反馈。

**General feedback:** 如果你对本书的任何方面有疑问，请发送电子邮件至 `customercare@packtpub.com`，并在邮件主题中提及书名。

**Errata:** 尽管我们已尽一切努力确保内容的准确性，但错误难免。如果你在本书中发现错误，我们将不胜感激你向我们报告。请访问 `www.packtpub.com/support/errata` 并填写表格。

**Piracy:** 如果你在互联网上发现我们作品的任何形式的非法副本，我们将不胜感激你提供位置地址或网站名称。请联系我们 `copyright@packt.com` 并附上材料链接。

**If you are interested in becoming an author:** 如果你有专业知识的主题并且有兴趣撰写或为一本书做出贡献，请访问 `authors.packtpub.com`。

# 分享你的想法

一旦你读完 *Mastering Microsoft Endpoint Manager*，我们很想听听你的想法！请点击此处直接前往本书在 Amazon 的评论页面并分享你的反馈。

你的评论对我们和技术社区都很重要，并将帮助我们确保提供卓越质量的内容。
