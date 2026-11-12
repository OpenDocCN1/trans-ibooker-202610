## 致谢

首先，一切荣耀和赞美都归于上帝，祂通过祂的儿子耶稣基督，使万有皆有可能，并赐予我们永生的礼物。

特别感谢Sybex的精彩团队，他们在本项目中做出了出色的工作。感谢Kenyon Brown，高级采购编辑，为我们提供了撰写本书的机会。同时感谢Stephanie Barton，开发编辑，她使工作有条不紊，并使本书更具呈现效果。Steph，感谢你的辛勤工作和勤奋。技术编辑David Clinton出色地完成了本书中所有代码的核对工作，并提出了改进内容的建议。同时感谢年轻有才的Daniel Anez（theanez.com）为本书插画所做的工作。我们还想感谢Waterside Productions, Inc.的Carole Jelen，她为我们安排了这个机会，并在我们的写作生涯中给予帮助。

Christine特别想感谢她的丈夫Timothy，感谢他的鼓励、耐心和愿意倾听，即使他完全不明白她在说什么。Christine还想表达对Samantha和 Cameron的爱，“愿上帝丰盛地祝福你们的婚姻。”

Rich特别想感谢他的妻子Barbara，感谢她在项目期间忍受他乖戾的态度，并通过烘焙食品帮助他保持精神振奋。

## 关于作者

Christine Bresnahan，CompTIA Linux+，30多年前在IT行业作为系统管理员开始接触计算机。Christine是Ivy Tech Community College的兼职教授，教授Linux认证和Python编程课程。她还编写书籍并为课堂制作教学资源。

Richard Blum，CompTIA Linux+ ce, CompTIA Security+ ce，在IT行业工作也超过30年，既是系统管理员也是网络管理员，并出版了大量Linux和开源图书。Rich是美国各地大学和学院使用的Linux和网络编程课程的在线讲师。当他不做电脑极客时，Rich喜欢和妻子Barbara以及两个女儿Katie和Jessica共度时光。

## 一目了然的目录
## 目录
## Chapter 2 Managing Software and Processes 67

### Looking at Package Concepts 68
### Using RPM 69
- RPM Distributions and Conventions 69
- The rpm Command Set 71
- Extracting Data from RPMs 77
- Using YUM 78
- Using ZYpp 83
### Using Debian Packages 86
- Debian Package File Conventions 87
- The dpkg Command Set 87
- Looking at the APT Suite 92
- Using apt-cache 93
- Using apt-get 94
- Reconfiguring Packages 97
### Managing Shared Libraries 98
- Library Principles 98
- Locating Library Files 99
- Loading Dynamically 100
- Library Management Commands 100
### Managing Processes 102
- Examining Process Lists 102
- Employing Multiple Screens 109
- Understanding Foreground and Background Processes 116
- Managing Process Priorities 120
- Sending Signals to Processes 121
### Summary 126
### Exam Essentials 127
### Review Questions 129

## Chapter 3 Configuring Hardware 133

### Configuring the Firmware and Core Hardware 134
- Understanding the Role of Firmware 134
- Device Interfaces 136
- The /dev Directory 138
- The /proc Directory 139
- The /sys Directory 143
- Working with Devices 144
- Hardware Modules 148
### Storage Basics 154
- Types of Drives 154
- Drive Partitions 155
- Automatic Drive Detection 155
### Storage Alternatives 156
### Multipath 156
### Logical Volume Manager 157
### Using RAID Technology 158
### Partitioning Tools 158
### Working with fdisk 158
### Working with gdisk 161
### The GNU parted Command 162
### Graphical Tools 163
### Understanding Filesystems 164
- The Virtual Directory 164
- Maneuvering Around the Filesystem 166
- Formatting Filesystems 167
- Common Filesystem Types 167
- Creating Filesystems 169
- Mounting Filesystems 170
- Manually Mounting Devices 170
- Automatically Mounting Devices 172
### Managing Filesystems 173
- Retrieving Filesystem Stats 173
- Filesystem Tools 173
### Summary 174
### Exam Essentials 175
### Review Questions 177

## Chapter 4 Managing Files 181

### Using File Management Commands 182
- Naming and Listing Files 182
- Exploring Wildcard Expansion Rules 186
- Understanding the File Commands 189
- Compressing File Commands 199
- Archiving File Commands 202
- Managing Links 213
### Managing File Ownership 218
- Assessing File Ownership 219
- Changing a File's Owner 219
- Changing a File's Group 220
### Controlling Access to Files 221
- Understanding Permissions 221
- Changing a File's Mode 223
- Setting the Default Mode 226
- Changing Special Access Modes 228
### Locating Files 229
- Getting to Know the FHS 229
- Employing Tools to Locate Files 231
### Summary 239
### Exam Essentials 239
### Review Questions 241

## Chapter 5 Booting, Initializing, and Virtualizing Linux 245

### Understanding the Boot Process 246
- The Boot Process 246
- Extracting Information about the Boot Process 247
- Looking at Firmware 249
- The BIOS Startup 249
- The UEFI Startup 250
- Looking at Boot Loaders 251
- Boot Loader Principles 251
- Using GRUB Legacy as the Boot Loader 251
- Using GRUB 2 as the Boot Loader 255
- Adding Kernel Boot Parameters 259
- Using Alternative Boot Loaders 260
### The Initialization Process 261
- Using the systemd Initialization Process 262
- Exploring Unit Files 263
- Focusing on Service Unit Files 265
- Focusing on Target Unit Files 268
- Looking at systemctl 270
- Examining Special systemd Commands 273
- Using the SysV Initialization Process 276
- Understanding Runlevels 277
- Investigating SysVinit Commands 280
- Stopping the System 283
- Notifying the Users 284
### Virtualizing Linux 286
- Looking at Virtual Machines 287
- Understanding Containers 291
- Looking at Infrastructure as a Service 293
### Summary 295
### Exam Essentials 295
### Review Questions 298

# Part II Exam 102-500 303

## Chapter 6 Configuring the GUI, Localization, and Printing 305

### Understanding the GUI 306
### Understanding the X11 Architecture 307
### Examining X.Org 308
### Figuring Out Wayland 309
### Managing the GUI 311
- Standard GUI Features 311
- The X GUI Login System 313
- Common Linux Desktop Environments 314
- Providing Accessibility 323
### Using X11 for Remote Access 325
- Remote X11 Connections 326
- Tunneling your X11 Connection 326
- Using Remote Desktop Software 328
- Viewing VNC 328
- Grasping Xrdp 330
- Exploring NX 332
- Studying SPICE 332
### Understanding Localization 333
- Character Sets 333
- Environment Variables 334
- Setting Your Locale 335
- Installation Locale Decisions 335
- Changing Your Locale 336
### Looking at Time 338
- Working with Time Zones 338
- Setting the Time and Date 339
### Configuring Printing 343
### Summary 345
### Exam Essentials 346
### Review Questions 348

## Chapter 7 Administering the System 353

### Managing Users and Groups 354
- Understanding Users and Groups 354
- Configuring User Accounts 355
- Configuring Groups 371
### Managing Email 375
- Understanding Email 375
- Choosing Email Software 376
- Working with Email 377
### Using Log and Journal Files 384
- Examining the syslog Protocol 385
- Viewing the History of Linux Logging 387
- Logging Basics Using rsyslogd 387
- Journaling with systemd-journald 394
### Maintaining the System Time 403
- Understanding Linux Time Concepts 403
- Viewing and Setting Time 404
- Understanding the Network Time Protocol 408
- Using the NTP Daemon 411
- Using the chrony Daemon 413
### Summary 416
### Exam Essentials 416
### Review Questions 419

## Chapter 8 Configuring Basic Networking 423

### Networking Basics 424
- The Physical Layer 424
- The Network Layer 426
- The Transport Layer 430
- The Application Layer 431
### Configuring Network Features 433
- Network Configuration Files 433
- Graphical Tools 436
- Command-Line Tools 438
- Getting Network Settings Automatically 445
- Bonding Network Cards 445
### Basic Network Troubleshooting 447
- Sending Test Packets 447
- Tracing Routes 448
- Finding Host Information 449
### Advanced Network Troubleshooting 452
- The netstat Command 452
- Examining Sockets 455
- The netcat Utility 456
### Summary 457
### Exam Essentials 458
### Review Questions 460

