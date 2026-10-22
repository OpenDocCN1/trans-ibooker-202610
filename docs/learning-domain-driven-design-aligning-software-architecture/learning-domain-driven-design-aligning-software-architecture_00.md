# 领域驱动设计入门指南：使软件架构与业务战略保持一致










# Praise for *Learning Domain-Driven Design*

> “Vladik Khonononov is a unique thinker who has been applying DDD to solve real business problems for years. His ideas constantly move the whole DDD community forward, and this book will inspire beginning DDD practitioners.”
> 
> —Nick Tune, Technology Consultant

> “Reflecting on my readings of drafts of this book, the thing that comes to mind, with a great deal of joy at the thought, is that it delivers on its title! It is an inviting and informative practice guide, covering the scope of DDD from strategy to technical design. I’ve gained new insight and understanding in areas where I have experience and filled in concepts and practices I’d had less exposure to. Vlad is a wonderful teacher!”
> 
> —Ruth Malan, Architecture Consultant at Bredemeyer Consulting

> “Vlad has a lot of hard-won experience as a DDD practitioner working on some deeply complex projects and has been generous in sharing that knowledge. In this book, he tells the story of DDD in a unique way providing a great perspective for learning. This book is aimed at newcomers, yet as a longtime DDD practitioner who also writes and speaks about DDD, I found that I learned so much from his perspective.”
> 
> —Julie Lerman, Software Coach, O’Reilly Author, and Serial DDD Advocate






















# Revision History for the First Edition






[LSI]

# Foreword

Domain-driven design provides a set of practices for a collaborative approach to building software from the perspective of the business—that is, the domain, and its problems that you are targeting. It was originally coined by Eric Evans in 2003 with the publication of what is fondly known in the DDD community as “The Blue Book.” The book’s title is *Domain-Driven Design: Tackling Complexity in the Heart of Software.*

While tackling complexity and providing a path to clarity is the goal of domain-driven design, there are so many great ideas that can be applied to even less complicated software projects. DDD reminds us that software developers are not the only people involved in building software. The domain experts, for whom the software is being built, bring critical understanding of the problems being solved. We create a partnership throughout the stages of creation as we first apply “strategic design” to understand the business problem, a.k.a. the domain, and break the problem down into smaller, solvable, interconnected problems. The partnership with the domain experts also drives us to communicate in the language of the domain, rather than forcing those on the business side to learn the technical language of software.

The second stage of a DDD-based project is “tactical design,” where we transform the discoveries of strategic design into software architecture and implementation. Again, DDD provides guidance and patterns for organizing these domains and avoiding further complexity. Tactical design continues the partnership with the domain experts who will recognize their domain language even as they look at the code built by the software teams.

Over the years since the publication of “The Blue Book,” not only have many organizations benefited from the ideas, but a community of experienced DDD practitioners has evolved. And the collaborative nature of DDD has resulted in this community sharing their experiences and perspective and creating tools to help teams

embrace and benefit from these ideas. In a keynote at Explore DDD in 2019, Eric Evans encouraged the community to continue to evolve DDD—not only its practices but in finding ways to more effectively share its ideas.

And this brings me to why I am such a fan of *Learning Domain-Driven Design*. I was already a fan of Vlad through his conference speaking and other writings. He has a lot of hard-won experience as a DDD practitioner working on some deeply complex projects and has been generous in sharing that knowledge. In this book, he tells the “story” of DDD (not its history, but its concepts) in a unique way, providing a great perspective for learning. This book is aimed at newcomers, yet as a longtime DDD practitioner who also writes and speaks about DDD, I found that I learned so much from his perspective. I was eager to reference his book in my DDD Fundamentals course on Pluralsight before the book was even published and have already been sharing some of this perspective in conversations with clients.

Getting started with DDD can be confusing. Just as we use DDD to reduce the complexity of projects, Vlad presents DDD in a way that reduces the complexity of the topic itself. And he does more than explain the principles of DDD. The latter portion of the book shares some important practices that have evolved from DDD, such as EventStorming, addresses the problem of evolving the business focus or organization and how this might affect the software, and discusses how DDD aligns with microservices and how you can integrate it with a slew of well-known software patterns. I think *Learning Domain-Driven Design* will be an excellent introduction to DDD for newcomers, and a very worthy read for experienced practitioners as well.

*Julie Lerman*
*Software Coach, O’Reilly Author, and Serial DDD Advocate*

# Preface

I vividly remember the day I started my first real software engineering job. I was both ecstatic and terrified. After hacking software for local businesses during my high school years, I was eager to become a “real programmer” and write some code for one of the country’s largest outsourcing companies.

