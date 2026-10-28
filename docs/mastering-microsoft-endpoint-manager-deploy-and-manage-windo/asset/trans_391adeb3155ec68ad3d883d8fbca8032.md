

## Windows 365 – 库镜像

如前一节所述，镜像选择选项是配置策略的一部分。还有选择运行 Windows 10 或 11 的库镜像的选项，并按工作负载类型提供预先制作的镜像。

例如，我们的第二个许可规格 2vCPU/4 GB/64 GB 及更高版本的镜像开箱即用包含 Microsoft 365 应用和 Teams AV 优化，而 1vCPU/2 GB/64 GB Lite 镜像则为该特定工作负载类型提供优化的操作系统体验，以获得最佳体验，因为我们已从镜像内部移除了不同内置服务，而这些服务对于支持的工作负载类型是不需要的：

![](img/a3e9f26159d3739e434fe676b2987c1d_87_0.png)

图 3.9 – 选择 Windows 365 镜像

现在，我们已经解释了如何使用首选路径为 Windows 365 Cloud PC 选择镜像。在下一节中，我们将解释如何使用自定义镜像。

## 自定义镜像

一些客户更喜欢使用自己预先构建的自定义镜像，也称为黄金镜像。在 Windows 365 中也支持这种方法，可在配置策略向导期间选择。

支持的自定义镜像服务如下：

- Azure 管理镜像
- 共享镜像库

客户可以自由选择最适合其需求的解决方案，因为我们同时支持 Windows 10 和 11。一些客户更喜欢更现代化的管理方法，即使用一个包含最新 Windows 更新和基础应用的基础镜像，其余部分将通过 Microsoft Intune 应用交付配置文件添加：

![](img/a3e9f26159d3739e434fe676b2987c1d_88_0.png)

图 3.10 – 添加自定义镜像

本章后面我们将解释如何创建自己的自定义镜像以用作 Windows 365 的基础。

## 角色和委派

Windows 365 在 Microsoft Endpoint Manager 中提供在现有 Intune 角色之上委派访问功能和功能的能力。例如，您希望为帮助台员工提供对 Windows 365 门户的只读访问权限，以检查 Cloud PC 是否不可用。其他示例包括将安全相关设置作为安全策略的一部分进行分离。让我在下面的表格中更深入地解释所有角色：

| 角色 | 在 | 可以执行的操作 |
|---|---|---|
| 应用管理器 | Windows 365 Enterprise Cloud PC | Intune 应用管理器可以管理应用程序、读取设备信息并查看设备配置配置文件。 |
| Cloud PC 管理员 | Windows 365 Enterprise Cloud PC | Cloud PC 管理员可以在 MEM 中的 Cloud PC L2 节点中执行读取和写入操作。 |
| Cloud PC 读者 | Windows 365 Enterprise Cloud PC | Cloud PC 读者可以在 MEM 中的 Cloud PC L2 节点中执行读取操作。 |
| 端点安全管理员 | Windows 365 Enterprise Cloud PC | Intune 端点安全管理员可以管理安全和合规功能，例如安全基线、设备合规性、条件访问和 Microsoft Defender for Endpoint，以管理其 Cloud PC。 |
| 帮助台操作员 | Windows 365 Enterprise Cloud PC | Intune 帮助台操作员可以在 Cloud PC 上执行远程任务并将应用程序或策略分配给设备。 |
| Intune 管理员角色 | Windows 365 Enterprise Cloud PC | Intune 管理员角色可以将 Intune 角色（内置和自定义）分配给其他管理员，但不能分配 Cloud PC 角色。 |
| 策略和配置文件管理器 | Windows 365 Enterprise Cloud PC | Intune 策略和配置文件管理器可以管理 Cloud PC 的合规策略、配置配置文件、企业设备标识符和安全基线。 |
| 只读操作员 | Windows 365 Enterprise Cloud PC | Intune 只读操作员对所有 Intune 节点包括 Cloud PC 节点具有只读访问权限。 |
| 学校管理员 | Windows 365 Enterprise Cloud PC | Intune 学校管理员可以在 Intune for Education 中管理 Windows 10 设备。 |

