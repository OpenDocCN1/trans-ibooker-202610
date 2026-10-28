
如果你没有将路由器（以及交换机，你将在实验8.03中看到）锁在房间里，任何能够物理接触设备的人都可以按照本实验的步骤对其造成严重破坏。“Cisco路由器密码恢复”是对本实验的一种“友好”描述，但另一种（同样准确）说法是“入侵Cisco路由器”。同样，与许多网络安全概念一样，这些步骤既可以用于正当目的（密码恢复），也可以用于恶意目的（恶意入侵）。

## 学习目标

在本实验中，你将无需通过凭据认证即可获得Cisco路由器的最高权限。完成本实验后，你将能够：

- 进入Cisco路由器的特权执行模式
- 理解这样做带来的好处和可能造成的损害

## 实验材料与准备

本实验所需的材料包括：

- 《计算机安全原理：CompTIA Security+ 及进阶》教科书
- 一个可联网的Web浏览器
- 一台Cisco路由器（几乎任何型号均可；此过程在几乎所有型号上都是相同的）
- 一根rollover/console线缆
- 一个USB转串口转换器（如果你的rollover/console线缆是8P8C转串口）

## 开始操作！

在开始本实验之前，你需要完成相关硬件和软件的设置。将路由器连接到PC后，你需要下载并安装软件，以便通过键盘向路由器发送按键操作，并在显示器上接收路由器的输出。

![](img/6e921d997751f0a0f87e5170ac9657a1_248_0.png)

**图8-5 设备管理器**

回到PuTTY中，将串行行端口更改为设备管理器中标识的端口。

在左侧的“类别”窗格中，单击“Serial”。

在“控制本地串行线的选项”下，确认以下设置（最后一个应为唯一的非默认值）：

要连接的串行行：COMx（其中x代表端口号）

```text
Router(config)#enable secret ccna
```

d. 输入 **exit** 返回特权执行模式：

```text
Router(config)#exit
```

e. 输入 **show running-config** 查看路由器RAM中的运行配置（通过running-config文件）：

```text
Router#show running-config
```

你应该会看到一行以 *enable secret* 开头的内容，后面跟着一个数字（表示所使用的具体哈希函数）以及启用密码的哈希值。

f. 输入 **copy running-config startup-config** 将此RAM中的配置文件复制到NVRAM，并存储为startup-config文件：

```text
Router#copy running-config startup-config
```

下次路由器启动时，startup-config文件将被加载到RAM中，成为新的running-config文件。

g. 输入 **reload** 重新启动路由器：

```text
Router#reload
```

h. 在提示符处输入 **enable**：

```text
Router>enable
```

i. 在“Password”提示符处，输入你的名字：

```text
Password: <你的名字>
```

j. 再输入两个错误的密码。输入完这三个错误密码后，你将再次看到用户执行模式提示符。

```text
Password: <你的名字>
Password: <错误密码>
Password: <错误密码>
% Bad secrets
Router>
```

2a–2i

### 步骤 2

此时，你需要恢复密码以重新获得对路由器的访问权限。你将再通过另一种模式来完成这一操作。

running-config文件：

```text
Router#copy startup-config running-config
```

等等！我们不是刚刚通过ROMMON运行配置绕过了这一步吗？是的，确实如此！如果没有该配置，我们就需要为特权执行模式提供密码。现在我们已经“进入”，就可以恢复配置了。我们可以将其与富有创造力的银行劫匪作比较。他们不是抢劫银行后带着钱逃走，而是在深夜无人时把整个银行抬起来，放到自己所在的位置，这样当建筑落下来时，他们就已经和所有钱一起站在金库里面了！

h. 输入 **configure terminal** 进入全局配置模式：

```text
Router#configure terminal
```

i. 在Router(config)#提示符处，输入 **config-register 0x2102** 将配置寄存器恢复为先前的值（在ROMMON模式中修改之前的值），这会使路由器下次启动时从NVRAM加载startup-config作为running-config到RAM中：

```text
Router(config)#config-register 0x2102
```

![](img/6e921d997751f0a0f87e5170ac9657a1_252_0.png)

### 步骤 3

此时，你已经完全控制了路由器，并且它已设置为正常启动。请想一想网络攻击者在这里能做的所有恶意操作。现在是时候重置密码了，因为如果不这样做，你将发现自己再次陷入需要执行密码恢复的境地！

- a. 配置一个新的启用密码。
- b. 将运行配置保存到启动配置中。
- c. 重新加载路由器，并使用刚配置的新密码进入特权执行模式。

![](img/6e921d997751f0a0f87e5170ac9657a1_252_1.png)

## 实验8.03：Cisco交换机密码恢复

在本实验中，你将对交换机而不是路由器执行密码恢复（或者说入侵）操作。

### 学习目标

在本实验中，你将无需有效凭据认证即可获得Cisco交换机的最高权限。完成本实验后，你将能够：

- 进入Cisco交换机的特权执行模式
- 理解这样做带来的好处和可能造成的损害

### 实验材料与准备

本实验所需的材料包括：

- 《计算机安全原理：CompTIA Security+ 及进阶》教科书
- 上一个实验中的PuTTY
- 一台Cisco交换机（几乎任何型号均可；此过程在几乎所有型号上都是相同的）
- 一根rollover/console线缆
- 一个USB转串口转换器（如果你的rollover/console线缆是8P8C转串口）

### 开始操作！

首先按照实验8.02“开始操作！”部分中的相同设置进行操作（通过console连接到交换机并运行PuTTY），然后完成该实验的所有“步骤1”（设置启用密码并在提示时无法提供正确密码）。本实验的步骤1假定你已经在上一个实验中按上述步骤操作过。如果你是第一次使用该设备且设备已恢复出厂设置，请本实验的步骤2f开始，然后再返回步骤1。

### 步骤 1

拔掉交换机的电源线。有趣的是，Cisco交换机不像Cisco路由器那样有电源按钮。

这可能看起来有点奇怪，但要让Cisco交换机密码恢复顺利进行，你需要在将电源线重新插回交换机的同时，按住交换机前面板最左侧的“Mode”按钮。和上一个实验一样，每条命令后按回车键。

2a–2h

### 步骤 2

正如你刚刚完成的硬件步骤一样，你现在要进行的软件过程（在交换机上执行密码恢复）与你在上一个实验中对路由器执行的密码恢复过程有很大不同。

a. 此时，交换机应启动进入switch:模式。在switch:提示符处输入 `flash_init` 以初始化闪存文件系统：

```text
switch: flash_init
```

b. 在switch:提示符处，输入 `load_helper` 以加载并初始化引导帮助程序镜像，这些镜像可以扩展或修补引导加载程序的功能：

```text
switch: load_helper
```

c. 要查看闪存内容，在switch:提示符处输入 `dir flash:`：

```text
switch: dir flash:
```

d. 在switch:提示符处，输入 `rename flash:config.text flash:config.old` 以在交换机启动期间隐藏其配置：

```text
switch: rename flash:config.text flash:config.old
```

Cisco交换机将配置保存在两个内容相同的文件中。NVRAM中的startup-config文件在启动过程中由交换机使用，就像路由器一样。然而，交换机在闪存中还有另一个文件config.text，本实验正是利用该文件进行密码恢复。startup-config文件与config.text文件相关联，所以这一步是必需的。当你将config.text重命名为config.old时，交换机就无法将startup-config文件映射到config.text（因为它已不存在）。作为

