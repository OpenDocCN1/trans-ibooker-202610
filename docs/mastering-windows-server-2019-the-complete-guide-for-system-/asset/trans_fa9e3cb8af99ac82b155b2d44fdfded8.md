
不幸的是，这些功能（至少目前为止）并未移植到 Microsoft VPN 世界中。无论您是通过传统 VPN 连接 Windows 7 客户端，还是让 Windows 10 客户端使用 Always On VPN 连接，RRAS VPN 的后端基础设施都是相同的，并且没有内置对多个服务器或站点的支持。当然可以做到，使该 VPN 系统实现冗余，但这需要您自行使用外部负载均衡器进行设置，并且通常还需要使用全局站点/服务器负载均衡器来使流量正常流动。

任何过去设置过任何类型的负载均衡 VPN 的人可能都非常了解这一过程并且能够轻松配置，这很好。但这对于服务器、网络设备和 IT 经验有限的小型企业客户来说绝对是一个限制因素。总的来说，与 DirectAccess 相关的控制台中内置的额外功能使其在构建具有故障恢复能力的远程访问基础设施方面领先于任何 VPN 解决方案。

## Distribution of client configurations
客户端配置的分配

最后需要考虑的主要因素是在决定远程访问方向时，客户端设置如何推送到各自计算机的方式：

- **Third-party VPN:** 我们已经讨论过处理第三方 VPN 厂商软件应用程序的缺点。如果您可以使用 Windows 操作系统内置的功能，那似乎是不言而喻的选择。
- **Always On VPN:** 推广 AOVPN 设置到客户端计算机的推荐方式是通过使用 MDM 解决方案，即 SCCM 或 Intune。如果您拥有其中一种系统，将 AOVPN 设置推广到员工中非常轻松。如果您没有这些系统中的任何一个，仍然可行，但过程并不简单。
- **DirectAccess:** 我认为 DA 的客户端设置分发方法绝对是最容易使用且最灵活的。您必须记住 DirectAccess 仅适用于加入域的系统。鉴于您可以期望所有客户端都加入域，您可以通过组策略推广 DirectAccess 连接设置，而组策略存在于任何 Microsoft 驱动的基础设施中。

我真心希望未来能在 Always On VPN 配置推广中增加组策略分发选项。如果引入此类功能，我完全有信心它会立即成为推广 AOVPN 设置最流行的方式。

总结整个主题，将 DirectAccess 与传统的手动启动 VPN 相比，DA 毫无悬念地获得第一名。两者真的没有可比性。现在我们有了 Always On VPN 可用，两者之间的优势（DA 或 AOVPN）变得相当模糊。它们以不同的方式实现了很多相同的功能。到目前为止，大多数客户的主要决定因素似乎是客户端推广能力、是否可以访问 MDM 解决方案，以及设备隧道连接对他们的重要性。微软的目标是让 AOVPN 与 DirectAccess 功能对等，并且已经接近。Always On VPN 还具有 DirectAccess 不具备的一些高级身份验证功能，例如与 Windows Hello for Business 或 Azure MFA 的集成。

## Web Application Proxy
Web Application Proxy

DirectAccess 和 VPN 都是出色的远程访问技术，将两者结合起来可以为您的组织提供完整的远程访问解决方案，而无需支付费用或与第三方解决方案合作。更妙的是，在 Windows Server 2019 中，RemoteAccess 角色中还有另一个可用的组件。远程访问故事的第三部分是 **Web Application Proxy (WAP)**。这本质上是一种反向代理机制，使您能够将托管在企业网络内部的某些 HTTP 和 HTTPS 应用程序安全地发布到互联网。过去十年一直在边界网络领域使用 Microsoft 技术的人们可能都会认识一个名为 Forefront **Unified Access Gateway (UAG)** 的产品，它实现了类似的功能。UAG 是一款全面的 SSL VPN 解决方案，也旨在通过 SSL 将内部应用程序发布到互联网。它的功能远比简单的反向代理强大，包括预身份验证、SSTP VPN 和 RDS 网关等组件；DirectAccess 本身甚至可以通过 UAG 运行。

如果您的所有移动工作人员都可以启动 DirectAccess 或 VPN，那么您可能不需要 WAP。然而，随着云思维的增长，用户期望从任何计算机、任何地方打开网页浏览器并访问其某些应用程序的情况非常普遍。文档访问现在通常由 SharePoint 等 Web 服务提供。通过访问 Outlook Web Access，电子邮件访问可以从任何计算机远程获得。使用 Azure 和 Microsoft 365 等云服务时，这很容易实现，但很多人没有意识到这对于本地 Exchange 和 SharePoint 服务也是如此。

如此多的应用程序和数据只能通过网页浏览器访问，这使员工能够在无需建立 VPN 等完整的企业隧道的情况下访问这些数据。那么 WAP 在现实世界中的用例是什么？您不希望其 VPN 连接的家庭计算机。这样，您就不必太担心家庭或用户拥有的计算机的健康状况和状态，因为它们与公司的唯一交互是通过网页浏览器。这限制了这些计算机可能将恶意活动带入您网络的可能性。正如您所见，像 WAP 这样的技术在远程访问市场中确实有其一席之地。

我希望随着时间的推移，WAP 会继续得到改进，从而使其成为 UAG 的真正替代品。UAG 运行在 Windows Server 2008 R2 上，现在已被微软正式停用为产品。微软现在最接近 UAG 的解决方案是 WAP 角色。它还不像 UAG 那样全面，但他们正在努力改进。目前，WAP 可用于发布对简单 Web 应用程序的访问。您还可以发布对使用基本 HTTP 身份验证的富客户端的访问，例如 Exchange ActiveSync。还包括能够向使用 MSOFBA 的客户端发布数据，例如当用户尝试从本地计算机上运行的 Word 或 Excel 应用程序中提取公司数据时。

WAP 可用于反向代理（发布）对 Exchange 和 SharePoint 环境等内容的远程访问。这不是一件小事，因为这些是几乎每个人都在使用的技术，因此为这些资源发布安全访问而实施 WAP 对您的公司来说肯定是有益的；这肯定比直接将 Exchange 服务器 NAT 到互联网更好。

### WAP as AD FS Proxy
### WAP 作为 AD FS 代理

利用 WAP 服务器的另一种有用方式是在网络中设置 Active Directory Federation Services (AD FS) 时（这可能是目前 WAP 最常见的用途）。AD FS 是一种旨在为用户启用单点登录和与其他公司联合的技术，因此它涉及将来自互联网的流量引入内部网络。过去，有一个伴随 AD FS 的 Windows Server 角色组件，称为 AD FS 代理。在最新的 AD FS 版本中，这个单独的角色不再存在，已被 Remote Access 角色的 Web Application Proxy 组件取代。这更好地统一了远程访问解决方案，将传入的 AD FS 流量通过官方 Remote Access 服务器传输，而无需单独的 AD FS 代理服务器。任何在环境中实施面向外部的 AD FS 的人很可能会在某个时候被要求部署 WAP。
