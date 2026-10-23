

该 cmdlet 会输出磁盘存储的账户类型，在本例中为 Standard_LRS，这意味着本地冗余存储（LRS）。这里的关键点很简单，即你知道这一点，并且你知道你的解决方案的依赖关系也会影响解决方案的可用性。考虑到可用性集在故障域和更新域之间是本地冗余的，你也可以选择将 VM 部署到可用性区域，并获得除域功能之外的 ZRS 依赖性。最后，之前运行的 Get-AzVmssVm PowerShell cmdlet 是针对单个托管磁盘上的单个 VM。记住在练习 4.11 中你创建了一个跨三个区域的 VM；因此，如果你在三个托管磁盘上都运行该命令，它仍然会显示 Standard_LRS，但每个磁盘都位于不同的区域。

## 保护 Azure 虚拟机

第 2 章涵盖了安全，但并未专门针对 Azure 虚拟机的上下文。让我们现在再讨论一下。需要额外讨论的安全方面有三个。

- 谁可以通过门户或客户端访问 VM？
- 谁可以使用 SSH 或 RDP 访问 VM？
- 谁可以访问在 VM 上运行的应用程序？

Azure 门户、Azure PowerShell、Azure CLI 和 REST API 都提供了强大的管理功能。你已经知道限制谁可以做什么的方法是通过 RBAC 以及 VM 门户刀片上的访问控制（IAM）菜单项。确定在资源层次结构中的哪个级别对哪个资源谁可以做什么需要思考、设计和维护。我是说，点击一下按钮，VM 就可以被删除。这可能没那么糟糕，考虑到你现在对映像和托管磁盘的工作方式的了解。VM 可以相对快速地恢复，但如果磁盘或映像被删除了怎么办……不小心？你需要防止这种情况发生。而你应该已经知道如何做到这一点。

当你创建 VM 时，流程的一部分是创建管理员用户名和密码。你需要保护它们并有意识地决定谁获得凭据。不要共享管理员凭据，而是创建具有最小权限集的特定帐户，使用即时访问策略并使用 VM 刀片上门户中的连接监视菜单链接监视连接活动。Microsoft 不会代表你备份磁盘的内容，但你可以这样做，并且建议这样做。Microsoft 只允许你按自己的意愿使用已配置的资源，同时保持其高度可用。一旦有人怀着恶意意图进入服务器，如果他们拥有凭据，那么你就无法做太多来防止事故或不受欢迎的行为。删除文件、替换文件、运行某些恶意软件或植入漏洞都是你在运行工作负载时必须注意的事情。如果你认为 VM 的凭据已被泄露，则 VM 刀片上有一个重置密码菜单项。如果你不小心忘记了密码，这也有帮助，有时这种情况会发生，因为你从未写下它们，因为那是不允许的。还有一个安全菜单选项，它是进入 Azure 安全中心（ASC）的入口。ASC 有助于监控本段中讨论的活动。

从应用程序角度来看，这在很大程度上取决于你选择的身份提供者和其实现方式。然而，你可以添加诸如网络安全组、强制 TLS 1.2 和 HTTPS 的 Azure 策略，以及可能的托管标识等保护措施。所有这些应用程序级安全技术都有助于保护在 VM 上运行的应用程序。我们在第 2 章中也介绍过最后一个技术，那就是数据静态加密的概念。如果托管磁盘的内容需要加密，例如存储非常敏感的数据、知识产权或运行应用程序的代码，则这很重要。要加密托管磁盘，请完成练习 4.12。

## 练习 4.12

### 加密托管磁盘

- 1. 记住在练习 2.9 中你创建了 Azure Key Vault 密钥？完成本练习时你需要再次使用它。打开一个 Azure PowerShell 会话并执行以下 cmdlet。输出应类似于图 4.36。
  
  ```
  Get-AzVmDiskEncryptionStatus -ResourceGroupName <name> -VMName <name>
  ```

### 图 4.36 使用 PowerShell 检查托管磁盘的加密状态

![](img/95043b3df926bcdadadb082b49262037_337_0.png)

- 2. 注意 OsVolumeEncrypted 状态设置为 NotEncrypted。执行以下 Azure PowerShell cmdlet 来加密操作系统驱动器：
  
  ```
  $KeyVault = Get-AzKeyVault -VaultName <name> -ResourceGroupName <name>
  $DAR = Get-AzKeyVaultKey -VaultName <name> -Name <name>
  
  Set-AzVmDiskEncryptionExtension -ResourceGroupName <name> \
      -VMName <name> -DiskEncryptionKeyVaultUrl $KeyVault.VaultUri \
      -DiskEncryptionKeyVaultId $KeyVault.ResourceId \
      -KeyEncryptionKeyVaultId $KeyVault.ResourceId \
      -KeyEncryptionKeyUrl $DAR.Id -SkipVmBackup -VolumeType All
  ```

