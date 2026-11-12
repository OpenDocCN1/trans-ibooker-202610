
在第 8 章中进行了讨论。）在路由器和交换机上，您使用 **hostname** 命令来更改设备的名称。以下是将名称 *Router* 更改为名称 *Perimeter* 的简单示例：

```
Router(config)# hostname Perimeter
Perimeter(config)#
```

首先，注意设备的新的名称（*Perimeter*）放在 **hostname** 命令之后。一旦按下 ENTER，新的 CLI 提示符会立即更改——它包含设备的新的名称。

要撤消更改或否定 Cisco 设备上的命令，您可以在命令前加上 **no** 参数。例如，要将主机名改回出厂默认值，请使用以下命令：

```
Perimeter(config)# no hostname Perimeter
Router(config)#
```

在某些情况下，否定命令时不必包含命令的参数。在前面的示例中，您可以轻松输入 **no hostname** 来完成更改，但在其他情况下，您必须输入 **no** 并跟上整个配置命令。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_323_0.png)

**EXAM TIP** 对于 Cisco CCT 认证，请记住 **hostname** 命令为 IOS 设备分配一个本地有意义的标识符。在某些考试模拟问题中，您可能需要在配置中配置主机名。在考试中，名称区分大小写！

![](img/0eafdcebe219ef3756f0e57bcf1eb997_323_1.png)

VIDEO 7.03. 本书附带的数字资源包含在 Cisco 设备上配置主机名的多媒体演示。

## 子配置模式

IOS 设备上的某些配置命令会进入特定的子配置模式，通常称为 *subcommand mode*。表 7-2 显示了您在 IOS 设备上可能会看到的几个子配置模式。

| IOS Prompt | Subconfiguration Mode |
|---|---|
| (config-if)# | 接口子配置模式 |
| (config-subif)# | 子接口子配置模式 |
| (config-line)# | 线路子配置模式 |
| (config-controller)# | 控制器子配置模式 |
| (config-router)# | IP 路由协议子配置模式 |

Table 7-2 Subconfiguration Modes on IOS Devices

![](img/0eafdcebe219ef3756f0e57bcf1eb997_324_0.png)

**NOTE** 并非所有子配置模式都受所有 IOS 设备支持。当您在子配置模式下工作时，您输入的命令仅影响路由器或交换机的特定组件。

要离开子配置模式并返回到全局配置模式，请使用 **exit** 命令。使用 **end** 命令或按 CTRL-Z 始终会带您返回特权 EXEC 模式，无论您当前正在哪个配置模式下工作。以下是从子配置模式到全局配置模式的示例：

```
IOS(config)# line con 0
IOS(config-line)# exit
IOS(config)#
```

在许多情况下，当您在子配置模式下工作并输入全局配置模式命令时，IOS 会执行它并且模式会更改为全局配置模式，如下所示：

```
IOS(config)# line con 0
IOS(config-line)# hostname 2960
2960(config)#
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_325_0.png)

**EXAM TIP** 请记住，许多全局配置模式命令会影响整个 IOS 设备，例如 hostname 命令：您只需输入一次命令，它就会影响整个设备。子配置模式命令仅影响 IOS 设备的特定组件，例如接口、线路或路由协议，仅举几例。使用 exit 命令返回一个配置级别。使用 end 命令或按 CTRL-Z 退出配置模式。

请注意，当在线路子配置模式下执行 hostname 命令时，交换机不仅更改了模式，还更改了其名称。有时，您会从路由器收到无效命令响应；在这种情况下，您必须先退出子配置模式，然后在全局配置中执行命令。

## 线路配置

IOS 设备上的线路提供对 EXEC shell 的访问；有时这称为字符模式访问，因为它涉及 CLI。表 7-3 显示了受支持的线路类型。line 命令用于在配置模式下引用线路。您必须指定要配置的线路类型以及线路号，其中线路号从 0 开始递增。请注意，控制台和辅助线路只有一个线路号：0。配置 VTY 和 TTY 时，您可以指定数字范围。执行 **line** 命令会进入子配置模式，您在其中输入的命令会影响该特定线路，或者使用 VTY 和电传终端（TTY）时可能影响多条线路。

| Line Type | Description | Command Nomenclature |
|---|---|---|
| Console | 提供初始 CLI 访问的物理端口；当无法远程访问 IOS 设备时用于故障排除。设备上仅支持一个。 | line con 0 |
| Auxiliary | 通常用于拨号进入 IOS 设备的备份物理端口；并非所有设备都有辅助端口，每个设备仅支持一个。 | line aux 0 |
| VTY | 允许通过 telnet 和 SSH 等远程访问连接访问 CLI 的逻辑端口。IOS 设备上支持多个 VTY，数量取决于设备、型号和软件版本。 | line vty 0 4 |
| TTY | 用于连接调制解调器以进行终端服务或访问其他设备的控制台端口的物理端口。这些端口仅在路由器上找到，支持的数量取决于路由器型号和安装的异步卡。 | line tty 0 31 |

Table 7-3 IOS Line Types

### 更改非活动超时

默认情况下，IOS 设备在 10 分钟不活动后会自动注销您。您可以使用 **exec-timeout** 线路子配置模式命令更改此设置：

```
IOS(config)# line line_type line_#
IOS(config-line)# exec-timeout minutes seconds
```

请记住，这仅更改您指定的线路（或线路）的配置——其他线路仍将使用默认空闲时间。如果您不希望特定线路超时，您可以将分钟和秒值设置为 **0**，如下所示：

```
IOS(config)# line console 0
IOS(config-line)# exec-timeout 0 0
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_327_0.png)

**CAUTION** 在生产环境中禁用线路的超时是不推荐的，因为它会造成安全问题。但是，出于培训目的，例如学习 CCNA 考试，这是可以的。

如果您想完全禁用线路的使用，请使用以下配置：

```
IOS(config)# line line_type line_#
IOS(config-line)# no exec
```

要验证线路配置，请使用 **show line** 命令：

```
IOS# show line con 0
  Tty Typ  Tx/Rx A Modem Roty AccO AccI Uses Noise Overruns
* 0 CTY       -   -    -    -    -    0    0    0/0

Line 0, Location: "", Type: ""
Length: 24 lines, Width: 80 columns
Status: Ready, Active
Capabilities: none
Modem state: Ready
Special Chars: Escape Hold Stop Start Disconnect Activation
             ^^x   none  -    -      none
Timeouts: Idle EXEC Idle Session Modem Answer Session Dispatch
          never    never                     none    not set
Session limit is not set.
Time since activation: 0:04:49
Editing is enabled.
History is enabled, history size is 10.
Full user help is disabled
Allowed transports are pad telnet mop.  Preferred is telnet.
No output characters are padded
No special data dispatching characters
```

在此示例中，请注意超时值设置为 **never**。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_328_0.png)

**TIP** 请注意，**show** 命令不是从配置模式输入的。要在配置模式中输入用户和特权 EXEC 模式命令，请在命令前加上 **do** 字，如下所示：**do show line con 0**。这可以避免您每次都要退出以使用 **show** 命令并查看更改的效果。节省大量时间！

### 限制 VTY 协议访问

默认情况下，允许 VTY 的所有访问方法，包括 telnet 和 SSH（使用 telnet 和 SSH 在第 8 章中讨论）。控制 VTY 上允许的连接类型是通过 **transport input**
