

为了能够仅在租户支持代理的工作时间内将新工单路由给他们，WolfDesk 允许录入每个代理的班次安排。

由于 WolfDesk 以零最低费用提供服务，它必须以最小化新租户入驻成本的方式优化其基础设施。为此，WolfDesk 利用无服务器计算，可根据活跃工单上的操作弹性扩展其计算资源。

## 本书使用的约定

本书使用以下排版约定：

*Italic*

表示新术语、URL、电子邮件地址、文件名和文件扩展名。

*Constant width*

用于程序列表，以及在段落中引用程序元素，如变量或函数名、数据库、数据类型、环境变量、语句和关键字。

> NOTE
> 
> 此元素表示一般性说明。

## 使用代码示例

补充材料（代码示例、练习等）可从 https://learning-ddd.com 下载。

本书中呈现的所有代码示例均使用 C# 语言实现。通常，您在章节中看到的代码示例是展示所讨论概念的摘录。

当然，本书中讨论的概念和技术并不局限于 C# 语言或面向对象编程方法。所有内容对其他语言和其他编程范式同样相关。因此，请随意使用您喜欢的语言实现本书的示例并与我分享。我很乐意将它们添加到本书网站上。

如果您对代码示例有技术问题或使用问题，请发送电子邮件至 bookquestions@oreilly.com。

本书旨在帮助您完成工作。一般来说，如果本书提供了示例代码，您可以在自己的程序和文档中使用它。除非您复制代码的很大一部分，否则无需联系我们征求许可。例如，编写一个使用本书中多个代码片段的程序不需要许可。出售或分发 O’Reilly 书籍中的示例需要许可。引用本书并引用示例代码来回答问题不需要许可。将大量示例代码纳入您产品的文档中需要许可。

我们感谢，但通常不要求署名。署名通常包括标题、作者、出版商和 ISBN。例如：“Learning Domain-Driven Design by Vlad Khonononov (O’Reilly). Copyright 2022 Vladislav Khononov, 978-1-098-10013-1。”

如果您认为代码示例的使用超出了合理使用范围或上述许可，请随时联系我们 permissions@oreilly.com。

## O’Reilly Online Learning

### NOTE

40 多年来，*O’Reilly Media* 一直为公司提供技术和商业培训、知识和见解，帮助公司取得成功。

我们独特的专家和创新者网络通过书籍、文章和我们的在线学习平台分享他们的知识和专业知识。O’Reilly 的在线学习平台为您提供按需访问实时培训课程、深入学习路径、交互式编码环境，以及来自 O’Reilly 和 200 多家其他出版商的庞大文本和视频收藏。欲了解更多信息，请访问 http://oreilly.com。

## 如何联系我们

请将有关本书的评论和问题发送给出版商：

O’Reilly Media, Inc.

1005 Gravenstein Highway North

Sebastopol, CA 95472

800-998-9938（美国或加拿大境内）

707-829-0515（国际或本地）

707-829-0104（传真）

我们为本书设有网页，在该网页上列出勘误表、示例和任何其他信息。您可以在 https://oreil.ly/lddd 访问此页面。

发送电子邮件至 bookquestions@oreilly.com 对本书发表评论或询问技术问题。

有关我们的书籍和课程的新闻和信息，请访问 http://oreilly.com。

在 Facebook 上找到我们：http://facebook.com/oreilly

在 Twitter 上关注我们：http://twitter.com/oreillymedia

在 YouTube 上观看我们：http://youtube.com/oreillymedia

## 致谢

最初，这本书的标题是“什么是领域驱动设计？”并于 2019 年作为报告出版。没有这份报告，《学习领域驱动设计》就不会问世，我必须感谢那些使“什么是领域驱动设计？”成为可能的人：Chris Guzikowski、Ryan Shaw 和 Alicia Young。¹

如果没有 O’Reilly 的内容总监兼多元化人才负责人 Melissa Duffield，这本书也不可能实现，她推动了该项目并使其成为现实。谢谢你，Melissa，感谢你所有的帮助！

Jill Leonard 是本书的开发编辑、项目经理和主教练。Jill 在这项工作中的作用怎么强调都不为过。Jill，非常感谢你所有的辛勤工作和帮助！特别感谢你在我考虑改名并躲到外国时仍然保持我的动力。

非常感谢制作团队让这本书不仅可写而且可读：Kristen Brown、Audrey Doyle、Kate Dullea、Robert Romano 和 Katherine Tozer。顺便说一下，我想感谢整个 O’Reilly 团队你们所做的大好工作。与你们合作是梦想成真！

感谢我采访和咨询的所有人：Zsofia Herendi、Scott Hirleman、Trond Hjorteland、Mark Lisker、Chris Richardson、Vaughn Vernon 和 Ivan Zakrevsky。感谢你们的智慧，并在我需要帮助时在那里！

特别感谢阅读早期草稿并帮助我塑造最终书籍的审阅团队：Julie Lerman、Ruth Malan、Diana Montalion、Andrew Padilla、Rodion Promyshlenikov、Viktor Pshenitsyn、Alexei Torunov、Nick Tune、Vasiliy Vasilyuk 和 Rebecca Wirfs-Brock。你们的支持、反馈和批评帮助很大。谢谢！

我还要感谢 Kenny Baas-Schwegler、Alberto Brandolini、Eric Evans、Marco Heimeshoff、Paul Rayner、Mathias Verraes 以及其他优秀的领域驱动设计社区成员。你们知道自己是谁。你们是我的老师和导师。感谢你们在社交媒体、博客和会议上分享知识！

我最感激我亲爱的妻子 Vera，她始终支持我在这些疯狂的项目中，并试图保护我免受可能分散我写作注意力的事情的干扰。我保证最终会清理地下室。很快就会实现！

最后，我想将这本书献给我们深爱的 Galina Ivanovna Tyumentseva，她在这项项目中给予了我很多支持，而我们在撰写本书期间不幸失去了她。我们将永远记住您。

#AdoptDontShop

## 引言

软件工程很难。要想在其中取得成功，我们必须不断学习，无论是尝试新语言、探索新技术，还是跟上新的流行框架。然而，每周学习一个新的 JavaScript 框架并不是我们工作的最困难方面。理解新的业务领域可能更具挑战性。

在整个职业生涯中，我们经常需要为各种各样的业务领域开发软件：金融系统、医疗软件、在线零售商、营销等。从某种意义上说，这就是我们的工作与其他大多数职业不同的地方。其他领域工作的人在发现软件工程涉及如此多的学习时常常感到惊讶，尤其是在更换工作场所时。

未能把握业务领域会导致业务软件实现不佳。不幸的是，这很常见。据研究，大约 70% 的软件项目无法按时、按预算或按照客户要求交付。换句话说，绝大多数软件项目都失败了。这个问题如此根深蒂固和普遍，以至于我们甚至有一个专门的术语来称呼它：软件危机。

*软件危机* 一词早在 1968 年就被引入。¹ 人们会认为在过去的 50 年里情况会有所改善。在这些年里，引入了许多方法、方法论和学科来使软件工程更有效：敏捷宣言、极限编程、测试驱动开发、高级语言、DevOps 等等。不幸的是，情况并没有改变太多。项目仍然经常失败，软件危机仍然存在。

已经进行了许多研究来调查常见项目失败的原因。² 尽管研究人员无法 pinpoint 唯一原因，但他们的大多数发现都有一个共同点

