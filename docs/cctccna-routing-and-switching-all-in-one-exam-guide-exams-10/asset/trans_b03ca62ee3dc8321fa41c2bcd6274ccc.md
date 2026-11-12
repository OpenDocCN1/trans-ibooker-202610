

- 您的路由器是否具有所需的最小 RAM 容量？
- 您的路由器是否具有所需的最小闪存容量？

您可以使用 **show version** 命令查看已安装的 RAM 和闪存容量（参见第 6 章）。同时请记得验证您的 IOS 设备是否有足够的空闲闪存来添加其他设备（**show flash** 或 **dir** 命令）；如果没有，您需要删除不需要的文件，为新映像腾出空间。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_409_0.png)

**提示** 在 Cisco 设备上升级 IOS 之前，您应验证新 IOS 的闪存和 RAM 要求：您的 IOS 设备是否支持新映像的要求？您可以使用 **show version** 命令验证 IOS 设备上已安装的 RAM 和闪存容量。您可以使用 **show flash** 或 **dir** 命令验证可用闪存容量。

如果您要升级 IOS 或加载先前保存的 IOS 映像，则需要将 IOS 映像放置在外部服务器上（例如 TFTP 或 FTP），并使用 **copy tftp flash** 命令。您将被提示输入与使用 **copy flash tftp** 命令时所需的相同信息；但是，输入信息后发生的过程不同。输入信息后，IOS 首先验证 TFTP 服务器上是否存在该映像。如果服务器上存在该文件，IOS 随后会询问您是否要擦除闪存。如果闪存空间不足以容纳旧映像和新映像，请回答 **y**。如果回答 **y**，则会擦除并重新编程闪存；在此步骤进行时，屏幕上会显示一行 e。

初始化闪存后，路由器会从 TFTP 服务器获取 IOS 映像。与配置文件的复制操作一样，一串感叹号表示复制成功，而句点表示复制失败。

以下是将 IOS 映像加载到路由器中的示例：

```
Router# copy tftp flash
Address or name of remote host []? 192.168.1.10
Source filename []? c3640-js-mz.120-7
Destination filename [c3640-js-mz.120-7]?
%Warning:There is a file already existing with this name
Do you want to over write? [confirm] y
Accessing tftp://192.168.1.10/c3640-js-mz.120-7...
Erase flash: before copying? [confirm] y
Erasing the flash filesystem will remove all files! Continue?
[confirm] y
Erasing device... eeeeeeeeeeeeeeeeeeeeeeeeeeeeee ...erased
Erase of flash: complete
Loading c3640-js-mz.120-7 from 192.168.1.10 (via FastEthernet0/0):
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
.
.
.
[OK - 6754416/13508608 bytes]

Verifying checksum... OK (0xCAF2)
6754416 bytes copied in 66.968 secs (102339 bytes/sec)
Router#
```

在此示例中，路由器注意到 TFTP 服务器上存在的映像名称与闪存中的名称相同，并验证您是否要覆盖它。另请注意，路由器会提示您擦除闪存——这是 IOS 的默认行为，将删除所有文件。如果您不想完全擦除闪存，请回答 n。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_410_0.png)

**注意** 将文件复制到闪存时，请记住默认行为是在执行复制之前擦除闪存。

路由器将 IOS 映像复制到闪存后，必须重新启动路由器才能使用新映像。但是，默认情况下，Cisco 设备会加载闪存中的第一个有效 IOS 映像。要使用其他映像，您需要使用 **boot system** 命令在配置中定义此项，并将其保存为启动配置文件的一部分。**boot system** 命令会在启动配置文件中按顺序处理——如果其中有一个不再需要，您应该使用相应的 **no boot system** 命令将其删除，后跟旧的 IOS 映像名称。

您可以通过两种方式重新启动路由器：关闭路由器电源后再打开（硬重启），或使用特权 EXEC **reload** 命令（软重启）。以下命令指示 Cisco 设备从闪存中的 IOS 映像 c2900-universalk9-mz.SPA.151-4.M4.bin 启动：

```
boot system flash c2900-universalk9-mz.SPA.151-4.M4.bin
```

如果需要，您也可以使用 **boot system tftp <IOS_filename>** 命令从 TFTP 服务器上的 IOS 启动系统。请注意，启动过程会慢得多，因为设备需要通过网络读取 IOS。

