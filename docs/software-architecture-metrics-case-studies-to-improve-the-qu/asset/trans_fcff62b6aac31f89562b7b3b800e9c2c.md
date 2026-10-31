
**提示**：此元素表示提示或建议。
**注意**：此元素表示一般性说明。
**警告**：此元素表示警告或注意。

# 使用代码示例

PROD: 请联系作者确认他们是否会将代码示例上传到 oreilly.com 或其自己的网站（例如 GitHub）。如果没有代码下载，请删除整个章节。如果有，在向 digidist 发送邮件并附上链接时，请告知他们您填写的 `title_title`（应尽可能接近书名，例如 `learning_python_2e`）。此信息将决定 digidist 加载文件的位置。
补充材料（代码示例、练习等）可在 https://github.com/oreillymedia/title_title 下载。
如果您在使用代码示例时有技术问题或遇到困难，请发送电子邮件至 bookquestions@oreilly.com。
本书旨在帮助您完成工作。一般而言，如果本书提供了示例代码，您可以在自己的程序和文档中使用它。除非您复制了代码的很大一部分，否则无需联系我们获得许可。例如，使用本书中若干代码片段编写程序不需要许可。出售或分发 O’Reilly 书籍中的示例则需要许可。通过引用本书并引用示例代码来回答问题不需要许可。将大量示例代码纳入您产品的文档中则需要许可。
我们感谢但通常不要求署名。署名通常包括书名、作者、出版商和 ISBN。例如：“Software Architecture Metrics by Christian Ciceri et al. (O’Reilly). Copyright 2022 Apiumhub S.L., 978-1-098-11216-5。”
如果您认为对代码示例的使用超出了合理使用范围或上述许可，请随时联系我们 permissions@oreilly.com。

# O’Reilly 在线学习

### 注意
四十多年来，O’Reilly Media 一直为企业提供技术和商业培训、知识和见解，帮助企业取得成功。
我们独特的专家和创新者网络通过书籍、文章和我们的在线学习平台分享知识和专业技能。O’Reilly 的在线学习平台为您提供按需访问实时培训课程、深入学习路径、交互式编程环境，以及来自 O’Reilly 和 200+ 其他出版商的庞大文本和视频收藏。更多信息请访问 https://oreilly.com。

# 如何联系我们

请将有关本书的评论和问题发送给出版商：
* O’Reilly Media, Inc.
* 1005 Gravenstein Highway North
* Sebastopol, CA 95472
* 800-998-9938（在美国或加拿大）
* 707-829-0515（国际或本地）
* 707-829-0104（传真）
我们为本书提供了一个网页，在此列出勘误、示例和任何其他信息。您可以通过 https://oreil.ly/software-architecture-metrics 访问该页面。
请不要忘记更新上面的链接。
发送电子邮件至 bookquestions@oreilly.com 以对本书发表评论或提出技术问题。
有关我们的书籍和课程的新闻和信息，请访问 https://oreilly.com。
在 LinkedIn 上找到我们：https://linkedin.com/company/oreilly-media。
在 Twitter 上关注我们：https://twitter.com/oreillymedia。
在 YouTube 上观看我们：https://www.youtube.com/oreillymedia。

# 致谢

本书若没有作者、O’Reilly 编辑和 Apiumhub 的贡献就不可能实现，他们将我们聚集在一起。我们还想额外感谢：
* Apiumhub 首席营销官 Ekaterina Novoseltseva，她负责本书的撰写过程以及与 O’Reilly 的出版工作，并撰写了导言
* O’Reilly 高级收购编辑 Melissa Duffield，她照顾了我们，使我们在 O’Reilly 的体验顺畅愉快
* O’Reilly 发展编辑 Sarah Grey，她构建了我们的内容，使其易于阅读
* O’Reilly 生产团队：Katherine Tozer、Adam Lawrence、Steve Fenton、Gregory Hyman 和 Kristen Brown，他们对本书进行了校对和发行

## Christian Ciceri
我要感谢 Ekaterina Novoseltseva 和 Apiumhub 给予我撰写这本书的机会，这一直是我的梦想。感谢 Global Software Architecture Summit 让我结识这些软件架构师，他们推动我前进并引发有趣的讨论。感谢 VYou app，让我创新并实施新的软件架构指标。还要特别感谢我的猫，它始终陪伴在我身边，在任何情况下都支持我。

## Dave Farley
感谢 Apiumhub 和 O’Reilly 的同事们为驯服“猫群”并组织我以及其他人，使本书成为可能。

## Neal Ford
感谢 Ekaterina 和 Apiumhub 的其他同事们做了实现这一切所需的“猫群管理”。感谢我的雇主 Thoughtworks 以及所有员工，他们总以在技术世界中的热情和投入程度让我惊讶。最后并且始终，感谢我的妻子 Candy 忍受我所有的写作，这让我远离了她和我们正在长大的小猫。

## Andrew Harmel-Law
感谢我的妻子和孩子们容忍我，也感谢 Thoughtworks 的同事们激励我并让我将这种方法推向逻辑的结论。
如果我没有机会在一个真正理解并信任我的组织中将这一切付诸实践，本章中的论断可能只是一半理论。Open GI 是英国和爱尔兰保险行业的专业 SaaS 提供商，是那家客户，感谢我在那里工作的每一个人。我的共同开发者/同谋 Pete Hunter 值得特别提及。他立即理解了我们在做什么，不懈地倡导它，不懈地改进它（我们在每一步都结对进行），并教会了我很多关于如何使其发挥作用的知识。
最后感谢 Ekaterina 和 Apiumhub 邀请我参与，催促我，并回答我所有的愚蠢问题。

## Michael Keeling
我衷心感谢 Anastas Stoyanovsky、Colin Dean、George Fairbanks、Joe Runde 和 Ricky Kotermanski，他们都帮助审阅了早期章节草稿。此外，感谢我所有同事，无论是现任还是前任，我有幸与他们共事。像本书中这样的经验报告只能由敢于冒险并尝试新想法的团队撰写！永远不要停止寻求变得比现在更出色的办法！
Marie，我的女王，感谢你帮我抽出时间从事像这样写作项目。Owen，谢谢你。Finn，saaanoot！

## Carola Lilienthal
