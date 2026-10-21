

## 1.7.4 PDU 连接限制

支持两种可能的 PDU 额定值：60/63 安培和 30/32 安培。PDU 额定值由用于将 PDU 连接到电气供电的电源线确定。每个电源线支持的系统节点数和 I/O 扩展抽屉数列于表 1-22。

表 1-22 按电源线最大支持机柜数

| Feature code | Wall plug | PDU Rating | Maximum supported system nodes per PDU pair | Maximum supported I/O drawers with no system nodes |
|---|---|---|---|---|
| 6653 | IEC 309, 3P+N+G, 16 A | 60 Amps | Two system nodes and 1 I/O expansion drawer | 8 |
| 6489 | IEC309 3P+N+G, 32 A | 60 Amps | Two system nodes and 1 I/O expansion drawer | 8 |
| 6654 | NEMA L6-30 | 30 Amps | One system node and 1 I/O expansion drawer | 4 |
| 6655 | RS 3750DP (watertight) | 30 Amps | One system node and 1 I/O expansion drawer | 4 |
| 6656 | IEC 309, P+N+G, 32 A | 30 Amps | One system node and 1 I/O expansion drawer | 4 |
| 6657 | PDL | 30 Amps | One system node and 1 I/O expansion drawer | 4 |
| 6658 | Korean plug | 30 Amps | One system node and 1 I/O expansion drawer | 4 |
| 6492 | IEC 309, 2P+G, 60 A | 60 Amps | Two system node and 1 I/O expansion drawer | 4 |
| 6491 | IEC 309, P+N+G, 63 A | 60 Amps | Two system nodes and 1 I/O expansion drawer | 8 |

## 1.7.5 架装规则

将系统安装到机架时，请考虑以下主要规则：

- 为保证机架稳定性，从底部开始填充机架。
- IBM 建议在任何 IBM 或非 IBM 机架中安装系统时使用 IBM 批准的升降工具。
- IBM 不支持将服务器节点安装在 29U 位置以上。
- 机架中剩余的空间可用于安装其他系统或外围设备。确保不超过机架的最大允许重量并遵循这些设备的安装规则。
- 在将系统置于服务位置之前，请遵循机架制造商关于机架稳定性的安全说明。

## 1.7.6 有用的机架附加件

本节重点介绍 IBM Power 机架式系统的几种机架附加件解决方案。

### IBM System Storage 7226 Model 1U3 多媒体机箱

IBM System Storage 7226 Model 1U3 多媒体机箱可容纳多达两台磁带驱动器、两个 RDX 可移动磁盘驱动器停靠站，或多达四个 DVD-RAM 驱动器。

IBM System Storage 7226 多媒体机箱支持 LTO Ultrium 和 DAT160 磁带技术、DVD-RAM 和 RDX 可移动存储需求，适用于以下 IBM 系统：

- IBM POWER6 处理器系统
- IBM POWER7 处理器系统
- IBM POWER8® 处理器系统
- IBM POWER9 处理器系统
- IBM Power10 处理器系统

IBM System Storage 7226 多媒体机箱提供广泛的驱动器功能选项，如表 1-23 所列。

表 1-23 7226-1U3 支持的驱动器功能

| Feature code | Description | Status |
|---|---|---|
| 1420 | DVD-RAM SAS Optical Drive | Available |
| 1422 | DVD-RAM Slim SAS Optical Drive | Available |
| 5762 | DVD-RAM USB Optical Drive | Available |
| 5763 | DVD Front USB Port Sled with DVD-RAM USB Drive | Available |
| 5757 | DVD RAM Slim USB Optical Drive | Available |
| 8348 | LTO Ultrium 6 Half High Fibre Tape Drive | Available |
| 8341 | LTO Ultrium 6 Half High SAS Tape Drive | Available |
| 8441 | LTO Ultrium 7 Half High SAS Tape Drive | Available |
| 8546 | LTO Ultrium 8 Half High Fibre Tape Drive | Available |
| EU03 | RDX 3.0 Removable Disk Docking Station | Available |

以下选项可用：

- LTO Ultrium 6 Half-High 2.5 TB SAS and FC Tape Drive：数据传输速率最高可达 320 MBps（假设压缩比为 2.5:1），LTO Ultrium 6 驱动器可读写 LTO Ultrium 6 和 5 介质，并可只读 LTO Ultrium 4。通过使用数据压缩，LTO-6 磁带盒可存储多达 6.25 TB 的数据。
- LTO Ultrium 7 驱动器提供最高 300 MBps 的数据速率，带压缩。它还提供与 Ultrium 7 和 Ultrium 6 介质格式的读写兼容性，以及与 Ultrium 5 介质格式的只读兼容性。通过使用数据压缩，LTO-7 磁带盒可存储多达 15TB 的数据。
- LTO Ultrium 8 驱动器提供最高 300 MBps 的数据速率，带压缩。它还提供与 Ultrium 8 和 Ultrium 7 介质格式的读写兼容性。它与其它 Ultrium 介质格式不兼容读写。通过使用数据压缩，LTO-8 磁带盒可存储多达 30 TB 的数据。