## Chapter 9 Writing Scripts 465

### Shell Variables 466
- Global Environment Variables 466
- Local Environment Variables 468
- Setting Local Environment Variables 470
### Setting Global Environment Variables ... 472
- Locating System Environment Variables ... 472
- Using Command Aliases ... 474
### The Basics of Shell Scripting ... 475
- Running Multiple Commands ... 475
- Redirecting Output ... 476
- Piping Data ... 477
- The Shell Script Format ... 478
- Running the Shell Script ... 479
### Advanced Shell Scripting ... 481
- Displaying Messages ... 481
- Using Variables in Scripts ... 482
- Command-Line Arguments ... 484
- Getting User Input ... 484
- The Exit Status ... 488
- Writing Script Programs ... 489
- Command Substitution ... 489
- Performing Math ... 490
- Logic Statements ... 492
- Loops ... 496
- Functions ... 498
- Running Scripts in Background Mode ... 500
- Running in the Background ... 501
- Running Multiple Background Jobs ... 502
- Running Scripts Without a Console ... 503
- Sending Signals ... 504
- Interrupting a Process ... 504
- Pausing a Process ... 504
- Job Control ... 506
- Viewing Jobs ... 506
- Restarting Stopped Jobs ... 508
### Running Like Clockwork ... 509
- Scheduling a Job Using the at Command ... 509
- Scheduling Regular Scripts ... 513
### Summary ... 515
### Exam Essentials ... 516
### Review Questions ... 518

## Chapter 10 Securing Your System ... 523

### Administering Network Security ... 524
- Disabling Unused Services ... 524
- Using Super Server Restrictions ... 534
- Restricting via TCP Wrappers ... 538





### 管理本地安全 539

- 保护密码 539
- 限制 root 访问 543
- 审计用户访问 547
- 设置登录、进程和内存限制 549
- 查找 SUID/SGID 文件 551

### 探索加密概念 553

- 发现关键概念 553
- 保护数据 554
- 对传输进行签名 555

### 查看 SSH 555

- 探索基本 SSH 概念 555
- 配置 SSH 558
- 生成 SSH 密钥 560
- 使用 SSH 密钥进行身份验证 561
- 使用身份验证代理进行身份验证 564
- 隧道 565
- 安全使用 SSH 567

### 使用 GPG 567

- 生成密钥 568
- 导入密钥 569
- 加密和解密数据 570
- 对消息签名和验证签名 571
- 撤销密钥 573

### 摘要 574

### 考试要点 575

### 复习问题 577

### 复习问题答案 583

- 第 1 章：探索 Linux 命令行工具 584
- 第 2 章：管理软件和进程 587
- 第 3 章：配置硬件 590
- 第 4 章：管理文件 593
- 第 5 章：引导、初始化和虚拟化 Linux 597
- 第 6 章：配置 GUI、本地化和打印 601
- 第 7 章：管理系统 605
- 第 8 章：配置基本网络 608
- 第 9 章：编写脚本 611
- 第 10 章：保护您的系统 615

## 索引 619

## 练习表

| 练习 | 标题 | 页码 |
|---|---|---|
| **练习** **8.1** | 确定网络环境 | 457 |
| **练习** **9.1** | 编写 Bash 脚本以查看系统用户密码信息 | 514 |

## 简介

Linux 已成为服务器环境中增长最快的操作系统之一。大多数公司在其基础架构中使用某种类型的 Linux 系统，而 Linux 也是云计算领域的主要参与者之一。构建和管理 Linux 系统的能力是许多公司目前正在寻找的技能。您对 Linux 越了解，在当今的计算机行业中就越具市场竞争力。

Linux 专业学院 (LPI) 开发了一系列认证，帮助您在 Linux 世界中规划职业生涯。其 LPIC-1 认证是面向希望进入涉及 Linux 职业的人员的入门级认证。考试旨在证明您具备安装、操作和排除 Linux 系统故障所需的技能，并熟悉 Linux 特定概念和基本硬件。

本书的目的是帮助您通过 LPIC-1 考试（101 和 102），这些考试在 2019 年更新为版本 5（通常称为 101-500 和 102-500）。由于这些考试涵盖基本 Linux 安装、配置、维护、应用程序、网络和安全，因此本书重点强调了这些主题。您将学到足够的知识，使 Linux 系统运行起来，并为许多常见任务对其进行配置。即使在您参加并通过 LPIC-1 考试之后，本书也应继续作为有用的参考。

## 为什么要成为 Linux 认证专业人员？

随着 Linux 的日益普及（以及 Linux 相关工作的增加），也带来了炒作。围绕 Linux 的所有炒作使得雇主很难区分称职的 Linux 管理员和那些只知道流行语的人。这就是 LPIC-1 认证的用武之地。

凭借 LPIC-1 认证，您将树立起自己作为熟悉 Linux 平台并能够安装、维护和排除任何类型 Linux 系统故障的 Linux 管理员的形象。LPI 创建 LPIC-1 考试，是为了让雇主有信心，知道通过考试的员工将具备完成工作所需的技能。

## 如何获得认证

任何通过两门必考科目（101 和 102）的人都可以获得认证。当前版本的考试为版本 5，表示为 101-500 和 102-500。

考试由 Pearson VUE 管理。考试可以在任何 Pearson VUE 考试中心参加。如果通过，您将通过邮件收到一张证书，证明您已通过考试。如需了解 Pearson VUE 联系信息，请联系 (877) 619-2096。

> 要向 Pearson VUE 注册考试，请致电 (877) 619-2096 或在 www.vue.com 上在线注册。无论您以何种方式注册，都会被要求提供您的姓名、邮寄地址、电话号码、雇主、参加考试的时间和地点（*即*，哪个考试中心）以及信用卡号码（必须在注册时完成付款安排）。

## 谁应该购买本书

任何希望通过 LPIC-1 认证考试的人都将从本书中受益，但这不是购买本书的唯一原因。本书涵盖了初次接触 Linux 世界的人开始使用 Linux 所需了解的所有内容。在您熟悉 Linux 基础知识后，本书将作为查找日常 Linux 问题答案的优秀参考书。

本书假设您熟悉基本的计算机和网络原理。虽然从本书中受益不需要 Linux 经验，但如果您知道如何在 Windows 或 macOS 世界中操作计算机（例如如何使用键盘、使用光盘和使用 USB 拇指驱动器），将会很有帮助。

拥有一个可供跟学的 Linux 系统也会很有帮助。每章都包含一个简单的练习，将引导您了解本章介绍的基本概念。这提供了您通过考试并在 Linux 世界中取得成功所需的关键实践经验。

> 虽然 LPI LPIC-1 考试与 Linux 发行版无关，但不可能编写适用于所有 Linux 发行版的练习。话虽如此，本书中的练习假设您已安装 Ubuntu 18.04 LTS 或 CentOS 7。您可以使用 Oracle VirtualBox 软件（可在 https://virtualbox.org 获取）在虚拟环境中安装其中一种或两种 Linux 发行版。

## 本书组织方式

本书包含 10 章及补充信息：在线术语表、本简介以及简介后的评估测试。各章组织如下：

- 第 1 章，“探索 Linux 命令行工具”，介绍与 Linux 交互所需的基本工具。这些工具包括 shell、重定向、管道、文本过滤器和正则表达式。
- 第 2 章，“管理软件和进程”，介绍用于管理软件的程序。这项任务大部分集中在 RPM 和 Debian 软件包管理系统周围。本章还介绍处理共享库和管理进程（即运行中的程序）。
- 第 3 章，“配置硬件”，侧重于 Linux 与其运行所在硬件的交互。具体硬件和使用过程包括 BIOS、扩展卡、USB 设备、硬盘以及硬盘上使用的分区和文件系统。
- 第 4 章，“管理文件”，介绍用于管理文件的工具。这包括用于管理文件、所有权和权限的命令，以及 Linux 的标准目录树和用于归档文件的工具。
- 第 5 章，“引导、初始化和虚拟化 Linux”，介绍 Linux 的引导过程以及如何在 Linux 中编辑文件。具体主题包括 GRUB Legacy 和 GRUB 2 引导加载程序、引导诊断和运行级别。它还介绍了如何在虚拟机环境中运行 Linux。
- 第 6 章，“配置 GUI、本地化和打印”，介绍 Linux GUI 和打印子系统。主题包括 X 配置、管理 GUI 登录、配置特定于位置的功能、启用辅助功能以及设置 Linux 以使用打印机。
- 第 7 章，“管理系统”，介绍各种行政任务。这些任务包括用户和组管理、调整用户环境、管理日志文件以及设置时钟。
- 第 8 章，“配置基本网络”，侧重于基本网络配置。主题包括 TCP/IP 基础、在 TCP/IP 网络上设置 Linux 以及网络诊断。
- 第 9 章，“编写脚本”，介绍如何在 Linux 中自动执行简单任务。脚本是管理员常用于帮助自动执行常见任务的小程序。能够构建简单脚本并使其在指定时间自动运行，可以大大简化您的管理员工作。
- 第 10 章，“保护您的系统”，介绍安全性。具体主题包括网络安全、本地安全以及使用加密来提高安全性。

