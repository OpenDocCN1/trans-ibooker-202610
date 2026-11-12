
The **username** command specifies the name and password for the user. The main difference between the **secret** and **password** parameters is that the **secret** parameter tells IOS to encrypt the password with an MD5 hash, and the **password** parameter doesn’t (the password is stored in clear text). This is true of the line subconfiguration mode **password** command: it also is stored in clear text.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_346_0.png)

**注意** The **login local** command can be used on any of the lines on Cisco device to tell the device to perform authentication using the local username and password database. You can use this on console, auxiliary, and VTY ports to force the Cisco device to require a username *和* password when someone connects.

## 特权 EXEC 密码保护

Along with protecting access to the lines on an IOS device, you can also control access to privileged EXEC mode by assigning a password to it. Two configuration options are shown here:

```
Switch(config)# enable password Privilege_EXEC_password
```

or

```
Switch(config)# enable secret Privilege_EXEC_password
```

Both of these commands configure the privileged EXEC password. The main difference between the two, as with the **username** command, is that the **secret** parameter encrypts the privileged EXEC password and the **password** parameter doesn’t. If you configure both of these commands, the password configured with the **enable secret** command always takes precedence over the password configured with the **enable password** command. The **enable password** command is still supported by Cisco for backward-compatibility purposes.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_347_0.png)

**考试提示** For the exam, you’ll need to remember that without user EXEC *和* privileged EXEC authentication (passwords), you will not be able to access IOS device remotely via telnet or SSH, by default.

## 密码加密

Passwords that are not encrypted can be encrypted by using the **service password-encryption** Global Configuration mode command. However, the **enable secret** command’s encryption is *非常* stronger than using the **service password-encryption** command to do the encryption.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_347_1.png)

**考试提示** Remember for the CCT and CCNA exam that you use the **enable password** or **enable secret** command to secure privileged EXEC access. Remember that the **enable secret** command creates an encrypted secret value that takes precedence over the password if both are configured. With VTY access, you should also configure a password on the VTY ports and you must also specify the **login** command to have the device prompt for the password. Use the **service password-encryption** command to encrypt all clear-text passwords within your configuration: this includes the **password** command for the VTYs and other lines, the **enable password** command, and other commands that have a clear-text password.

## 练习 7-2：配置密码

In this exercise you will continue with the configuration of the previous exercise and add passwords to the two routers. You will configure a console password, an auxiliary password (if you have an aux port), and VTY port passwords. See Figure 7-3.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_348_0.png)

图 7-3 练习的网络拓扑

- 1. 确保您已通过控制台连接到 VAN-R1。
- 2. 要在 VAN-R1 上配置控制台密码 conpass，请输入以下命令：
- 3. 要在 VAN-R1 上配置 aux 密码 auxpass，请输入以下命令：

```
VAN-R1>enable
VAN-R1#config term
VAN-R1(config)#line con 0
VAN-R1(config-line)#password conpass
VAN-R1(config-line)#login
```

```
VAN-R1(config-line)#line aux 0
VAN-R1(config-line)#password auxpass
VAN-R1(config-line)#login
```

4. 通过配置 VTY 端口来配置 telnet 密码 telnetpass，请输入以下命令：

```
VAN-R1(config-line)#line vty 0 4
VAN-R1(config-line)#password telnetpass
VAN-R1(config-line)#login
```

5. 创建 enable 密码 enablepass：

```
VAN-R1(config-line)#exit
VAN-R1(config)#enable password enablepass
```

6. 创建 enable secret 为 enablesecret：

```
VAN-R1(config)#enable secret enablesecret
VAN-R1(config)#exit
VAN-R1#
```

7. 通过输入以下命令将配置更改保存到 NVRAM：

```
VAN-R1# copy running-config startup-config
```

请注意，您也可以使用 write 命令，它不会提示您确认保存操作的目标。

8. 退出并在您连接到控制台端口时验证每个密码；然后进入特权 EXEC 模式。请注意，您输入的第一个密码将是控制台密码，您输入的第二个密码是 secret 密码，因为它优先于 enable 密码。只有在未设置 enable secret 时才会使用 enable 密码。

9. 现在在路由器 LA-R1 上配置密码。确保您已通过控制台连接到 LA-R1。

10. 在 LA-R1 上配置控制台密码 LAconpass。

11. 在 LA-R1 上配置 aux 密码 LAauxpass。

12. 在 LA-R1 上配置 VTY 密码 LAtelnetpass。

13. 创建 enable 密码 LAenablepass。

- 14. 创建 enable secret 为 *LAenablesecret*。
- 15. 使用 **write** 命令将配置更改保存到 NVRAM。
- 16. 退出并在您连接到控制台端口时验证每个密码；然后进入特权 EXEC 模式。请注意，您输入的第一个密码将是控制台密码，您输入的第二个密码是 secret 密码，因为它优先于 enable 密码。只有在未设置 enable secret 时才会使用 enable 密码。
- 17. 保持设备运行以进行下一次练习。

![](img/0eafdcebe219ef3756f0e57bcf1eb997_350_0.png)

**视频 7.07。** The digital resources that accompany this book contain a multimedia demonstration of encrypting passwords within the configuration file.

## 配置横幅

Banners provide a way for you to display messages to administrators when they connect to your Cisco device. There are different types of banners, such as message-of-the-day (MOTD) and login banners, which differ in the timing of when they appear. The following is a list of some of the common types of banners that can be set:

- **MOTD banner** The MOTD banner appears before the administrator is asked to log in and is used to display a temporary message that may change from day to day.
- **Login banner** The login banner displays before the administrator logs in, but after the MOTD banner appears, and is used to display a more permanent message to the administrator.
- **Exec banner** The exec banner is used to display a message after the administrator authenticates to the system and after he enters user EXEC mode.

要查看 Cisco 设备上支持的横幅列表，请使用 **banner** 命令的帮助功能：

```
Switch(config)#banner ?
  LINE            c banner-text c, where 'c' is a delimiting character
  exec            Set EXEC process creation banner
  incoming        Set incoming terminal line banner
  login           Set login banner
  motd            Set Message of the Day banner
  prompt-timeout  Set Message for login authentication timeout
  slip-ppp        Set Message for SLIP/PPP
```

The setup of each of the banners is similar, so let’s take a look at setting a few of the common ones.

### MOTD 横幅

You can set up a MOTD banner on your IOS device that will display a message *之前* the administrator is asked to log in to the device (before the console, aux, or VTY passwords). Use the **banner motd** command to create the MOTD banner:

```
IOS(config)# banner motd delimiting_char your_banner end_with_delimiting_char
IOS(config)#
```

After the **banner motd** command, you must enter the delimiting character, which is used to signify the beginning and ending of the banner—it cannot appear in the actual banner. After you enter the delimiting character, when the IOS parser sees this character later in your message text, IOS terminates the banner and returns you to the CLI prompt. One nice feature of the banner is that when you press the ENTER key, it doesn’t terminate the banner, so you can have banners that span multiple lines of text. Whenever you have completed your banner, type the delimiting character to end the command.

以下是设置 MOTD 横幅的示例：
