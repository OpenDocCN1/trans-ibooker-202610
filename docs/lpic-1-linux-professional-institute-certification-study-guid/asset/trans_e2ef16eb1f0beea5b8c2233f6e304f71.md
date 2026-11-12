

## 使用设备

Linux 提供了大量命令行工具，可用于使用连接到系统的设备，也可在遇到问题时监控和排查设备故障。本节将介绍使用 Linux 设备时需要了解的一些常用工具。

### 查找设备

对于新的 Linux 管理员来说，首先要完成的任务之一就是查找 Linux 系统上安装的不同设备。幸运的是，有几个命令行工具可以提供帮助。

`lssdev` 命令行命令显示 Linux 系统上安装的硬件设备的信息。它从 `/proc/interrupts`、`/proc/ioports` 和 `/proc/dma` 虚拟文件中检索信息，并将它们合并在一个输出中，如清单 3.5 所示。

### **清单 3.5:** lssdev 命令的输出

```
$ sudo lssdev
Device            DMA   IRQ   I/O Ports
...
acpi                9
ACPI                4000-4003 4004-4005 4008-400b 4020-4021
ahci                d240-d247 d248-d24b d250-d257 d258-d25b
ata_piix    14 15   0170-0177 01f0-01f7 0376-0376 03f6-03f6
cascade     4
dma                 0080-008f
dma1                0000-001f
dma2                00c0-00df
e1000               d010-d017
enp0s3      19
fpu                 00f0-00ff
i8042       1 12
Intel               d100-d1ff   d200-d23f
keyboard            0060-0060   0064-0064
ohci_hcd:usb1   22
PCI                 0000-0cf7 0cf8-0cff 0d00-ffff
pic1                0020-0021
pic2                00a0-00a1
piix4_smbus         4100-4108
rtc0        8       0070-0071
rtc_cmos            0070-0071
snd_intel8x0        21
timer       0
timer0              0040-0043
timer1           0050-0053
vboxguest        20
vboxvideo        18
vga+             03c0-03df
$
```

这为您提供了一个查看系统上运行设备的所有重要信息的地方，从而轻松找出可能引起问题的任何冲突。

lsblk 命令显示 Linux 系统上安装的块设备的信息。默认情况下，lsblk 命令显示所有块设备，如清单 3.6 所示。

### 清单 3.6: lsblk 命令的输出

```
$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0       7:0    0  34.6M  1 loop /snap/gtk-common-themes/818
loop1       7:1    0  2.2M   1 loop /snap/gnome-calculator/222
loop2       7:2    0  34.8M   1 loop /snap/gtk-common-themes/1122
loop3       7:3    0 169.4M   1 loop /snap/gimp/113
loop4       7:4    0  2.3M   1 loop /snap/gnome-calculator/238
loop5       7:5    0   13M   1 loop /snap/gnome-characters/117
loop6       7:6    0  34.2M   1 loop /snap/gtk-common-themes/808
loop7       7:7    0  89.5M   1 loop /snap/core/6130
loop8       7:8    0  14.5M   1 loop /snap/gnome-logs/45
loop9       7:9    0  53.7M   1 loop /snap/core18/719
loop10      7:10   0   91M   1 loop /snap/core/6350
loop11      7:11   0 140.7M   1 loop /snap/gnome-3-26-1604/74
loop12      7:12   0  53.7M   1 loop /snap/core18/594
loop13       7:13   0 169.4M   1 loop /snap/gimp/105
loop14       7:14   0  14.5M   1 loop /snap/gnome-logs/43
loop15       7:15   0   13M   1 loop /snap/gnome-characters/124
loop16       7:16   0   13M   1 loop /snap/gnome-characters/139
loop17       7:17   0   14.5M   1 loop /snap/gnome-logs/40
loop18       7:18   0 140.7M   1 loop /snap/gnome-3-26-1604/78
loop19       7:19   0   3.7M   1 loop /snap/gnome-system-monitor/57
loop20      7:20   0   2.3M   1 loop /snap/gnome-calculator/260
loop21      7:21   0   3.7M   1 loop /snap/gnome-system-monitor/54
loop22      7:22   0 169.4M   1 loop /snap/gimp/110
loop23      7:23   0  53.7M   1 loop /snap/core18/677
loop24      7:24   0   91M   1 loop /snap/core/6405
loop25      7:25   0 140.9M   1 loop /snap/gnome-3-26-1604/70
loop26      7:26   0   3.7M   1 loop /snap/gnome-system-monitor/51
sda        8:0    0   10G  0 disk
├─sda1     8:1    0   10G  0 part
│ └─ubuntu--vg-root 253:0  0    9G  0 lvm  /
│ └─ubuntu--vg-swap_1 253:1  0  976M  0 lvm  [SWAP]
sr0       11:0    1 1024M  0 rom
$
```

请注意，在清单 3.6 的末尾，lsblk 命令还指示相关块，例如与设备映射的 LVM 卷和关联的物理硬盘。您可以通过添加命令行选项来修改 lsblk 输出，以查看其他信息或仅显示信息子集。-S 选项仅显示系统上 SCSI 块设备的信息：

