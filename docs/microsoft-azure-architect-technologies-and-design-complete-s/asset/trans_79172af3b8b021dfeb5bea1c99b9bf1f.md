

这是一个不错的选择，因为调整大小并不复杂。当你创建 Azure VM 时，会选择一个默认大小（例如 D2SV3），但其下方有一个链接可让你更改大小。如果你决定保留该大小，后来发现需要更多计算能力，Azure VM 导航栏中有一个名为 size 的链接。点击该链接会打开 Size 刀片，并显示现有大小以及其他选项列表，如图 4.24 所示。

## FIGURE 4.24 Listing different Azure VM sizes

![](img/95043b3df926bcdadadb082b49262037_316_0.png)

只需选择所需大小，然后点击 Resize 按钮，Azure VM 就会按比例扩展到该大小。请注意，更改大小会导致应用程序重启，这可以预期，因为与 VM 关联的计算能力配置是一项重大更改。也可以使用 PowerShell 进行相同的更改。执行以下 cmdlet 并查看图 4.25 中的输出：

```
Get-AzVMSize -ResourceGroupName "<RG-NAME>" -VMName "<VM-NAME>"
$vm = Get-AzVM -ResourceGroupName "RG-NAME" -VMName "<VM-NAME>"
$vm.HardwareProfile.VmSize = "Standard_DS3_v2"
Update-AzVM -VM $vm -ResourceGroupName "<RG-NAME>"
```

第一个 PowerShell cmdlet 列出该区域中该 Azure VM 可用的所有可能 VM 大小。这有助于了解你拥有的选项以及稍后在决定大小后所需的命名法。Azure 门户中的选项限制更严格；使用 PowerShell 你会得到一个未经过滤的列表。PowerShell 脚本的下一行将 VM 获取到 PowerShell 对象中，设置 VmSize 并更新它。等待一段时间后，在该 VM 上运行的工作负载将被按比例扩展到新分配的计算系列。额外测试一下，将你在练习 4.7 中创建的资源锁更新为只读，然后尝试刚才讨论的相同更新过程。它会失败，因为 VM 的配置选项此时为只读，无法更改。

## FIGURE 4.25 Resizing an Azure VM using PowerShell

![](img/95043b3df926bcdadadb082b49262037_317_0.png)

让我们稍微转换一下思路，考虑更新的问题。当你选择 IaaS 时，你拥有的主要责任之一是管理操作系统。这意味着你需要安排和管理安全补丁、错误修复和热修复（即从 Windows 角度来看 KB 补丁）。有一个名为 Update Management 的产品可以帮助你完成此操作。Update Management 与 Log Analytics 协同使用，Log Analytics 在第 9 章中讨论。一旦你配置了 Update Management，它将对目标 VM 进行分析，并提供有关任何缺失更新的详细信息。这适用于 Windows 和 CentOS、Red Hat、SUSE 以及 Ubuntu Linux VM。如果发现缺少更新，Update Management 的功能中还有一个附加功能，允许你安排更新部署。更新可以安排为一次性运行或定期运行，并且你可以配置更新应用后是否重启。那是一个重要的选项，即是否允许重启。我在 IT 职业生涯早期，安装操作系统补丁存在一些高风险。多次补丁根本无法安装并导致服务器崩溃，或者补丁安装后重启后从未恢复。在这两种情况下，我们唯一的选项就是重建整个服务器，这部分相对容易。安装、配置和测试新构建的应用程序才是困难的部分。我非常感激现在可以节省我许多小时、深夜和周末的镜像、备份和部署槽。我的经验是，指出选择重启选项和自动化操作系统补丁安装需要考虑回滚或故障排除场景。Azure VM 还有另一个名为 boot diagnostics 的功能，如果在更新安装后 VM 无法恢复，或者由于某种原因在重启后 VM 挂起，该功能可以帮助你。

## Setting a Resource Lock to Read-Only on a VM

在前两段中我讨论了资源锁，在练习 4.7 中你创建了一个类型为 Delete 的资源锁。我还讨论了只读锁类型并转向了更新管理。事实证明，当 VM 上存在只读锁时，你无法配置更新管理。我遇到了一些错误，花了一些时间才弄明白发生了什么。在 Azure 上很容易感到迷失。继续前进，继续学习。

