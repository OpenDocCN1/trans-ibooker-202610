

4. 验证是否显示了数据存储，并且信息（例如，容量、类型等）是否全部正确。例如，在我下面的截图中，你会看到 *Sam256* 数据存储有一个警报，因为其容量已用尽。

![](img/430161835ae267467687add4a1cdc429_345_0.png)

5. 在导航器中，选择一个数据存储并点击所有标签页，最后到 **VMs**。验证在该数据存储上创建的虚拟机是否已显示。

![](img/430161835ae267467687add4a1cdc429_345_1.png)

## Networking

要使用 vSphere Client 探索网络，请执行以下步骤：

1. 在 **Menu** 下拉菜单中，选择 **Networking**。

![](img/430161835ae267467687add4a1cdc429_346_0.png)

2. 验证 **Networking** 图标是否在导航窗格顶部下方有下划线，然后展开数据中心。

![](img/430161835ae267467687add4a1cdc429_346_1.png)

3. 在主窗格顶部，选择 **Networks** 标签。

4. 验证网络是否已显示，并且信息（例如，类型、主机等）是否正确。

![](img/430161835ae267467687add4a1cdc429_347_0.png)

5. 在导航器中，选择 **VM Network** 并点击所有标签页，最后到 **VMs**。验证使用该网络的虚拟机是否已显示。

![](img/430161835ae267467687add4a1cdc429_347_1.png)

4. 验证虚拟机是否已显示，并且显示的所有信息（例如，状态、状态等）是否正确。

![](img/430161835ae267467687add4a1cdc429_349_0.png)

5. 在导航器中，选择一个虚拟机并依次点击所有标签页。

![](img/430161835ae267467687add4a1cdc429_349_1.png)

## Cloning Virtual Machines

你可以使用 vSphere Client 克隆虚拟机。使用 ESXi Host Client 时，我们需要手动复制文件来复制虚拟机。可以克隆已开机和已关机的虚拟机，但克隆已开机虚拟机所需时间更长。克隆虚拟机时，可以选择不同的数据存储。

要使用 vSphere Client 克隆虚拟机，请执行以下步骤：

1. 在导航器中，右键单击虚拟机并选择 **Clone ➤ Clone to Virtual Machine.**

![](img/430161835ae267467687add4a1cdc429_350_0.png)

2. 输入克隆虚拟机的信息，接受默认值，验证设置，然后单击 **Finish.**

![](img/430161835ae267467687add4a1cdc429_350_1.png)

3. 在 vSphere Client 的下部窗格中监控任务，直到完成。

## vMotion

vSphere Client 允许在虚拟机仍在运行时将其从一个 ESXi 主机传输到另一个主机——这是一项非常强大的工具，从业务连续性和可用性的角度来看。这可能是使 VMware 在数据中心确立地位的特性之一。基于 Arm 的虚拟机无法 vMotion 到 x64 服务器。

在生产环境中，你会设置一个单独的虚拟网络来处理 vMotion 流量。由于 RPi 只有一个 NIC，我们没有设置它。

在下面的示例中，我使用了具有多个 ESXi 主机的 vCenter Server。

要 vMotion 虚拟机，请执行以下操作：

1. 在导航器中，右键单击正在运行或已关机的虚拟机并选择 **Migrate**。

![](img/430161835ae267467687add4a1cdc429_351_0.png)

2. 选择 **Change compute resource only** 并单击 **Next.**

![](img/430161835ae267467687add4a1cdc429_352_0.png)

3. 选择要将虚拟机迁移到的主机（将显示虚拟机可以迁移到的所有主机）。

4. 选择迁移后虚拟机要使用的网络。

5. 设置 vMotion 优先级。

![](img/430161835ae267467687add4a1cdc429_352_1.png)

6. 验证信息并单击 **Finish.**

![](img/430161835ae267467687add4a1cdc429_352_2.png)

7. 在 vSphere Client 的下部窗格中监控任务，直到完成。

![](img/430161835ae267467687add4a1cdc429_353_0.png)

## Storage vMotion

vSphere Client 不仅允许在虚拟机仍在运行时将其从一个 ESXi 主机传输到另一个主机，还可以更改其所在的数据存储。这允许维护存储并将其退租，或因性能原因将虚拟机移离该存储。虚拟机可以从任何数据存储迁移到另一个数据存储；这包括从本地附加存储迁移到网络存储，反之亦然。

要存储 vMotion 虚拟机，请执行以下操作：

1. 在导航器中，右键单击正在运行或已关机的虚拟机并选择 **Migrate**。

