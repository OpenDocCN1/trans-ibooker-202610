

如果工业控制系统（ICS）的物理安全是足够的，那么主要威胁来自互联网。在这种情况下，一个合理且常见的补偿性控制措施是对工业控制设备所使用的网络进行物理隔离。这如果得到严格执行可以提供足够的安全性，但保持隔离网络的安全需要谨慎的尽职调查和技术控制，以防止外来设备被连接。如果外部设备必须定期连接，例如出于维护目的，请考虑在维护笔记本电脑和 ICS 网络之间使用防火墙或代理，以提供额外的安全层。

破坏隔离 ICS 的经典例子是 Stuxnet，它被用来对伊朗的铀浓缩设施造成相当大的破坏。据报道，隔离网络是通过将感染病毒的 USB 闪存盘插入 ICS 网络上的计算机而被攻破的。

至少，用于维护和管理工业系统的计算机绝不能用于任何其他用途（或被带出设施）。它们必须定期更新，采用反恶意软件保护，并在使用前仔细扫描任何可移动介质（例如 USB 闪存盘）。请参阅前面列出的组织发布的指南以获取更多具体信息。

如果无法完全与企业局域网和互联网隔离，则必须通过使用精心配置的防火墙和网络代理来限制和筛选允许访问 ICS 网络的流量。应考虑进一步细分 ICS 网络本身，以限制入侵的横向传播。

对于必须能够远程访问的 ICS，应考虑采用安装 Web 代理或 VPN 等补偿性控制措施，在 ICS 本身实施的访问控制之上增加额外的安全层。

安全意识培训也尤为重要，并在所有技术人员和工厂操作员中倡导安全原则至关重要。如果那些承担遵守安全控制需要的人员理解与这些控制相关的重要性和理由，他们就更不可能将它们视为对其运行工厂工作的阻碍，而试图避免或规避。

### 基于云的系统

根据 NIST，“*云计算*是一种使能够实现无处不在、便捷、按需通过网络访问共享的、可配置的计算资源池（例如网络、服务器、存储、应用程序和服务）的模型，这些资源可以快速配置并在最少的管理工作或服务提供商交互的情况下释放。”简而言之，基于云的系统是远程位置、单独管理的系统，可通过互联网访问。

NIST SP 800-145 和 ISO/IEC 17788 定义了描述云计算的若干特征：

*   **广域网络访问：**资源（物理和虚拟）可通过网络访问和管理。
*   **可计量服务：**用户仅为所使用的服务付费。
*   **按需自助服务：**用户可以使用自动化工具在无需人工交互的情况下配置和管理服务。
*   **快速弹性与可扩展性：**服务可以快速自动地根据需求进行扩展或缩减。
*   **资源池化和多租户：**物理或虚拟资源被聚合以服务于多个用户，同时保持其数据隔离且无法被其他租户访问。

共有三种主要的 *云服务模型*，如 [Table 3.2](#c03.xhtml#c03-tbl-0002) 所示。

**[TABLE 3.2](#c03.xhtml#R_c03-tbl-0002)  Cloud Service Models**

| CLOUD SERVICE MODEL || SERVICE PROVIDED || SERVICE PROVIDER RESPONSIBILITIES || CUSTOMER RESPONSIBILITIES |
| --- | --- | --- | --- |
| Software as a service (SaaS) || Software application accessible to the customer over the internet (via a browser or API) || Provide and manage all infrastructure from server and network hardware to applications software || Provide the client device and manage user-specific configuration settings |
| --- | --- | --- | --- |
| Platform as a service (PaaS) || Web-based framework for developers to create customized applications || Provide and manage all infrastructure from server and network hardware to the libraries and runtime services necessary to run applications || Provide the application and manage the hosting environment |
| Infrastructure as a service (IaaS) || Infrastructure, including servers, network, storage, and operating systems, delivered through virtualization technology || Provide network and server infrastructure to support VMs and other virtualized resources || Provide and manage all components that run on the VM as well as limited aspects of network services |

对于每个服务模型，显然云服务提供商提供的组件与客户提供的组件之间存在不同的划分。这就产生了云共享责任模型，如 [Figure 3.9](#c03.xhtml#c03-fig-0009) 所示。云部署的安全性由客户和提供商共同承担。云服务提供商负责云的安全性，客户负责云中的安全性。这也意味着在评估云部署的风险时，必须考虑提供商的安全能力和承诺。

![FIGURE 3.9 The cloud shared responsibility model for IaaS, PaaS, and SaaS](images/c03f009.png)

**[FIGURE 3.9](#c03.xhtml#R_c03-fig-0009) The cloud shared responsibility model for IaaS, PaaS, and SaaS**

特别是，云服务提供商对以下内容负有排他责任：

*   物理安全
*   环境安全
*   硬件（即服务器和存储设备）
*   网络（即电缆、交换机、路由器、防火墙和互联网连接）

云服务提供商和客户对以下内容共同负责：

*   **漏洞和补丁管理：**以 [Figure 3.9](#c03.xhtml#c03-fig-0009) 为指导确定哪个组织负责哪些活动，云服务提供商负责修补责任分界线以下的软件，客户负责线以上的一切（即在 SaaS 的情况下，客户仅负责用于访问云的计算机和浏览器）。
*   **配置管理：**云服务提供商负责基础设施，客户负责其上的一切。网络配置和安全是一个特殊情况，因为 IaaS 提供商的客户通常可以通过 API 和基于 Web 的管理控制台对网络路由和防火墙的重大方面进行配置控制。由于这些网络控制是 IaaS 云部署安全性的关键组成部分，客户必须非常小心地确保其正确配置。
*   **培训：**云提供商和客户都对其各自人员所需的特定培训负责。

在 IaaS 情况下，客户对许多网络安全方面负有责任，提供边界和内部安全需要不同于传统数据中心安全的方法。在云中，客户很少有能力安装实际的网络或安全设备（例如防火墙、路由器、交换机等），而是被授予对有限数量的防火墙和路由器功能的 API 访问权限，可用于控制对其 VM 的访问。此外，根据云服务提供商的不同，网络流量的访问，如在数据中心使用网络 TAP 或 SPAN（镜像）端口所期望的那样，可能不可用，或者可能仅以 NetFlow 或 sFlow 等采样方式可用。

