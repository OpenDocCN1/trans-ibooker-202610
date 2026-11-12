
![](img/0eafdcebe219ef3756f0e57bcf1eb997_266_0.png)

图 6-9 一个集成的 CSU/DSU

## 背靠背串行连接

你还可以使用串行端口在两台路由器之间创建背靠背连接，这需要一根背靠背串行电缆，如图 6-10 所示。使用背靠背串行电缆连接两台路由器可以在测试环境中模拟 WAN 链路。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_266_1.png)

图 6-10 背靠背串行电缆

![](img/0eafdcebe219ef3756f0e57bcf1eb997_266_2.png)

注意 使用背靠背串行电缆时，请确保电缆一端标记为“DTE”，另一端标记为“DCE”。连接到 DCE 端的路由器需要配置额外的命令，正如你在第 7 章中将学到的。

## 使用 Cisco 硬件

Cisco CCT 认证考试要求你了解将遇到的硬件组件类型以及如何操作它们。

### 了解收发器模块

收发器模块是当今 Cisco 设备的重要特性。Cisco 不为特定类型的接口对 Cisco 设备进行硬连线，而是在部分设备中安装收发器端口，这样你就可以购买带有所需布线端口类型的收发器模块。例如，而不是要求你为千兆接口将 RJ-45 端口插入设备，你的 Cisco 设备可能包含一个收发器端口，这样你就可以购买一个已经在其上包含 RJ-45 端口的收发器模块。这提供了灵活性，因为 Cisco 设备不是为一种特定类型的接口硬编码的。然后，如果你后来决定需要将光纤电缆连接到设备，只需购买该类型的收发器并将其插入 Cisco 设备上的收发器端口即可。

图 6-11 显示了两种用于千兆网络的收发器模块：左侧的 GLC-T 模块使用 RJ-45 连接器连接到非屏蔽双绞线（UTP）布线，以及连接到光纤电缆的 GLC-SX-MM 模块。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_267_0.png)

图 6-11 用于 UTP 的 GLC-T SFP 收发器模块（左侧），以及 GLC-SX-MM 光纤模块

安装收发器模块很简单：将模块滑入 Cisco 设备上的 SFP 端口并用夹具固定，如图 6-12 所示。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_268_0.png)

图 6-12 将收发器安装到 SFP 插槽

一些示例收发器类型、其传输速率以及它们使用的介质类型（连接器类型）列在表 6-2 中。

| OEM 零件编号 | 传输速率 | 介质类型 |
|---|---|---|
| GLC-T | 1 Gbps | UTP |
| GLC-SX-MM | 1 Gbps | 多模光纤 |
| GLC-LH-SM | 1 Gbps | 单模光纤 |
| SFP-10G-SR-S | 10 Gbps | 多模光纤 |
| SFP-10G-LR-S | 10 Gbps | 单模光纤 |
| QSFP-40G-SR-S | 40 Gbps | 多模光纤 |
| QSFP-40G-LR-W | 40 Gbps | 单模光纤 |

表 6-2 示例收发器模块

### 安装和更换硬件

购买 Cisco 设备后，你需要将其安装在位于服务器机房或通信机柜的机架中。以下是有关安装 Cisco 设备的一些常见指南：

- 确保设备安装在机架中时，能够看到设备前面板上的 LED，并且可以访问所有端口。
- 确保电源线能够到达设备安装位置，并确保网络布线远离电源线和荧光灯等电气噪声源。
- 确保 Cisco 设备周围有足够的空间，以便通风口的空气流通不受限制。
- 确保电缆长度不超过其规格规定的最大距离。例如，双绞线电缆的最大距离为 100m（328 英尺）。
- 确保设备周围的温度不超过 113°F（45°C）。

从包装盒取出设备后，安装安装支架，以便设备可以固定到通信机架中。使用十字螺丝刀将安装支架拧到设备侧面，然后将设备固定到机架中。

### 安装和更换模块

Cisco CCT 认证考试要求你知道如何在 Cisco 路由器或交换机上安装和更换模块。以下是可能安装在路由器上的一些常见模块示例（见图 6-13）：

![](img/0eafdcebe219ef3756f0e57bcf1eb997_269_0.png)

图 6-13 Cisco WAN 模块。WIC-1DSU-T1（左）、WIC-1T（中）、WIC 2A/S（右）

- **WIC-1T** 这种带有 DB-60 端口的 WAN 接口卡提供一个串行端口，用于连接到 WAN 环境。
- **WIC 1DSU/T1** 这种带有单个 RJ-45 端口的 WIC 卡是内部 CSU/DSU 卡，用于连接到 T1 线路。
- **WIC 2A/S** 这种带有两个 V.35 串行连接器的 WIC 模块可实现与 WAN 环境的连接。

请记住，每个 WIC 卡金属板上的标签列出了卡类型。每种模块类型都由特定设备支持，并且有最低 IOS 版本要求才能识别该模块。例如，WIC 1DSU/T1 模块在我的 Cisco 1921 路由器中无法识别，因为我的路由器需要该模块的 HWIC 版本。*高速 WAN 接口卡*（HWIC）是 Cisco WIC 卡的新版本，支持更高的速度和更好的吞吐量。

以下是安装或更换 WIC 或 HWIC 模块的方法：

- 1. 通过在 Cisco 网站上搜索模块型号，验证该模块是否受你的设备和操作系统版本支持。
- 2. 使用菲利普斯螺丝刀拧下要从插槽中移除的空白板或现有模块。然后从插槽中取出空白板或现有模块。
- 3. 将新模块滑入插槽，确保其完全推入。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_271_0.png)

- 4. 拧紧模块以将其固定到位。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_271_1.png)

安装新硬件模块后，打开 Cisco 设备的电源，然后使用 **show inventory** 命令验证模块是否被检测到。通过以下代码示例，我们可以获得有关硬件的一些详细信息。首先，注意 **show inventory** 命令显示了 Cisco 设备的型号，仍然是 Cisco 1921 系列设备。此外，注意我们可以看到设备的序列号。

```
Router>enable
Router#show inventory
NAME: "CISCO1921/K9", DESCR: "CISCO1921/K9 chassis, Hw Serial#: FGL1551222Y,
Hw Revision: 1.0"
PID: CISCO1921/K9 , VID: V04 , SN: FGL1551222Y

NAME: "WAN Interface Card - HWIC CSU/DSU on Slot 0 SubSlot 0", DESCR: "WAN Interface Card - HWIC CSU/DSU"
PID: HWIC-1DSU-T1 , VID: V02 , SN: FOC19092U9T

NAME: "WAN Interface Card - HWIC Serial 2T on Slot 0 SubSlot 1", DESCR: "WAN Interface Card - HWIC Serial 2T"
PID: HWIC-2T , VID: V02 , SN: FOC14226262
```

查看此输出时，你还可以看到设备信息下方安装在设备插槽中的 WIC 卡信息。请注意，第一张 WIC 卡是插槽 0/0 中的 HWIC CSU/DSU，型号为 HWIC-1DSU-T1，卡上的序列号为 FOC19092U9T。请注意，第二张卡是位于插槽 0/1 中的 HWIC-2T，序列号为 FOC14226262。

如果运行 **show inventory** 命令后看不到新的硬件模块，可能是因为硬件模块不受设备支持或你未运行最低 IOS 版本。务必查看卡（硬件模块）的文档，以确保了解受支持的设备和最低 IOS 版本。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_272_0.png)

**考试提示** 请记住，对于 Cisco CCT 考试，**show version** 命令和 **show inventory** 命令可用于查看硬件设备的型号信息和序列号。