第 1 至 5 章涵盖 101-500 考试，第 6 至 10 章涵盖 102-500 考试。它们分别构成本书的第一部分和第二部分。

每章开头都列出了该章涵盖的考试目标。本书并未按顺序介绍目标。因此，您不应对书中目标的一些奇怪顺序感到惊讶。每章末尾，您将找到几个可用于备考的元素：

**考试要点** 本节总结了本章中涵盖的重要信息。您应该能够执行每一项任务或传达所请求的信息。





**复习问题** 每章末尾都有 20 道复习问题。你应该回答这些问题，并将你的答案与问题后提供的答案进行核对。如果你不能正确回答至少 80% 的这些问题，请回头复习该章节，或者至少复习那些让你感到困难的部分。

**警告** 本书包含的复习问题、评估测试和其他测试元素并*不*源自实际考试题目，因此不要死记硬背这些问题的答案，并假设这样做就能让你通过考试。你应该学习本书文本中描述的底层主题。这将使你能够回答本书提供的问题*并*通过考试。学习底层主题也是对你在工作中最有益的方法——这是认证的最终目标。

为了充分利用本书，你应该从头到尾阅读每一章，然后使用章末元素来检查你的记忆和理解。即使你已经熟悉某个主题，也应该浏览该章；Linux 足够复杂，完成一项任务通常有多种方法，因此即使你在某个领域已经很称职，也可能会学到新东西。

## 其他学习工具

本书读者可以访问一个包含多种其他学习工具的网站，包括以下工具：

**注意** 读者可以通过访问 www.sybex.com/go/lpic5e 来使用这些工具。

**样例测试** 本书中的所有问题都将包含在内，此外还包括本引言末尾的评估测试以及每章末尾复习部分的 200 道问题。此外，还有两套 50 道题的加分考试。测试引擎可在 Windows、Linux 和 macOS 上运行。

**电子闪存卡** 其他学习工具包括 150 道闪存卡格式的问题（一个问题后跟一个正确答案）。你可以使用这些工具来复习你对考试目标的了解。闪存卡可在 Windows 和 Linux 上运行。

**术语表 PDF 文件** 此外，还有可搜索的 PDF 格式术语表，可在所有支持 PDF 的平台上阅读。

## 本书使用的约定

本书使用某些排版样式，以帮助你快速识别重要信息，并避免对屏幕提示等词语含义产生混淆。特别是，请注意以下样式：

- *斜体文本* 表示在章节中首次详细描述的关键术语。（斜体也用于强调。）
- 等宽字体表示配置文件的内容、文本模式 Linux shell 提示符下显示的消息、文件名、文本模式命令名和 Internet URL。
- *斜体等宽文本* 表示变量——即在不同系统或命令运行之间有所不同的信息，例如客户端计算机的名称或进程 ID 号。
- **粗体等宽文本** 是你需要键入到计算机中的信息，通常在 Linux shell 提示符下输入。该文本也可以斜体显示，以指示你应为你的系统替换一个适当的值。（当命令单独占据一行时，命令前会加上非粗体等宽 $ 或 # 命令提示符，分别表示普通用户或系统管理员用途。）

除了这些可应用于单个单词或整个段落的文本约定外，还有几种约定用于突出显示文本段落：

![](img/ca7617340ddbe2360d36ce3428729fa9_26_0.png)

注释表示有用或有趣但与主文本有些无关的信息。例如，注释可能与少数网络相关，或者可能指过时的功能。

![](img/ca7617340ddbe2360d36ce3428729fa9_26_1.png)

提示提供可以节省时间或挫折感且可能并不完全明显的信息。提示可能描述如何绕过限制或如何使用功能来执行不寻常的任务。

![](img/ca7617340ddbe2360d36ce3428729fa9_26_2.png)

警告描述潜在的陷阱或危险。如果你忽视警告，可能会花费大量时间从错误中恢复，甚至可能需要从头开始恢复整个系统。

## 练习

## 练习

练习是你应该在自己的计算机上尝试的操作过程，以帮助你学习章节中的内容。不过，不要局限于练习中描述的操作过程！尝试其他命令和操作过程，以真正了解 Linux。

**现实世界场景**

现实世界场景是一种边栏类型，描述的是特别基于现实世界的任务或示例。这可能是我们或我们认识的人遇到过的情况，也可能是关于如何绕过在真实、运行中的 Linux 环境中常见问题的建议。

# 考试目标

每个计算机行业考试背后你都可以肯定能找到考试目标——考试开发者希望确保你具备能力的广泛主题。官方考试目标列于此。（它们也印在涵盖它们的章节开头处。）

考试目标可能会随时由 LPI 自行决定更改，恕不另行通知。请访问 LPI 网站（www.lpi.org）查看最新的考试目标列表。

## Exam 101-500 目标

以下是通过 101-500 考试必须具备的能力领域。该考试分为四个主题（101–104），每个主题有三到八个目标。每个目标都有相关的权重，反映了其对整个考试的重要性。请参阅 LPI 网站以查看每个目标的相关权重。四个主要主题为：

| 主题领域 |
|---|
| 101 系统架构 |
| 102 Linux 安装和包管理 |
| 103 GNU 和 Unix 命令 |
| 104 设备、Linux 文件系统、文件系统层次标准 |

## 101 系统架构

### 101.1 确定和配置硬件设置（第 3 章）

- 启用和禁用集成外围设备。
- 区分各种大容量存储设备类型。
- 确定设备的硬件资源。
- 列出各种硬件信息的工具和实用程序（例如，lsusb、lspci 等）。
- 操作 USB 设备的工具和实用程序。
- 理解 sysfs、udev、halld、dbus 的概念。
- 使用的文件、术语和实用程序的部分列表如下：/sys、/proc、/dev、modprobe、lsmod、lspci、lsusb。

### 101.2 启动系统（第 5 章）

- 向引导加载程序提供常用命令，并在启动时向内核提供选项。
- 展示从 BIOS/UEFI 到启动完成的启动序列知识。
- 理解 SysVinit 和 systemd。
- 了解 Upstart。
- 在日志文件中检查启动事件。
- 使用的文件、术语和实用程序的部分列表如下：dmesg、journalctl、BIOS、UEFI、bootloader、kernel、init、initramfs、SysVinit、systemd。

### 101.3 更改运行级别/启动目标以及关闭或重新启动系统（第 5 章）

- 设置默认运行级别或启动目标。
- 在运行级别/启动目标之间切换，包括单用户模式。
- 从命令行关闭和重新启动系统。
- 在切换运行级别/启动目标或其他重大系统事件之前向用户发出警报。
- 正确终止进程。
- 了解 acpid。
- 使用的文件、术语和实用程序的部分列表如下：/etc/inittab、shutdown、init、/etc/init.d、telinit、systemd、systemctl、/etc/systemd/、/usr/lib/systemd/、wall。

## 102 Linux 安装和包管理

### 102.1 设计硬盘布局（第 3 章）

- 将文件系统和交换空间分配到独立的分区或磁盘。
- 根据系统的预期用途调整设计。
- 确保 /boot 分区符合启动所需的硬件架构要求。
- 了解 LVM 的基本功能。
- 使用的文件、术语和实用程序的部分列表如下：/（根）文件系统、/var 文件系统、/home 文件系统、/boot 文件系统、交换空间、挂载点、分区、EFI 系统分区（ESP）。

### 102.2 安装引导管理器（第 5 章）

