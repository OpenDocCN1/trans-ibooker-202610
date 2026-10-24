

一些 Unix 操作系统内置了加密文件系统功能。Unix 操作系统中作为标准部分包含的最初加密工具是 *crypt*。然而，这个工具被认为是一种功能非常弱的加密工具，相对容易被破解。因此，它并没有被广泛使用。数据加密标准（DES）是一种更强的加密工具，用于许多 Unix 发行版。Pretty Good Privacy（PGP）是另一种广泛使用的 Unix 加密工具。该工具同时支持私钥和公钥的加密/解密，并提供了一种非常强大的数据安全方法。

基于 Free BSD 内核的 Unix 发行版通过 PEFS、GELI 和 GBDE 实用程序提供加密服务。私有加密文件系统（PEFS）是一种基于文件的加密系统。GELI 和基于 GEOM 的磁盘加密（GBDE）是磁盘级别的加密实用程序。另一个用于其他 Unix 或类 Unix 操作系统的著名加密工具是 EncFS。

FreeBSD 和 OpenBSD 发行版还提供了多种内置防火墙实用程序，用于控制数据流入本地计算机。这些选项包括 ipfirewall（ipfw）、IPFilter 和 Packet Filter（PF）。与前面提到的集成 Windows 防火墙一样，这些应用程序使本地计算机能够通过称为 *数据包过滤* 的过程来控制传入和传出数据的流动。本地防火墙和数据包过滤将在本章后面详细讨论。

> **关于加密工具的一点说明**
> 
> 许多国家已制定法律禁止使用强加密。因此，不同的操作系统可能会因销售地点不同而提供或不提供不同的加密工具。

## Linux 操作系统发行版

许多个人电脑用户运行的是一个名为 Linux 的自由分发、开源操作系统版本。Linux 是一个非常强大的命令行操作系统，可用于各种硬件平台，包括 Windows PC 和 Apple Mac 系统。

一个程序员社区与 Linux 监督委员会合作，不断升级和增强 Linux 的基本结构，使其保持领先和竞争力。此外，一些公司开发了专有的加密服务，可在单个文件或目录上应用，而无需显著的磁盘管理开销。

Linux 发行版通过一个名为 Netfilter 的内核实用程序提供内置的本地防火墙功能。除了为本地防火墙实现提供数据包过滤外，该实用程序还通过称为网络地址转换（NAT）和端口地址转换（PAT）的过程提供额外保护，用于在网络中引导数据包，同时对外部网络的主机屏蔽私有 IP 地址。

## Apple OS 版本

Apple Inc. 生产的个人电脑在本质上与 PC 不兼容。它们具有截然不同的硬件设计，并且不能直接运行为 Win/PC 环境开发的软件包。

所有较新的 Apple Mac 电脑都运行在一个名为 Apple OS X 的专有 Unix 版本上。虽然 OS X 的结构基于 Unix，但系统的用户交互部分采用了 Apple 标志性的基于 GUI 的桌面。这赋予了 Mac 一个非常强大而稳定的引擎，以及非常友好的用户界面：

- Apple 客户端版本：macOS、OS X 和 Mac OS X
- Apple 服务器版本：macOS Server、Mac OS X Server 和 OS X Server

macOS 被设计用于支持 Intel、Cyrix、VIA 和 AMD 的 x86 和 x86 64 位微处理器类型，以及 IBM 和 Motorola 的 PowerPC 处理器。这些微处理器都支持 NX-bit 功能，以保护内存的某些区域免受病毒操纵。

macOS 支持前面描述的 ISO9660、FAT、NFS、UFS 和 UDF 文件系统格式。此外，不同的 macOS 版本还提供对以下 FMS 标准的支持：

