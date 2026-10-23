
| Linux 操作系统 | Linux 版本 |
|---|---|
| Clear Linux OS | 基础版、容器、机器学习 |
| SUSE Linux Enterprise | 15、12 SP4 |
| Debian | 8、9 |

在 Linux 上下文中，有一个概念被称为*官方认可的镜像*或*认可的发行版*。如前所述，你可以使用任何操作系统和配置构建任何机器，并尝试将其部署到 Azure。这里的关键词是*尝试*。可能的配置如此之多，以至于目前要实现 100% 的覆盖是不可想象的。我曾尝试部署一个未经官方认可的 Linux 操作系统，但没有成功。当你部署并遇到某种问题时，你往往会求助于串行控制台。串行控制台允许你与 VM 建立硬件连接，而不是使用需要网络连接的 SSH。默认情况下，串行控制台监听一个名为 tty0 的虚拟控制台。而我当时部署的 Linux 镜像被配置为监听 tty1，因此我无法连接。我只是偶然发现了这一点，但这就是你不使用认可或推荐的镜像时会发生的情况。可能会出现许多“一次性情况”，延误你的部署，甚至更糟，完全阻止部署。因此，最谨慎的做法是让你的应用程序以 Azure 市场中的镜像之一为目标；不过，还有一些被认为官方认可和推荐的 Linux 发行版并不存在于 Azure 市场中。它们列在表 4.6 中。

## 表 4.6 其他受认可的 Linux 发行版

| Linux 操作系统 | Linux 版本 |
|---|---|
| CentOS | 6.3, 7.0 |
| CoreOS | 494.4 |
| Debian | 7.9, 8.2 |
| Oracle Linux | 6.4, 7.0 |
| Red Hat Enterprise | 6.7, 7.1, 8.0 |
| SUSE Linux Enterprise | 适用于 SAP 的 SLES、11 SP4、12 SP1 |
| openSUSE | Leap, 42.2 |
| Ubuntu | 12.04 |

与 Windows 机器一样，VM 大小的建议基于类别和操作系统版本。请参阅表 4.7 中按 Linux 操作系统发行版给出的 VM 大小建议。若要更详细地了解 Linux VM 实例的详细信息，请查看 https://docs.microsoft.com/en-us/azure/virtual-machines/linux/sizes-memory。随着计算资源分配量的增加，成本也会增加；因此，选择合适的大小非常重要。

## 表 4.7 Linux 版本到 Azure VM 大小的建议

| Linux 版本 | 类别 | 实例/系列 |
|---|---|---|
| Ubuntu CoreOS | 通用型 | DS1V2, DS2V2, D2SV3, D4SV3, D1V2, D1, DS1, DS2 |
|  | 内存优化型 | DS11V2, E2SV3, DS11 |
| Red Hat Enterprise | 通用型 | DS2V2, D2SV3, D4SV3, D2V2, D2V3, D2, DS2 |
| Clear Linux OS | 通用型 | D1, D3, DS3, DS4 |
| SUSE Linux Enterprise | 通用型 | DS1V2, DS2V2, D1V2, D1, Ds1, DS2 |
|  | 内存优化型 | DS11V2, DS11 |

没有专门针对运行 Linux 的 Azure VM 的限制或配额；它们与前面表 4.4 中涵盖的相同。Microsoft 完全支持 Linux，并且没有任何政策或做法会故意限制该操作系统。

## Azure VM 扩展

扩展是小型程序或自动化活动，有助于执行部署后任务、安全性、监控和自动化部署。如果你在之前创建 Azure VM 的练习中感到好奇，那里有一个名为“高级”的选项卡，该选项卡上有一个区域允许你选择扩展。在练习中，我通常会跳过这些选项卡，但你可能会查看它们并想知道所有这些功能是什么。请考虑访问门户并模拟创建新的 Azure VM。首先，请注意，在选择基于 Linux 或 Windows 的 VM 时，可安装扩展的列表也不同。区域在扩展列表中起作用，所以这又是一个例子，说明在承诺使用某个区域之前，需要了解每个区域中可用的功能。

对于 Windows，有一些不错的扩展，例如 PowerShell 期望状态配置扩展，它有助于执行部署后活动，确保 VM 在每种情况下都以相同的方式配置。一旦你的工作负载变得相当复杂并需要自动化部署（下一节将更详细地讨论），这一点就很重要。还有反恶意软件、云安全和其他与安全相关的代理，可以作为扩展部署、配置并在你的 Azure VM 上运行。当你创建初始 Azure VM 时，你需要配置所有这些特定于环境的功能，然后捕获你的镜像，供以后的自动化或手动部署使用。

