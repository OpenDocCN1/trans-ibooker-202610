

# 书籍收藏

# 学习路径

# 改进

# 你的

# 渗透

# 测试技能

使用 Kali Linux 和 Metasploit 加强您对网络攻击的防御

Gilberto Najera-Gutierrez, Juned Ahmed Ansari, Daniel Teixeira 和 Abhinav Singh

Packt
www.packt.com

WOW! eBook
www.wowebook.org

# Improving your Penetration Testing Skills

使用 Kali Linux 和 Metasploit 加强您对网络攻击的防御

Gilberto Najera-Gutierrez
Juned Ahmed Ansari
Daniel Teixeira
Abhinav Singh

![](img/6b6e557533100c0e47951d0e5e14c4ee_1_0.png)

BIRMINGHAM - MUMBAI

# Improving your Penetration Testing Skills

Copyright © 2019 Packt Publishing

保留所有权利。未经出版商事先书面许可，本书的任何部分不得以任何形式或任何手段复制、存储在检索系统中或传输，除非在简短引文嵌入关键文章或评论的情况下。

在准备本书时已尽一切努力确保所提供信息的准确性。然而，本书中包含的信息按原样出售，不提供任何明示或暗示的保证。作者、Packt Publishing 及其经销商和分销商均不对因本书直接或间接造成的任何损害承担责任。

Packt Publishing 已尽力提供有关本书中提及的所有公司和产品的商标信息，通过适当使用大写字母。然而，Packt Publishing 不能保证该信息的准确性。

首次出版：2019 年 6 月

生产参考：1180619

Packt Publishing Ltd 出版
Livery Place
35 Livery Street
Birmingham
B3 2PB, UK.

ISBN 978-1-83864-607-3

www.packtpub.com

![](img/6b6e557533100c0e47951d0e5e14c4ee_3_0.png)

mapt.io

Mapt 是一个在线数字图书馆，可让您完全访问超过 5,000 本书籍和视频，以及领先的行业工具，帮助您规划个人发展并推进您的职业生涯。有关更多信息，请访问我们的网站。

## 为什么订阅？

- 使用来自 4,000 多名行业专业人士的实用电子书和视频，花更少的时间学习，花更多的时间编码
- 使用专为你打造的技能计划提升你的学习
- 每月免费获得一本电子书或视频
- Mapt 完全可搜索
- 复制、粘贴、打印和书签内容

## Packt.com

您知道 Packt 为每本出版的书籍提供电子书版本吗？PDF 和 ePub 文件均可使用。您可以在 www.packt.com 升级到电子书版本，作为印刷书籍客户，您有权享受电子书副本的折扣。请联系 customercare@packtpub.com 了解更多详情。

在 www.packt.com，您还可以阅读大量免费技术文章，注册一系列免费通讯，并获得 Packt 书籍和电子书的独家折扣和优惠。

## 贡献者

## 关于作者

**Gilberto Najera-Gutierrez** 是一名经验丰富的渗透测试员，目前在一家澳大利亚顶级安全测试服务提供商工作。他获得了领先的安全和渗透测试认证，即 Offensive Security Certified Professional (OSCP)、EC-Council Certified Security Administrator (ECSA) 和 GIAC Exploit Researcher and Advanced Penetration Tester (GXPN)。

Gilberto 自 2013 年起担任渗透测试员，近 20 年来一直是安全爱好者。他已成功对墨西哥和澳大利亚一些最大公司、政府机构和金融机构的网络和应用程序进行了渗透测试。

**Juned Ahmed Ansari** 是一位驻孟买的网络安全研究员。他目前领导一家知名跨国公司渗透测试和进攻安全团队。Juned 曾担任大型私营企业顾问，指导其网络安全计划。他还与初创公司合作，帮助他们使最终产品安全。

Juned 曾举办过多次高级渗透测试培训课程，重点教授学生在高度安全环境中的隐蔽和规避技术。他的主要关注领域是渗透测试、威胁情报和应用程序安全研究。