![](img/430161835ae267467687add4a1cdc429_353_1.png)

2. 选择 **Change storage only** 并单击 **Next**.

![](img/430161835ae267467687add4a1cdc429_354_0.png)

3. 选择要将虚拟机迁移到的存储。

![](img/430161835ae267467687add4a1cdc429_354_1.png)

4. 验证信息并单击 **Finish**.

5. 在 vSphere Client 的下部窗格中监控任务，直到完成。

## Automatic VM Startup with a Host

vCenter Server 是一个功能强大的工具，通过 vSphere Client 浏览其众多功能，你可以开始掌握其强大的功能以及如何在数据中心利用这种功能。其中一个例子是它能够与 ESXi 主机一起自动启动和关闭虚拟机。随着更先进技术的出现，这一功能的使用不如以前普遍，但我仍然认为它是 vCenter Server 强大功能的良好示例。

要使虚拟机随主机启动，请执行以下步骤：

1. 在 **Menu** 下拉菜单中，选择 **Hosts and Clusters**。

![](img/430161835ae267467687add4a1cdc429_355_0.png)

2. 验证 **Hosts and Clusters** 图标是否在导航窗格顶部下方有下划线，然后展开数据中心。

3. 在导航器中，选择一个 ESXi 主机，然后打开 **Configure** 标签。

![](img/430161835ae267467687add4a1cdc429_355_1.png)

4. 展开 **Virtual Machines**，选择 **VM Startup/Shutdown**，然后单击 **Edit**。

![](img/430161835ae267467687add4a1cdc429_356_0.png)

5. 选择 **System influence** 复选框。

**Edit VM Startup/Shutdown Configuration | 10.0.0.238**

**Default VM Settings**

| Setting | Value |
| --- | --- |
| System influence | ☑ Automatically start and stop the virtual machines with the system |
| Startup delay | 120 |
| Continue if VMware Tools is started | ☐ |
| Shutdown delay | 120 |
| Shutdown action | Power off |

6. 选择其中一个虚拟机并单击 **Move Up**。

7. 验证 **Startup Order** 下现在显示 **Automatic**，并且虚拟机位于该列中。

![](img/430161835ae267467687add4a1cdc429_357_0.png)

使用 vCenter Server 的自动启动和关闭功能，你可以与 ESXi 主机协同优雅地关闭和重新启动多层应用程序。例如，你可能希望在承载数据库的虚拟机启动后几分钟再启动承载 Web 应用程序的虚拟机，而该功能允许你做到这一点。如果 VCSA 托管在 ESXi 主机上，你甚至可以将其设置为系统启动时自动启动，因为这些功能不直接依赖于 VCSA。

## Using Third-Party Products with VCSA

VMware 很早就意识到他们无法满足所有人的所有需求，因此他们积极推广供应商生态系统来填补其产品线中的空白和细分市场。VMware 有数百家，如果不是数千家这样的供应商。如果你曾经有机会参观 VMworld，VMware 的年度用户大会，你会看到 VMware 第三方供应商的一个小样本，从软件备份工具到大型硬件供应商的大型显示屏。

截至撰写本文时，我还没有看到任何供应商明确表示他们支持 ESXi on Arm，但同时我也没有看到任何供应商表示他们不支持。理论上，由于 ESXi 主机和 vCenter Server 使用相同的 API 与 VMware 产品集成，只要它不直接与主机上的虚拟机交互，它们应该也能很好地与 ESXi on Arm 以及 x64 ESXi 主机上的 ESXi 一起使用。

### ControlUp

第三方产品为 VMware 提供了丰富性，在本节中，我将向你展示一个特定的第三方产品 ControlUp Real-Time Console 如何增强 VMware 的基础产品。

> **Note**   作为免责声明，截至撰写本文时，我担任 ControlUp 的技术营销工程师，我选择使用该产品是因为我对其很熟悉。

尽管我相当确定许多，如果不是大多数，第三方产品将与 ESXi on Arm 兼容，但我还是想验证这一点。我认为 ControlUp 是一个不错的选择，因为我知道它稳定并且使用 API 与 vCenter Server 进行深度集成。

ControlUp 广泛用于监控和管理 VDI 环境。它已有十多年的历史，在早期专注于 Citrix VDI 环境，即 Citrix 虚拟桌面及其运行的基础设施。由于绝大多数 Citrix 桌面运行在 vSphere 上，ControlUp 从早期阶段就熟悉了 VCSA 与第三方产品集成方面的细微差别。ControlUp 现在支持各种 VDI 环境，包括 VMware Horizon。正如所讨论的