Microsoft 提供了许多监控功能；但是，它也通过此扩展功能完全支持其他公司提供更具体的监控功能。一些可安装的第三方监控产品包括 Datadog、APM Insight 和 Dynatrace。第 9 章将更详细地介绍监控，但将重点介绍基于 Azure 平台的功能，而不是 IaaS 中的第三方扩展。如果你有兴趣了解有关这些扩展的更多信息，请查看此在线文档：

```
docs.microsoft.com/en-us/azure/virtual-machines/extensions/overview.
```

## 自动化部署

第 8 章将介绍部署和迁移，重点介绍 ARM 和代码部署（又称内容部署）。你已经知道，部署应用程序有很多方法，而且需要部署许多组件才能使其工作。如果这些部署任务中的任何部分可以自动化，就会减少所需的工作量。回想之前的许多练习，在 Azure VM 预配完成后，你被要求通过 RDP 或 Bastion 连接到服务器，并使用一些 PowerShell cmdlet 手动安装 IIS。如果你只需要部署一台或两台需要 IIS 的服务器，这是一种可接受的方法；但是，如果要部署 50 台或 100 台，那么这种方法真的不值得考虑。手动登录 50 多台服务器并逐一进行手动配置是不现实的。练习 4.6 是一种类似的方法，可以达到同样的效果：创建一个镜像，并将其用作所有未来部署的基线。使用自动化脚本是另一个可选方案，当你使用 PowerShell 部署 Azure VM 时，它也很有用。在某些情况下，这两种功能的组合甚至可以带来巨大的价值。

自定义镜像和自动化部署脚本都有用的一个示例场景是：没有公共镜像安装有运行脚本所需的实用程序。例如，如果你想要运行 Az PowerShell cmdlet，那么镜像必须预先安装这些 cmdlet 才能执行。目前，这需要首先运行以下 PowerShell 安装命令；你可能还记得上一章中的这个命令。

```
Install-Module -Name Az -AllowClobber -Scope AllUsers
```

如前一节所述，在 Azure VM 创建边栏选项卡的“高级”选项卡上有一个名为“扩展”的选项。单击“选择要安装的扩展”会打开一个窗口，让你选择要安装的扩展。用于执行自定义脚本的扩展分别称为“Custom Script Extension for Windows”和“Custom Script for Linux”。构建 Windows 操作系统 VM 时，你可以将以下 PowerShell cmdlet 保存到例如名为 iis.ps1 的文件中，并在门户中配置 Azure VM 的构建属性时上传该文件。

```
Set-AzVMExtension `
    -ExtensionName IIS `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"Add-WindowsFeature `
        -name Web-Server -IncludeManagementTools"}'
```

然后，一旦创建了 Azure VM，就会使用扩展安装 IIS。你还可以使用扩展安装例如 SQL Server 和 .NET Framework。从 Linux VM 的角度来看，除了扩展之外，“高级”选项卡上还有一项功能，可以实现 cloud-init 脚本，这些脚本可以配置用户和安全，并安装软件包。客户和开发人员在此工具中的创造力各不相同，而 Azure 的奇妙之处在于它提供了一个实现这种创造力的平台。我意识到本节有点抽象。我只是指出此功能是在将应用程序部署到 Azure VM 时可以追求和考虑的一个选项。

现在，你应该对如何创建 Azure VM（无论是 Windows 还是 Linux）有了很好的理解。如果被问到，你还应该知道这些操作系统的哪些版本是受认可的，以及尝试部署不受认可的镜像可能意味着什么。你还应该了解 VM 的不同类别/系列的含义，例如内存优化型和通用型。给定一个表，其中列出了不同 VM 及其操作系统、CPU 要求和内存要求，你需要知道哪些 VM 受 Azure 认可，以及所请求的资源是否超出任何配额或资源限制。

## Azure 专用主机

当你预配 Azure 虚拟机时，你会从主机上运行的现有虚拟机池中获得计算资源。可用的计算容量池通常由所有 Azure 客户共享。不过，请放心，在解除分配之后，不会有任何内容或配置残留。虚拟机在放回可用资源池之前会被完全清理。如果你希望或需要让所有虚拟机都在自己的主机（即物理机）上运行，并且该主机不是从共享计算资源池中解除分配或预配而来的，那么你可以选择 Azure 专用主机。访问此站点以了解有关此产品/服务的更多信息：

docs.microsoft.com/en-us/azure/virtual-machines/windows/dedicated-hosts.

Azure 专用主机的成本高于在共享托管环境中运行的成本。这是因为你需要为主机上所有可用的计算能力付费，而不仅仅是主机已消耗的计算能力。使用 Azure 专用主机的一个优势是能够控制任何可能产生影响的基础设施变更