- 提供替代引导位置和备用引导选项。
- 安装和配置引导加载程序，例如 GRUB Legacy。
- 对 GRUB 2 进行基本配置更改。
- 与引导加载程序交互。
- 使用的文件、术语和实用程序的部分列表如下：/boot/grub/menu.lst、grub.cfg 和 grub.conf、grub-install、grub-mkconfig、MBR。

### 102.3 管理共享库（第 2 章）

- 识别共享库。
- 识别系统库的典型位置。
- 加载共享库。
- 使用的文件、术语和实用程序的部分列表如下：ldd、ldconfig、/etc/ld.so.conf、LD_LIBRARY_PATH。

### 102.4 使用 Debian 包管理（第 2 章）

- 安装、升级和卸载 Debian 二进制包。
- 查找包含特定文件或库（无论是否已安装）的包。
- 获取包信息，如版本、内容、依赖项、包完整性和安装状态（是否已安装）。
- 了解 apt。
- 使用的文件、术语和实用程序的部分列表如下：/etc/apt/sources.list、dpkg、dpkg-reconfigure、apt-get、apt-cache。





## 102.5 使用 RPM 和 YUM 包管理（第 2 章）

- 使用 RPM、YUM 和 Zypper 安装、重新安装、升级和删除软件包。
- 获取 RPM 软件包的信息，例如版本、状态、依赖关系、完整性和签名。
- 确定软件包提供了哪些文件，以及查找特定文件来自哪个软件包。
- 以下是所用文件、术语和实用程序的部分列表：rpm, rpm2cpio, /etc/yum.conf, /etc/yum.repos.d/, yum, zypper.

## 102.6 作为虚拟化客体的 Linux（第 5 章）

- 理解虚拟机和容器的一般概念。
- 理解 IaaS 云中虚拟机的常见元素，例如计算实例、块存储和网络。
- 理解当系统被克隆或用作模板时需要更改的 Linux 系统独有属性。
- 理解如何使用系统镜像部署虚拟机、云实例和容器。
- 理解将 Linux 与虚拟化产品集成的 Linux 扩展。
- 了解 cloud-init。
- 以下是所用文件、术语和实用程序的部分列表：Virtual machine, Linux container, Application container, Guest drivers, SSH host keys, D-Bus machine ID.

## 103 GNU 和 Unix 命令

### 103.1 在命令行上工作（第 1 章）

- 使用单个 shell 命令和单行命令序列在命令行上执行基本任务。
- 使用和修改 shell 环境，包括定义、引用和导出环境变量。
- 使用和编辑命令历史。
- 调用定义路径内外的命令。
- 以下是所用文件、术语和实用程序的部分列表：bash, echo, env, export, pwd, set, unset, type, which, man, uname, history, .bash_history, Quoting.

### 103.2 使用过滤器处理文本流（第 1 章）

- 将文本文件和输出流通过文本实用程序过滤器传递，以使用 GNU textutils 包中查找到的标准 UNIX 命令修改输出。
- 以下是所用文件、术语和实用程序的部分列表：bzcat, cat, cut, head, less, md5sum, nl, od, paste, sed, sha256sum, sha512sum, sort, split, tail, tr, uniq, wc, xzcat, zcat.

### 103.3 执行基本文件管理（第 4 章）

- 单独复制、移动和删除文件及目录。
- 递归复制多个文件和目录。
- 递归删除文件和目录。
- 在命令中使用简单和高级的通配符规范。
- 使用 find 根据类型、大小或时间定位并操作文件。
- tar、cpio 和 dd 的用法。
- 以下是所用文件、术语和实用程序的部分列表：cp, find, mkdir, mv, ls, rm, rmdir, touch, tar, cpio, dd, file, gzip, gunzip, bzip2, bunzip2, xz, unxz, file globbing.

### 103.4 使用流、管道和重定向（第 1 章）

- 重定向标准输入、标准输出和标准错误。
- 将一个命令的输出通过管道传送到另一个命令的输入。
- 将一个命令的输出用作另一个命令的参数。
- 将输出同时发送到 stdout 和文件。
- 以下是所用文件、术语和实用程序的部分列表：tee, xargs.

### 103.5 创建、监控和终止进程（第 2 章）

- 在前台和后台运行作业。
- 向程序发送信号，使其在注销后继续运行。
- 监控活动进程。
- 选择并排序进程以供显示。
- 向进程发送信号。
- 以下是所用文件、术语和实用程序的部分列表：&, bg, fg, jobs, kill, nohup, ps, top, free, uptime, pgrep, pkill, killall, watch, screen, tmux.

### 103.6 修改进程执行优先级（第 2 章）

- 了解创建的作业的默认优先级。
- 以高于或低于默认值的优先级运行程序。
- 更改正在运行的进程的优先级。
- 以下是所用文件、术语和实用程序的部分列表：nice, ps, renice, top

### 103.7 使用正则表达式搜索文本文件（第 1 章）

- 创建包含多个表示元素的简单正则表达式。
- 理解基本正则表达式和扩展正则表达式之间的区别。
- 理解特殊字符、字符类、量词和锚点的概念。
- 使用正则表达式工具在文件系统或文件内容中执行搜索。
- 使用正则表达式删除、更改和替换文本。
- 以下是所用文件、术语和实用程序的部分列表：grep, egrep, fgrep, sed, regex(7).

### 103.8 基本文件编辑（第 5 章）

- 使用 vi 浏览文档。
- 理解和使用 vi 模式。
- 在 vi 中插入、编辑、删除、复制和查找文本。
- 了解 Emacs、nano 和 vim。
- 配置标准编辑器。
- 以下是所用文件、术语和实用程序的部分列表：vi, /, ?, h, j, k, l, i, o, a, d, p, y, dd, yy, ZZ, :w!, :q!, EDITOR.

## 104 设备、Linux 文件系统、文件系统层次标准

### 104.1 创建分区和文件系统（第 3 章）

- 管理 MBR 和 GPT 分区表。
- 使用各种 mkfs 命令创建各种文件系统，例如：ext2, ext3, ext4, XFS, VFAT 和 exFAT。
- 了解 Btrfs 的基本功能，包括多设备文件系统、压缩和子卷。
- 以下是所用文件、术语和实用程序的部分列表：fdisk, gdisk, parted, mkfs, mkswap.

### 104.2 维护文件系统完整性（第 3 章）

- 验证文件系统的完整性。
- 监控空闲空间和 inode。
- 修复简单的文件系统问题。
- 以下是所用文件、术语和实用程序的部分列表：du, df, fsck, e2fsck, mke2fs, tune2fs, xfs tools (such as xfs_repair, xfs_fsr, and xfs_db).

### 104.3 控制文件系统的挂载和卸载（第 3 章）

- 手动挂载和卸载文件系统。
- 配置启动时的文件系统挂载。
- 配置用户可挂载的可移除文件系统。
- 使用标签和 UUID 识别和挂载文件系统。
- 了解 systemd 挂载单元。
- 以下是所用文件、术语和实用程序的部分列表：/etc/fstab, /media/, mount, umount, blkid, lsblk.

### 104.4 (已删除)

### 104.5 管理文件权限和所有权（第 4 章）

- 管理普通文件、特殊文件及目录的访问权限。
- 使用 suid、sgid 和粘滞位等访问模式来维护安全性。
- 知道如何更改文件创建掩码。
- 使用组字段向组成员授予文件访问权限。
- 以下是所用文件、术语和实用程序的部分列表：chmod, umask, chown, chgrp.

### 104.6 创建和更改硬链接和符号链接（第 4 章）

- 创建链接。
- 识别硬链接和/或软链接。
- 复制与链接文件的区别。
- 使用链接支持系统管理任务。
- 以下是所用文件、术语和实用程序的部分列表：ln, ls.

### 104.7 查找系统文件并将文件放置在正确位置（第 4 章）

- 理解 FHS 下文件的正确位置。
- 在 Linux 系统上查找文件和命令。
- 了解 FHS 定义的重要文件和目录的位置和用途。
- 以下是所用文件、术语和实用程序的部分列表：find, locate, updatedb, whereis, which, type, /etc/updatedb.conf.

## 102-500 考试目标

102-500 考试包含六个主题（105–110），每个主题包含三个或四个目标。六个主要主题是：

| 主题领域 |
|---|
| 105 Shell 和 Shell 脚本 |
| 106 用户界面和桌面 |
| 107 管理任务 |
| 108 必要系统服务 |
| 109 网络基础 |
| 110 安全 |