- DVD-RAM：9.4 GB SAS Slim 光驱，具有 SAS 和 USB 接口选项，与大多数标准 DVD 光盘兼容。
- RDX 可移动磁盘驱动器：RDX USB 停靠站在使用相同操作系统时与大多数 RDX 可移动磁盘驱动器盒兼容。7226 提供以下 RDX 可移动驱动器容量选项：
  - 500 GB (#1107)
  - 1.0 TB (#EU01)
  - 2.0 TB (#EU2T)

可移动 RDX 驱动器位于坚固的盒中，插入 RDX 可移动（USB）磁盘停靠站（#1103 或 #EU03）中。RDX 驱动器与停靠站兼容，这些停靠站安装在 POWER8、POWER9 和 Power10 处理器服务器内部，视情况而定。

图 1-12 显示了 IBM System Storage 7226 多媒体机箱。

![](img/7c8373945565ba432037f60bbf8458eb_53_0.png)

IBM System Storage 7226 多媒体机箱提供客户可更换单元（CRU）维护服务，以帮助使新驱动器的安装或更换更高效。其他 7226 组件也设计用于 CRU 维护。

IBM System Storage 7226 多媒体机箱与提供当前级别 AIX、IBM i 和 Linux 操作系统的多数 POWER8、POWER9 和 Power10 处理器系统兼容。

> Unsupported: IBM i does not support 7226 USB devices.

有关支持 IBM System Storage 7226 多媒体机箱的主机软件版本和发布级别的完整列表，请参阅 System Storage Interoperation Center（SSIC）。

> Note: Any of the existing 7216-1U2, 7216-1U3, and 7214-1U2 multimedia drawers are also supported.

### 平板显示器选项

IBM 7316 Model TF5 是一款机架式平板控制台套件，还可配置为托盘向前拉出且显示器向上折叠，提供 HMC 操作员的完整查看和操作功能。

Model TF5 是 Model TF4 的后续产品，提供以下功能：

- 仅占用 19 英寸标准机架 1U（1.75 英寸）空间的纤薄、时尚、轻量级显示器设计
- 18.5 英寸（409.8 mm x 230.4 mm）平板 TFT 显示器，图像真实准确，几乎无失真
- 可将 IBM Travel Keyboard 安装在 7316-TF5 机架键盘托盘中
- 支持 IBM 1x8 Rack Console Switch（#4283）IBM 键盘/视频/鼠标（KVM）开关

#4283 是一款 1x8 控制台开关，安装在 TF5 后面的 1U 空间中。它是基于 CAT5 的开关。它包含八个模拟机架接口（ARI）端口，用于连接 PS/2 或 USB 控制台开关线缆。它支持使用 IBM Conversion Options 开关线缆（#4269）进行服务器的链式连接。该功能提供四根线缆，可将 KVM 开关连接到系统，或可在菊花链场景中使用，将多达 128 个系统连接到单个 KVM 开关。它还支持服务器端 USB 连接

## 1.7.7 原始设备制造商机架

如果机架符合 19 英寸机架的 EIA-310-D 标准，则系统可安装在合适的 OEM 机架中。该标准由电气工业联盟发布。有关更多信息，请参阅 IBM Documentation。

IBM Documentation 提供了一般的机架规格，包括以下信息：

- 机架或机柜必须符合 1992 年 8 月 24 日发布的 19 英寸机架 EIA 标准 EIA-310-D。EIA-310-D 标准规定了内部尺寸，例如机架开口的宽度（机箱宽度）、模块安装法兰的宽度以及安装孔间距。
- 前机架开口的最小宽度必须为 450 mm（17.72 in.），导轨安装孔的中心间距必须为 465 mm 加减 1.6 mm（18.3 in. 加减 0.06 in.）。

图 1-13 是显示机架规格尺寸的俯视图。

![](img/7c8373945565ba432037f60bbf8458eb_54_0.png)

图 1-13 机架规格（俯视图）

- 安装孔之间的垂直距离必须由三孔组组成，从底部到顶部间距分别为 15.9 mm（0.625 in.）、15.9 mm（0.625 in.）和 12.7 mm（0.5 in.）中心距，使得每个三孔组的垂直孔间距为 44.45 mm（1.75 in.）中心距。

