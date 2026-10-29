# Python 道德黑客从零开始：像道德黑客一样思考，避免被检测，并成功开发、部署、检测和避免恶意软件







## 贡献者

### 关于作者

**Fahad Ali Sarwar** 在不同的在线平台上教授道德黑客和渗透测试，拥有扎实的学生基础。他对网络安全和道德黑客工具开发充满热情。

Fahad 特别喜爱 Python 的简洁和易用性，在本书中，他选择专注于 Python，正是因为它所提供的特性。

### 关于审稿人

**Omar Ahmed** 专注于应用程序和网络渗透测试。他曾为各行各业的客户执行过数十次道德黑客任务，涵盖政府、金融、零售和制造业等领域。Omar 拥有独特的机会来评估新应用程序和技术的安全性，范围从支持 Web 的电子商务应用程序到专有应用程序。

他的安全职业生涯始于 2012 年，专注于网络和应用程序安全。Omar 在渗透测试、应用程序评估、社会工程（物理和虚拟）、漏洞评估和日志分析方面表现出色。您可以通过 Twitter 联系他：@mistspark。

**Marquel Waites** 是一名网络分析师和退伍军人，在美国陆军拥有 21 年的领导经验，未来的职业目标是成为 IT 安全总监（CISO）。在充满活力和快节奏的环境中，他带领超过 100 人的组织取得了可衡量的成果。他在财务管理运营、网络安全运营、事件响应协调、安全分析和监控、网络安全政策与技术合规性以及网络安全风险与漏洞管理方面拥有全面的背景。Marquel 在多个方面管理风险，以保护价值超过 2.56 亿美元的资产、财产和设备，同时超越高级行政利益相关者的期望。他曾多次因杰出表现和专业精神而获奖。他的职业背景包括：Trident University 的信息技术管理理学学士学位、Colorado Technical University 的网络安全政策理学硕士学位，以及 EC Council University 的网络安全与云安全架构理学硕士学位。他拥有多项认证，包括道德黑客认证、认证网络防御架构师和 Security+。

> *我要感谢 Packt 给我机会审阅这本书，希望这本书能在专业上帮助到每个人。我要感谢 EC Council University 的所有教授和 Coalfire 团队，他们不断激励我成为一名更好的网络安全专业人士。*

为什么订阅？191

其他您可能喜欢的书

索引





## 前言

道德黑客是一个庞大的领域，随着现代组织对网络安全的需求不断增加，道德黑客和渗透测试人员的需求也在迅速增长。本书旨在帮助你在网络安全领域开启你的旅程。Python 是一种通用编程语言，由 Guido van Rossum 于 1991 年开发。自那时起，它获得了巨大的粉丝群。Python 一直被黑客评为最受欢迎和最强大的编程语言，能够将其用于渗透测试是专业人士非常渴望掌握的技能。网络安全有太多内容需要学习，需要多年经验才能掌握这一领域的事物。几乎没有其他领域像网络安全一样变化如此迅速。在本书中，我们将通过学习黑客的基础知识开始我们的旅程，后续章节将重点学习黑客如何构建自己的工具。

## 本书适合谁

本书适用于那些希望通过自行开发工具来学习道德黑客，而不是仅仅使用预制工具的人；你将学习如何从零开始构建黑客工具。本书也适用于希望深入道德黑客世界的 Python 开发者。

## 本书涵盖内容

第 1 章，黑客入门，将帮助你学习黑客的基础知识。

第 2 章，入门 – 设置实验室环境，将帮助你设置实验室环境。

第 3 章，侦察和信息收集，涵盖了解受害者的内容。

第 4 章，网络扫描，教你如何探索本地网络。

第 5 章，中间人攻击，深入讲解如何攻击本地网络。

第 6 章，恶意软件开发，涵盖开发自己的恶意软件。

第 7 章，高级恶意软件，探讨开发高级功能。

第 8 章，后渗透，[content]

