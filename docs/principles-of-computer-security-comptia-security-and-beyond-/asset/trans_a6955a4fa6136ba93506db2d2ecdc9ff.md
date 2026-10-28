

## 2.2 盘和文件系统管理

磁盘（Disk）是计算机系统中存储数据的主要设备，文件系统（File System）是操作系统管理和存储文件的方法。在 Windows 系统中，常见的磁盘管理工具有磁盘管理（Disk Management）和命令行工具 DiskPart。文件系统常见的有 FAT32、NTFS 等。

磁盘管理是 Windows 系统提供的一个图形化管理工具，可以查看和管理磁盘分区、格式化磁盘、更改驱动器号等。DiskPart 是命令行下的磁盘管理工具，功能更加强大，可以完成许多磁盘管理任务。

- 查看磁盘信息
- 创建和删除分区
- 格式化磁盘
- 分配驱动器号
- 扩展和压缩卷

> 注意：格式化磁盘会清除磁盘上的所有数据，请谨慎操作。

前往 https://images.google.com/ 并使用 Google 图片搜索一个 JPEG/JFIF 文件以及一个带有 EXIF 元数据的 JPEG 文件。将文件从网站下载到你的硬盘中，并在 HxD 中打开它们。继续操作直到你各获得一个。截取每个文件的头信息。

- c. 前往我的 FLCC 页面 https://www.flcc.edu/directory/staff-profile.cfm?email=Jonathan.Weissman@flcc.edu&dept=430。
- 下载我的头像并检查 EXIF 数据。照片拍摄的日期和时间是什么？

- d. 用 HxD 打开任意一个 JFIF 文件，将 FF D8 FF E1 签名更改为 46 4C 43 43，通过在左侧窗格中点击并输入新的十六进制数字。你会注意到在解码文本部分出现了 FLCC 的 ASCII/Unicode 字符。
- 参考 asciitable.com 获取一个很好的字符值映射表。
- 选择 FLCC 中的最后一个 C 并按下键盘上的小写 c。你会注意到十六进制数字 43 变成了小写 c 的值 63。现在将 63 改回 43，并观察小写 c 变回大写 C。

- e. 点击 File，点击 Save As，输入文件名 flcc.jpg，浏览到桌面，然后点击 Save 按钮。

- f. 在桌面上双击 flcc.jpg 文件。会发生什么？为什么？

- g. 如果嫌疑人想进行反取证并挫败任何取证调查，那么嫌疑人是更改文件的扩展名还是更改文件的签名更好？为什么？

3c–3e

# Step 3 In this last step of the lab exercise, you can pivot in many different directions.

你即将进入一个精彩的取证场景的世界，其中包含文档、工件和证据。该场景描述如下，来自 https://digitalcorpora.org/corpora/scenarios/m57-patents-scenario：

# Chapter 24
法律问题与伦理

# Lab Exercises

- 24.01 ACM Ethics
- 24.02 USENIX Ethics
- 24.03 Ethical Scenarios
- 24.04 Copyright
- 24.05 Creative Commons
- 24.06 FSF and GNU
- Lab Analysis
- Key Term Quiz

伦理和道德都与“正确”和“错误”的行为有关。伦理是一套适用于个人或群体的行为原则，由外部来源提供，例如社会或职业。另一方面，道德是个体自身关于对错的原则。

以下是四个众所周知的伦理标准：

- **Rights** 个人的基本需求和福祉
- **Justice** 某项行动或政策的成本和收益如何在群体成员之间公平分配
- **Utility** 某项行动或政策对公众产生的积极和消极影响
- **Care** 我们与其他个人的关系

- 了解 ACM 伦理与职业行为准则
- 将 ACM 伦理与职业行为准则与你自己的经验联系起来

# Lab Materials and Setup

你需要完成本实验的材料有

- *Principles of Computer Security: CompTIA Security+ and Beyond* 教科书
- 带有互联网连接的网络浏览器

# Let’s Do This!

阅读 ACM 伦理与职业行为准则（版权所有 © 2018 由 Association for Computing Machinery）中列出的每项内容及其描述（参考 https://ethics.acm.org/）。对于每项内容，给出一个你作为计算专业人员在伦理或非伦理方面的例子。如果你从未遇到过所描述的情况，请提及这一点，但同时也添加你对所描述内容的想法。

1–4

Step 1 Give an example of where you, as a computing professional, acted ethically or non-ethically relating to these general ethical principles identified by the ACM.