**Daniel Teixeira** 是一名 IT 安全专家、作者和培训师，专注于红队行动、渗透测试和漏洞评估。他的主要关注领域是对手模拟、现代对手战术、技术和程序的仿真；漏洞研究和漏洞开发。

**Abhinav Singh** 是一位知名信息安全研究员。他是 Packt 出版的 Metasploit Penetration Testing Cookbook（第一版和第二版）和 Instant Wireshark Starter 的作者。他是安全社区的积极贡献者——论文出版物、文章和博客。他的作品已被多家安全和隐私杂志以及数字门户引用。他经常在著名的国际会议上发表演讲——Black Hat 和 RSA。他的专业领域包括恶意软件研究、逆向工程和云安全。

## Packt 正在寻找像你这样的作者

如果您有兴趣成为 Packt 的作者，请访问 `authors.packtpub.com` 并立即申请。我们已与数千名开发人员和技术专业人士合作，就像您一样，帮助他们与全球技术社区分享他们的见解。您可以进行一般申请，申请我们正在招聘作者的特定热门主题，或提交您自己的想法。

## 目录

- Preface 1
- Chapter 1: Introduction to Penetration Testing and Web Applications 8
  - Proactive security testing 9
    - Different testing methodologies 9
    - Ethical hacking 10
    - Penetration testing 10
    - Vulnerability assessment 10
    - Security audits 11
  - Considerations when performing penetration testing 11
    - Rules of Engagement 11
    - The type and scope of testing 11
    - Client contact details 12
    - Client IT team notifications 13
    - Sensitive data handling 13
    - Status meeting and reports 13
  - The limitations of penetration testing 14
  - The need for testing web applications 16
  - Reasons to guard against attacks on web applications 17
  - Kali Linux 17
  - A web application overview for penetration testers 18
    - HTTP protocol 18
    - Knowing an HTTP request and response 19
      - The request header 20
      - The response header 21
    - HTTP methods 22
      - The GET method 22
      - The POST method 23
      - The HEAD method 23
      - The TRACE method 23
      - The PUT and DELETE methods 23
      - The OPTIONS method 24
    - Keeping sessions in HTTP 24
      - Cookies 25
      - Cookie flow between server and client 25
      - Persistent and nonpersistent cookies 26
      - Cookie parameters 27
    - HTML data in HTTP response 27
      - The server-side code 28
    - Multilayer web application 28
      - Three-layer web application design 28
      - Web services 30
- Introducing SOAP and REST web services 30
- HTTP methods in web services 32
- XML and JSON 32
- AJAX 33
  - Building blocks of AJAX 34
  - The AJAX workflow 35
- HTML5 37
- WebSockets 37
- Chapter 2: Setting Up Your Lab with Kali Linux 39
  - Kali Linux 39
    - Latest improvements in Kali Linux 40
    - Installing Kali Linux 41
      - Virtualizing Kali Linux versus installing it on physical hardware 43
      - Installing on VirtualBox 44
        - Creating the virtual machine 44
        - Installing the system 47
  - Important tools in Kali Linux 54
    - CMS & Framework Identification 56
      - WPScan 56
      - JoomScan 56
      - CMSmap 57
    - Web Application Proxies 57
      - Burp Proxy 57
        - Customizing client interception 59
        - Modifying requests on the fly 59
        - Burp Proxy with HTTPS websites 60
      - Zed Attack Proxy 61
      - ProxyStrike 62
    - Web Crawlers and Directory Bruteforce 62
      - DIRB 62
      - DirBuster 62
      - Uniscan 63
    - Web Vulnerability Scanners 63
      - Nikto 63
      - w3af 64
      - Skipfish 64
    - Other tools 64
      - OpenVAS 64
      - Database exploitation 67
      - Web application fuzzers 67
      - Using Tor for penetration testing 67
  - Vulnerable applications and servers to practice on 69
    - OWASP Broken Web Applications 69
    - Hackazon 71
    - Web Security Dojo 71
    - Other resources 72
- Chapter 3: Reconnaissance and Profiling the Web Server 73
  - Reconnaissance 74

