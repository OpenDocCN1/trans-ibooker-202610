

3. 在“登录”窗口中，输入之前配置的 azureadmin 用户名和密码以验证到您的 Azure 订阅，如以下屏幕截图所示：

![](img/4fa338e84e0a590967a142dd798ba4e2_74_0.png)

图 2.22 – 验证到 Azure

4. 在“自定义部署”窗口中，配置以下参数：

Subscription: 选择要部署渗透测试 VM 的订阅。

Resource group: 创建新 → pentester-vm-rg → 确定。

Region: 选择靠近您所在位置的 Azure 区域。

Storagename: 保留默认设置。

Vm-dns: 保留默认设置。

Admin User: pentestadmin。

Admin Password: 输入一个复杂的密码。记下您使用的密码。我们建议您在本书的各个场景中选择一个复杂的密码以保持简单。

Vmsize: 保留默认设置。

Location: 保留默认设置。

_artifacts Location: 保留默认设置。

_artifacts Location Sas Token: 保留默认设置。

5. 单击“查看 + 创建”。

该过程在以下屏幕截图中突出显示：

```text
# Custom deployment
Deploy from a custom template
Basics Review + create
Template
Customized template
7 resources
Edit template
Edit parameters
Deployment scope
Select the subscription to manage deployed resources and costs. Use resource groups like folders to organize and manage all your resources.
Subscription *
Resource group *
Create new
Parameters
Region *
Storagename
Vm-dns
Admin User *
Admin Password *
Vmsize
Location
_artifacts Location
_artifacts Location Sas Token

(New) pentester-vm-rg

UK South

[concat('ptvmstrg', uniqueString(resourceGroup().id))]

[concat('pentestvm-', uniqueString(resourceGroup().id))]

pentestadmin

************

Standard_D4s_v3

[resourceGroup().location]

[deployment().properties.templateLink.uri]

Review + create
< Previous
Next : Review + create >
```

图 2.23 – 配置模板参数

8. 在您的客户端系统上，打开远程桌面协议（RDP）客户端并输入您之前记下的 pentest-vm-dns 值。单击“连接”。这里的说明描述了使用 Windows RDP 客户端。如果您使用的是不同的 RDP 客户端，说明可能与您不同。

要打开 Windows RDP 客户端，请从 Windows 运行对话框执行 mstsc，或在 Windows 开始菜单中键入 mstsc。

以下屏幕截图显示了使用 RDP 连接到渗透测试 VM 的过程：

![](img/4fa338e84e0a590967a142dd798ba4e2_77_0.png)

图 2.26 – 使用 RDP 连接到渗透测试 VM

9. 提示登录时，单击“更多选项” → “使用其他帐户”。输入以下内容：

**Username:** `pentestadmin`。

**Password:** 输入您在模板部署期间配置的密码。

这在以下屏幕截图中进行了说明：

![](img/4fa338e84e0a590967a142dd798ba4e2_78_0.png)

图 2.27 – 使用 RDP 客户端进行身份验证

10. 提示证书警告时，选中“不再为此计算机的连接询问我”选项，然后单击“是”，如以下屏幕截图所示：

![](img/4fa338e84e0a590967a142dd798ba4e2_79_0.png)

图 2.28 – 跳过证书警告

11. 您现在应该拥有渗透测试 VM 的 RDP 会话！我们可以在以下屏幕截图中看到其概览。保持此会话打开，因为您将在下一个练习中需要它：

![](img/4fa338e84e0a590967a142dd798ba4e2_79_1.png)

图 2.29 – 到渗透测试 VM 的 RDP 会话

在本练习中，您在 Azure 中部署了一个渗透测试 VM。这是我们将在本书中用于发起攻击场景的 VM。

> **重要说明**
> 如果您想采取额外的步骤来锁定对该 VM 的访问，您可以将 RDP 访问限制为您的外部**Internet 协议（IP）**地址。这被认为是一种最佳实践，可以在门户中在 VM 的**网络**部分下的**入站安全规则**中完成。这也可以通过**网络安全组**菜单访问。

在下一个练习中，我们将介绍在渗透测试 VM 上安装**Windows Subsystem for Linux**（**WSL**）。

## 实践练习：在您的渗透测试 VM 上安装 WSL

我们将用于攻击场景的一些工具是基于 Linux 的。为了避免为不同的工具集设置两个渗透测试系统，我们将在刚刚部署的渗透测试 VM 上安装**WSL**。WSL 将允许我们在 Windows 上直接运行 Linux 环境，而无需部署传统的 VM。以下是我们将在本练习中完成的任务：