- HFS/HFS+ – 分层文件系统是 Apple 专有的文件系统，作为其使用 macOS 的 Mac 电脑系列的主要文件系统而开发。它也用于 Apple 的 iPod 音乐设备系列。对更新版 HFS+ 的支持已包含在非 Apple 操作系统中，包括 Linux 内核和 Windows（包括 Apple 的 Xbox 360 游戏机中的支持）。
- SMBFS/CIFS – 服务器消息块文件系统，也称为通用 Internet 文件系统，是为了提供对文件和设备的共享访问以及网络通信而开发的。在 Active Directory 出现之前，该 FMS 主要用于运行 Windows 的计算机。然而，对更新版 SMB 版本的支持一直延续到所有当前的 Microsoft 操作系统版本，并且还进入了多个非 Microsoft 操作系统。

Apple macOS 操作系统通过名为 FileVault 的服务提供基于磁盘级别的数据加密；FileVault2 macOS 用户还可以使用内置磁盘工具来加密其磁盘并存储其主目录的子集。

## iOS

iOS 是 Apple 专有的移动操作系统，旨在支持 Apple 的 iPhone 和 iPad 产品线。虽然 iOS 与 macOS 共享许多结构，但它与 macOS 应用程序不兼容。

iOS 被设计用于支持 Acorn RISC Machines (ARM) Ltd 公司的精简指令集计算（RISC）处理器类型。这些设备比其 x86 复杂指令集计算（CISC）竞争对手消耗的能量显著更少，产生的热量也少得多。这使得 RISC 处理器自然适用于小型电池供电设备，如手机、平板电脑和 pad。从 ARMv6 版本开始，该架构支持了 Execute Never（XN）页面保护功能。

iOS 系统包含集成的设备级文件加密，以在运行设备丢失或被盗时保护数据。它还提供了防止未经授权的用户使用或修改数据的保护。除了其硬件加密功能外，iOS 还提供了一种称为 Data Protection 的基于类别的工具。使用此保护工具，每次数据写入设备的闪存结构时都会创建基于文件的加密密钥。

## Android OS

Android 操作系统是为主要使用触摸屏手势进行操作和控制的平板电脑和智能手机设备而开发的。由于其在众多智能手机和平板设备中的使用，它目前是最流行的移动操作系统，如图 8.6 中的图表所示。

- ► JFFS2 – Android 默认日志闪存文件系统（版本 2）。此 FS 版本取代了 YAFFS2（Yet Another Flash File System）作为早期内核版本中使用的 Android 默认闪存文件系统。
- ► Ext2/Ext3/Ext4 – Linux 扩展文件系统的版本，取代了 F2FS 和 JFFS2 作为 Android 内部闪存设备的文件系统。

除了这些闪存文件系统之外，Android 设备通常还支持 Microsoft 文件分配表（FAT）文件系统（FAT12、FAT16 和 FAT32）及其 VFAT 扩展。

Android 操作系统提供基于名为 dm-crypt 的实用程序的磁盘加密，用于将数据存储在设备的闪存设备上。该实用程序是 Android Linux 内核的一个组成部分。

## 操作系统安全选择

由于其普及性，Microsoft Windows 是恶作剧和恶意软件及灰色软件（令人讨厌的不需要的软件）作者的最大目标。因此，Windows 承受了所有与病毒和间谍软件相关的攻击中无与伦比的比例。

这一事实导致一些 Windows 客户采用了其他操作系统平台，如 Linux 或 macOS，这些平台成为恶意软件目标的可能性要小得多。表 8.1 总结了与各种操作系统相关的一些常见安全功能。

表 8.1 操作系统安全比较

| 名称 | 资源访问控制 | 子系统隔离机制 | 集成防火墙 | 加密文件系统 | 禁止执行（NX） |
| Linux | POSIX、ACL、MAC | chroot、基于能力的安全、seccomp、SELinux、AppArmor | Netfilter，因发行版而异 | 是 | 硬件/模拟 |
| macOS | POSIX、ACL | chroot、使用 chflags 设置的 BSD 文件标志 | PF | 是 | 硬件/模拟 |

