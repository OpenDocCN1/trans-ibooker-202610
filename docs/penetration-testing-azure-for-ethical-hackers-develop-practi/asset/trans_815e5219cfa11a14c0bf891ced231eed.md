

## 横向网络移动机会

由于 VM 已部署到 Azure 中客户管理的虚拟网络中，因此组织通常会将其传统网络与 Azure 虚拟网络建立连接。能够在 Azure VM 上运行命令使我们有机会发现可能与其连接的网络中的服务和资源，并寻找利用它们的途径。

运行命令的能力也可能带来绕过防御的机会。在某些情况下，本地 Azure 访问和远程 Azure 访问的 **多因素身份验证 (MFA)** 要求可能不同。作者曾见过要求从互联网访问时进行 MFA，但从受信任的内部本地 IP 地址进行连接时则不要求 MFA 的环境。

在本地环境中提升权限后，dump 并破解租户管理员账户的凭据后，可能可以直接从本地系统或 Azure VM 登录到 Azure 管理界面。

## 租户凭据搜寻

在某些情况下，组织会在 Azure 中托管特权访问工作站或跳板机。这些是管理员可用于对其环境执行特权操作的系统。我们会尝试从这些机器收集缓存的访问令牌或其他凭据以供重用。

本章后面我们将介绍如何在 Azure VM 上识别托管身份。这些身份也可以通过从元数据服务生成令牌的方式，从 VM 升级到 Azure 租户。生成的令牌随后可用于从可能原本无法访问的其他服务收集敏感信息。

现在我们了解了目标，让我们来看一下实现这些目标的不同利用场景。

## 利用 Contributor 权限的 Azure 平台功能

拥有 Contributor 角色访问权限后，我们拥有丰富的平台级功能，可用于管理环境中的 VM。由于我们是以渗透测试者的身份来使用这些功能，我们对这些功能的使用案例可能与普通用户略有不同。

## 利用密码重置功能

Azure VM 的密码重置功能旨在简化使用安装在每台 Azure VM 上的 VM 代理重置本地 Azure VM 用户密码的过程。然而，此功能可被滥用来在 Azure 中的 Windows 和 Linux VM 上创建具有管理权限的新本地用户！

此功能可从 Azure 门户（图 5.6）或 Azure 命令行工具中使用。诸如 Lava 之类的利用工具也可以利用此功能大规模重置 VM 密码。渗透测试者可以利用此功能从 Azure 平台横向移动到 IaaS 工作负载，以推进攻击链：

![](img/4fa338e84e0a590967a142dd798ba4e2_195_0.png)

图 5.6 – 门户中 VM 的示例密码重置

要使用 Azure CLI 为 VM 创建本地用户，我们将使用以下命令：

```
az vm user update -u username -p password -n <VM_Name> -g <Resource_Group>
```

要使用 Azure PowerShell 为 VM 创建本地用户，我们可以使用以下命令：

```
Set-AzVMAccessExtension -ResourceGroupName "<Resource_Group>" -Location "<Location>" -VMName "<VM_Name>" -Name "<Extension_Name>" -TypeHandlerVersion "2.4" -UserName "<Username>" -Password "<Password>"
```

接下来，我们将介绍使用此功能以本地管理员身份访问 VM 的过程。

## 实践练习 – 利用密码重置功能创建本地管理员用户

在本练习中，我们将利用 Azure VM 的密码重置功能创建一个可用于连接 VM 的本地管理员用户，在其中搜寻凭据。以下是本练习中我们将完成的任务：

- 1. 在你的渗透测试 VM 上打开 PowerShell。
- 2. 在 PowerShell 控制台中，使用以下命令对 Azure 进行身份验证：
- 3. 在提示时，输入你在本章第一个练习的脚本输出中获得的 Contributor 用户凭据：

```
Connect-AzAccount
```

以下是此操作的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_196_0.png)

图 5.7 – 对订阅进行身份验证

![](img/4fa338e84e0a590967a142dd798ba4e2_196_1.png)

图 5.8 – Connect-AzAccount 身份验证提示

你现在已使用 Contributor 凭据进行身份验证。

4. 使用以下命令获取订阅中的 VM 列表：

```
Get-AzVM
```

以下是命令输出的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_197_0.png)

图 5.9 – 可用 VM 列表

