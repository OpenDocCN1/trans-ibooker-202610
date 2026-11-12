

![](img/0eafdcebe219ef3756f0e57bcf1eb997_967_0.png)

**提示** 请记住，使用 Cisco 时，可以通过在命令开头添加 **no** 来删除某个功能。因此，如果要删除 enable secret，可以使用 **no enable secret** 命令；如果要删除 enable password，可以使用 **no enable password** 命令。

## 保护控制台和辅助端口

下一步要在设备上配置的密码是控制台密码，当有人连接到设备的控制台端口时需要此附加密码。请记住，管理员甚至在进入特权 EXEC 模式之前就需要提供控制台端口密码，并且还需要提供 enable 密码或 enable secret。要配置控制台密码，请从全局配置模式导航到 line con 0 端口，然后设置密码，如下所示：

```
VAN-R1>enable
VAN-R1#config term
VAN-R1(config)#line con 0
VAN-R1(config-line)#password conpass
```

下一步非常重要。在配置了前面的命令后，如果您连接到设备的控制台端口，您会发现系统不会提示输入密码。这是因为在设置密码后，必须使用 **login** 命令来向 Cisco 设备指示该端口需要身份验证，并提示输入密码：

```
VAN-R1(config-line)#login
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_967_1.png)

**考试提示** 对于 Cisco CCNA 考试，请记住，设置密码后需要使用 **login** 命令才能要求对该端口进行身份验证。

您的 Cisco 路由器在控制台端口旁边可能有一个辅助 (AUX) 端口。辅助端口用作备用线路，可以连接调制解调器，以便您远程访问路由器并执行管理更改。通常，您会使用路由器的 IP 地址通过 SSH 远程访问路由器，但如果接口出现问题且无法从网络另一端连接，您可以将调制解调器连接到路由器的辅助端口，然后拨号进入路由器。请注意，如果未向辅助端口连接调制解调器，它将像另一个控制台端口一样工作，您可以本地连接到它来管理设备。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_968_0.png)

**提示** 这里的关键是，无论您是否向辅助端口连接调制解调器，都需要在该端口上配置密码。我看到许多网络管理员没有在辅助端口上配置密码，因为他们没有意识到有人可能会将其视为第二个控制台端口进行连接。

要在辅助端口上配置密码，请从全局配置模式导航到 AUX 线路，然后使用接下来显示的命令设置密码。请注意，此处也使用了 **login** 命令；如果您忘记此命令，配置的密码将无效，因为设备不会在该端口上要求身份验证。

```
VAN-R1>enable
VAN-R1#config term
VAN-R1(config)#line aux 0
VAN-R1(config-line)#password auxpass
VAN-R1(config-line)#login
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_969_0.png)

**视频 17.02.** 随本书提供的数字资源包含在 Cisco 设备上配置控制台密码和辅助密码的多媒体演示。

## 保护 VTY 端口

如果您想使用 telnet 或 SSH 远程管理设备，则需要在虚拟类型终端 (VTY) 端口上配置密码。要在 VTY 端口上配置密码，请导航到 VTY 线路，然后使用 **password** 命令。此处还必须使用 **login** 命令，以强制设备在连接到这些端口时通过提示输入密码来要求身份验证。

```
VAN-R1>enable
VAN-R1#config term
VAN-R1(config)#line vty 0 15
VAN-R1(config-line)#password telnetpass
VAN-R1(config-line)#login
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_969_1.png)

**注意** 本书前文中我曾提到过，这里再次强调：您应该使用 SSH 而不是 telnet，因为 telnet 会以明文形式从管理员计算机向设备发送所有通信。这包括身份验证流量，因此任何能够截取该通信的人都可以获取您的设备用户名和密码。SSH 会加密从管理员计算机到设备的所有通信，包括身份验证流量。您将在本章后面学习如何在设备上配置 SSH。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_970_0.png)

VIDEO 17.03. 随本书提供的数字资源包含在 Cisco 设备上配置 VTY 密码的多媒体演示。

## 练习 17-1：在 Cisco 路由器上配置密码

在此练习中，您将为 VAN-R1 和 LA-R1 路由器配置控制台、辅助和 VTY 密码。以下插图显示了本章练习所用的网络设置：

![](img/0eafdcebe219ef3756f0e57bcf1eb997_970_1.png)

- 1. 确保已按照网络图连接所有物理组件（如路由器和交换机），然后打开设备电源。
- 2. 将工作站连接到 VAN-R1 的控制台端口。然后启动 PuTTY，选择串行连接，设置正确的 COM 端口，并选择“打开”。
- 3. 在 PuTTY 窗口中按 ENTER 以在 Cisco 设备上获得提示符。
- 4. 进入 VAN-R1 的特权 EXEC 模式并配置以下密码。确保路由器在尝试连接时会提示输入这些密码：
  - 控制台端口密码：labconpass
  - 辅助端口密码：labauxpass
  - VTY 密码：labvtypass
- 5. 配置以下 enable 密码和 secret 值：
  - Enable 密码：labenablepass
  - Enable secret：labenablesecret
- 6. 使用 do write 命令将更改保存到 NVRAM。
- 7. 使用 do show running-config 命令验证配置。
- 8. 在 LA-R1 路由器上配置相同的密码。使用前面的命令作为指南。

```
VAN-R1>enable
VAN-R1#config term
Enter configuration commands, one per line. End with CNTL/Z.
VAN-R1(config)#line con 0
VAN-R1(config-line)#password labconpass
VAN-R1(config-line)#login
VAN-R1(config-line)#line aux 0
VAN-R1(config-line)#password labauxpass
VAN-R1(config-line)#login
VAN-R1(config-line)#line vty 0 15
VAN-R1(config-line)#password labvtypass
VAN-R1(config-line)#login
```

```
VAN-R1(config-line)#exit
VAN-R1(config)#enable password labenablepass
VAN-R1(config)#enable secret labenablesecret
VAN-R1(config)#
```

- 9. 在 VAN-R1 路由器上，退出控制台连接，然后重新连接。根据需要提供密码（请注意，您必须先提供控制台密码，再提供 enable secret 密码）。

请注意，您已配置 enable 密码和 enable secret，但现在进入特权 EXEC 模式时，必须将 enable secret 值作为密码提供。

## 配置用户

在为控制台端口、辅助端口或 VTY 端口设置安全性时，可以通过要求用户名和密码（而不仅仅是密码）来提高安全性。在本节中，您将学习如何在 Cisco 设备上管理用户列表，并了解特权级别。

## 创建用户帐户

您可以创建一个用户帐户名和密码列表，这些信息会本地存储在 Cisco 设备上。为此，请使用 **username** 命令，并如下所示填写用户名和密码参数：

```
VAN-R1>enable
VAN-R1#config term
VAN-R1(config)#username edtet z password edpass
VAN-R1(config)#username glenclarke password glenpass
```

![](img/0eafdcebe219ef3756f0e57bcf1eb997_972_0.png)

**视频 17.04.** 随本书提供的数字资源包含在 Cisco 设备上配置用户帐户的多媒体演示。

接下来，在将使用该列表进行身份验证的每个端口类型上使用 **login local** 命令。在 **login local** 命令中，**login** 意味着强制身份验证（与前面的密码选项类似