在下面的屏幕截图中，请查看 Microsoft Endpoint Manager 管理中心门户中可用的角色列表：

![](img/a3e9f26159d3739e434fe676b2987c1d_90_0.png)

图 3.11 – Microsoft Endpoint Manager 中可用的 Cloud PC 角色

直接将它们分配给您的用户或 Azure AD 组，以使其更动态地可用。

## Watchdog 服务

我们之前分享了 Windows 365 的愿景，即作为复杂 VDI 相关基础设施的替代方案，使事情更易于使用。Watchdog 服务是煤矿里的金丝雀，是一个很好的例子，说明了我们承担了您通常必须自行排查的工作。

在您完成本地网络连接的配置后（本章后面会更详细地解释），Watchdog 服务将检查您的环境是否满足使用 Windows 365 的所有先决条件，因此请考虑以下项目：

- 检查服务 URL
- 网络访问
- DNS 解析
- 在正确的组织单位中创建计算机帐户的权限
- Azure AD Connect 配置
- 子网范围 – 您的部署是否有足够的 IP 地址可用？
- 端点连接性（MEM 和 AV 服务 URL）

这个功能/服务的另一个优点是它会不断在后台运行。例如，当您的环境中发生某些变化时，它会尝试为您修复 – 或向您作为 IT 管理员发送通知并提供问题的解决方案！

在下面的屏幕截图中，您可以找到一次成功的 Watchdog 检查结果。您可以看到它概述了成功 Windows 365 部署的所有要求，以及更多内容：

![](img/a3e9f26159d3739e434fe676b2987c1d_91_0.png)

图 3.12 – Watchdog 服务 – 检查

## Windows 365 上的优化 Teams

镜像库 – Microsoft 准备的 Windows 10 Enterprise 镜像 – 包含提供 Cloud PC 优化 Teams 体验所需的所有软件。作为 IT 管理员或用户，您只需安装和配置 Microsoft Teams 应用程序即可使用。

这种方法的主要好处是，您可以直接从物理端点到另一方进行 Teams 音视频通话，点对点进行，这有效地创建了与在运行 Microsoft Teams 的物理端点上相同的体验。

优化的一些主要好处如下：

- 由 WebRTC 促进的高性能点对点流媒体 – 流量将点对点流动并通过端点呈现。
- 设备将作为相同的硬件设备重定向，提供更好的硬件重定向支持。
- 在 Windows 10 客户端上，现代媒体堆栈的所有好处，包括硬件视频解码。

## Microsoft Edge

Microsoft Edge Chromium 已被证明非常快速，并且其与其他 Microsoft 服务（如 Microsoft Endpoint Manager）设置策略以及跨平台支持应用同步，例如个人历史记录和收藏夹设置，得到了广泛好评。这导致 Edge 至今成为 Windows 10 Enterprise 的默认浏览器。

## 休眠标签页

Edge 工程团队不久前为 Edge Chromium 添加了休眠标签页作为 Edge 的新原生功能。此功能使您可以在浏览器处于非活动模式时减少每个标签页的 RAM 使用量。使用此功能时 CPU 利用率也会降低。

该功能是在 Chromium 冻结技术的核心之上构建的。冻结会暂停标签页的脚本计时器以最大限度地减少资源使用。休眠标签页在点击时会自动恢复，这与丢弃的标签页不同，后者需要完全重新加载页面。此设置默认启用，设置为 1 小时。

## 启动加速

启动加速可使您的浏览器启动速度快达 41%！在工作和在线浏览时，速度很重要。启动加速通过显著减少设备重启或重新打开浏览器后打开浏览器的耗时来最大化计算机的性能。初步测试显示，使用此功能后启动时间可提高 29% 至 41%。