5. 使用以下命令在 winvm01 VM 上创建新的本地用户：

```
Set-AzVMAccessExtension -ResourceGroupName "PENTEST-RG" -VMName "winvm01" -Credential (get-credential) -typeHandlerVersion "2.0" -Name VMAccessAgent
```

在提示时，输入 pentestuser 作为用户名，secretPass123 作为密码，然后单击确定。

以下是此过程的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_197_1.png)

图 5.10 – 密码重置功能的示例

6. 使用以下命令获取 winvm01 的公共 IP：

```
Get-AzPublicIpAddress -Name winvm01* | Select IpAddress
```

请记住，如果 VM 没有公共 IP 或 RDP 端口未打开，你始终可以使用 Contributor 角色权限将服务暴露在公共 IP 上。这在大多数环境中都是重大的状态更改并增加风险，因此不建议在大多数环境中使用。

以下是命令的截图：

![](img/4fa338e84e0a590967a142dd798ba4e2_198_0.png)

图 5.11 – 列出 VM 的公共 IP

7. 通过 RDP 连接到公共 IP，并使用用户名 pentestuser 和密码 secretPass123 进行身份验证。

恭喜！你已成功使用 Contributor 角色在 VM 上创建了本地管理员用户。

这是一个非常方便的功能，但在评估期间并不总是合适的工具。假设我们在测试期间希望在行动上更隐蔽一些。在系统上创建新的本地管理员用户可能会触发一些警报。作为替代方案，我们可以使用原生的 Azure 功能直接在 VM 上运行命令，而无需额外的账户。

## 利用 Run Command 功能

Azure VM 的 Run Command 功能是使用 VM 代理远程在 VM 上运行脚本的平台功能。它可用于在 Windows VM 上运行 PowerShell 脚本，在 Linux VM 上运行 shell 脚本。它通常由管理员或开发人员用于快速诊断和修复 VM 访问和网络问题，并使 VM 恢复到良好状态。

通过选择 **RunPowerShellScript**，或针对 Linux 选择 **RunShellScript**，我们可以运行自己的自定义命令在 VM 上。作为概念验证，我们将展示 whoami 命令的运行情况：

图 5.13 – Azure Run Command "whoami" 执行

虽然这可能是运行 VM 命令的最简单方法之一，但它并不具可扩展性，并且门户在及时将命令执行结果返回给浏览器方面容易出现问题。这可能导致命令结果被截断，并且在命令完成执行之前发生会话超时。

例如，先前发出的简单 whoami 命令花费了 21 秒才将输出返回给浏览器。虽然对于如此简单的命令来说这是一个相当长的时间，但更复杂且处理更密集的命令可能需要更长的时间（数分钟）才能返回。

## 从 Az PowerShell 模块运行命令

为了获得更具可扩展性的命令执行选项，我们可以使用 Azure PowerShell 模块通过 PowerShell 管道的魔力在多个 VM 上排队命令。在 Azure 身份验证的 PowerShell 会话中，你可以使用以下命令列出正在运行的 Windows VM 并将其转换为 VMs 变量：

```
PS C:\> $VMs = Get-AzVM -Status | where {$_.PowerState -EQ "VM running"} -and ($_.StorageProfile.OSDisk.OSType -eq "Windows")}

PS C:\> $VMs | select ResourceGroupName,Name
ResourceGroupName       Name
------------------       ----
DEFAULT                  RunCommandTestVM
```

在前面的示例中，我们知道系统正在运行 Windows，因此我们将 ps1 文件传递给主机。我们首先需要将 whoami 命令输入到 ps1 文件中：

```
PS C:\> echo "whoami" > whoami.ps1
```

要执行命令，我们需要传入资源组和我们运行命令的 VM 的名称。得益于 PowerShell 管道的魔力，我们可以将 VMs 变量传递给 Invoke-AzVMRunCommand 函数，并附带本地脚本：

```
PS C:\> $VMs | Invoke-AzVMRunCommand -CommandId 'RunPowerShellScript' -ScriptPath .\whoami.ps1

Value[0]       :
  Code         : ComponentStatus/StdOut/succeeded
  Level        : Info
  DisplayStatus : Provisioning succeeded
  Message      : nt authority\system
Value[1]       :
  Code         : ComponentStatus/StdErr/succeeded
```