如果在路由器上放置了错误的映像——例如，在 2800 系列路由器上放置 3600 系列映像——路由器将无法启动。您需要进入 ROM 监视器（ROMMON）模式，并执行 TFTP 启动或从 ROM 中的 Mini IOS 启动（如果存在）。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_411_0.png)

**注意** 要从 TFTP 复制，请使用 **copy flash tftp://*URL*** 命令备份 IOS 映像，并使用 **copy tftp://*URL* flash** 命令恢复或升级 IOS。**reload** 命令可重新启动路由器。在升级过程中，如果服务器不可达或 IP 地址或文件名配置错误，您将在 CLI 中收到错误消息。

在升级过程中，如果访问远程服务器时遇到问题，您将收到错误消息。这可能是因为您在复制命令中配置了错误的服务器 IP 地址，或输入了不存在的 IOS 映像名称。以下示例说明了此问题：

```
Router# copy tftp://192.168.101.66/iosimage.bin flash
Destination filename [iosimage.bin]?
Accessing tftp://192.168.101.66/iosimage.bin...
%Error opening tftp://192.168.101.66/iosimage.bin (Timed out)
```

## 使用 tftpdnld

如果 IOS 丢失或损坏，您可以使用 **tftpdnld** 命令选项将 IOS 加载到 Cisco 设备上。当 IOS 未启动且您已进入 ROMMON 模式时，会使用此方法。要使用 **tftpdnld**，需要先加载 ROMMON，并且需要网络连接才能通过 **tftpdnld** 加载 IOS。需要注意关于 ROMMON 模式下 **tftpdnld** 命令选项的两个关键点：

- 在下载指定的 IOS 映像之前，它会擦除闪存中存在的所有文件。
- 传输通过设备上的第一个端口进行。

启动到 ROMMON 模式后，在使用 **tftpdnld** 选项之前，必须先配置用于执行复制的几个环境变量：

```
rommon 1> IP_ADDRESS=12.0.0.50
rommon 2> IP_SUBNET_MASK=255.0.0.0
rommon 3> DEFAULT_GATEWAY=12.0.0.1
rommon 4> TFTP_SERVER=12.0.0.100
rommon 5> TFTP_FILE=c2900-universalk9-mz.SPA.151-4.M4.bin
```

设置环境变量后，可以使用 ROMMON 中的 **tftpdnld** 命令从 TFTP 服务器启动 IOS 下载：

```
rommon 6> tftpdnld
    IP_ADDRESS: 12.0.0.50
    IP_SUBNET_MASK: 255.0.0.0
    DEFAULT_GATEWAY: 12.0.0.1
    TFTP_SERVER: 12.0.0.100
    TFTP_FILE: c2900-universalk9-mz.SPA.151-4.M4.bin
Invoke this command for disaster recovery only.
WARNING: all existing data in all partitions on flash will be lost
Do you wish to continue? y/n: [n]: y
```

下载完成后，您将看到 ROMMON 提示符。然后可以使用 reset 命令重新启动 Cisco 设备。

## 使用 xmodem

要使用 ROMMON tftpdnld 命令，您必须具有网络连接，但如果 Cisco 设备未加载 IOS 且没有网络连接该怎么办？您可以启动到 ROMMON 提示符，然后使用 xmodem 命令，该命令将通过控制台端口将连接到控制台端口的计算机上的 Cisco IOS 复制到您的 Cisco 设备。如果没有网络连接，这非常方便，但使用计算机上的 COM 端口复制 Cisco IOS 也是一种非常慢的技术。

要使用 xmodem 命令，您需要一个允许通过 Xmodem 发送文件的终端程序，例如 Tera Term。您可以使用此开源终端模拟器连接到 Cisco 设备进行管理。

按照以下步骤在 Cisco 交换机上使用 xmodem 命令：

- 1. 通过拔掉交换机电源、按住模式按钮，然后重新插入电源，将交换机启动到 ROMMON 模式。按住模式按钮，直到设备启动到 switch: 提示符。
- 2. 启动 Tera Term 并选择通过串行连接进行连接。使用串行连接连接到连接到 Cisco 设备控制台端口的 COM 端口。
- 3. 如果您正在处理 Cisco 交换机，可能需要使用以下命令初始化闪存：

```
switch: flash_init
```

