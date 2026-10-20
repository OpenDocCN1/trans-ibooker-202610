

因为我们相信更以实施为基础的方法，而维护分析解决方案的第一要素却在列表的末尾。实际考试的结构也不与列表顺序相同，因此我们将在本指南中以随机顺序向你呈现问题。

## TIP

现在参加微软考试时需要注意的一个关键点是：如果你只是阅读了像这样一份学习指南加上 Microsoft Learn 的文档，就不能指望通过考试。你还需要通过使用该技术获得实践经验，因此我们鼓励你尝试本书中介绍的所有内容。

本书将帮助你理解通过 DP-600 考试所需的技术领域和知识。它将带你了解考试的不同领域，教你如何入门，并向你介绍考试的基础课程。你可以将其用作学习指南、考试练习题的来源（见本书末尾的问题库），以及未来项目的参考资料。但你也需要在 Microsoft Fabric 平台的真实实施和项目中获得实践、动手经验。换句话说，通过考试并不是你仅靠阅读就能实现的！

过去，一些微软考试主要测试考生记忆能力，问题要求考生从文档中回忆硬性事实，而不是他们如何在微软的不同技术领域实际执行实施。但最近，考试已转向衡量考生的技能水平，这意味着考生需要具备实践经验。

## Book Summary

本书包含三个部分共八章。第 1 章介绍了 Microsoft Fabric，第 2 章将指导你了解不同的集成实现方式。第 3 章涵盖转换，第 4 章涵盖查询和分析功能。第 5 章向你展示如何构建语义模型，第 6 章向你展示如何管理企业级模型。第 7 章是治理和安全的指南，第 8 章涵盖开发的生命周期。

第 9 章，即最后一章，是一个包含 100 道练习题的问题库，你可以练习以帮助为考试做准备。请注意，这些问题都不是你在实际考试中会遇到的题目；它们是旨在帮助你测试技能并熟悉考试中出现的测试题形式的练习题。做这些题目将帮助你熟悉问题类型、如何阅读和理解问题，以及如何在考试中管理时间。

每一章都是独立的，我们已尽可能使每章彼此独立。然而，你会在全书中看到对其他章节和部分的引用，以提醒你细节和上下文。这将帮助你在将本书用作参考指南时看到全貌。

我们还尽可能使本书对具有不同 Microsoft Fabric 经验水平的广泛读者都易于使用。如果你是该平台的新手，我们鼓励你从第 1 章开始并从头到尾阅读本书。或者，如果你是经验丰富的 Microsoft Fabric 用户，请随意跳跃并专注于那些能帮助你提升现有技能的章节。

说了这么多，我们，Brian 和 Valerie，祝你在考试中好运，并希望本书能很好地帮助你开始通往认证的个人旅程。

## Conventions Used in This Book

本书中使用了以下排版约定：

*Italic*

表示新术语、URL、电子邮件地址、文件名和文件扩展名。

Constant width

用于程序列表，以及在段落中引用程序元素，如变量或函数名、数据库、数据类型、环境变量、语句和关键字。

**Constant width bold**

显示用户应逐字输入的命令或其他文本。

*Constant width italic*

显示应由用户提供的值或由上下文确定的值替换的文本。

## TIP

此元素表示提示或建议。

## NOTE

此元素表示一般说明。

## WARNING

此元素表示警告或注意。

## O’Reilly Online Learning

## NOTE

40 多年来，*O’Reilly Media* 一直为公司提供技术和商业培训、知识和见解，帮助公司取得成功。

我们独特的专家和创新者网络通过书籍、文章和我们的在线学习平台分享他们的知识和专业知识。O’Reilly 的在线学习平台为你提供按需访问实时培训课程、深入学习路径、交互式编码环境，以及来自 O’Reilly 和 200 多家其他出版商的庞大文本和视频收藏。有关更多信息，请访问 https://oreilly.com。

## How to Contact Us

请将有关本书的评论和问题发送给出版商：

O’Reilly Media, Inc.

141 Stony Circle, Suite 195

Santa Rosa, CA 95401

800-889-8969（美国或加拿大境内）

707-827-7019（国际或本地）

707-829-0104（传真）

support@oreilly.com

https://oreilly.com/about/contact.html

我们为本书设有网页，在那里我们列出勘误表和任何额外信息。你可以在 https://oreil.ly/microsoft-fabric-analytics-study-guide 访问此页面。

有关我们的书籍和课程的新闻和信息，请访问 https://oreilly.com。

在 LinkedIn 上找到我们：https://linkedin.com/company/oreilly-media。

在 YouTube 上观看我们：https://youtube.com/oreillymedia。

## Acknowledgements

From Brian

写书一直是我愿望清单上的一项很长时间，我终于通过与才华横溢的 Valerie Junk 的合作实现了这一雄心。

这个项目之所以能够实现，多亏了 O’Reilly 提供的机会。我欠我们的编辑 Shira Evans 巨大的感谢，她在整个紧张的日程中坚定地支持我们，并花费了许多小时确保我们按时保质完成。

我还欠技术审阅者 Nikola Ilic、Alexander Arvidsson、Ben Weissman、Reitse Eskens 和 Johnny Winter 很大的人情，他们对每一页的细致审查和对每个代码片段的测试，确保了成功认证指南所需的准确性。

我还要特别感谢 *Guy in a Cube* 背后的精彩团队，Adam Saxton 和 Patrick LeBlanc 为撰写前言。你们的工作真正激励了整个社区。

这种工作无疑是马拉松，而不是短跑。对我终生的伴侣和配偶 Christa，感谢你默默的时光，我们不得不错过的电视剧，以及无论我选择追求什么事业，你都坚定不移地相信我。

最后，致你，读者：我真诚希望本指南能帮助你通过 DP-600 考试，更重要的是，释放你作为成功的 Microsoft Fabric 分析工程师的潜力。

## From Valerie

写这本书是一次宝贵的学习经历。与 Brian Bønk 一起完成这样规模的项目，确实推动我在新的方式上成长。

感谢 O’Reilly 提供的机会，感谢我们的编辑 Shira Evans 在每一步引导我们。感谢技术审阅者 Alexander Arvidsson、Johnny Winter、Ben Weissman、Reitse Eskens 和 Nikola Ilic，花时间仔细审阅所有内容。

特别感谢我的伴侣 Jochem van Iterson，在漫长的写作日里给予支持、茶水和零食。再次感谢 Alexander Arvidsson，他在需要讨论事情时是很好的陪练伙伴。

- 1. Activator is not a standalone service in Microsoft Azure.
- 2. Read the complete exam details here: “Study Guide for Exam DP-600: Implementing Analytics Solutions Using Microsoft Fabric”.

