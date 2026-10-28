

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

## Edge信息亭自助部署场景 253
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

## 监控与分析 496
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

