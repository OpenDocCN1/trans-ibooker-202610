# M. Riley - 便携 Python 项目. 用树莓派运行你的家庭 (2022)
















## 关于 Pragmatic Bookshelf

Pragmatic Bookshelf 是一家敏捷出版公司。我们在这里是因为我们希望改善开发者的生活。我们通过创建及时、实用的书籍，由程序员为程序员撰写来实现这一点。

我们的 Pragmatic 课程、工作坊和其他产品可以帮助你和你的团队创建更好的软件并获得更多乐趣。如需了解更多信息以及最新的 Pragmatic 书籍，请访问我们 http://pragprog.com。

我们的电子书不包含任何数字版权管理，并且一直以来都是 DRM 免费的。我们开创了 beta 书籍的概念，你可以在书籍仍在撰写时购买并阅读，并向作者提供反馈以帮助人人打造更好的书籍。所有购买者的免费资源包括源代码下载（如适用）、勘误表和讨论论坛，均可在本书在 pragprog.com 的主页上获取。我们在这里让你的生活更轻松。

## 新书公告

想了解我们最新的书籍和公告，以及偶尔的特别优惠？只需在 pragprog.com 创建一个账户（只需一个电子邮件地址和密码）并选择接收新闻通讯的复选框。你也可以在 twitter 上关注我们 @pragprog。

## 关于电子书格式

如果你直接从 pragprog.com 购买，你将以一个价格获得所有可用的电子书格式。你可以通过 Dropbox 在所有设备（包括 iPhone/iPad、Android、笔记本电脑等）之间同步你的电子书。你在该版本生命周期内可免费更新。当然，你也可以随时回来重新下载你的书籍。从 Amazon Kindle 商店购买的电子书受 Amazon 政策约束。Amazon 文件格式的限制可能导致电子书在不同设备上显示不同。有关更多信息，请参阅我们位于 pragprog.com/#about-ebooks 的常见问题解答。要了解有关本书的更多信息并访问免费资源，请访问 https://pragprog.com/book/mrpython，即本书的主页。

感谢你的持续支持，

Andy Hunt
The Pragmatic Programmers

制作本书的团队包括：Dave Rankin（首席执行官）、Janet Furlow（首席运营官）、Tammy Coron（主编）、Jacquelyn Carter（开发编辑）、L. Sakhi MacMillan（校对编辑）、Potomac Indexing, LLC（索引）、Gilson Graphics（版式）、Andy Hunt 和 Dave Thomas（创始人）

如需客户支持，请联系 support@pragprog.com。

如需国际版权，请联系 rights@pragprog.com。









































































































## Portable Python Projects 的早期好评

本书以一种美妙的积木式方法涵盖了计算机技术的许多方面，通过每个项目教导读者。从简单的第一个项目到更复杂的项目，建造者熟练地学习了关于 Raspberry Pi 及其硬件、Python 及其库、自动化，以及最重要的，从所有这些部分设计出有用的东西。读完本书后，读者能够继续向许多新方向探索。我喜欢这本书。

→ Michael Bengtson
Engineering Manager, Sprocks

我会向我认识的大多数经验丰富的软件开发者推荐这本书。它是秋天漫长夜晚的完美读物，当你有时间学习新东西时。本书可以帮助你改善你的家庭，并能激发你在阅读之前从未想过的无数新想法。

→ Maik Schmidt
Software Developer and Writer

*Portable Python Projects* 采取了实用方法，通过完整实现和简洁有意义的解释来学习 Raspberry Pi Python。我学到了许多有用的 Python 策略以及大量关于 Raspberry Pi 平台的知识。Python 和 Pi 是完美的组合，因为它们一起构成了一个简单、有效且强大的平台。我强烈推荐 *Portable Python Projects* 给初学者和长期爱好者。

→ John Cairns

Staff Engineer, Blocknative

凭借这本书，Mike Riley 为任何对微控制器和一点 Python 代码能做什么感到好奇的人打开了大门。新手会发现完成一些创新项目的优秀分步指导，而老手会找到将事物提升到新水平的灵感。

→ Sven Davies
IT Director, AC Americas

Mike 对实用 Pi 项目的深刻见解将使新手和经验丰富的人都能快速构建创新解决方案。

→ Jim Szubryt
Senior Delivery Principal, Slalom

