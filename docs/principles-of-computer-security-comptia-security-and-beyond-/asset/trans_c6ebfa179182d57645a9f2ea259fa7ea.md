

（Exchange ActiveSync）。

电子邮件头应自下而上阅读。下面是一个标准邮件头示例（不包含扩展，稍后将会讨论），它展示了一封从 noah@starwars.com 发送到 jacob@beatles.com 的电子邮件。此示例中的 IP 地址是私有 IP 地址，因此无法在互联网上路由。

```
Delivered-To: jacob@beatles.com
Received: by 10.1.2.3 with SMTP id PeNnyLaNe67; Wed, 12 Aug 2020 13:06:52 -0400 (EDT)
Return-Path: noah@starwars.com
Received: from mail.starwars.com (mail.starwars.com [172.16.17.18]) by mx.beatles.com with SMTP id RoTs05.2020.08.12.13.06.52; Wed, 12 Aug 2020 13:06:52 -0400 (EDT)
Message-ID: <20200812130650.10314.mail@mail.starwars.com>
Received: from [192.168.1.108] by mail.starwars.com via HTTP; Wed, 12 Aug 2020 13:06:50 EDT
Date: Wed, 12 Aug 2020 13:06:50 -0400 (EDT)
From: Noah Weissman <noah@starwars.com>
Subject: This is for Daddy’s book!
To: Jacob Weissman <jacob@beatles.com>
```

邮件头会在三个时间点被添加到电子邮件中。第一个是 Noah 发送电子邮件时：

```
Date: Wed, 12 Aug 2020 13:06:50 -0400 (EDT)
From: Noah Weissman <noah@starwars.com>
Subject: This is for Daddy’s book!
To: Jacob Weissman <jacob@beatles.com>
```

Date: 和 From: 字段会自动填充，而 Subject: 和 To: 字段由 Noah 手动输入。

第二个时间点是电子邮件到达 Noah 的 SMTP 服务器 mail.starwars.com 时：

```
Message-ID: <20200812130650.10314.mail@mail.starwars.com>
Received: from [192.168.1.108] by mail.starwars.com via HTTP; Wed, 12 Aug 2020 13:06:50 EDT
```

365 是唯一一种通过 X-Originating-IP 标头字段包含源 IP 地址的基于 Web 的电子邮件服务。Gmail、Yahoo 以及所有其他服务都不包含它。原因可以归结为 2013 年 Microsoft Answers 论坛中的一则回复：

> 请注意，Microsoft 已选择屏蔽 X-Originating IP 地址。这是 Microsoft 方面为了保障客户的福祉与安全而计划进行的更改。

不过请记住，电子邮件头中的某些字段可以被伪造，而源 IP 地址就是非常容易被伪造的字段之一。

## 学习目标

在本实验练习中，你将探索电子邮件头。完成本实验练习后，你将能够：

- 查找任何域的 SMTP 服务器的完全限定域名（FQDN）和 IP 地址
- 阅读并解读电子邮件头

## 实验材料与设置

完成本实验所需的材料如下：

- *《计算机安全原理：CompTIA Security+ 及超越》*教科书
- 任意装有命令行界面（CLI）且可连接互联网的操作系统
- 可连接互联网的网页浏览器
- 一个 Gmail 帐户

## 开始动手吧！

你将同时使用 CLI 和图形用户界面（GUI）来获取与电子邮件相关的不同类型的信息。让我们马上开始吧！

![](img/6e921d997751f0a0f87e5170ac9657a1_626_0.png)

## 1c

第 1 步 要发现有关 SMTP 服务器的信息，你将使用 nslookup——一种用于发出特定 DNS 查询的网络/系统管理工具。这将模拟发送方的 SMTP 服务器如何发现目的地的 SMTP 服务器，并随后将电子邮件发送到目标域。

- a. 在 Windows 机器上打开命令提示符（或在 Linux 或 Mac 机器上打开终端，因为本章的实验练习可在任何平台上运行），然后输入

```
nslookup
```

以使用 nslookup 的交互模式。

- b. 要设置对 MX DNS 资源记录的查询，请输入以下内容：

```
set q=mx
```

