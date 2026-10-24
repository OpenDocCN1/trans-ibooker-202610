## ACKNOWLEDGMENTS

*As always, I want* to thank the staff at ETG/Marcraft for making it easy to turn out a good product. In particular, thanks to Cathy Boulay and Luke Johns from the Product Development department for their excellent work in getting the text and graphics ready to go and looking good.

Many thanks as well to Jeff Riley, whom I've known and worked with in the book production business for many years. Thanks for putting together another great project.

—Charles Brooks

I would like to start by thanking some of the many people who have made what has become my repository of knowledge and skill available to help make this book possible. First there is my father David P. Grow. His knowledge, mentoring, patience, and understanding started my journey down the career path of computer support and computer networking.

I would also like to thank all of my mentors along the way who have increased my skills and knowledge. Whether they were employers or colleagues, each mentor has made contributions to my knowledge and skill that helped make this all possible. Especially the support staff and leadership here with my current employer at ETG/Marcraft: Charles Brooks, Kevin Smith, Cathy Boulay, Grant Ter-Oganov and any personnel working behind the scenes that I did not meet.

Lastly I would like to thank my close friends and family for all their help and support as I worked through the process of creating my contribution to this book.

—Christopher Grow

To the folks who commit their lives and careers developing new approaches to cybersecurity that protects the immense landscape of computing infrastructures from acts of malicious and sometimes deadly outcomes of cyber attacks, I dedicate these works to you. The next generation of cyber-protectors will gain significant value from this book and hopefully will find its content sparking new dedication to the cyber challenges we will face in the years ahead.

To the leadership at ETG/Marcraft whose vision recognizes the value of the teaching through hands-on experiences and not just the texts, thank you for recognizing and implementing your approach to our trade.

—Philip Craig

I would like to thank my customers and associates from the past 25 plus years who have helped me grow and learn at a rate I would not have thought possible.

—Donald Short

## ABOUT THE AUTHORS

*Charles J. Brooks is* currently co-owner and vice president of Educational Technologies Group Inc., as well as co-owner of eITPrep LLP, an online training company. He is in charge of research and product development at both organizations.

A former electronics instructor and technical writer with the National Education Corporation, Charles taught and wrote on post-secondary ETG curriculum, including introductory electronics, transistor theory, linear integrated circuits, basic digital theory, industrial electronics, microprocessors, and computer peripherals.

Charles has authored several books, including seven editions of *A+ Certification Training Guide*, *The Complete Introductory Computer Course*, and *IBM PC Peripheral Troubleshooting and Repair*. He also writes about green technologies, networking, residential technology integration, and IT convergence.

*Christopher M. Grow is* currently the Technical Services Manager for Educational Technologies Group. He is responsible for product support, solution development, onsite implementation/installation, and instructor support and training for a wealth of cybersecurity and information technology products. He also is involved in program management and contributes in R&D of new products and revisions of current offerings.

Christopher has been a consultant and contractor in the IT industry for over 20 years. As an Information Security and Surveillance manager for a casino in Washington State, Christopher helped design and implement security policies, frameworks, and training to protect and segregate public and private information for the casino and their customers. He also helped to design procedures and train personnel on the physical security aspects of the casino industry.

*Philip Craig is the* founder of BlackByte Cyber Security, LLC, a consultancy supporting the Pacific Northwest National Laboratory (PNNL) research and national security agendas as well as the National Rural Electric Cooperative Association and National Rural Telecommunications Cooperative.

For many years, Phil served as a Senior Cyber Security Research Scientist at PNNL, where he provided engineering and program management support in the fields of cybersecurity, supervisory control and data acquisition (SCADA) technologies, computing, and communications infrastructure.

This included development of complex system and policy solutions in a variety of critical infrastructures including the nuclear power, electric power, and





## 关于作者

水务行业。他为电力和核行业开发并部署了战略性和战术性的网络安全防御解决方案。