## 致谢

写书就像生孩子，只有经历过的人才真正知道这种体验是什么样的。这是我的第四本由 Pragmatic Bookshelf 出版的书籍，和之前的书籍一样，如果没有编辑和出版商的巨大支持、持续的鼓励和指导就不可能实现。Jackie Carter 是我所有 Pragmatic 书籍的开发编辑，她再次通过这项工作证明了她是 Pragmatic 最优秀的编辑之一。Jackie，感谢你的一切！

我还要感谢那些在我的早期草稿中提供宝贵反馈以带来更多清晰度和具体性的人对项目做出贡献的人。Mike Bengtson、John Cairns、Sven Davies、Jim Szubryt、John Winans，以及 Pragmatic Bookshelf 的作者 Maik Schmidt 在指导本书达到你即将阅读的最终形式方面发挥了特别关键的作用。你们太棒了！

最后，不用说，没有 Andy Hunt 和 Dave Thomas，Pragmatic Bookshelf 的创始人，本书就不会存在。他们是科技行业最聪明的科技人员和书籍作者之一，并在首席执行官 Dave Rankin 的专家指导下建立了一个备受信赖的平台，是业内最受尊敬的科技图书出版商之一。能与这样一群优秀的顶尖人士联系在一起是我的荣幸。





## Introduction

欢迎使用 Pi。接下来的旅程将指导你完成获取和配置合适的硬件和软件，以创建独特且有趣的应用。

关于 Pi 的书籍有很多，但几乎所有书籍都只关注基础硬件和初级的电子任务，未能展示最新 Pi 硬件和扩展配件所具备的强大功能。我们将在 Pi 上配置的基础软件与世界级开发者使用的应用相同。而我们将创建的项目提供了一个功能性的基础，你可以据此扩展和定制，以满足你自己的理想应用。

Raspberry Pi 硬件自 2012 年首次发布以来已发生了显著演变。第四代设计在将廉价而强大的计算机带给大众方面实现了重大飞跃。Pi 4 可支持高达 8 GB RAM，是几年前售出的许多台式机和笔记本电脑的两倍。更多的内存意味着可以同时运行更复杂的应用程序。

Python 语言的语法和结构对初学者来说易于学习，同时又足够强大，供专业软件开发者每天使用。它具有可移植性，即在一种受支持的操作系统上编写的脚本可以在另一种操作系统上几乎无需更改或完全无需更改即可重用。

Raspberry Pi 在尺寸上也具有可移植性，其大小小于一副扑克牌，可以在家中或办公室的任何地方设置或轻松重新定位。这就是为什么 Pi 在家庭自动化爱好者中如此受欢迎。得益于 Pi 的低功耗 CPU 和坚固紧凑的设计，它可以放置在电视后面、洗衣机和烘干机附近，甚至可以放在灯开关隔间内。只要有 Pi 可以连接的电源和互联网连接，其位置和用途的可能性就是无限的。

## About the Book

本书的第一部分介绍了 Raspberry Pi 计算机、有效操作它所需的周边设备，以及为 Pi 提供扩展数据处理能力的众多巧妙传感器。我们还将安装操作系统和其他软件，以为本书中的项目做好 Pi 的准备。

本书的第二部分包含项目。每个项目都以类似的方式呈现，首先以简短的介绍描述目标。设置部分包含构建项目所需的所有额外硬件和配件，以及该项目所需的任何额外软件库和包。接下来的部分将指导你完成组件的组装以及使它们执行有趣操作所需的 Python 脚本。项目以下一步部分结束，鼓励你自定义和扩展项目基础，以打造最适合你需求的解决方案。

## Online Resources

本书中展示的应用和示例可以在 Pragmatic Programmers 网站的本书页面上找到。[1] 在那里你还可以找到勘误提交表单，可以在其中报告文本问题或提出未来版本的建议。

在下一章中，我们将回顾本书项目中使用的推荐硬件购物清单。除了价格和功能外，我们还将讨论选择一种硬件配置而非另一种的理由。因此，准备好迎接一次富有教育意义、有趣、实用且令人满意的体验吧！

Mike Riley
mailto:mike@mikeriley.com
January, 2022

##### Footnotes

[1] http://pragprog.com/titles/mrpython/
