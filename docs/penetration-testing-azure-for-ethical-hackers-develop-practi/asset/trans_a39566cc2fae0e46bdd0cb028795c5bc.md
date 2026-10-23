

## 利用 Reader 权限

在本章中，我们将介绍使用 Reader 角色可以执行的过程，这些过程有望帮助访问敏感信息，或在订阅中提升权限。该过程将通过以下主题进行说明：

- 准备 Reader 利用场景
- 收集资源清单
- 审查常见的明文数据存储
- 利用动态组成员身份

需要澄清一点：在本章中，我们将 Reader 角色视为应用于订阅级别的 Azure AD 用户，而不是管理组、资源组或单个资源级别。

## 技术要求

本章我们将使用以下工具：

- PowerZure – `https://github.com/hausec/PowerZure`
- MicroBurst – `https://github.com/NetSPI/MicroBurst`

MicroBurst 应该已经从前面章节安装在你的渗透测试虚拟机上了，但 PowerZure 是本章的新增内容。本章的所有示例都可以使用我们在 *第 2 章，构建你自己的环境* 中创建的渗透测试虚拟机，或者使用相同配置的 Windows 桌面来执行。

在开始任何场景之前，我们将先介绍为即将进行的示例设置测试订阅的过程。

## 准备 Reader 利用场景

这个动手练习将为我们完成本章其余练习做好准备。要跟随本章将要介绍的场景，你需要在自有的 Azure 订阅中设置一个具有 Reader 权限的用户，以及一些易受攻击的工作负载配置。我们已使用一个 PowerShell 脚本自动化了此过程，你可以从 Azure Cloud Shell 运行该脚本。

以下是完成此练习的方法：

1. 打开 Web 浏览器，浏览到 Azure 门户 `https://portal.azure.com`。使用 azureadmin 凭据登录。
2. 在 Azure 门户中，单击左上角的 Cloud Shell 图标。选择 PowerShell：

![](img/4fa338e84e0a590967a142dd798ba4e2_139_0.png)

图 4.1 – Azure Cloud Shell 图标

3. 在 Cloud Shell 窗格内的 PowerShell 会话中，运行以下命令下载脚本，该脚本将创建一个具有 Reader 权限的用户帐户，并设置所需的可利用工作负载：

```
PS C:\> Invoke-WebRequest http://bit.ly/reader-scenarios -O reader-scenario.ps1
PS C:\> Get-ChildItem
```

这个短链接指向此 GitHub URL：https://raw.githubusercontent.com/PacktPublishing/Penetration-Testing-Azure-for-Ethical-Hackers/main/chapter-4/resources/reader-scenario.ps1。

下面是它的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_139_1.png)

图 4.2 – 下载并确认设置脚本

4. 使用以下命令运行下载的脚本，以配置本章练习所需的对象和资源：

```
PS C:\> ./reader-scenario.ps1
```

当提示输入密码时，输入 myPassword123（或你选择的密码）并按 Enter。等待脚本部署完成。部署大约需要 8 分钟：

![](img/4fa338e84e0a590967a142dd798ba4e2_140_0.png)

图 4.3 – 设置脚本的示例输出

### 脚本的作用是什么？

该脚本在 Azure 订阅中创建一个具有 Reader 权限的用户帐户。该脚本还会设置图 4.4 中所示的资源和对象：

- 一个容器注册表实例
- 一个 Ubuntu Linux 虚拟机
- 一个 Azure 自动化帐户
- 一个 Web 应用资源
- 一个 MySQL 平台数据库：

![](img/4fa338e84e0a590967a142dd798ba4e2_140_1.png)

图 4.4 – 脚本创建的資源关系图

5. 脚本完成后，你完成本练习其余部分所需的关键信息将显示在输出部分。将这些信息复制到记事本文档中，以备后用。你需要从输出部分获取两个值：

- Azure Reader 用户：这是对 Azure 订阅具有 Reader 权限的练习用户名。我们将在下一步中授予它对 Azure AD 的 Reader 权限。
- Azure Reader 用户密码：这是你之前输入的 Azure Reader 用户的密码：

```
Transcript started, output file is reader-account-output.txt
################
# Script Output #
################
Azure Reader User: readeruser@azurepentesting.com
Azure Reader User Password: myPassword123
Transcript stopped, output file is /home/azureadmin/reader-account-output.txt
Deployment Ended 06/03/2021 12:38:15
PS /home/azureadmin>
```

图 4.5 – 设置脚本的最终输出

6. 返回 Azure 门户（仍以 azureadmin 用户身份进行身份验证），浏览到 Azure Active Directory。单击“角色和管理员”，然后选择“全局读取者”角色：

![](img/4fa338e84e0a590967a142dd798ba4e2_141_0.png)

图 4.6 – 添加“全局读取者”Azure AD 角色

7. 在“**全局读取者 | 分配**”边栏选项卡中，单击“**添加分配**”，选择“**readeruser**”对象，然后点击“**添加**”。这将确保 `readeruser` 帐户除了拥有订阅的 Reader 权限外，还拥有 Azure AD 中的全局读取者权限：

