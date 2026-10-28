

book. 是时候搭建基础设施了！

## Step 1 Download and install VMware Workstation Player.
- a. VMware 的网站一直在变化，所以在 Google 上搜索 **Download VMware Workstation Player**。Google 顶部结果应该会带你到 www.vmware.com 上的相应页面。
- b. 在你的 Windows 10 机器上下载并安装 Windows 版本。
- c. 接受条款，并可自行接受或更改默认选择。

## Step 2 Download Kali Linux and prepare it to be installed in VMware Workstation Player.
- a. Kali Linux 网站一直在变化，所以在 Google 上搜索 **Download Kali Linux**。Google 顶部结果应该会带你到 www.kali.org 上的相应页面。
- b. 下载并安装 Kali Linux 64 位安装程序 ISO 文件（不是 torrent）。在出版时，它位于 Bare Metal 类别中。保留该 ISO，因为你在第 5 章的一个涉及哈希的实验练习中会用到它。
- c. 运行 VMware Workstation Player。
- d. 点击 Create A New Virtual Machine。
- e. 浏览并选择 Kali ISO 文件，它将在你的 Downloads 文件夹中，然后点击 Next。
- f. 点击 Linux 单选按钮，并在 Version 下拉菜单中选择 Debian 10.x 64-Bit。然后点击 Next。
- g. 在文本框中，将 Debian 10.x 64-Bit 的名称更改为 Kali（或其它名称）。然后点击 Next。
- h. 保留默认的最大磁盘大小，但选择 Store Virtual Disk As A Single File 单选按钮而不是默认选择。然后点击 Next。
- i. 点击 Customize Hardware。然后在左侧选择 Network Adapter。选择 Bridged: Connected Directly To The Physical Network 单选按钮并选中 Replicate Physical Network Connection State 复选框。
- j. 可通过点击 Memory 并增加分配的内存来增加 VM 的 RAM。
- k. 点击 Close 然后点击 Finish。

## Step 3 Install and log in to the Kali Linux operating system.
- a. 在上一步点击 Finish 后，活动屏幕应已在 VMware Workstation Player 窗口中自动选中 VM。现在你已准备好安装 Kali Linux 操作系统。在右下角点击 Play Virtual Machine。选择 Graphical Install 然后按 ENTER。
- b. 接受或更改默认选择 Language（English）、Location（United States）和 Keyboard（American English），每次选择后点击 Continue。
- c. 在 Configure The Network 屏幕中，保留或更改默认 Hostname（kali）然后点击 Continue。
- d. 在下一屏幕中，将 Domain Name: 字段留空并点击 Continue。
- e. 输入你的 Full Name 和账户的 Username，输入后点击 Continue。
- f. 输入密码，验证它，然后点击 Continue。
- g. 选择时区并点击 Continue。
- h. 在 Partition Disks 屏幕中，保留默认的 Guided – Use Entire Disk 选择并点击 Continue。
- i. 在下一屏幕中保留磁盘分区的默认选择并点击 Continue。
- j. 在下一屏幕中保留默认的 All Files In One Partition（推荐给新用户）选择并点击 Continue。
- k. 保留 Finish Partitioning And Write Changes To Disk 选择并点击 Continue。

- f. 浏览并选择 Windows 10 Enterprise Edition ISO 文件然后点击 Next。
- g. 将 Windows Product Key 字段留空，输入你的 Full Name 和 Password，确认密码，然后点击 Next。
- h. 当你看到关于未输入产品密钥的警告时，点击 Yes。保留或更改 Virtual Machine Name 然后点击 Next。
- i. 保留默认的 Maximum Disk Size，但选择 Store Virtual Disk As A Single File 单选按钮而不是默认选择。然后点击 Next。
- j. 点击 Customize Hardware。然后在左侧选择 Network Adapter 并选择 Bridged: Connected Directly To The Physical Network 单选按钮和 Replicate Physical Network Connection State 复选框。
- k. 可通过点击 Memory 并增加分配的内存来增加 VM 的 RAM。点击 Close 然后点击 Finish。

## Step 5 Install and sign in to the Windows 10 operating system.
- a. 在上一步点击 Finish 后，Windows 10 操作系统的安装会立即开始。一旦开始，选择 Player | Removable Devices | Floppy | Disconnect（见 Figure 1-3）以断开虚拟软盘驱动器，该驱动器过去曾导致问题。如果不这样做，你可能会看到消息：“Microsoft cannot find the Microsoft Software License Terms. Make sure the installation sources are valid and restart the installation。”

you may see a message: “Microsoft cannot find the Microsoft Software License Terms. Make sure the installation sources are valid and restart the installation.”

- b. 保留默认值或更改 Language To Install、Time And Currency Format 和 Keyboard Or Input Method。然后点击 Next。
- c. 点击 Install Now。然后选择 Windows Server 2019 Standard Evaluation (Desktop Experience) 并点击 Next。
- d. 选中 I Accept The License Terms 复选框并点击 Next。
- e. 选择 Custom: Install Windows Only (Advanced)，选择 Drive 0 Unallocated Space，然后点击 Next。Windows Server 2019 现在正在安装。
- f. 在 Customize Settings 屏幕中，输入内置管理员账户的 Password 然后重新输入。
- g. 在自动重启后，按 CONTROL-ALT-INSERT（此序列用于向 VM 发送 CONTROL-ALT-DELETE），然后使用你刚刚为 Administrator 账户配置的密码登录。或者，点击 VMware Workstation Player 中的 Player 菜单项（左上角）并选择 Send CTRL+ALT+DEL。
- h. 你将看到 Windows Server 2019 桌面。Server Manager 工具将默认打开。如果提示，请对 Networks 问题做出选择。
- i. 在任务栏中，点击 Windows Start 按钮或点击 Windows 搜索框，输入 cmd，然后点击 Command Prompt。在提示符下，输入 slmgr -dlv 然后按 ENTER。接下来你将看到 Windows Script Host 窗口，其中包含大量信息，包括有关试用许可期的信息。非常重要的是 Remaining Windows Rearm Count 值，即许可期可以延长次数。

本书撰写时，Remaining Windows Rearm Count 为 6，因此 90 天试用期可延长至总共 630 天（90 + 540）！

- j. 如果你想延长试用许可期，在任务栏中点击 Windows Start 按钮或点击 Windows

virtual machine (VM)

virtual private network (VPN)

- 1. Networks of ___________ 来自网络安全公司的信息收集用于网络威胁地图的网络威胁信息。
- 2. LinkedIn 是一个面向 ___________ 的社交媒体网络，涵盖各种行业和领域。
- 3. 你绝不应在不使用 __________ 的情况下使用公共 Wi-Fi。
- 4. VMware Workstation Player 是一个 ___________，它使你能够同时运行一个或多个 ___________。

Verizon DBIR 为我在许多课程的讲座和演示中提供了大量有用的信息。它经常被媒体引用，被认为是一个官方且可靠的来源。

### Note
本书付印时，第 1 步中给出的链接指向 2020 DBIR，该链接未来应指向每年的 DBIR。如果链接在你尝试访问时无效，只需前往 https://enterprise.verizon.com 并搜索 “DBIR”。搜索结果中将列出各种版本和部分，点击后将导向许多链接，其中包括下载完整报告的链接。谷歌搜索 “Verizon DBIR” 也同样有效。

![](img/6e921d997751f0a0f87e5170ac9657a1_49_0.png)

Step 1 Head on over to https://enterprise.verizon.com/resources/reports/dbir/ (Figure 2-1)。