# 1. GENERAL ETHICAL PRINCIPLES.

*A computing professional should...*

- 1.1 Contribute to society and to human well-being, acknowledging that all people are stakeholders in computing.
- 1.2 Avoid harm.
- 1.3 Be honest and trustworthy.
- 1.4 Be fair and take action not to discriminate.
- 1.5 Respect the work required to produce new ideas, inventions, creative works, and computing artifacts.

1.6 Respect privacy.
1.7 Honor confidentiality.

Step 2 Give an example of where you, as a computing professional, acted ethically or non-ethically relating to these professional responsibilities identified by the ACM.

# 2. PROFESSIONAL RESPONSIBILITIES.

A computing professional should...

- 2.1 Strive to achieve high quality in both the processes and products of professional work.
- 2.2 Maintain high standards of professional competence, conduct, and ethical practice.
- 2.3 Know and respect existing rules pertaining to professional work.
- 2.4 Accept and provide appropriate professional review.
- 2.5 Give comprehensive and thorough evaluations of computer systems and their impacts, including analysis of possible risks.
- 2.6 Perform work only in areas of competence.
- 2.7 Foster public awareness and understanding of computing, related technologies, and their consequences.
- 2.8 Access computing and communication resources only when authorized or when compelled by the public good.
- 2.9 Design and implement systems that are robustly and usably secure.

Step 3 Give an example of where you, as a computing professional, acted ethically or non-ethically relating to these professional leadership principles identified by the ACM.

# 3. PROFESSIONAL LEADERSHIP PRINCIPLES.

Leadership may either be a formal designation or arise informally from influence over others. In this section, “leader” means any member of an organization or group who has influence, educational responsibilities, or managerial responsibilities. While these principles apply to all computing professionals, leaders bear a heightened responsibility to uphold and promote them, both within and through their organizations.

*A computing professional, especially one acting as a leader, should...*

- 3.1 Ensure that the public good is the central concern during all professional computing work.
- 3.2 Articulate, encourage acceptance of, and evaluate fulfillment of social responsibilities by members of the organization or group.
- 3.3 Manage personnel and resources to enhance the quality of working life.
- 3.4 Articulate, apply, and support policies and processes that reflect the principles of the Code.
- 3.5 Create opportunities for members of the organization or group to grow as professionals.
- 3.6 Use care when modifying or retiring systems.
- 3.7 Recognize and take special care of systems that become integrated into the infrastructure of society.

**Step 4** Give an example of where you, as a computing professional, acted ethically or non-ethically relating to compliance with the ACM code.

# 4. COMPLIANCE WITH THE CODE.

*A computing professional should...*

- 4.1 Uphold, promote, and respect the principles of the Code.
- 4.2 Treat violations of the Code as inconsistent with membership in the ACM.

60 MINUTES

# Lab Exercise 24.02: USENIX Ethics

本实验练习还涉及分析伦理的良好基础。USENIX 支持先进的计算系统社区，并致力于推动创新研究的覆盖范围。虽然以组织会议和发表研究而闻名，但其最强大的优势在于在计算系统中建立社区。欲了解更多背景信息，请查看 USENIX 的网站 www.usenix.org/about。

# Note

USENIX 最初在 1975 年成立时被称为 Unix Users Group。1977 年，名称更改是因为他们被告知 UNIX 已被商标化。此后，他们一直是 USENIX：The Advanced Computing Systems Association。USENIX 不是缩写。

另请阅读关于 2016 年 LISA 特别兴趣小组（SIG） for Sysadmins 退役的文章，网址为 www.usenix.org/blog/refocusing-lisa-community。

如今，LISA 指的是大型安装系统管理大会（www.usenix.org/conferences/byname/5）。

最后，调查 League of Professional System Administrators（LOPSA）网站 https://lopsa.org/。LOPSA 推动系统管理实践，通过与系统管理问题相关的教育和外联为公众服务。从业者得到支持、认可、教育并受到鼓励。

# Learning Objectives

在本实验练习中，你将探索由 USENIX、LISA 和 LOPSA 于 2006 年共同签署的 USENIX 系统管理员伦理准则。在本实验练习结束时，你将能够

- 了解 USENIX 系统管理员伦理准则
- 将 USENIX 系统管理员伦理准则与你自己的经验联系起来

# Lab Materials and Setup

你需要完成本实验的材料有