![](img/4fa338e84e0a590967a142dd798ba4e2_142_0.png)

图 4.7 – 将 readeruser 帐户添加为全局读取者

8. 使用 RDP 连接到渗透测试虚拟机，并以管理员身份打开 PowerShell。使用以下命令向 Azure 进行身份验证：

```
PS C:\> Connect-AzAccount
```

9. 当提示进行身份验证时，使用 **Azure Reader 管理员用户** 值（来自脚本输出）作为用户名，并将 **Azure Reader 管理员用户密码** 值（也来自脚本输出）作为密码：

![](img/4fa338e84e0a590967a142dd798ba4e2_142_1.png)

图 4.8 – 以 readeruser 帐户成功进行身份验证

现在你已经以新创建的 `readeruser` 帐户进行了身份验证。该帐户在 Azure AD 中被分配了“全局读取者”角色，在 Azure 订阅中被分配了“Reader”角色。此帐户将用于后续练习，以利用 Azure 环境中的各种弱点。请保持 PowerShell 会话打开，供下一个练习使用。

## 收集资源清单

在获得具有 Reader 角色的 Azure AD 帐户访问权限后，我们的第一步应该是枚举。我们需要知道我们正在处理什么，以及外部环境中可能还存在哪些额外的暴露点。

在之前的枚举示例中，我们依靠字典和暴力枚举来发现订阅中可能存在的 Azure 服务。有了 Reader 访问权限，我们可以轮询所有可用的服务，以收集一份包含内部和外部攻击面的完整资源清单。

在本节中，我们将展示一种使用 Azure 门户内置功能快速枚举 Azure 订阅中所有可用资源的简单方法。这是在参与测试期间跟踪资产的好方法：

1. 使用 `readeruser` 帐户登录门户。
2. 导航到“**所有资源**”边栏选项卡：

![](img/4fa338e84e0a590967a142dd798ba4e2_143_0.png)

3. 从顶部菜单栏选择“**导出到 CSV**”。
4. 下载 CSV，并使用 Excel 按“**类型**”对资源数据进行排序。
5. 为“数量”添加一个额外的列，并在每一行中将该列填为 1。
6. 使用 Excel 的“分类汇总”功能（使用以下设置）在每次类型变化时进行计数：

![](img/4fa338e84e0a590967a142dd798ba4e2_144_0.png)

图 4.10 – 创建资源类型的分类汇总

7. 这应该会得到一个简洁的总结列表，显示订阅中所有可用的资源：

| 名称 | 类型 | 资源组 | 位置 | 订阅 | 数量 |
|---|---|---|---|---|---|
| App Service 数量 |  |  |  |  | 11 |
| App Service 计划数量 |  |  |  |  | 7 |
| 自动化帐户数量 |  |  |  |  | 3 |
| 可用性集数量 |  |  |  |  | 1 |
| Azure Cosmos DB 帐户数量 |  |  |  |  | 1 |
| 认知服务数量 |  |  |  |  | 1 |
| 容器注册表数量 |  |  |  |  | 1 |
| 磁盘数量 |  |  |  |  | 103 |
| 映像数量 |  |  |  |  | 4 |
| IoT 中心数量 |  |  |  |  | 1 |
| 密钥保管库数量 |  |  |  |  | 3 |
| Kubernetes 服务数量 |  |  |  |  | 1 |
| Log Analytics 工作区数量 |  |  |  |  | 2 |
| 网络接口数量 |  |  |  |  | 107 |
| 网络安全组数量 |  |  |  |  | 89 |
| 公共 IP 地址数量 |  |  |  |  | 122 |
| 恢复服务保管库数量 |  |  |  |  | 1 |
| 路由表数量 |  |  |  |  | 1 |
| Runbook 数量 |  |  |  |  | 15 |
| SendGrid 帐户数量 |  |  |  |  | 2 |
| 快照数量 |  |  |  |  | 2 |
| 解决方案数量 |  |  |  |  | 5 |
| SQL 数据库数量 |  |  |  |  | 1 |
| SQL 服务器数量 |  |  |  |  | 2 |
| 存储帐户数量 |  |  |  |  | 43 |
| 虚拟机数量 |  |  |  |  | 24 |
| 虚拟机规模集数量 |  |  |  |  | 1 |
| 虚拟网络数量 |  |  |  |  | 35 |
| 总计 |  |  |  |  | 589 |

图 4.11 – Azure 资源汇总列表

虽然门户是一个很棒的视觉工具，但它并不总是收集配置信息的最有效方法。为此，我们需要使用能够通过脚本和 PowerShell 模块自动收集配置的工具。

## PowerZure 简介

PowerZure 是一款基于 PowerShell 的工具，用于枚举 Azure 中的资源并利用服务中的不安全配置。它包含多个内置函数，可用于收集 Azure 环境的信息，并利用已发现的资源。我们将在本章及后续章节中使用其中的一些函数。

