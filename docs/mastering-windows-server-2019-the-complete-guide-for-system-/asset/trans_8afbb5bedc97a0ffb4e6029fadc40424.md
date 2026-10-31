
Windows Defender 防火墙高级安全窗口将打开并准备接受您的输入。我们不必翻阅控制面板或打开常规的 Windows 防火墙然后点击高级设置链接，这些都是通过鼠标进入此控制台的常见方式。通过知道我们的 MSC 快捷方式名称，我们能够直接打开完整的 WFAS 控制台，我经常去那里检查特定的防火墙规则或状态：

![](img/d2d47a2ce8c1af61acefc2cdd6d9df70_656_0.png)

图 15.29：访问 Windows 防火墙最快的方式是通过 WF.MSC

现在您已经了解了 MSC 命令的工作原理，并且再次说明，您可以在许多不同的地方输入 MSC 名称并调用它，我想给您留下一份常用的 MSC 控制台列表，您可以使用它们快速访问服务器上的许多管理控制台：

- DSA.MSC：活动目录用户和计算机
- DSSITE.MSC：活动目录站点和服务
- DNSMGMT.MSC：DNS 管理器
- GPEDIT.MSC：本地组策略编辑器
- GPMC.MSC：组策略管理控制台
- CERTSRV.MSC：证书颁发机构管理
- CERTTMPL.MSC：证书模板管理
- CERTLM.MSC：本地计算机证书存储
- CERTMGR.MSC：当前用户证书存储
- COMPMGMT.MSC：计算机管理
- DEVMGMT.MSC：设备管理器
- DHCPMGMT.MSC：DHCP 管理器
- DISKMGMT.MSC：磁盘管理
- EVENTVWR.MSC：事件查看器
- PERFMON.MSC：性能监视器
- SECPOL.MSC：本地安全策略控制台
- FSMGMT.MSC：共享文件夹
- WF.MSC：Windows Defender 防火墙高级安全
- shell:startup：虽然不是 MSC，但这是一个快捷方式，可打开已登录用户的“开始菜单\程序\启动”文件夹。这是查找在启动时运行但在其他地方找不到的程序时很有用的位置，或当您希望在该用户每次登录计算机时启动应用程序时
- shell:common startup：类似于 shell:startup，但它会打开到公共用户的“启动”文件夹。在此处添加的快捷方式将在任何用户登录系统时自动启动

## 摘要

设计和构建全新的服务器很少会遇到挑战，如果遇到，通常也是低优先级的问题，因为新服务器尚未影响生产工作队伍。另一方面，排除服务器故障可能非常有压力，需要快速回忆操作系统内部可用的工具和命令集，以便识别问题和解决方案。我希望本章能为您提供所需的弹药，让您在这些情况下更有信心，并在诊断服务器问题和深入挖掘出现的问题时成为您 IT 部门的“首选”人员。

这就为我们关于 Windows Server 2019 的故事画上了句号。我们讨论的许多主题都可以填满整本书，我希望本卷提供的思路足以促使您进一步深入研究您计划使用的技术。微软技术在全球大多数数据中心占据主导地位。Windows Server 2019 中的新功能和更新功能将确保这一趋势在未来很长一段时间内持续下去。

## 问题

- 1. 打开活动目录用户和计算机的 MSC 快捷方式是什么？
- 2. 真或假 – 出于性能原因，您应该始终将服务器备份存储在您备份的同一台服务器上。
- 3. 哪个 Windows 监视工具可用于在 24 小时内绘制 CPU 利用率图表？
- 4. 真或假 – System Insights 将所有数据存储在集中的 RADIUS 服务器上。
- 5. 打开 Windows Defender 防火墙高级安全的 MSC 快捷方式是什么？
- 6. 哪个 PowerShell cmdlet 可用于收集 Windows 日志文件？
- 7. 哪个事件 ID 可用于计算系统正常运行时间？

## 附录：章节末问题答案

### 第 1 章：开始使用 Windows Server 2019