```
$ lsblk -S
NAME   HCTL       TYPE VENDOR   MODEL             REV TRAN
sda    2:0:0:0    disk ATA      VBOX HARDDISK     1.0 sata
sr0    1:0:0:0    rom  VBOX     CD-ROM            1.0 ata
$
```

这是查看系统上安装的不同 SCSI 驱动器的快速方法。

## 使用 PCI 卡

lspci 命令允许您查看当前在 Linux 系统上安装和识别的 PCI 和 PCIe 卡。可以使用许多命令行选项与 lspci 命令一起使用，以显示系统上安装的 PCI 和 PCIe 卡的信息。表 3.1 显示了最常见的选项。

### 表 3.1 lspci 命令行选项

| 选项 | 说明 |
| :--- | :--- |
| -A | 定义访问 PCI 信息的方法 |
| -b | 从卡的角度显示连接信息 |
| -k | 显示每个已安装 PCI 卡的内核驱动程序模块 |
| -m | 以机器可读格式显示信息 |
| -n | 以数字而不是文本显示供应商和设备信息 |
| -q | 查询集中式 PCI 数据库，获取有关已安装 PCI 卡的信息 |
| -t | 显示树形图，显示卡与总线之间的连接 |
| -v | 显示关于卡的其他信息（详细信息） |
| -x | 显示卡信息的十六进制输出转储 |

不带任何选项的 lspci 命令输出显示连接到系统的所有设备，如清单 3.7 所示。

### 清单 3.7: 使用 lspci 命令

```
$ lspci
00:00.0 Host bridge: Intel Corporation 440FX - 82441FX PMC [Natoma] (rev 02)
00:01.0 ISA bridge: Intel Corporation 82371SB PIIX3 ISA [Natoma/Triton II]
00:01.1 IDE interface: Intel Corporation 82371AB/EB/MB PIIX4 IDE (rev 01)
00:02.0 VGA compatible controller: InnoTek Systemberatung GmbH VirtualBox Graphics Adapter
00:03.0 Ethernet controller: Intel Corporation 82540EM Gigabit Ethernet Controller (rev 02)
00:04.0 System peripheral: InnoTek Systemberatung GmbH VirtualBox Guest Service
00:05.0 Multimedia audio controller: Intel Corporation 82801AA AC'97 Audio Controller (rev 01)
00:06.0 USB controller: Apple Inc. KeyLargo/Intrepid USB
00:07.0 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 08)
00:0d.0 SATA controller: Intel Corporation 82801HM/HEM (ICH8M/ICH8M-E) SATA Controller [AHCI mode] (rev 02)
$
```

您可以使用 lspci 命令的输出来排查 PCI 卡问题，例如当卡未被 Linux 系统识别时。

## 使用 USB 设备

您可以使用 lsusb 命令查看连接到 Linux 系统的 USB 设备的基本信息。表 3.2 显示了该命令可用的选项。

### 表 3.2 lsusb 命令选项

| 选项 | 说明 |
| :--- | :--- |
| -d | 仅显示指定供应商 ID 的设备 |
| -D | 仅显示具有指定设备文件的设备的信息 |
| -s | 仅显示使用指定总线的设备的信息 |
| -t | 以树形格式显示信息，显示相关设备 |
| -v | 显示有关设备的其他信息（详细模式） |
| -V | 显示 lsusb 程序的版本 |

### 清单 3.8: lsusb 输出

```
$ lsusb
Bus 001 Device 003: ID abcd:1234 Unknown
Bus 001 Device 002: ID 80ee:0021 VirtualBox USB Tablet
Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
$
```

大多数系统都集成了标准 USB 集线器，用于将多个 USB 设备连接到 USB 控制器。幸运的是，市场上的 USB 集线器只有少数几种，因此所有 Linux 发行版都包含与这些 USB 集线器通信所需的设备驱动程序。这保证了您的 Linux 系统至少会在连接 USB 设备时检测到它。

## 硬件模块

Linux 内核需要设备驱动程序才能与安装在 Linux 系统上的硬件设备进行通信。但是，将所有已知硬件设备的设备驱动程序编译到内核中会生成一个非常大的内核二进制文件。

为了避免这种情况，Linux 内核使用内核模块，这些模块是单个硬件驱动程序文件，可以在运行时链接到内核。这样，系统可以仅链接系统中存在的硬件所需的模块。

如果内核配置为加载硬件设备模块，则单个模块文件也必须可用于系统。如果您正在编译新的 Linux 内核，还需要连同新内核一起编译任何硬件模块。

模块文件可以作为需要编译的源代码分发，也可以作为准备动态链接到主内核二进制程序的 Linux 系统上的二进制对象文件分发。如果模块文件作为源代码文件分发，则必须对其进行编译以创建二进制对象文件。.ko 文件扩展名用于标识模块对象文件。

存储模块对象文件的标准位置在 /lib/modules 目录中。这是 Linux 模块实用程序（例如 insmod 和 modprobe）默认查找模块对象库文件的位置。

> 某些硬件供应商仅为其硬件模块发布模块对象文件，而不发布源代码。这有助于他们保护其硬件的专有功能，同时仍然允许其硬件产品在开源环境中使用。尽管这种安排违反了开源代码的核心思想，但它已成为试图保护其产品秘密的公司与希望在其系统上使用最新硬件的 Linux 爱好者之间的共同点。