- c. 现在指定你想查看其 SMTP 服务器的域。输入以下内容以发现 rit.edu 域的 SMTP 服务器，包括末尾的圆点，它指向 DNS 层次结构的根（如果没有末尾的圆点，你将无法获得预期结果）：

```
rit.edu.
```

你看到了多少个 RIT SMTP 服务器？

- d. 要获取 SMTP 服务器的 IP 地址，只需查询每台服务器的 A 资源记录即可。首先使用以下命令将查询类型改为 A：

```
set q=a
```

- e. 输入其中一个 RIT SMTP 服务器的 FQDN（无论末尾是否有圆点，此命令都会以相同方式工作）：

```
mx03c-in01r.rit.edu.
```

你应该会看到该 RIT SMTP 服务器的 IP 地址是 129.21.3.13。

- f. 用类似的方式，查找 FLCC SMTP 服务器的 MX 和 A 记录。

另一个网站。

- a. 前往 https://www.iptrackeronline.com/email-header-analysis.php 并粘贴另一个完整的电子邮件头。
- b. 点击“Submit Header For Analysis”按钮。
- c. 这个工具与之前的那个工具相比如何？

60 分钟

## 实验练习 17.02：SPF、DKIM 与 DMARC

垃圾邮件和网络钓鱼占了互联网上每日发送流量的大部分。美国国家标准与技术研究院（NIST）对垃圾邮件的定义如下：

> 电子垃圾邮件，或滥用电子消息系统不加区分地发送未经请求的群发消息。

垃圾邮件是未经请求的电子邮件，以群发方式同时发送给许多收件人。它也被称为垃圾邮件（junk mail），其主要目的是以陌生推销的方式，在互联网上向成百上千甚至更多的人销售产品或服务。

垃圾邮件也可以通过即时通讯（IM）或短信服务（SMS）发送。

垃圾邮件发送者从公开来源和数据库中收集电子邮件地址，甚至利用规律来生成可能的电子邮件地址（例如，bob1@gmail.com、bob2@gmail.com、bob3@gmail.com 等等）。

垃圾邮件的内容可以是产品广告、优惠券、推销信函、新闻通讯等等。垃圾邮件本质上是商业性的，不一定是恶意的。一些垃圾邮件定义包含了网络钓鱼的特征，甚至包括传播恶意软件的概念。

“spam”一词实际上源自 Monty Python 的一个短剧。你可以在 https://en.wikipedia.org/wiki/Spam_(Monty_Python) 阅读相关内容，并在 YouTube 上找到它的视频。

NIST 对网络钓鱼的定义如下：

> 一种通过电子邮件或网站中的欺诈性请求来尝试获取敏感数据（如银行账户号码）的技术，其中实施者伪装成合法企业或信誉良好的人。

垃圾邮件是商业性的、未经请求且不受欢迎的，而网络钓鱼则具有恶意设计，旨在对个人和企业造成伤害并窃取信息。钓鱼者会设计出看似来自可信发件人的合法电子邮件。钓鱼电子邮件旨在收集银行账户信息、用户名和密码，以及其他有助于窃取身份、盗取钱财并尽可能在多个层面上破坏机密性的信息。此外，恶意软件也会通过钓鱼电子邮件传播。

## 交叉引用

你在第 4 章中详细学习了网络钓鱼，并且实际上还发送了一封自己的钓鱼电子邮件。

为了阻止垃圾邮件和钓鱼电子邮件，已确立了三项标准。首先，发件人策略框架（SPF）允许将某个域的 SMTP 服务器公开发布为该域经授权的电子邮件发件人。其次，域名密钥识别邮件（DKIM）使用数字签名来验证消息来源，并确保电子邮件的完整性。第三，基于域的消息认证、报告与一致性（DMARC）允许域所有者告知接收邮件服务器：如果收到来自该域的伪造电子邮件，应如何处理。

## 学习目标

在本实验练习中，你将获得有关 SPF、DKIM 和 DMARC 的知识和实践经验。完成本实验练习后，你将能够：

- 理解 SPF 并查询 SPF 记录
- 理解 DKIM 并查询 DKIM 记录
- 理解 DMARC 并查询 DMARC 记录

