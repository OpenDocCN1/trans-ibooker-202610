

# C# 8 和 .NET Core 3 使用 Azure 的项目

## 第二版

构建满足现代软件要求的专业桌面、移动和 Web 应用程序

![](img/762492a8287f369cc1ac4621a212f193_0_0.png)

- Paul Michaels
- Dirk Strauss
- Jas Rademeyer

Packt
www.packt.com
BIRMINGHAM - MUMBAI

# C# 8 和 .NET Core 3 使用 Azure 的项目

## 第二版

版权所有 © 2019 Packt Publishing

版权所有。未经出版商事先书面许可，除了批判性文章或评论中嵌入的简短引文外，本书任何部分不得以任何形式或以任何方式复制、存储在检索系统中或传播。

在编写本书的过程中，已尽力确保所呈现信息的准确性。但是，本书中包含的信息不提供任何明示或默示的担保而出售。作者、Packt Publishing 及其经销商和分销商对因本书直接或间接造成或声称造成的任何损害概不负责。

Packt Publishing 已致力于通过适当使用大写字母提供本书中提及的所有公司和产品的商标信息。但是，Packt Publishing 不能保证此信息的准确性。

- **委托编辑：** Richa Tripathi
- **获取编辑：** Alok Dhuri
- **内容开发编辑：** Digvijay Bagul
- **高级编辑：** Rohit Singh
- **技术编辑：** Pradeep Sahu
- **文字编辑：** Safis Editing
- **项目协调员：** Francy Puthiry
- **校对员：** Safis Editing
- **索引编制：** Priyanka Dhadke
- **制作设计师：** Alishon Mendonsa

初版：2018 年 3 月
第二版：2019 年 12 月

制作参考号：1301219

由 Packt Publishing Ltd. 出版。
Livery Place
35 Livery Street
伯明翰
B3 2PB，英国。

ISBN 978-1-78961-208-0

www.packt.com

致我的女儿 Abigail，她慷慨、高尚、机智、幽默，并相信我；以及我的儿子 Dylan，他比我聪明得多，善良、温和，总是面带微笑。我视自己为世上最富有的人，能称你们俩为我的孩子，我为你们俩感到骄傲！

- Paul Michaels

# Packt

Packt.com

订阅我们的在线数字图书馆，即可完整访问超过 7,000 本书籍和视频，以及行业领先的工具，帮助您规划个人发展并推进职业生涯。如需更多信息，请访问我们的网站。

# 为什么要订阅？

- 利用超过 4,000 位行业专业人士提供的实用电子书和视频，减少学习时间，增加编码时间
- 使用专为您构建的技能计划，改善您的学习体验
- 每月获取一本免费电子书或视频
- 完全可搜索，轻松访问重要信息
- 复制粘贴、打印和加书签内容

你知道吗？Packt 为每本出版的书籍提供电子书版本，并提供 PDF 和 ePub 文件。您可以在 www.packt.com 升级到电子书版本，作为印刷书籍客户，您有权获得电子书副本的折扣。如需更多详情，请联系 customercare@packtpub.com。

在 www.packt.com 上，您还可以阅读一系列免费技术文章，订阅各种免费简报，并获取 Packt 图书和电子书的独家折扣和优惠。

# 贡献者

# 关于作者

**Paul Michaels** 是一名拥有 20 多年经验的首席开发人员。他喜欢编程、玩转新技术和解决问题。不工作时，您可以看到他在峰区骑自行车或散步、打乒乓球，或者尝试为妻子和两个孩子做饭。您可以在 Twitter 上关注他 @paul_michaels，或在 LinkedIn 上搜索 pcmichaels 找到他。他还写博客，链接可在他的 LinkedIn 和 Twitter 个人资料中找到。

> *特别感谢 Abigail Michaels 为本书提供了插画。*

**Dirk Strauss** 是 Embrace 的全栈开发人员。他喜欢学习并与他人分享所学内容。Dirk 为 Packt 出版了关于 C# 的书籍，也为 Syncfusion 出版了电子书。业余时间，他通过弹吉他和尝试学习 Jimi Hendrix 的乐句来放松。您可以在 Twitter 上找到他 @DirkStrauss。

> *我想感谢我的妻子、儿子和女儿的支持，她们始终陪伴在我身边。我全心全意地爱你们。*

**Jas Rademeyer** 在 IT 行业拥有超过 15 年的经验，职业生涯大部分时间专注于软件方面。他拥有信息科学学位，专攻多媒体，参与过开发的各个方面，从架构和解决方案设计到用户体验和培训。他目前担任技术解决方案经理，负责管理微软领域各种项目上的开发团队。他是一个重视家庭的人，内心是个音乐家，业余时间与妻子和两个孩子共度，并在教堂敬拜乐队中服事。

# 关于评审员

Alvin Ashcraft 是一名居住在费城附近的开发人员。他 23 年的职业生涯一直在使用 C#、Visual Studio、WPF、ASP.NET 等构建软件。他九次荣获 Microsoft MVP 称号。您可以在他的博客 Morning Dew 上阅读他每天为 .NET 开发人员分享的链接。他担任 Allscripts 的首席软件工程师，构建医疗保健软件。他之前曾受雇于包括 Oracle 在内的软件公司。他还为 Packt Publishing 评审过其他书名，例如 Mastering ASP.NET Core 2.0、Mastering Entity Framework Core 2.0 和 Learning ASP.NET Core 2.0。

> 我想感谢我可爱的妻子 Stelene 和我们三个了不起的女儿的支持。在我晚上和周末阅读并评审这些章节以帮助为 .NET 开发者交付一本有用、高质量的书籍时，她们非常体谅。

# Packt 正在寻找像您这样的作者

如果您有兴趣成为 Packt 的作者，请访问 authors.packtpub.com 并立即申请。我们与成千上万像您一样的开发人员和技术专业人士合作，帮助他们向全球技术社区分享见解。您可以提交通用申请，申请我们正在招募作者的特定热门话题，或提交自己的想法。

## 目录

- 前言 1
- 第 1 章：电子书管理器和目录应用 - .NET Core 桌面应用 7
  - 技术要求 8
  - 创建新 WinForms 应用程序 8
    - 虚拟存储空间和扩展方法 20
    - DocumentEngine 类 24
    - ImportBooks 窗体 25
      - 填充 TreeView 控件 29
      - 填充存储空间列表 31
      - 将选定的书籍保存到存储空间 35
    - 创建主 eBookManager 窗体 37
    - 运行 eBookManager 应用程序 43
  - 升级到 .NET Core 3 47
    - 修复编译错误 52
    - 资源文件 53
    - eBookManager 屏幕 54
    - importBooks 屏幕 56
    - ProcessStartInfo 58
    - 升级到 .NET Core 3 的优势 59
  - 了解可空引用类型 60
  - 探索 XAML 岛 64
    - UWP TreeView 64
    - WindowsXamlHost 65
    - ItemTemplate 66
    - TreeView 项目模型和 ItemsSource 67
    - 删除现有 TreeView 71
  - Tree shaking 和编译为单个可执行文件 72
  - 摘要 73
- 第 2 章：使用 Cosmos DB 的 ASP.NET Core MVC 应用任务 Bug 日志记录 74
  - 技术要求 74
  - 使用 Cosmos DB 的优势 75
    - 为什么选择云？扩展是微软的工作，不是你的 75
    - 为什么选择 Cosmos？与行业领先的 API 兼容且支持全球分布 75
  - 设置 Azure Cosmos DB 76
    - 订阅/资源组 79
    - 账户名 81
    - API 81

