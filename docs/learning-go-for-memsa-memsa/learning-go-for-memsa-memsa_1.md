# memsa memsa 的 Go 入门指南












Add additional revdate spans below as needed.

# Revision History for the Early Realease

* 2026-05-30: First Release

See [`oreilly.com/catalog/errata.csp?isbn=0642572348533`](http://oreilly.com/catalog/errata.csp?isbn=0642572348533) for release details.




<p class="printer">\[LSI\]</p>




















italicize any chapter titles that are not yet available in ER

## Preface

I wasn’t sure there should be a third edition of Learning Go.

When the term “vibe coding” was coined in early 2025 by Andrej Karpathy, it was a bit of a joke. By the end of 2025, no one was laughing. LLMs got *really* good at writing code *really* fast. Developers were using them for larger and more complicated projects. If anything, the momentum behind automating coding is increasing.

Given this, why would anyone want to write (or read) an updated book on a programming language? Who cares about idiomatic usage and new features when most code is automatically generated? Maybe my time would be better spent writing romance novels before LLMs master those, too.

After some additional thinking, I realized that a book on idiomatic Go might be more needed now than ever before. There are two reasons. The first is that idiomatic code works better. When you write code in an idiomatic way, the Go runtime is able to execute your code more efficiently. If you try to bend Go into something like Java, C++, or Python you will likely find your programs running more slowly and using more memory than if you just adopted the style and techniques that the Go runtime is optimized to use. When a programmer who is familiar with one language asks an LLM to generate code in another language, they need to understand what a Go program looks like to know if it is going to run well.

And that leads to the other reason, which gets to the heart of what “idiomatic” means and why programs should be written in an idiomatic style. Harold Abelson, Gerald Jay Sussman, and Julie Sussman’s book *Structure and Interpretation of Computer Programs* was the introductory computer science textbook at MIT for decades. It includes the famous quote, “Programs must be written for people to read, and only incidentally for machines to execute.” This was true in 1984 and it is still true over 40 years later, even if programs are writing programs.

An idiom is just a word for a familiar pattern. When a program follows a familiar pattern, that is a shortcut to understanding what it is doing and why it is doing it. No matter how code was produced, at some point a person is going to need to look at it and understand what that code does. If it follows a familiar pattern, it will be easier. If it does not, it will be harder, which makes the inevitable maintenance more difficult. In short, non-idiomatic code is likely to be buggy, unmaintainable code, and no one wants that.

This insight helped me figure out what sort of updates should be in the third edition. In addition to covering changes to the Go language, runtime, and ecosystem, this edition includes tips on what to look for when reviewing code, as well as additional exercises to help you get more comfortable writing idiomatic Go code and knowing what it looks like. In addition, there’s a new chapter on monitoring and measuring your Go code so you can better understand how your Go program is working, both internally and in collaboration with other software.

Welcome to the latest edition of Learning Go. I hope you learn best practices for writing and reviewing Go code. And if you turn off the LLM to experience the joy of writing code all by yourself, I won’t tell anyone.

## Who Should Read This Book

This book is targeted at developers who are looking to pick up a second (or fifth) language. The focus is on people who are new to Go. This ranges from those who don’t know anything about Go other than that it has a cute mascot to those who have already worked through a Go tutorial or even written some Go code. The focus of *Learning Go* isn’t just how to write programs in Go; it’s how to write Go *idiomatically*. More experienced Go developers can find advice on how to best use the newer features of the language. The most important thing is that you’re here to learn how to write Go code that looks like Go.

I assume you have some experience with the tools of the developer trade, such as version control (preferably `Git`) and IDEs. You should be familiar with basic computer science concepts like concurrency and abstraction, as the book explains how they work in Go. You can download most of the code examples from `GitHub`, and try out dozens more online on [The Go Playground](https://go.dev/play/). While an internet connection isn’t required, it is helpful when reviewing executable examples. Since Go is often used to build and call HTTP servers, some examples also assume familiarity with basic HTTP concepts.





虽然 Go 的大多数特性在其他语言中都能找到，但 Go 做出了不同的权衡，因此用它编写的程序具有不同的结构。*Learning Go* 首先介绍如何设置 Go 开发环境，然后涵盖变量、类型、控制结构和函数。如果你想跳过这些内容，请抵制这种冲动并看一看。细节往往决定了你的 Go 代码是否地道。乍一看似乎显而易见的东西，在深入思考时可能会出人意料地微妙。

## 本书使用的约定

本书使用了以下排版约定：

*Italic*

表示新术语、URL、电子邮件地址、文件名和文件扩展名。

`Constant width`

用于程序列表，以及在段落中引用程序元素，如变量或函数名、数据库、数据类型、环境变量、语句和关键字。

**`Constant width bold`**

显示用户应逐字输入的命令或其他文本。

*`Constant width italic`*

显示应由用户提供的值或由上下文确定的值替换的文本。

#### Tip
此元素表示提示或建议。

#### Note
此元素表示一般说明。

#### Warning
此元素表示警告或注意事项。

## O’Reilly Online Learning

#### Note
40 多年来，[*O’Reilly Media*](http://oreilly.com) 一直为公司提供技术和商业培训、知识和见解，帮助公司取得成功。

我们独特的专家和创新者网络通过书籍、文章和我们的在线学习平台分享他们的知识和专业知识。O’Reilly 的在线学习平台为您提供按需访问实时培训课程、深入学习路径、交互式编码环境，以及来自 O’Reilly 和 200 多家其他出版商的庞大文本和视频集合。有关更多信息，请访问 [*http://oreilly.com*](http://oreilly.com)。

## 如何联系我们

请将有关本书的评论和问题发送给出版商：

* O’Reilly Media, Inc.
* 1005 Gravenstein Highway North
* Sebastopol, CA 95472
* 800-889-8969 (in the United States or Canada)
* 707-827-7019 (international or local)
* 707-829-0104 (fax)
* *support@oreilly.com*
* [*https://www.oreilly.com/about/contact.html*](https://www.oreilly.com/about/contact.html)

Don't forget to update the link above.

有关我们的书籍和课程的新闻和信息，请访问 [*https://oreilly.com*](https://oreilly.com)。

在 LinkedIn 上找到我们： [*https://linkedin.com/company/oreilly-media*](https://linkedin.com/company/oreilly-media)

在 Twitter 上关注我们： [*https://twitter.com/oreillymedia*](https://twitter.com/oreillymedia)

在 YouTube 上观看我们： [*https://youtube.com/oreillymedia*](https://youtube.com/oreillymedia)

## Acknowledgments for the 3rd Edition

第三版致谢即将公布。

## Acknowledgments for the 2nd Edition

我对第一版 *Learning Go* 的反响感到谦卑和受宠若惊。它始于 2019 年底，于 2020 年底完成，并于 2021 年初发布。无意中，它成了我自己的疫情项目。有些人烤面包；我解释指针。

完成后，人们问我下一本书会是什么。我告诉他们，我将好好休息，然后着手写一本浪漫小说。但在我开始写关于海盗和公主的故事之前，发生了令人惊奇的事情：*Learning Go* 取得了巨大成功。它在 O’Reilly Learning 上花费了将近一年的时间，成为五本最受欢迎的书籍之一。随着销量的持续，我在第一版中看到了想要修复的东西，并收到了读者指出错误和遗漏的信件。有些在后来的印刷中作为勘误表进行了修复，但我联系了 O’Reilly，看看他们是否想要新版，他们对此感兴趣。

重返 *Learning Go* 和 O’Reilly 进行第二版是一次愉快的经历。Rita Fernando 提供了指导、反馈和编辑。本版在 Jonathan Amsterdam、Leam Hall、Katie Hockman、Thomas Hunter、Max Horstmann 和 Natalie Pistunovich 的反馈下变得不可估量地更好。Chris Hines 做了非常彻底的工作来找出我的错误并提出更好的示例。Abby Deng 和 Datadog 的 Go 书籍俱乐部的成员的评论和反馈让我能够根据 Go 新手开发人员的反馈做出更改。剩下的（希望很少的）错误都是我的。

我的妻子和孩子们足够善良，忍受我在弄清楚如何最好地描述新的 Go 特性时缺席家庭电影之夜。

我还要感谢第一版的读者，他们向我传达了他们的善意。我感谢你们所有的支持和鼓励。

## Acknowledgments for the 1st Edition

写书似乎是一项孤独的任务，但它不可能没有大量人的帮助。我在 2019 年的 GopherCon 上向 Carmen Andoh 提到我想写一本关于 Go 的书，她把我介绍给了 O’Reilly 的 Zan McQuade。Zan 指导我完成了收购流程，并在我撰写 *Learning Go* 期间继续为我提供建议。Michele Cronin 编辑了文本，提供了反馈，并在不可避免的困难时期倾听。Tonya Trybula 的校对和 Beth Kelly 的生产编辑使我的草稿达到了生产质量。

在写作期间，我收到了许多人的关键反馈（和鼓励），包括 Jonathan Altman、Jonathan Amsterdam、Johnny Ray Austin、Chris Fauerbach、Chris Hines、Bill Kennedy、Tony Nelson、Phil Pearl、Liz Rice、Aaron Schlesinger、Chris Stout、Kapil Thangavelu、Claire Trivisonno、Volker Uhrig、Jeff Wendling 和 Kris Zaragoza。我特别想认可 Rob Liebowitz，他详细的笔记和快速的回复使这本书比没有他的努力要好得多。

我的家人忍受了我晚上和周末在电脑前而不是和他们在一起。特别是我的妻子 Laura，慷慨地假装我凌晨 1 点或更晚上床时没有吵醒她。

最后，我想纪念四十年前开启我这条道路的两个人。第一个是童年朋友的父亲 Paul Goldstein。1982 年，Paul 向我们展示了一台 Commodore PET，输入了 PRINT 2 + 2，然后按下了 Enter 键。当屏幕显示 4 时，我感到惊讶，并立即着迷。他后来教我如何编程，甚至让我借用了 PET 几周。其次，我想感谢我的母亲，她鼓励我对编程和计算机的兴趣，尽管她完全不知道这些有什么用。她为我买了 Atari 2600 的 BASIC 编程卡带、一台 VIC-20，然后是一台 Commodore 64，以及启发我想要有一天写自己的编程书籍。

感谢你们所有人帮助实现我的梦想。

Fill in...