In my first days there, my new colleagues were showing me the ropes. After setting up the corporate email and going through the time-tracking system, we finally moved on to the interesting stuff: the company’s coding style and standards. I was told that “here, we always write well-designed code and use the layered architecture.” We went through the definition of each of the three layers—the data access, business logic, and presentation layers—and then discussed the technologies and frameworks for addressing the layers’ needs. Back then, the accepted solution for storing data was Microsoft SQL Server 2000, and it was integrated using ADO.NET in the data access layer. The presentation layer rocked either WinForms for desktop applications or ASP.NET WebForms for the web. We spent quite some time on these two layers, so I was puzzled when the business logic layer didn’t get any attention:

> “But what about the business logic layer?”

> “That one is straightforward. Here is where you implement the business logic.”

> “But what is business logic?”

> “Oh, business logic is all the loops and ‘if-else’ statements you need in order to implement the requirements.”

That day I began my journey to find out what exactly business logic is and how on earth it should be implemented in well-designed code.





花了我三年多时间才最终找到答案。

这个答案在 Eric Evans 的开创性著作 *Domain-Driven Design: Tackling Complexity in the Heart of Software* 中。事实证明我并没有错。业务逻辑确实很重要：它是软件的核心！不幸的是，随后又花了我三年时间才理解 Eric 分享的智慧。这本书非常先进，而英语是我的第三语言，这也没有帮助。

最终，一切都理顺了，我与领域驱动设计（DDD）方法论达成了和解。我学习了 DDD 的原则和模式，建模和实现业务逻辑的复杂性，以及如何应对我所构建的软件核心中的复杂性。尽管障碍重重，这绝对是值得的。深入领域驱动设计对我来说是一次改变职业生涯的经历。

### 我为何撰写本书

在过去的 10 年里，我向不同公司的同事介绍了领域驱动设计，进行了面对面授课，并教授在线课程。教学视角不仅帮助我加深了知识，也让我能够优化解释领域驱动设计原则和模式的方式。

正如经常发生的那样，教学比学习更具挑战性。我是 Eliyahu M. Goldratt 作品和教导的忠实拥趸。Eliyahu 曾说，即使是最复杂的系统，从正确的角度看也是本质上简单的。在我多年教授 DDD 的过程中，我一直在寻找一种能够揭示领域驱动设计固有简单性的方法论模型。

本书是我努力的结果。它的目标是使领域驱动设计民主化；使其更容易理解，更易于采用。我相信 DDD 方法论绝对无价，尤其是在设计现代软件系统时。本书将为你提供足够多的工具，让你在日常工作中开始应用领域驱动设计。

### 谁应该阅读本书

我相信，掌握领域驱动设计原则和模式的知识对所有级别的软件工程师都有用：初级、高级、员工级和首席工程师。DDD 不仅提供建模和有效实现软件的工具和技术，还阐明了软件工程中一个常被忽视的方面：上下文。配备了对系统业务问题的了解，你将更有效地选择合适的解决方案。一个既不过度也不欠设计的解决方案，而是满足业务需求和目标的解决方案。

领域驱动设计对软件架构师更重要，对有志成为软件架构师的人更是如此。其战略设计决策工具将帮助你将大型系统分解为组件——服务、微服务或子系统——并设计组件如何集成以形成系统。

最终，在本书中我们将讨论的不仅是如何设计软件，还如何随着其业务上下文的变化而共同演进设计。这种软件工程的关键方面将帮助你随着时间的推移保持系统设计的“良好状态”，并防止其退化成一团乱麻。

### 如何阅读本书

本书分为四个部分：战略设计、战术设计、DDD 实践，以及 DDD 与其他方法论和模式的关系。在第一部分，我们介绍用于做出大规模软件设计决策的工具和技术。在第二部分，我们关注代码：实现系统业务逻辑的不同方式。

第三部分讨论在实际项目中应用 DDD 的技术和策略。第四部分继续讨论领域驱动设计，但这次是在其他方法论和模式的背景下。

以下是各章节内容的简要总结：