## 105 Shell、脚本和数据管理

### 105.1 自定义和使用 shell 环境（第 9 章）

- 在登录或生成新 shell 时设置环境变量（例如，PATH）。
- 为经常使用的命令序列编写 Bash 函数。
- 为新用户帐户维护骨架目录。
- 使用适当的目录设置命令搜索路径。
- 以下是所用文件、术语和实用程序的部分列表：., source, /etc/bash.bashrc, /etc/profile, env, export, set, unset, ~/.bash_profile, ~/.bash_login, ~/.profile, ~/.bashrc, ~/.bash_logout, function, alias.




### 105.2 自定义或编写简单脚本 (第 9 章)

- 使用标准 sh 语法（循环、测试）。
- 使用命令替换。
- 测试命令返回的值，判断成功或失败，或命令提供的其他信息。
- 执行链式命令。
- 条件性地向超级用户发送邮件。
- 通过 shebang (#!) 行正确选择脚本解释器。
- 管理脚本的位置、所有权、执行权限和 suid 权限。
- 以下是使用的文件、术语和实用程序的部分列表：for, while, test, if, read, seq, exec, ||, &&.

## 106 用户界面和桌面

### 106.1 安装和配置 X11 (第 6 章)

- 了解 X11 架构。
- 基本了解和掌握 X Window 配置文件。
- 覆盖 Xorg 配置的特定方面，例如键盘布局。
- 了解桌面环境的组件，例如显示管理器和窗口管理器。
- 管理对 X 服务器的访问并在远程 X 服务器上显示应用程序。
- 了解 Wayland。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/X11/xorg.conf, /etc/X11/xorg.conf.d, ~/.xsession-errors, xhost, xauth, DISPLAY, X.

### 106.2 图形桌面 (第 6 章)

- 了解主要桌面环境。
- 了解访问远程桌面会话的协议。
- 以下是使用的文件、术语和实用程序的部分列表：KDE, Gnome, Xfce, X11, XDMCP, VNC, Spice, RDP.

### 106.3 辅助功能 (第 6 章)

- 具备视觉设置和主题的基本知识。
- 具备辅助技术 (ATs) 的基本知识。
- 以下是使用的文件、术语和实用程序的部分列表：高对比度/大字体桌面主题，屏幕阅读器，盲文显示器，屏幕放大镜，屏幕键盘，粘滞/重复键，慢速/反弹/切换键，鼠标键，手势，语音识别.

## 107 管理任务

### 107.1 管理用户和组账户及相关系统文件 (第 7 章)

- 添加、修改和删除用户及组。
- 管理密码/组数据库中的用户/组信息。
- 创建和管理特殊用途及受限账户。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/passwd, /etc/shadow, /etc/group, /etc/skel, chage, getent, groupadd, groupdel, groupmod, passwd, useradd, userdel, usermod.

### 107.2 通过计划作业自动执行系统管理任务 (第 9 章)

- 管理 cron 和 at 作业。
- 配置用户对 cron 和 at 服务的访问。
- 了解 systemd 计时器单元。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/cron.{d,daily,hourly,monthly,weekly}, /etc/at.deny, /etc/at.allow, /etc/crontab, /etc/cron.allow, /etc/cron.deny, /var/spool/cron/, crontab, at, atq, atrm, systemctl, systemd-run.

### 107.3 本地化和国际化 (第 6 章)

- 配置区域设置和环境变量。
- 配置时区设置和环境变量。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/timezone, /etc/localtime, /usr/share/zoneinfo, 环境变量 (LC_*, LC_ALL, LANG, TZ), /usr/bin/locale, tzselect, timedatectl, date, iconv, UTF-8, ISO-8859, ASCII, Unicode.

## 108 必需的系统服务

### 108.1 维护系统时间 (第 7 章)

- 设置系统日期和时间。
- 将硬件时钟设置为正确的 UTC 时间。
- 配置正确的时区。
- 使用 ntp 和 chrony 进行基本 NTP 配置。
- 了解使用 pool.ntp.org 服务。
- 了解 ntpq 命令。
- 以下是使用的文件、术语和实用程序的部分列表：/usr/share/zoneinfo, /etc/timezone, /etc/localtime, /etc/ntp.conf, /etc/chrony.conf, date, hwclock, timedatectl, ntpd, ntpdate, chronyc, pool.ntp.org.

### 108.2 系统日志 (第 7 章)

- rsyslogd 的基本配置。
- 了解标准设施、优先级和操作。
- 查询 systemd 日志。
- 按日期、服务或优先级等条件过滤 systemd 日志数据。
- 删除旧的 systemd 日志数据。
- 从救援系统或文件系统副本中检索 systemd 日志数据。
- 了解 rsyslogd 与 systemd-journald 的交互。
- logrotate 的配置。
- 了解 syslog 和 syslog-ng。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/rsyslog.conf, /var/log, logger, logrotate, /etc/logrotate.conf, /etc/logrotate.d/, journalctl, systemd-cat, /etc/systemd/journal.conf, /var/log/journal/.

### 108.3 邮件传输代理 (MTA) 基础 (第 7 章)

- 创建电子邮件别名。
- 配置电子邮件转发。
- 了解常用的 MTA 程序 (postfix, sendmail, qmail, exim)（无需配置）。
- 以下是使用的文件、术语和实用程序的部分列表：~/.forward, sendmail 仿真层命令, newaliases, mail, mailq, postfix, sendmail, exim.

### 108.4 管理打印机和打印 (第 6 章)

- CUPS 的基本配置（适用于本地和远程打印机）。
- 管理用户打印队列。
- 排除常规打印问题。
- 从配置的打印机队列中添加和删除任务。
- 以下是使用的文件、术语和实用程序的部分列表：CUPS 配置文件、工具和实用程序；/etc/cups；lpd 旧版接口 (lpr, lprm, lpq).

## 109 网络基础

### 109.1 互联网协议基础 (第 8 章)

- 演示对网络掩码和 CIDR 表示法的理解。
- 了解私有和公有“点分”IP 地址的区别。
- 了解常见 TCP 和 UDP 端口 (20, 21, 22, 23, 25, 53, 80, 110, 123, 139, 143, 161, 162, 389, 443, 465, 514, 636, 993, 995)。
- 了解 UDP、TCP 和 ICMP 的区别及主要特性。
- 了解 IPv4 和 IPv6 之间的主要区别。
- 了解 IPv6 的基本特性。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/services, IPv4, IPv6, 子网划分, TCP, UDP, ICMP.

### 109.2 持久网络配置 (第 8 章)

- 了解基本 TCP/IP 主机配置。
- 使用 NetworkManager 配置以太网和 Wi-Fi。
- 了解 systemd-networkd。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/hostname, /etc/hosts, /etc/nsswitch.conf, /etc/resolv.conf, nmcli, hostnamectl, ifup, ifdown.

### 109.3 基础网络故障排除 (第 8 章)

- 手动配置网络接口，包括使用 iproute2 查看和更改网络接口配置。
- 手动配置路由，包括使用 iproute2 查看和更改路由表以及设置默认路由。
- 调试与网络配置相关的问题。
- 了解旧版 net-tools 命令。
- 以下是使用的文件、术语和实用程序的部分列表：ip, hostname, ss, ping, ping6, traceroute, traceroute6, tracepath, tracepath6, netcat, ifconfig, netstat, route.

### 109.4 配置客户端 DNS (第 8 章)

- 查询远程 DNS 服务器。
- 配置本地名称解析并使用远程 DNS 服务器。
- 修改名称解析的顺序。
- 调试与名称解析相关的错误。
- 了解 systemd-resolved。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/hosts, /etc/resolv.conf, /etc/nsswitch.conf, host, dig, getent.

## 110 安全

### 110.1 执行安全管理任务 (第 10 章)

- 审计系统以查找设置了 suid/sgid 位的文件。
- 设置或更改用户密码及密码过期信息。
- 能够使用 nmap 和 netstat 发现系统上的开放端口。
- 对用户登录、进程和内存使用设置限制。
- 确定哪些用户已登录系统或当前正在登录。
- sudo 的基本配置和用法。
- 以下是使用的文件、术语和实用程序的部分列表：find, passwd, fuser, lsof, nmap, chage, netstat, sudo, /etc/sudoers, su, usermod, ulimit, who, w, last.

### 110.2 设置主机安全 (第 10 章)

- 了解影子密码及其工作原理。
- 关闭未使用的网络服务。
- 了解 TCP 包装器的作用。
- 以下是使用的文件、术语和实用程序的部分列表：/etc/nologin, /etc/passwd, /etc/shadow, /etc/xinetd.d/, /etc/xinetd.conf, /etc/inetd.d/, /etc/inetd.conf, systemd-socket, /etc/inittab, /etc/init.d/, /etc/hosts.allow, /etc/hosts.deny.

### 110.3 使用加密保护数据 (第 10 章)

- 执行 OpenSSH 2 客户端的基本配置和用法。
- 了解 OpenSSH 2 服务器主机密钥的作用。
- 执行 GnuPG 的基本配置、用法和吊销。
- 使用 GPG 加密、解密、签名和验证文件。
- 了解 SSH 端口隧道（包括 X11 隧道）。
- 以下是使用的文件、术语和实用程序的部分列表：ssh, ssh-keygen, ssh-agent, ssh-add, ~/.ssh/id_rsa 和 id_rsa.pub, ~/.ssh/id_dsa 和 id_dsa.pub, ~/.ssh/id_ecdsa 和 id_ecdsa.pub, ~/.ssh/id_ed25519 和 id_ed25519.pub, /etc/ssh/ssh_host_rsa_key 和 ssh_host_rsa_key.pub, /etc/ssh/ssh_host_dsa_key 和 ssh_host_dsa_key.pub, /etc/ssh/ssh_host_ecdsa_key 和 ssh_host_ecdsa_key.pub, /etc/ssh/ssh_host_ed25519_key 和 ssh_host_ed25519_key.pub, ~/.ssh/authorized_keys, /etc/ssh_known_hosts, gpg, gpg-agent, ~/.gnupg/.




## 评估测试

- 1. 以下哪些是 shell 程序的名称？（多选）
  - A. Bash
  - B. Korn Shell
  - C. Born Shell
  - D. Dash
  - E. Z Shell

- 2. 你是 CentOS Linux 服务器上的系统管理员。你需要查看 /var/log/messages 文件中以日期 May 30 开头且以 IPv4 地址 192.168.10.42 结尾的记录。以下哪个是最佳的 grep 命令？
  - A. grep "May 30?192.168.10.42" /var/log/messages
  - B. grep "May 30.*192.168.10.42" /var/log/messages
  - C. grep -i "May 30.*192.168.10.42" /var/log/messages
  - D. grep -i "May 30?192.168.10.42" /var/log/messages
  - E. grep -v "May 30.*192.168.10.42" /var/log/messages

- 3. 以下哪个命令可确定 Problems.txt 文件中包含单词 error 的记录数量？
  - A. grep error Problems.txt | wc -b
  - B. grep error Problems.txt | wc -w
  - C. grep error Problems.txt | wc -l
  - D. grep Problems.txt error | wc -w
  - E. grep Problems.txt error | wc -l

- 4. 以下哪些符合 Debian 软件包文件的标准命名格式？（多选）
  - A. openssh-client_1%3a7.6pl-4ubuntu0.3_amd64.deb
  - B. openssh-client-3a7-24_86_x64.rpm
  - C. zsh_5.4.2-3ubuntu3.1_amd64.deb
  - D. zsh_5.4.2-3ubuntu3.1_amd64.dpkg
  - E. emacs_47.0_all.dpkg

- 5. 在命令行命令后放置 & 符号有什么作用？
  - A. 将命令与终端会话断开连接。
  - B. 以前台模式运行命令。
  - C. 以后台模式运行命令。
  - D. 将输出重定向到另一个命令。
  - E. 将输出重定向到文件。

- 6. 如果你使用 tmux 实用程序，如何创建一个新窗口？
  - A. screen
  - B. tmux create
  - C. tmux ls
  - D. screen -ls
  - E. tmux new

- 7. 哪种硬件接口使用中断、I/O 端口和 DMA 通道与 PC 主板通信？
  - A. USB
  - B. GPIO
  - C. PCI
  - D. Monitors
  - E. Printers

- 8. Linux FHS 专门留出哪个目录用于安装第三方程序？
  - A. /usr/bin
  - B. /usr
  - C. /opt
  - D. /usr/sbin
  - E. /tmp

- 9. 哪个命令允许你将分区挂载到正在运行的 Linux 系统的虚拟目录中？
  - A. mount
  - B. umount
  - C. fsck
  - D. dmesg
  - E. mkinitramfs

- 10. 系统管理员对一个归档文件应用了压缩实用程序。生成的文件扩展名为 .gz。使用了哪个压缩实用程序？
  - A. xz 实用程序
  - B. gzip 实用程序
  - C. bzip2 实用程序
  - D. zip 实用程序
  - E. dd 实用程序

- 11. 在应用 umask 设置之前，目录的默认权限八进制代码为以下哪个？
  - A. 111
  - B. 755
  - C. 666
  - D. 777
  - E. 888

- 12. 你需要查找 /tmp 目录或其子目录中的文件。这些文件应该是空的。假设你拥有超级用户权限，应该使用什么命令？
  - A. find / -name tmp
  - B. find /tmp -empty
  - C. find /tmp -empty 0
  - D. find /tmp/* -name empty
  - E. find / -empty

- 13. 系统 BIOS 尝试在哪里查找引导加载程序？（多选）
  - A. 内部硬盘驱动器
  - B. 外部硬盘驱动器
  - C. DVD 驱动器
  - D. USB 闪存驱动器
  - E. 网络服务器

- 14. 哪种固件方法在大多数现代 IBM 兼容计算机上取代了 BIOS？
  - A. FTP
  - B. UEFI
  - C. PXE
  - D. NFS
  - E. HTTPS

- 15. 以下哪些是系统初始化方法？（多选）
  - A. /sbin/init
  - B. /etc/init
  - C. SysVinit
  - D. systemd
  - E. cloud-init

- 16. Cinnamon 桌面环境使用哪个窗口管理器？
  - A. Mutter
  - B. Muffin
  - C. Nemo
  - D. Dolphin
  - E. LightDM

- 17. 你的 X.org 会话已挂起。使用哪些按键重新启动会话？
  - A. Ctrl+C
  - B. Ctrl+Z
  - C. Ctrl+Q
  - D. Ctrl+Alt+Delete
  - E. Ctrl+Alt+Backspace

- 18. Linux 中哪个文件夹包含时区模板文件？
  - A. /etc/timezone
  - B. /etc/localtime
  - C. /usr/share/zoneinfo
  - D. /usr/share/timezone
  - E. /usr/share/localtime

- 19. 哪个字段在 /etc/passwd 和 /etc/shadow 文件记录中包含相同的数据？
  - A. 密码
  - B. 账户过期日期
  - C. UID
  - D. GID
  - E. 用户账户的用户名

- 20. 哪个设备和优先级设置会记录警告及更高严重程度的内核消息？
  - A. kern.=warn
  - B. kern.*
  - C. *.info
  - D. kern.warn
  - E. kern.alert

- 21. 以下哪些可以在 Linux 上实现 NTP？（多选）
  - A. Exim
  - B. ntpd
  - C. Sendmail
  - D. Postfix
  - E. chronyd

- 22. 哪个网络层使用 Wi-Fi Protected Access (WPA) 加密？
  - A. network
  - B. physical
  - C. transport
  - D. application

- 23. 使用命令行在接口上设置 IP 地址、子网掩码和默认路由器信息的两个命令是哪两个？
  - A. netstat
  - B. ping
  - C. nmtui
  - D. ip
  - E. route

- 24. 哪个工具允许你向远程主机发送 ICMP 消息以测试网络连接？
  - A. netstat
  - B. ifconfig
  - C. ping
  - D. iwconfig
  - E. ss

- 25. 哪个 Bash shell 脚本命令允许你遍历一系列数据直到数据结束？
  - A. if
  - B. case
  - C. for
  - D. exit
  - E. ${()}

- 26. 哪个环境变量允许你检索运行 shell 脚本的用户账户的数字用户 ID 值？
  - A. $USER
  - B. $UID
  - C. $BASH
  - D. $HOME
  - E. $1

- 27. crontab 条目 `0 0 1 * * myscript` 何时运行指定命令？
  - A. 每天凌晨 1 点运行。
  - B. 每月第一天午夜运行。
  - C. 每周第一天午夜运行。
  - D. 每天下午 1 点运行。
  - E. 每天午夜运行。

- 28. 以下哪个实用程序允许你扫描系统并查看通过打开的文件提供或使用的网络服务？
  - A. fuser
  - B. lsof
  - C. nmap
  - D. netstat
  - E. ss

- 29. 应查看以下哪些 OpenSSH 指令，以确保面向公众的系统的用户正在安全地使用 SSH？
  - A. Port 指令
  - B. Protocol 指令
  - C. PermitRootLogin 指令
  - D. AllowTCPForwarding 指令
  - E. ForwardX11 指令

- 30. 关于 gpg-agent，以下哪些说法正确？（多选）
  - A. 它启动一个特殊的代理 shell，因此无需重新输入密码即可对远程系统进行身份验证。
  - B. 它独立于任何协议管理 GPG 密钥。
  - C. 它由 SysVinit 或 systemd 管理，具体取决于系统的初始化方法。
  - D. 它将先前使用过的私钥保存在 RAM 中。
  - E. 如果它需要的私钥不在 RAM 中，它会要求用户输入保护密钥的密码短语。

## 评估测试答案

- 1. A、B、D、E。选项 A、B、D 和 E 中的 shell 名称都是合法的 shell 程序名称，因此是正确答案。不存在 Born shell（你可能把这个名称与原始的 Bourne shell 混淆了），因此选项 C 是错误的选择。

- 2. B。选项 B 是最佳命令，因为此 grep 命令使用了正确的语法。它在 PATTERN 周围使用引号以避免意外结果，并使用 .* 正则表达式字符表示 May 30 和 IPv4 地址之间可以是任何内容。不需要额外的开关。选项 A 不是最佳 grep 命令，因为它使用了错误的正则表达式 ?，该表达式仅允许 May 30 和 IPv4 地址之间存在一个字符。选项 C 和 D 不是最佳 grep 命令，因为它们使用了 -i 开关来忽略大小写，而在此情况下不需要。选项 E 中的 grep 命令是错误的选择，因为它使用了 -v 开关，将显示与 PATTERN 不匹配的文本记录。

- 3. C。要查找 Problems.txt 文件中至少包含一次单词 error 的记录，需要使用 grep 命令。正确语法是 grep error Problems.txt。要统计记录数，将 grep 命令的 STDOUT 通过管道作为 STDIN 输入到 wc 实用程序中。统计记录的正确语法是 wc -l。因此，选项 C 是正确答案。选项 A 中的命令不正确，因为其 wc 命令统计的是每个输入记录中的字节数。选项 B 是错误答案，因为其 wc 命令统计的是每个输入记录中的单词数。选项 D 中的命令有两个问题。首先，其 grep 命令语法将搜索项和要搜索的文件颠倒了。此外，其 wc 命令统计的是每个输入记录中的单词数。因此，选项 D 是错误的选择。选项 E 是错误答案，因为其 grep 命令语法将搜索项和要搜索的文件颠倒了。

- 4. A、C。Debian 软件包文件遵循 PACKAGE-NAME-VERSION-RELEASE_ARCHITECTURE.deb 的标准命名格式。因此，选项 A 和 C 是正确答案。选项 B 中的软件包文件名具有 .rpm 文件扩展名，这立即使其不符合 Debian 软件包文件标准命名格式。因此，选项 B 是错误答案。选项 D 和 E 使用 .dpkg 作为文件扩展名，因此也是错误的选择。

- 5. C。& 符号告诉 shell 在终端会话中以后台模式运行指定命令，因此选项 C 正确。nohup 命令用于将命令与终端会话断开连接，因此选项 A 不正确。fg 命令将后台模式运行的命令移至前台，因此选项 B 不正确。管道符号（|）将命令的输出重定向到另一个命令，因此选项 D 不正确。大于号（>）将命令的输出重定向到文件，因此选项 E 也是错误的选择。

- 6. E。tmux new 将创建一个新窗口。因此，选项 E 是正确答案。GNU Screen 实用程序使用 screen 命令创建新窗口。因此，选项 A 是错误答案。tmux create 是一个虚构的 tmux 命令，因此选项 B 也是错误的选择。tmux ls 会显示分离的窗口，但不会创建它们，因此选项 C 是错误的选择。screen -ls 命令会显示任何分离的 GNU Screen 窗口，因此选项 D 也是错误的选择。





- 7. C. PCI 板使用中断、I/O 端口和 DMA 通道与 PC 主板发送和接收数据，因此选项 C 正确。USB 设备使用连接到主板的串行总线传输数据，不使用 DMA 通道，因此选项 A 不正确。GPIO 接口使用内存映射的专用 IC 芯片，而不使用中断和 I/O 端口，因此选项 B 不正确。显示器和打印机是硬件设备而非硬件接口，因此选项 D 和 E 不正确。

- 8. C. /opt 目录专门用于安装可选的第三方应用程序，因此选项 C 正确。/usr/bin 目录专门用于本地用户程序，而非第三方程序，因此选项 A 不正确。/usr 目录专门用于标准 Linux 程序，而非第三方程序，因此选项 B 不正确。/usr/sbin 目录专门用于系统程序和数据，而非第三方程序，因此选项 D 不正确。/tmp 目录专门用于临时文件，系统重启时通常会擦除这些文件，而非第三方程序，因此选项 E 不正确。

- 9. A. mount 命令允许你同时指定分区以及要在虚拟目录中将分区文件和目录追加到的位置。分区中包含的文件和目录随后会出现在虚拟目录的该位置。umount 命令（选项 B）用于卸载已挂载的分区。选项 C，fsck 命令，用于修复已损坏且无法挂载的硬盘，它并不实际挂载驱动器本身。选项 D 中的 dmesg 命令用于查看系统的启动消息，可能会告诉你硬盘在哪里追加到虚拟目录，但它并不实际执行追加操作。选项 E，mkinitramfs 命令，创建 initrd RAM 盘，并不直接处理将硬盘挂载到虚拟目录。

- 10. B. gzip 实用程序压缩数据文件并赋予其 .gz 文件扩展名。因此，选项 B 是正确答案。xz、bzip2 和 zip 压缩实用程序压缩数据文件并赋予其不同的文件扩展名，因此选项 A、C 和 D 是错误答案。dd 实用程序不是压缩程序。因此，选项 E 也是错误选择。

- 11. D. 在应用 umask 设置之前，目录的默认权限八进制代码为 777。因此，选项 D 是正确答案。选项 A 中的 111 八进制代码在应用 umask 设置之前不适用于任何创建的文件或目录。因此，选项 A 是错误答案。755 八进制代码是在应用 umask 设置 0022 后典型的目录权限设置结果。因此，选项 B 是错误选择。666 八进制代码是在应用 umask 设置之前文件的默认权限八进制代码。因此，选项 C 是错误答案。888 八进制代码不存在，因此选项 E 是错误选择。

- 12. B. find /tmp -empty 命令将定位 /tmp 目录或其子目录中的空文件。因此，选项 B 是正确答案。find / -name tmp 命令从根目录而不是 /tmp 目录开始，并搜索名称 tmp 的文件/目录。因此，选项 A 是错误答案。find /tmp -empty ø 命令在命令末尾添加了一个不正确的额外参数 ø，因此选项 C 也是错误答案。find /tmp/* -name empty 命令搜索名称 tmp 的文件/目录，并向要搜索的目录名添加不必要的通配符 *，因此选项 D 也是错误选择。find / -empty 命令从根目录而不是 /tmp 目录开始。因此，选项 E 是错误选择。

- 13. A、B、C、D 和 E。BIOS 固件可以在多个位置查找引导程序。最常见的是它查看系统上安装的内部硬盘，但是，如果未找到，它可以搜索其他位置。大多数系统允许你从外部硬盘或 DVD 驱动器启动。现代系统现在还提供了从插入工作站 USB 端口的 USB 存储棒启动的选项。最后，许多系统提供 PXE 启动选项，允许系统从网络服务器远程启动。

- 14. B. UEFI 固件方法已在大多数 IBM 兼容计算机中取代 BIOS，因此选项 B 正确。FTP、PXE、NFS 和 HTTPS 不是固件方法，而是加载 Linux 引导程序的方法，因此选项 A、C、D 和 E 均不正确。

- 15. C、D。SysVinit 和 systemd 都是系统初始化方法。因此，选项 C 和 D 是正确答案。init 程序可以位于 /bin/、/etc/ 或 /bin/ 目录中，虽然它被初始化方法使用，但它本身不是一种方法。因此，选项 A 和 B 是错误答案。cloud-init 程序是一个工具，允许你在本地或 IaaS 平台上的云镜像中从系统镜像创建 VM。但是，它不是系统初始化方法。因此，选项 E 也是错误答案。

- 16. B. Cinnamon 桌面环境使用 Muffin 窗口管理器。因此，选项 B 是正确答案。Mutter 是 GNOME Shell 桌面环境的窗口管理器，尽管 Muffin 从该项目分支出来。因此，选项 A 是错误答案。Nemo 是 Cinnamon 的文件管理器，因此选项 C 是错误选择。Dolphin 是 KDE Plasma 桌面环境的文件管理器。因此，选项 D 是错误选择。LightDM 是 Cinnamon 的显示管理器，因此选项 E 也是错误选择。

- 17. E. Ctrl+Alt+Backspace 将终止你的 X.org 会话并重新启动它，将你带到登录屏幕（显示管理器）。因此，选项 E 是正确答案。Ctrl+C 组合键发送中断信号，但不重新启动 X.org 会话。因此，选项 A 是错误答案。Ctrl+Z 按键组合发送停止信号，但不会重新启动 X.org 会话。因此，选项 B 也是错误答案。Ctrl+Q 组合键将释放被 Ctrl+S 暂停的终端。但是，它不重新启动 X.org 会话，因此也是错误选择。Ctrl+Alt+Delete 按键组合可以根据你的桌面环境设置为执行多种任务。在某些情况下，它会弹出关机、注销或重启菜单。但是，它不重新启动 X.org 会话，因此选项 D 是错误选择。

- 18. C. 基于 Debian 和 Red Hat 的 Linux 发行版都将时区模板文件存储在 /usr/share/zoneinfo 文件夹中，因此选项 C 正确。/etc/timezone 和 /etc/localtime 文件包含 Debian 和基于 Red Hat 的系统的当前时区文件，而不是时区模板文件，因此选项 A 和 B 不正确。/usr/share/timezone 和 /usr/share/localtime 文件夹在基于 Debian 或 Red Hat 的 Linux 发行版中都不存在，因此选项 D 和 E 也不正确。

- 19. E. 用户帐户的用户名是 /etc/passwd 和 /etc/shadow 记录中唯一包含相同数据的字段。因此，选项 E 是正确答案。虽然两个文件都有密码字段，但它们不包含相同的数据。密码只能存在于两个文件中的一个，最好是 /etc/shadow 文件。因此，选项 A 是错误答案。帐户过期日期只存在于 /etc/shadow 文件中，因此选项 B 也是错误选择。UID 和 GID 字段只存在于 /etc/passwd 文件中，因此选项 C 和 D 也是错误答案。

- 20. D. rsyslogd 应用程序按定义的严重程度或更高级别优先处理日志事件消息，因此选项 D 会记录 warn、alert 或 emerg 严重级别的所有内核事件消息，所以它是正确的。选项 A 的设备和优先级设置只会记录严重程度为 warning 的内核消息，所以它是不正确的。选项 B 会记录所有内核事件消息，而不仅仅是警告或更高级别，所以它是不正确的。选项 C 会记录所有设备类型的事件消息，但包括信息或更高级别的严重程度，所以它是不正确的。选项 E 会记录内核事件消息，但仅在 alert 或 emerg 严重级别，而不是 warning 级别，所以它也是不正确的。

- 21. B、E。ntpd 和 chronyd 都可以在 Linux 上实现网络时间协议客户端服务，因此选项 B 和 E 正确。Exim、Sendmail 和 Postfix 都是用于 Linux 的邮件传输代理（MTA），因此选项 A、C 和 D 是错误选择。

- 22. B. Wi-Fi Protected Access (WPA) 加密协议保护对无线接入点的访问。无线网络在物理网络层运行，因此选项 B 正确。网络层使用 IP 等寻址协议在网络上的系统之间发送数据，但不与无线信号交互，因此答案 A 不正确。传输层使用端口将网络流量定向到运行在应用层的特定应用程序，因此选项 C 和 D 都不正确。

- 23. C 和 D。nmtui 命令提供用于选择网络接口和设置网络参数的交互式文本菜单，ip 命令提供用于设置网络参数的命令行工具，因此选项 C 和 D 都正确。netstat 命令显示网络连接的信息，但不设置网络参数，因此选项 A 不正确。ping 命令可以向远程主机发送 ICMP 数据包，但不设置本地网络参数，因此选项 B 不正确。route 命令设置路由网络参数，但不设置 IP 地址或子网掩码，因此选项 E 不正确。

- 24. C. ping 命令向指定的远程主机发送 ICMP 数据包并等待响应，因此选项 C 是正确答案。netstat 命令显示关于网络接口的统计信息，所以它不正确。ifconfig 命令显示或设置网络信息，但不发送 ICMP 数据包，因此选项 B 不正确。iwconfig 命令显示或设置无线网络信息，但不处理 ICMP 数据包，因此选项 D 不正确。ss 命令显示系统上打开的连接和端口的信息，因此选项 E 也不正确。

- 25. C. for 命令允许你逐个遍历一系列数据，直到数据集耗尽，因此选项 C 正确。if-then 和 case 语句对对象执行单一测试以确定是否应运行一段命令，它们不遍历数据，因此选项 A 和 B 不正确。exit 命令停止 shell 脚本并退出到父 shell，因此选项 D 不正确。$() 命令将命令的输出重定向到 shell 脚本中的变量，它不遍历一系列数据，因此选项 E 不正确。

- 26. B. $UID 环境变量包含运行 shell 脚本的用户帐户的数值用户 ID，因此选项 B 正确。$USER 环境变量包含运行 shell 脚本的用户帐户的文本用户名，而不是数值用户 ID，因此选项 A 不正确。$BASH 环境变量包含可执行 Bash shell 的路径，因此选项 C 不正确。$HOME 环境变量包含运行 shell 的用户帐户的主目录位置，因此选项 D 不正确。$1 位置变量包含运行 shell 脚本时命令行上列出的第一个参数，因此选项 E 不正确。





- 27. B. cron 表格格式指定按分钟、小时、日期、月份和星期几来运行脚本的时间。因此，格式 0 0 1 * * 将在每月的第一天 00:00（午夜）运行命令，适用于每个月。这就使得选项 B 正确，选项 A、C、D 和 E 不正确。

- 28. A, B. fuser 和 lsof 实用工具允许你通过打开的文件查看正在提供或使用的网络服务。因此，选项 A 和 B 是正确答案。虽然 nmap、netstat 和 ss 实用工具允许你查看系统上正在提供（或使用）的各种网络服务，但它们并非通过打开的文件来实现。因此，选项 C、D 和 E 是错误选择。

- 29. A, B, C. Port 指令确定 OpenSSH 守护进程 (sshd) 用于监听传入连接请求的端口，因此任何面向公众的系统都应将其从默认的 22 更改。因此，选项 A 是正确答案。Protocol 指令确定使用哪种 SSH 协议，为了确保使用 OpenSSH 2，应将其设置为 2。因此，选项 B 是另一个正确答案。PermitRootLogin 指令正如其名称所示——允许或拒绝 root 帐户通过 OpenSSH 登录，你不希望允许 root 帐户使用 ssh 登录到系统，因此选项 C 也是正确选择。AllowTCPForwarding 指令切换是否允许 OpenSSH 端口转发，ForwardX11 切换是否可以通过 OpenSSH 加密隧道转发 X11 命令，在某些情况下这可以增强安全性，但除非需要这些功能，否则不需要审查。因此，选项 D 和 E 是错误选择。

- 30. B, D, E. gpg-agent 独立于任何协议管理 GPG 密钥，将先前使用过的私钥保存在 RAM 中，如果需要不在 RAM 中的私钥，它会要求用户输入保护该密钥的口令。因此，选项 B、D 和 E 都是正确答案。gpg-agent 不会启动特殊的 agent shell（这是 ssh-agent 做的事情），因此选项 A 是错误答案。gpg-agent 不由 SysVinit 或 systemd 管理，而是由 gpg 实用工具自动启动。因此，选项 C 也是错误选择。

## 考试 101-500