为了避免你的头脑过度混乱，对你的订阅和资源组中正在发生的事情进行概述可以帮助提供一些清晰度。这些监控主题将在第 6 章中更详细地介绍，我们在那里介绍合规性主题，并在第 9 章中介绍监控时进行介绍。然而，Azure VM 导航菜单上的 Inventory 链接可让你在 VM 上启用更改跟踪。一旦配置完成，你可以概览软件、文件、Windows 注册表和 Windows 服务添加、更改或删除的情况。对于 Linux VM，软件、文件和 Linux 守护进程的更改将被监控。我相信你会同意，了解机器上发生的事情对于维护和支持是有帮助的。如果某些东西停止工作，你可以查看 Inventory 刀片，检查是否有任何更改，而无需 RDP 或 SSH 到 VM 并手动查看。

最后，如果你还记得你多次创建 Azure VM 的经历，在 Management 选项卡上有一个名为 Boot Diagnostics 的功能。它默认启用并存储一些有用的信息。其中一个有用的信息是它会捕获屏幕截图，可能是 BSOD，或者对于 Linux 可能会显示一些文本，就像你在本地运行并直接连接到机器时在监视器上通常看到的那样。还有一个 Serial log，它提供一些可能有帮助的日志，其中包含错误和异常，可能导致根本原因和修复。另一个有用的工具是 Serial console，它提供对 VM 的 COM1 串行连接。我之前提到过 tty0；那是我在部署一个不受祝福的 Azure VM 映像时失败时工作的地方。我当时使用 boot diagnostics 和 Serial console（我无法连接）试图让 Azure VM 工作。这两个功能对于维护和故障排除工作都很有用。

## Costs and Sizes

选择合适的计算大小很重要，因为 Azure VM 的成本是固定的，不像运行按用量计费的 Azure Function。例如。你既不要太多，也不要太少，但从一开始就精确命中大小可能具有挑战性。如果你决定从小开始，然后随着进展进行增加或减少，这是一个好计划。图 4.25 中显示的 Azure VM 调整大小功能可以帮助你采用这种方法。此外，在 VM 未使用时启动和停止 VM

## Creating and Managing a Linux VM Using Azure CLI

- 1. 登录到 Azure 门户 https://portal.azure.com 并点击浏览器左上角的 + Create A Resource。接下来，点击 Compute 然后点击 Virtual Machine。
- 2. 选择订阅和资源组（例如 CSHARPGUITAR-SN1-RG）。输入虚拟机名称（例如 CSHARPGUITAR-LX），然后选择区域（例如 South Central US）。从 Image 下拉列表中选择 Ubuntu Server 18.04 LTS。点击 Password 单选按钮并输入用户名和密码。选择 Allow Selected Ports 单选按钮并允许 HTTP（80）、HTTPS（443）和 SSH（22）。点击 Next: Disks 按钮。
- 3. 保留默认值并点击 Next: Networking 按钮。选择上一章中创建的虚拟网络（我使用的是 CSHARPGUITAR-VNET-B）或创建一个新的。选择子网（我的为 csharp）并点击 + Next: Management 按钮。在 Auto-shutdown 部分选择 On 单选按钮，设置关机时间，设置时区，如果需要，启用关机前通知选项。
- 4. 保留其余默认设置，点击 Review + Create 按钮，然后点击 Create 按钮。
- 5. 完成后，打开浏览器并输入此互联网地址：shell.azure.com。输入你的凭据，然后执行以下 Azure CLI 命令：

```
az vm get-instance-view --name <VMName> --resource-group <Name>
    --query instanceView.statuses[1] --output table

az vm stop --resource-group <Name> --name <VMName>

az vm get-instance-view --name <VMName> --resource-group <Name>
    --query instanceView.statuses[1] --output table

az vm deallocate --resource-group <Name> --name <VMName>

az vm get-instance-view --name <VMName> --resource-group <Name>
    --query instanceView.statuses[1] --output table
```

结果应类似于图 4.26 所示。

