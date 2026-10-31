

## 精通 Windows Server 2019

系统管理员安装、管理和部署 Windows Server 2019 新功能的完整指南

## 第三版

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_0_0.png)

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_0_1.png)

## 精通 Windows Server
2019

*第三版*

系统管理员安装、管理和部署 Windows Server 2019 新功能的完整指南

Jordan Krause

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_1_0.png)

伯明翰 - 孟买

## 精通 Windows Server 2019

*第三版*

版权所有 © 2021 Packt Publishing

保留所有权利。未经出版商事先书面许可，本书的任何部分不得以任何形式或任何手段复制、存储在检索系统中或传输，除非是嵌入在评论或评论文章中的简短引文。

本书编写过程中已尽最大努力确保所提供信息的准确性。然而，本书中包含的信息按原样出售，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成的任何损害承担责任。

Packt Publishing 已尽力就本书中提及的所有公司和产品提供商标信息，适当使用大写字母。然而，Packt Publishing 不能保证这些信息的准确性。

**制作人：** Caitlin Meadows, Suman Sen
**收购编辑 – 同行评审：** Saby D'silva
**项目编辑：** Rianna Rodrigues
**内容开发编辑：** Bhavesh Amin
**校对编辑：** Safis Editing
**技术编辑：** Karan Sonawane
**校对员：** Safis Editing
**索引员：** Manju Arasan
**演示设计师：** Ganesh Bhadwalkar

首次出版：2016年10月
第二版：2019年3月
第三版：2021年7月

生产参考：2280721

由 Packt Publishing Ltd. 出版。
Livery Place
35 Livery Street
Birmingham B3 2PB, UK.

ISBN 978-1-80107-831-3

www.packt.com

## 贡献者

## 关于作者

**Jordan Krause** 是一位拥有超过20年经验的IT专业人士，因其在微软服务器和网络技术方面的工作而获得了9次微软MVP奖。作为世界上最早的微软DirectAccess专家之一，他热衷于帮助公司找到实现远程劳动力最佳方式的方法。致力于持续学习，Jordan 持有 MCSE、MCSA 和 MCITP 企业管理员认证，并著有多本关于微软技术的书籍。Jordan 生活在美丽的西密歇根州（美国），但每天与世界各地的公司合作。

> 我如果没有我那位 wonderful 的妻子的支持，就没有能力、专注力或动力去从事这样一项写作项目。感谢你陪伴我度过所有那些凌晨5:00的开始，Laura！

## 关于审阅者

**Luka Manojlović** 担任IT基础设施解决方案的外部顾问、架构师和实施者。拥有超过20年在存储、网络、虚拟化和服务器方面的经验，Luka 仍然热情地应对他所从事的挑战和项目。

> *首先，我要感谢作者 Jordan Krause。阅读本书的撰写过程是一种愉快的体验。我认为它将帮助读者更好地理解 Windows Server 技术的神奇世界。我还要感谢 Packt 的工作人员，他们提供了出色的合作。*

我们还要感谢 **Aaron Guilmette** 和 **Dishan Francis** 对 *第3章，Active Directory* 的反馈。

## 目录

- 前言 xv
- 第1章：开始使用 Windows Server 2019 1
  - Windows Server 的用途 3
  - 云化趋势 4
    - 公有云 5
    - 私有云 6
  - Windows Server 版本和许可 7
    - Standard 与 Datacenter 7
    - 三种不同的界面 8
      - Desktop Experience 8
      - Server Core 8
      - Nano Server – 现在仅用于容器 9
    - 许可模式 – SAC 和 LTSC 10
      - 半年度频道 (SAC) 10
      - 长期服务频道 (LTSC) 11
  - 新功能和更新功能概述 11
    - Windows 10 体验延续 12
    - 超融合基础设施 12
    - Windows Admin Center 13
    - Windows Defender 高级威胁防护 13
    - 禁止密码 14
    - 软重启 14
    - 与 Linux 集成 15
    - SAC 版本正在缩小！ 15
    - 增强型防护虚拟机 16
    - Azure 网络适配器 16
    - Always On VPN 17
- 系统洞察 17
- **Windows Server 2019 界面** 18
  - 更新的“开始”菜单 19
  - 快速管理任务菜单 20
  - 使用搜索功能 22
  - 将程序固定到任务栏 24
  - 右键单击的强大功能 25
- **使用更新的“设置”屏幕** 28
- **做同一件事的两种方法** 31
  - 通过控制面板创建新用户 31
  - 通过“设置”菜单创建新用户 32
- **任务管理器** 34
- **任务视图** 38
- **摘要** 40
- **问题** 41
- **第2章：安装和管理 Windows Server 2019** 43
  - 技术要求 44
- **安装 Windows Server 2019** 45
  - 烧录该 ISO 45
  - 创建可启动 USB 驱动器 46
  - 运行安装程序 47
- **安装角色和功能** 52
  - 使用向导安装角色 52
  - 使用 PowerShell 安装功能 58
- **集中管理和监控** 61
  - 服务器管理器 61
  - 远程服务器管理工具 (RSAT) 66
  - 这是否意味着 RDP 已死？ 69
    - 远程桌面连接管理器 70
- **Windows Admin Center (WAC)** 71
  - 安装 Windows Admin Center 73
  - 启动 Windows Admin Center 74
  - 向 Windows Admin Center 添加更多服务器 76
  - 使用 Windows Admin Center 管理服务器 78
  - 更改易如反掌 79
  - Azure 集成 80
- **使用 Sysprep 实现快速服务器部署** 81
  - 在新服务器上安装 Windows Server 2019 83
  - 在新服务器上配置自定义设置和更新 83
  - 运行 Sysprep 准备并关闭主服务器 84
  - 创建驱动器的主映像 86
- 使用主映像副本构建新服务器 87
- **摘要** 88
- **问题** 89
- **第3章：Active Directory** 91
  - **什么是域控制器？** 92
    - Active Directory 域服务 93
  - **创建您的第一个域** 94
    - 准备您的域控制器 95
    - 安装 AD DS 角色 97
    - 配置域 98
      - 树、林和...域？ 98
  - **为冗余配置多个域控制器** 103
  - **Active Directory 用户和计算机** 106
    - 用户账户 108
    - 安全组 109
    - 预先分发计算机账户 110
  - **Active Directory 域和信任** 113
    - 建立信任 114
      - 网络连接 115
      - 条件 DNS 转发 115
      - 配置信任 118
      - 测试一下！ 122
  - **Active Directory 站点和服务** 123
  - **Active Directory 管理中心** 125
    - 动态访问控制 126
    - 精细密码策略 127
  - **只读域控制器** 128
  - **组策略** 132
  - **摘要** 133
  - **问题** 133
- **第4章：DNS 和 DHCP** 135
  - **DNS 的用途** 136
  - **DNS 记录类型** 137
    - 主机记录 (A 或 AAAA) 138
    - 别名记录 – CNAME 140
    - 邮件交换器 (MX) 记录 142
    - TXT 记录 143
    - SPF 记录 143
      - 强制规则 -all 145
    - 名称服务器 (NS) 记录 146
      - 公共名称服务器记录 146