- 第 1 章确立了软件工程项目的背景：业务领域、其目标，以及软件打算如何支持这些目标。
- 第 2 章介绍了“通用语言”的概念：领域驱动设计用于有效沟通和知识共享的实践。
- 第 3 章讨论如何应对业务领域的复杂性并设计系统的高级架构组件：有界上下文。
- 第 4 章探讨组织有界上下文之间通信和集成的不同模式。
- 第 5 章以两种针对简单业务逻辑情况的模式开始讨论业务逻辑实现模式。
- 第 6 章从简单业务逻辑推进到复杂业务逻辑，并引入领域模型模式来应对其复杂性。
- 第 7 章增加了时间的视角，并引入了一种更先进的建模和实现业务逻辑的方法：事件溯源领域模型。
- 第 8 章将焦点转移到更高层级，并描述了三种用于构建组件的架构模式。
- 第 9 章提供了协调系统组件工作的所需模式。
- 第 10 章将前几章讨论的模式结合起来，形成一系列简化的经验法则，以简化设计决策过程。
- 第 11 章从时间的角度探讨软件设计，以及它在其生命周期中应如何变化和演进。
- 第 12 章介绍 EventStorming：一种低技术工作坊，用于有效共享知识、建立共同理解并设计软件。
- 第 13 章解决了在棕地项目中引入领域驱动设计时可能遇到的困难。
- 第 14 章讨论微服务架构风格与领域驱动设计之间的关系：它们在何处不同，在何处互补。
- 第 15 章在事件驱动架构的背景下探讨领域驱动设计模式和工具。
- 第 16 章将讨论从运营系统转移到分析数据管理系统，并讨论领域驱动设计与数据网格架构之间的相互作用。

所有这些章节都以若干练习问题结束，以强化学习。一些问题使用了虚构公司“WolfDesk”来展示领域驱动设计的各个方面。请阅读以下对 WolfDesk 的描述，并在回答相关练习问题时参考它。

### 示例领域：WolfDesk

WolfDesk 以服务的形式提供帮助台工单管理系统。如果你的初创公司需要为客户提供支持，使用 WolfDesk 的解决方案，你可以在很短时间内启动并运行。

WolfDesk 使用的计费模式与其竞争对手不同。它不按用户收费，而是允许租户按需设置任意数量的用户，租户按计费周期内打开的支持工单数量付费。没有最低费用，并且对每月工单的特定阈值有自动数量折扣：打开超过 500 张工单可享受 10% 折扣，打开超过 750 张工单可享受 20% 折扣，打开超过 1,000 张工单每月可享受 30% 折扣。

为防止租户滥用商业模式，WolfDesk 的工单生命周期算法确保不活跃的工单会自动关闭，鼓励客户在需要进一步支持时打开新工单。此外，WolfDesk 实施了欺诈检测系统，分析消息并检测同一工单中讨论无关主题的情况。

为帮助其租户简化与支持相关的工作，WolfDesk 实施了“支持自动驾驶”功能。自动驾驶分析新工单并尝试从租户的工单历史中自动找到匹配的解决方案。该功能可进一步减少工单的生命周期，鼓励客户为进一步的问题打开新工单。

WolfDesk 整合了所有安全标准和措施来验证和授权其租户的用户，并且还允许租户将其现有的用户管理系统与单点登录（SSO）配置。

管理界面允许租户配置工单类别的可能值，以及租户支持的产品列表。





为了能够仅在租户支持代理的工作时间内将新工单路由给他们，WolfDesk 允许录入每个代理的班次安排。

由于 WolfDesk 以零最低费用提供服务，它必须以最小化新租户入驻成本的方式优化其基础设施。为此，WolfDesk 利用无服务器计算，可根据活跃工单上的操作弹性扩展其计算资源。

### 本书使用的约定

本书使用以下排版约定：

*Italic*

表示新术语、URL、电子邮件地址、文件名和文件扩展名。

*Constant width*

用于程序列表，以及在段落中引用程序元素，如变量或函数名、数据库、数据类型、环境变量、语句和关键字。

> NOTE
> 
> 此元素表示一般性说明。

### 使用代码示例

补充材料（代码示例、练习等）可从 https://learning-ddd.com 下载。

本书中呈现的所有代码示例均使用 C# 语言实现。通常，您在章节中看到的代码示例是展示所讨论概念的摘录。

当然，本书中讨论的概念和技术并不局限于 C# 语言或面向对象编程方法。所有内容对其他语言和其他编程范式同样相关。因此，请随意使用您喜欢的语言实现本书的示例并与我分享。我很乐意将它们添加到本书网站上。

如果您对代码示例有技术问题或使用问题，请发送电子邮件至 bookquestions@oreilly.com。

本书旨在帮助您完成工作。一般来说，如果本书提供了示例代码，您可以在自己的程序和文档中使用它。除非您复制代码的很大一部分，否则无需联系我们征求许可。例如，编写一个使用本书中多个代码片段的程序不需要许可。出售或分发 O’Reilly 书籍中的示例需要许可。引用本书并引用示例代码来回答问题不需要许可。将大量示例代码纳入您产品的文档中需要许可。

