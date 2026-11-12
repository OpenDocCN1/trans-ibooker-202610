

讨论)，但 **本地** 意味着使用本地用户名和密码数据库进行身份验证（而不仅仅是端口密码）。例如，以下命令将配置控制台端口，要求用户使用用户名和密码登录才能本地访问设备：

```
VAN-R1(config)#line con 0
VAN-R1(config-line)#login local
```

您会在VTY端口和辅助端口上使用 **login local** 命令，如果您希望要求使用针对用户名和密码列表验证的身份验证。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_973_0.png)

**考试提示** 对于CCNA考试，请记住，**login local** 命令用于控制台、辅助和VTY端口，以强制使用本地用户名和密码数据库进行身份验证。

## 特权级别

Cisco设备上有16个特权级别，编号为0到15，其中0表示最低特权，15表示最高特权。为了让您了解各级别的功能，当您处于用户EXEC模式时，您的特权级别为1，这就是为什么您无法对设备进行更改。当您进入特权EXEC模式时，您的特权级别将提升到15（完全管理功能）。这就是为什么您必须进入特权EXEC模式才能对设备进行更改。

特权级别的有趣之处在于，您可以将特权级别分配给用户，然后将该特权级别关联到命令，以便用户可以执行该命令。要将特权级别关联到用户，请在 **username** 命令中使用 **privilege** 参数，如下所示：

```
VAN-R1(config)#username adminguy privilege 3 password adminpass
```

如果要更改命令的特权级别，以便用户可以执行该命令，请使用 **privilege exec** 命令为命令分配相同的特权级别：

```
VAN-R1(config)#privilege exec level 3 show running-config
```

在此示例中，我已将 **show running-config** 命令分配给级别3，以便用户需要具有至少级别3的特权级别才能运行该命令。

如果您不确定自己的特权级别，可以使用 **show privilege** 命令：

```
VAN-R1>show privilege
Current privilege level is 1
VAN-R1>enable
VAN-R1#show privilege
Current privilege level is 15
```

请注意，当我处于用户EXEC模式时，我的特权级别为1，但当我进入特权EXEC模式时，我的特权级别会变为15。

## 练习17-2：在Cisco设备上创建用户

在此练习中，您将在Cisco设备的本地用户帐户数据库中配置一些用户帐户，然后修改控制台端口，要求在登录到Cisco设备时使用用户名和密码。

- 1. 在继续此练习之前，请确保已完成上一个实验练习。
- 2. 将工作站连接到控制台端口VAN-R1。然后启动PuTTY，选择串行连接，设置正确的COM端口，并选择打开。
- 3. 在PuTTY窗口中按ENTER键以在Cisco设备上获取提示符。
- 4. 访问VAN-R1的特权EXEC模式并在路由器上配置以下用户名和密码：

用户名：**bob** / 密码：**bobpass**
用户名：**sue** / 密码：**suepass**

- 5. 断开并重新连接到控制台端口。系统是否提示您输入用户名和密码？________
- 6. 使用控制台密码labconpass登录到路由器，并通过提供密钥labenablesecret进入特权EXEC模式。
- 7. 要强制路由器对控制台访问、辅助端口访问和VTY访问使用用户名和密码列表，请导航到每个端口并使用 **login local** 命令：
- 8. 保存配置更改，然后从控制台端口断开连接并重新连接。系统是否要求您输入用户名和密码？________
- 9. 使用用户名bob和密码bobpass登录。
- 10. 为了更多练习，在LA-R1路由器上配置相同的用户名和密码。确保在连接到控制台端口、辅助端口和VTY端口时使用用户名和密码登录。
- 11. 将所有更改保存到两个路由器，并保持运行以备下一个练习使用。

```
VAN-R1(config)#line con 0
VAN-R1(config-line)#login local
VAN-R1(config-line)#line aux 0
VAN-R1(config-line)#login local
VAN-R1(config-line)#line vty 0 15
VAN-R1(config-line)#login local
```

## 加密密码

现在我们已为Cisco设备配置了所有密码，让我们使用 **write** 命令将其保存到启动配置中。让我们使用 **show startup-config** 命令查看配置文件。

```
VAN-R1#show startup-config
Using 1385 bytes
version 15.1
(output cut for briefness)
enable secret 5 $1$mERr$wZMkJsj2RVk4hay2C4T32.
enable password ciscopass
username adminguy privilege 3 password 0 adminpass
line con 0
  password conpass
  login
line aux 0
  password auxpass
  login
line vty 0 4
  password telnetpass
  login
line vty 5 15
  password telnetpass
  login
VAN-R1#
```

查看配置文件时，您可以看到，除了密钥外，所有密码都以明文形式存储。这意味着，如果有人能够物理访问您的路由器，他或她可以查看配置文件并看到您正在使用的密码。从安全角度来看，这是一种非常糟糕的配置！

您可以使用快速简单的命令 **service password-encryption** 加密Cisco配置文件中的所有密码：

```
VAN-R1#config term
VAN-R1(config)#service password-encryption
VAN-R1(config)#do write
```

执行该命令后，它会加密配置文件中的任何明文密码，因此查看配置文件的人不会知道您的密码是什么。执行命令后，您可以查看配置文件以验证密码是否已加密：

```
VAN-R1(config)#do show startup-config
Using 1444 bytes
version 15.1
(output cut for briefness)
service password-encryption
...
```

请注意，在输出中，**service password-encryption** 显示在配置文件中，并且已启用。另外，请注意突出显示的区域中，密码不再在配置中可见。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_977_0.png)

**视频17.05。** 伴随本书的数字资源包含在Cisco设备上配置密码加密的多媒体演示。

在配置文件中加密所有密码的价值在于，如果有人确实获得了对您设备的物理访问权限并能够查看配置文件，他将不知道您的密码是什么。例如，如果有人获得了对我的路由器的物理访问权限，使用密码恢复程序（您在第8章中了解过）绕过了密码，而我未加密密码，他可以查看我的VTY密码，然后稍后使用该密码远程登录我的网络。请记住这一点：加密密码！

![](img/0eafdcebe219ef3756f0e57bcf1eb997_977_1.png)

**考试提示** 对于考试，请记住，**service password-encryption** 命令用于加密配置文件中的所有密码。

## 练习17-3：在Cisco设备上加密密码

在此练习中，您将查看在Cisco设备上配置的密码，然后在Cisco配置文件中对其进行加密。

- 1. 在继续此练习之前，请确保已完成上一个实验练习。
- 2. 将工作站连接到VAN-R1的控制台端口。启动PuTTY，选择串行连接，设置正确的COM端口，并选择打开。
- 3. 在PuTTY窗口中按ENTER键以在Cisco设备上获取提示符。
- 4. 访问VAN-R1的特权EXEC模式，并使用 **show running-config** 命令查看正在运行的配置。记录哪些密码已加密：
- 5. 导航到全局配置并使用 **service password-encryption** 命令加密所有密码。然后查看正在运行的配置。
- 6. 记录哪些密码现在已加密：

|  | 是 | 否 |
|---|---|---|
| 启用密码 |  |  |
| 启用密钥 |  |  |
| 控制台密码 |  |  |
| 辅助密码 |  |  |
| VTY密码 |  |  |
| Bob的密码 |  |  |
| Sue的密码 |  |  |