- 3. 在提示时，阅读有关潜在重启和加密可能持续时间的警告消息。输入 Y 然后按 Enter 加密驱动器。加密完成后，执行以下 cmdlet 检查 OsVolumeEncrypted 状态。注意值现在为 Encrypted，如图 4.37 所示。
  
  ```
  Get-AzVmDiskEncryptionStatus -ResourceGroupName <name> -VMName <name>
  ```

### 图 4.37 使用 PowerShell 检查托管磁盘的加密状态

```
PS Azure:\> Get-AzVmDiskEncryptionStatus -ResourceGroupName CSHARP GUITAR-SN1-RG -VMName CSHARP GUITAR-A2
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : NotEncrypted
OsVolumeEncryptionSettings :
ProgressMessage            : No Encryption extension or metadata found on the VM
```

4. 加密托管磁盘也可以通过 Azure 门户完成。登录到 portal.azure.com 上的 Azure 门户。导航到附加了需要加密磁盘的 VM 并单击磁盘菜单项。单击磁盘刀片上的加密项并从“要加密的磁盘”下拉列表中选择你的操作系统和数据磁盘。输入 Key Vault。 （如果它存在于同一个资源组中，则应预先填充。）输入密钥和版本，然后单击加密刀片顶部的保存链接。执行以下 cmdlet。输出应与图 4.38 所示相似。

```
Get-AzVmDiskEncryptionStatus -ResourceGroupName <name> -VMName <name>
```

### 图 4.38 使用 PowerShell 检查托管磁盘的加密状态

```
PS Azure:\> Get-AzVmDiskEncryptionStatus -ResourceGroupName CSHARP GUITAR-SN1-RG -VMName CSHARP GUITAR-A2
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings :
ProgressMessage            : No Encryption extension or metadata found on the VM
```

就这样。阅读本节后，你现在对 Azure VM 有了深入的了解。你应该有信心，无论是参加会议还是参加 Azure 解决方案架构师专家考试，你的知识都已准备就绪。为了确保，回答以下问题，然后继续学习其他 Azure 计算产品。

将本地服务器或 VM 迁移到 Azure 虚拟机的最佳工具是什么？

- Azure Migrate
- SYSPREP
- AzCopy
- Azure Site Recovery

答案是 Azure Site Recovery，它不仅有助于规划和从灾难中恢复，还可用于将本地工作负载迁移到 Azure VM。SYSPREP 和 AzCopy 不适用于迁移 VM，而 Azure Migrate 更侧重于规划迁移而不是实际执行迁移。

## Azure 应用服务

Azure 应用服务是 Microsoft 的 PaaS 产品。此产品组合提供 Web 应用、API 应用、移动应用、容器 Web 应用（Linux）、应用服务环境和 Azure WebJobs。PaaS 将操作系统维护的责任从客户身上移除。在第 3 章中，我提到了沙箱的概念，Microsoft PaaS 产品在其中运行。沙箱限制了你和你的代码可以执行的操作。例如，不写入注册表、无法访问事件日志、不进行进程外 COM 以及无法访问 User32/GDI32 是最常见的有影响的限制之一。GitHub 上对此限制的完整列表位于此特定位置：

```
github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox
```

此外，受支持的框架运行时版本（例如 .NET、PHP、Python、Node 等）、第三方驱动程序、密码套件、注册表编辑或根证书均由云服务提供商驱动。这意味着，例如，如果你想定位 .NET Core 3.0 或 Python 3.8 而平台上没有运行时，则无法使用它。当你决定在 PaaS 产品上运行工作负载时，你通过将 OS 的责任委托出去获得了价值，例如修补、安全性和 OS 升级。然而，当涉及到进行自定义环境配置和定位包含最新技术的运行时新版本时，你会失去一些灵活性。

Azure 应用服务有多种定价层级和大小，这两种属性的组合称为应用服务计划（ASP）。从定价层级角度来看，有六个不同的选项，每个选项都有不同的配套功能。免费版和共享版是最经济实惠的选项，适用于测试和学习；你不应在上面运行任何生产工作负载，因为这两个层级都没有 SLA，并且有一些严格的计量容量限制。例如，在免费计划上运行，你的进程每天最多可消耗 60 分钟的 CPU 分钟，在共享计划上为 240 分钟。这可能足以满足一些小型测试站点，但正如前面提到的，它不适用于生产工作负载。其他四个层级是基本版、标准版、高级版和隔离版。隔离版也称为应用服务环境（ASE），稍后会讨论。基本版、标准版和高级版中提供的一些功能在表 4.10 中列出。

### 表 4.10 基本、标准和高级层级

| Feature | Basic | Standard | Premium |
|---|---|---|---|
| Disk space | 10 GB | 50 GB | 250GB |
| Max instances | Up to 3 | Up to 10 | Up to 20 |
| Deployment slots | X | ✓ | ✓ |
| Cloning | X | X | ✓ |
| VNET Integration | X | ✓ | ✓ |