我们感谢，但通常不要求署名。署名通常包括标题、作者、出版商和 ISBN。例如：“Learning Domain-Driven Design by Vlad Khonononov (O’Reilly). Copyright 2022 Vladislav Khononov, 978-1-098-10013-1。”

如果您认为代码示例的使用超出了合理使用范围或上述许可，请随时联系我们 permissions@oreilly.com。

### O’Reilly Online Learning

#### NOTE

40 多年来，*O’Reilly Media* 一直为公司提供技术和商业培训、知识和见解，帮助公司取得成功。

我们独特的专家和创新者网络通过书籍、文章和我们的在线学习平台分享他们的知识和专业知识。O’Reilly 的在线学习平台为您提供按需访问实时培训课程、深入学习路径、交互式编码环境，以及来自 O’Reilly 和 200 多家其他出版商的庞大文本和视频收藏。欲了解更多信息，请访问 http://oreilly.com。

### 如何联系我们

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

### 致谢

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

### 引言

软件工程很难。要想在其中取得成功，我们必须不断学习，无论是尝试新语言、探索新技术，还是跟上新的流行框架。然而，每周学习一个新的 JavaScript 框架并不是我们工作的最困难方面。理解新的业务领域可能更具挑战性。

在整个职业生涯中，我们经常需要为各种各样的业务领域开发软件：金融系统、医疗软件、在线零售商、营销等。从某种意义上说，这就是我们的工作与其他大多数职业不同的地方。其他领域工作的人在发现软件工程涉及如此多的学习时常常感到惊讶，尤其是在更换工作场所时。

未能把握业务领域会导致业务软件实现不佳。不幸的是，这很常见。据研究，大约 70% 的软件项目无法按时、按预算或按照客户要求交付。换句话说，绝大多数软件项目都失败了。这个问题如此根深蒂固和普遍，以至于我们甚至有一个专门的术语来称呼它：软件危机。

*软件危机* 一词早在 1968 年就被引入。¹ 人们会认为在过去的 50 年里情况会有所改善。在这些年里，引入了许多方法、方法论和学科来使软件工程更有效：敏捷宣言、极限编程、测试驱动开发、高级语言、DevOps 等等。不幸的是，情况并没有改变太多。项目仍然经常失败，软件危机仍然存在。

已经进行了许多研究来调查常见项目失败的原因。² 尽管研究人员无法 pinpoint 唯一原因，但他们的大多数发现都有一个共同点



theme: communication. 沟通问题阻碍项目可以以不同的方式表现出来；例如，不明确的需求、不确定的项目目标，或团队之间工作协调无效。 然而，多年来，我们又尝试通过引入新的沟通机会、流程和媒介来改善团队内部和团队之间的沟通。
不幸的是，我们项目的成功率仍然没有太大变化。

领域驱动设计（DDD）提出从不同角度攻击失败软件项目的根本原因。 有效的沟通是领域驱动设计工具和实践的核心主题，你将在本书中学习这些工具和实践。 DDD 可以分为两个部分：战略和战术。

DDD 的战略工具用于分析业务领域和战略，并促进不同利益相关者之间对业务的共同理解。 我们还将利用对业务领域的这一认知来驱动高层设计决策：将系统分解为组件并定义它们的集成模式。

领域驱动设计的战术工具解决沟通问题的不同方面。 DDD 的战术模式允许我们以反映业务领域、解决其目标并使用业务语言的方式编写代码。

DDD 的战略和战术模式与实践都使软件设计与业务领域保持一致。 这就是名称的由来：（业务）领域驱动（软件）设计。

领域驱动设计并不会像《黑客帝国》中那样直接将新 JavaScript 库的知识安装到你的大脑中。 然而，它会通过缓解理解业务领域的过程并根据业务战略指导设计决策，使你成为更有效的软件工程师。 正如你将在本书后面的章节中了解到的，软件设计与其业务战略之间的联系越紧密，维护和演进系统以满足业务未来需求就越容易，最终导致更成功的软件项目。

让我们从探索战略模式和实践开始我们的 DDD 之旅。

+   1 “Software Engineering.” Report on a conference sponsored by the NATO Science Committee, Garmisch, Germany, October 7–11, 1968.
+   2 See, for example, Kaur, Rupinder, and Dr. Jyotsna Sengupta (2013), “Software Process Models and Analysis on Failure of Software Development Projects,” https://arxiv.org/ftp/arxiv/papers/1306/1306.1068.pdf. See also Sudhakar, Goparaju Purna (2012), “A Model of Critical Success Factors for Software Projects.” *Journal of Enterprise Information Management* 25(6), 537–558.