Donald Short 是 One World Telecommunications, Inc. 的总裁，该公司是华盛顿州肯纳威克的一家互联网服务提供商，他既负责业务管理，也编写网页和数据库应用程序。

Don 拥有超过 35 年的药剂师和计算机科学家经验，曾在多种网络架构上使用多种编程语言工作，并开发了大型、复杂的在线内容和学习管理系统。

## 引言

*欢迎阅读《网络安全基础》。* 本书旨在为网络安全人员提供坚实的理论和实践平台。本版提供的关键信息包括：

- 关键基础设施安全系统和设备
- 本地智能计算、安全控制设备和系统
- 局域网组件和系统的安全
- 连接到互联网的用户和网络的网络安全

每章都以学习目标列表开始，建立基础并系统性地预览本章内容。

每章都包含大量图形图表和屏幕截图，以提供概念讨论的持续视觉强化。

章节中的关键思想、注意事项和警告以特殊框呈现，以引起额外关注。关键词在全文中以斜体呈现。这些术语也在本书末尾的综合词汇表中进行了定义，提供快速、便捷地访问每章出现的关键术语。

每个部分都以其内容的 extensive 重点回顾结束。

美国网络安全倡议持续发展的驱动力之一是美国国家标准与技术研究所（NIST）的网络安全框架。这些框架已开发出来，以协助政府和商业组织设计和开发系统和技术，为其关键基础设施提供安全保障。

## 安全挑战

本书的另一个出色教学特色是每个部分开头呈现的基于场景的 NIST 安全挑战。每个部分开头都有一个或多个基于场景的安全挑战，描述与将在本章中介绍的信息相关的特定安全设置。你将被要求阅读场景，扮演安全专业人员的角色，考虑你如何利用该场景的关键资产，然后思考你如何建立系统和策略来保护这些资产。





## 引言

这些挑战旨在为你提供真实、开放式的情境，以设定所学材料的期望水平。理想情况下，在你阅读章节的过程中，你会思考所遇到的理论和实践材料如何适用于这些情境。

在完成每个部分后，你将被要求返回这些安全挑战，并基于你增加的知识提出新的观察。你还将被要求将你的观察与专业安全专家对这些情境提供的反馈进行比较。

## 本书适合阅读人群

本书面向以下人群：

- 准备从事 IT、网络或网络安全职业的学生
- 希望提升网络安全技能的网络专业人员
- 需要了解所面临的网络安全威胁以及应对这些威胁的基本选项的管理人员

如果你对 CompTIA Security+ 或 Microsoft MTA – 98-367 Security Fundamentals 认证考试感兴趣，本书可以成为帮助你准备的绝佳资源。有关更多认证信息和资源，请参阅 https://certification.comptia.org/certifications/security 和 www.microsoft.com/en-us/learning/exam-98-367.aspx。

## 你将学到什么

你将学习如何以系统化的方法来保护 IT 网络和基础设施。这种方法始于从物理环境的外部边缘到存放最有价值资产的内部区域的物理安全问题。任何安全目标的第一半就是限制对资产的物理访问。如果无法接触到它，就无法窃取、损坏或毁坏它。你将学习以三层边界的概念来看待物理安全，并在每一层实施适当的工具。

在确保物理环境安全后，你将探索用于保护本地终端计算设备的工具和技术。遵循为物理安全制定的三层边界策略，你将从设备的外部边缘到其最宝贵的资产——你的数据，来解决这些设备的安全问题。

本节包括：监控摄像机、录像机、调制器和切换器。

第 4 章以入侵检测和报告系统部分完成基础设施安全材料。

第 5 章为第一部分中的情景和章节提供总结和复习。本章包括一份完整的相关要点总结列表和复习测验。它还将读者带回开始基础设施安全部分的情景，以便他们更新对情景挑战的回应，然后将其与活跃网络安全专业人员生成的回应进行比较。