## 前言

Ethical hacking is a vast field, and with the increased need for cybersecurity in modern organizations, the demand for ethical hackers and penetration testers is increasing rapidly. This book aims to get you started on your journey in cybersecurity. Python is a general-purpose programming language that was developed by Guido van Rossum in 1991. Since then, it has gained an enormous fanbase. Python is consistently ranked as the most preferred and most powerful programming language by hackers, and being able to use it for penetration testing is a highly desired skill for professionals. There is so much to learn about cybersecurity that it takes many years of experience to get the hang of things in this field. There is hardly any other field that changes as rapidly as cybersecurity. In this book, we will start our journey by learning about the basics of hacking, and later in the book, we will focus on learning how hackers build their own tools.

## Who this book is for

This book is intended for people who want to learn about ethical hacking by developing tools themselves instead of just using prebuilt tools; you will learn how to build hacking tools from scratch. This book is also intended for Python developers who want to dive into the world of ethical hacking.

## What this book covers

Chapter 1, Introduction to Hacking, is where you will learn the fundamentals of hacking.

Chapter 2, Getting Started – Setting Up a Lab Environment, sees you set up a lab environment.

Chapter 3, Reconnaissance and Information Gathering, covers getting to know the victim.

Chapter 4, Network Scanning, teaches you how to explore local networks.

Chapter 5, Man in the Middle Attacks, goes into depth on how to attack a local network.

Chapter 6, Malware Development, covers developing your own malware.

Chapter 7, Advanced Malware, explores developing advanced features.

Chapter 8, Post Exploitation, looks at exploiting the victim machine.

Chapter 9, System Protection and Perseverance, is all about protecting your system against external attacks.

## To get the most out of this book

To get the most out of this book, try to follow all the examples included. This book is designed to be hands-on, so practicing the development exercises will help you gain proper insight into attack methodologies. This book assumes that you are familiar with the Python programming language.

| Software/hardware covered in the book | OS requirements |
|---|---|
| Python version 3.8 | Windows, macOS X, or Linux |
| VMware/VirtualBox |  |
| Visual Studio Code IDE |  |

If you are using the digital version of this book, we advise you to type the code yourself or access the code via the GitHub repository (link available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.

## Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/Python-Ethical-Hacking. In case there's an update to the code, it will be updated on the existing GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: http://www.packtpub.com/sites/default/files/downloads/9781838829506_ColorImages.pdf.

## Conventions used

There are a number of text conventions used throughout this book.

Code in text: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. Here is an example: "This will open a dialog box, and you can select the kali machine ova file you just downloaded."

A block of code is set as follows:

```
subprocess.run(
    ["ifconfig", "eth0"],
    shell=True,
)
```

Any command-line input or output is written as follows:

```
sudo dpkg -i /path/to/file
```

**Bold**: Indicates a new term, an important word, or words that you see onscreen. For example, words in menus or dialog boxes appear in the text like this. Here is an example: "Press **Yes** and you will see the following screen."

**Tips or important notes**
Appear like this.

## Get in touch

Feedback from our readers is always welcome.

**General feedback**: If you have questions about any aspect of this book, mention the book title in the subject of your message and email us at customercare@packtpub.com.

**Errata**: Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you would report this to us. Please visit www.packtpub.com/support/errata, selecting your book, clicking on the Errata Submission Form link, and entering the details.

**Piracy**: If you come across any illegal copies of our works in any form on the Internet, we would be grateful if you would provide us with the location address or website name. Please contact us at copyright@packt.com with a link to the material.

**If you are interested in becoming an author**: If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit authors.packtpub.com.

## Reviews

Please leave a review. Once you have read and used this book, why not leave a review on the site that you purchased it from? Potential readers can then see and use your unbiased opinion to make purchase decisions, we at Packt can understand what you think about our products, and our authors can see your feedback on their book. Thank you!

For more information about Packt, please visit `packt.com`.