- **任务 1**：验证 Windows 版本是否支持 WSL。
- **任务 2**：启用 WSL（并重新启动 VM）。
- **任务 3**：下载 Ubuntu 20.04 Linux 发行版。
- **任务 4**：安装 Ubuntu 20.04 Linux 发行版。
- **任务 5**：启动 Linux 发行版。

让我们通过以下任务安装 WSL：

1. 在到您的渗透测试 VM 的 RDP 会话中，右键单击**开始**按钮并单击**Windows PowerShell（管理员）**，如以下屏幕截图所示：

图 2.30 – 以管理员身份打开 Windows PowerShell

2. 要启用 WSL，您的 Windows 操作系统（OS）需要为 Build 16215 或更高版本。您可以使用以下命令检查您的构建版本：

```powershell
systeminfo | Select-String "^OS Name","^OS Version"
```

这就是输出的样子：

![](img/4fa338e84e0a590967a142dd798ba4e2_82_0.png)

图 2.31 – 验证 Windows 构建版本

3. 使用以下命令启用 WSL。提示重新启动 VM 时，键入 Y 并按 Enter。等待几分钟让 VM 重新启动，使用 RDP 重新连接到它并以管理员身份打开 PowerShell 控制台：

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

这就是输出的样子：

![](img/4fa338e84e0a590967a142dd798ba4e2_82_1.png)

图 2.32 – 使用 PowerShell 启用 WSL

4. 更改目录到您的用户目录（C:\Users\$env:USERNAME\ 或 ~ 都可以工作）并使用以下命令下载 Ubuntu 20.04 发行版：

```powershell
cd C:\Users\$env:USERNAME\
```

```powershell
Invoke-WebRequest -Uri https://aka.ms/wslubuntu2004 -OutFile Ubuntu.appx -UseBasicParsing
```

5. 使用以下命令验证下载：

```powershell
Get-ChildItem
```

这就是输出的样子：

![](img/4fa338e84e0a590967a142dd798ba4e2_83_0.png)

图 2.33 – 验证下载的 Linux 发行版

6. 使用以下命令提取下载的发行版：

```powershell
Rename-Item .\Ubuntu.appx .\Ubuntu.zip
Expand-Archive .\Ubuntu.zip .\Ubuntu
```

7. 使用以下命令安装提取的 Linux 发行版。安装需要几分钟才能完成：

```powershell
Set-Location .\Ubuntu\
.\ubuntu2004.exe
```

8. 提示输入 Linux 安装的凭据时，完成以下操作：

   - **Enter new UNIX username:** pentestadmin。
   - **New password:** 为保持简单，输入与渗透测试 VM 相同的密码。
   - **Retype new password:** 重新输入相同的密码。

这在以下屏幕截图中进行了说明：

![](img/4fa338e84e0a590967a142dd798ba4e2_84_0.png)

图 2.34 – 为 WSL 配置凭据

9. 使用以下命令将 Linux 安装应用更新。提示时输入您刚刚配置的密码：

```bash
sudo apt update -y
```

10. 您可以关闭 PowerShell 控制台。从现在开始使用 WSL，您只需在渗透测试 VM 的 PowerShell 控制台或命令行控制台中键入 bash 并按 *Enter* 即可。

在本练习中，您在渗透测试 VM 上启用了 WSL。在以下练习中，我们将介绍安装 Azure PowerShell 模块和 Azure **命令行界面（CLI）**，这些是本书中我们将使用的其他渗透测试工具所需的。

## 实践练习：在您的渗透测试 VM 上安装 Azure 和 Azure AD PowerShell 模块

如第一章所述，**Azure PowerShell** 是管理员和开发人员可以添加到 PowerShell 以连接到 Azure 订阅并管理 Azure 资源的模块。**Azure AD PowerShell** 是 IT 专业人员用于管理 Azure AD 的模块。两个模块都包含渗透测试人员可以用来执行目标环境枚举的有用命令。Azure 特定的渗透测试工具如 **MicroBurst** 和 **PowerZure** 也需要安装这些模块。

在本练习中，我们将在渗透测试 VM 上安装这两个模块。以下是我们将完成的任务：

- **任务 1：** 验证是否安装了 PowerShellGet
- **任务 2：** 将 PowerShell 库配置为受信任的存储库