- 1. 在 Windows Server 2019 中，如何用两次鼠标点击启动提升的 PowerShell 提示？
  **Answer:** 右键单击 **开始** 按钮并从 **快速管理任务** 菜单中选择 **Windows PowerShell（管理员）**
- 2. 打开 **快速管理任务** 菜单的键盘组合是什么？
  **Answer:** *WinKey+X*
- 3. 微软的云服务产品名称是什么？
  **Answer:** Microsoft Azure
- 4. Windows Server 2019 的两个许可版本是什么？
  **Answer:** Standard 和 Datacenter
- 5. Windows Server 2019 Standard 主机上可以运行多少个虚拟机？
  **Answer:** 2
- 6. Windows Server 2019 的哪个安装选项没有图形用户界面？
  **Answer:** Server Core
- 7. Windows Server 2019 最新版本的正确术语是 **Long-Term Servicing Branch (LTSB)** 还是 **Long-Term Servicing Channel (LTSC)**？
  **Answer:** **Long-Term Servicing Channel (LTSC)**

### 第 2 章：安装和管理 Windows Server 2019

- 8. 从哪个正确的工具更改 Windows Server 2019 的配置，**Windows Settings** 还是 **Control Panel**？
  **Answer:** 两者都可以，但对于大多数配置选项，Windows Settings 是首选方法
- 1. 微软的新的基于 Web 的集中式服务器管理工具的名称是什么（有趣的事实，这个工具集以前被称为 Project Honolulu）？
  **Answer:** **Windows Admin Center (WAC)**
- 2. 真或假 – Windows Server 2019 需要安装在机架式服务器硬件上。
  **Answer:** False。Windows Server 2019 可以安装在物理硬件上，也可以作为虚拟机实例。
- 3. 真或假 – 通过选择 Windows Server 2019 的默认安装选项，您将获得一个看起来很像 Windows 10 的用户界面。
  **Answer:** False。Windows Server 2019 的默认选项是 Server Core，它没有图形用户界面。
- 4. 显示 Windows Server 2019 中当前已安装角色和功能的 PowerShell cmdlet 是什么？
  **Answer:** `Get-WindowsFeature | Where Installed.`
- 5. 真或假 – Server Manager 可用于同时管理许多不同的服务器。
  **Answer:** True。
- 6. 可以安装在 Windows 10 计算机上以便在该客户端工作站上运行 Server Manager 的工具集名称是什么？
  **Answer:** **Remote Server Administration Tools (RSAT)。**
- 7. 可以用于与 Windows Admin Center 交互的受支持的 Web 浏览器有哪些？
  **Answer:** 截至本文撰写时，Microsoft Edge 和 Google Chrome。请注意 Internet Explorer 不受支持。

### 第 3 章：Active Directory

- 1. 在 Active Directory 中，容纳计算机和用户帐户的容器（文件夹）称为...？
  **Answer:** Organizational Unit (OU)
- 2. 在将计算机加入域之前在 Active Directory 中创建计算机帐户的术语是什么？
  **Answer:** Prestaging the account
- 3. 哪个管理工具用于指定网络中特定物理位置绑定到特定 IP 子网？
  **Answer:** Active Directory Sites and Services
- 4. 特殊域控制器的名称是什么，它不能接受新信息，只能从现有域控制器同步？
  **Answer:** Read-Only Domain Controller (RODC)
- 5. 创建细粒度密码策略需要什么工具？
  **Answer:** Active Directory Administrative Center
- 6. 在建立林信任之前，DNS 中必须配置什么？
  **Answer:** Conditional forwarder

### 第 4 章：DNS 和 DHCP

- 1. 哪种 DNS 记录指导电子邮件流？
  **Answer:** MX record
- 2. 哪种 DNS 记录类型将名称解析为 IPv6 地址？
  **Answer:** AAAA record
- 3. 哪种 DNS 区域类型将 IP 地址向后解析为主机名？
  **Answer:** Reverse Lookup Zone
- 4. 哪种 DHCP 选项常用于 VoIP 电话配置？
  **Answer:** Option 66
- 5. 哪个 DHCP 故障转移模式常用于分支机构和主站点之间？
  **Answer:** Hot standby mode

