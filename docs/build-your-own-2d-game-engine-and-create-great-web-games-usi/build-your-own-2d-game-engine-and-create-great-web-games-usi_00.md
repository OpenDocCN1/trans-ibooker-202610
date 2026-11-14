# 自制2D游戏引擎构建指南与精彩Web游戏创建指南：HTML5、JavaScript和WebGL2使用指南

> *献给我的妻子 Clover，以及我们的女儿 Jean 和 Ruth，感谢你们让我的生活完整。*
> 
> **—凯尔文·宋**

> *献给我的家人，感谢他们一生对我的永恒支持。*
> 
> **—杰贝迪亚·帕夫利亚斯**

> *献给我的母亲 Linda，感谢她让我懂得在工作中寻找乐趣的价值。*
> 
> **—杰森·佩斯**

# 简介

欢迎阅读 *构建你自己的 2D 游戏引擎并创建精彩的 Web 游戏*。既然你拿起了这本书，你很可能对游戏引擎的细节以及创建可在互联网上玩的自己的游戏感兴趣。本书通过涵盖相关技术概念、演示示例实现，并展示如何组织大量源代码和资源文件以支持游戏开发，教你如何构建一个 2D 游戏引擎。本书还讨论了每个涵盖的技术主题领域如何与游戏设计元素相关联，以便你能够构建、游玩、分析并学习 2D 游戏引擎和游戏的开发。本书中的示例实现基于 `HTML5`、`JavaScript` 和 `WebGL2`，这些技术免费可用，且几乎所有 Web 浏览器都支持。读完本书后，你开发的游戏引擎及相关游戏将可通过 Web 浏览器从互联网上的任何地方进行游玩。

本书呈现了软件工程、计算机图形学、数学、物理学、游戏开发和游戏设计等相关概念——所有这些都在构建 2D 游戏引擎的背景下进行。讲解与源代码的分析和开发紧密结合；你将花费大量时间构建类游戏的概念项目，以演示游戏引擎组件的功能。通过在早期引入的源代码基础上构建，本书带领你踏上一段旅程，让你在掌握 2D 游戏引擎背后基本概念的同时，获得开发简单但可工作的 2D 游戏的实践经验。从第 4 章开始，每章末尾都包含一个“设计考量”部分，将涵盖的技术概念与游戏设计元素联系起来。读完本书，你将熟悉 2D 游戏引擎的概念和技术细节，有能力实现 2D 游戏引擎中的功能以支持常见的 2D 游戏需求，并能在构建有趣且引人入胜的游戏时，从游戏设计元素的角度考量游戏引擎技术主题。

### 第二版新增内容

第二版的主要新增内容包括 `JavaScript` 语言和 `WebGL` API 更新，以及专门章节中对物理和粒子系统组件的大量详细介绍。

全书所有示例均针对 `JavaScript` 语言的最新特性进行了优化。虽然一些更新较为平常，例如原型链语法替换，但最新语法使整体呈现和代码可读性显著提升。更简洁的异步支持促成了一个全新的资源加载架构，为整个引擎提供了单一同步点（第 4 章）。`WebGL` 上下文已更新以连接到 `WebGL 2.0`。专门的章节使得复杂的物理和粒子系统组件能够得到更详尽和循序渐进的介绍。在适当的地方包含了详细的数学推导。

### 适合阅读本书的人群

本书面向熟悉基本面向对象编程概念、并具备 `Java` 或 `C#` 等面向对象编程语言基础到中级知识的程序员。例如，如果你是修过几门编程入门课程的学生、初涉游戏和图形编程的经验丰富的开发者，或是自学编程的爱好者，你将能轻松跟上本书呈现的概念和代码。如果你是编程新手，建议你先熟悉 `JavaScript` 编程语言和面向对象编程概念，再学习本书提供的内容。

### 假设

你应具有使用面向对象编程语言（如 `Java` 或 `C#`）编程的经验。了解 `JavaScript` 将是加分项，但非必需。本书示例假设你理解数据封装和继承。此外，你应熟悉链表和字典等基本数据结构，并能熟练运用代数和几何基础，特别是线性方程和坐标系。

### 不适合阅读本书的人群

本书并非旨在教读者如何编程，也不试图解释 `HTML5`、`JavaScript` 或 `WebGL2` 的复杂细节。如果你没有使用面向对象编程语言开发软件的经验，可能会觉得本书中的示例难以理解。

另一方面，如果你在其他平台上有丰富的游戏引擎开发背景，本书内容对你来说可能过于基础；本书旨在面向没有 2D 游戏引擎开发经验的开发者。不过，你仍可能从本书中获得一些关于本书涵盖平台的 2D 游戏引擎和 2D 游戏开发的实用技巧。

### 本书结构

本书通过描述基础设施、图形系统、游戏对象行为、相机操控以及基于引擎的示例游戏创建，讲授如何开发游戏引擎。




# 代码示例

本书每章都包含示例，供你交互式地实验与学习新知识。你可以通过点击 [www.apress.com/9781484273760](http://www.apress.com/9781484273760) 页面上的 **下载源代码** 按钮，获取所有项目的源代码及关联资源（图片、音频片段或字体）。你将看到按章节编号组织的文件夹结构。每个文件夹内包含对应本书各节的 Visual Studio Code (VS Code) 项目子文件夹。

# 致谢

本书项目直接源于作者在 **游戏主题 CS1/2：赋能教师** 项目中构建游戏的学习经验，该项目由 **科学、技术、工程和数学本科教育变革 (TUES)** 计划、**国家科学基金会 (NSF)** 资助（奖项编号 DUE-1140410）。我们要感谢 NSF 项目官员 Suzanne Westbrook 对我们项目的信任，以及 Jane Prey、Valerie Bar 和 Paul Tymann 的鼓励。

第二版的出版得益于众多学生和合作者的推动。特别感谢华盛顿大学博塞尔分校 **CSS452：游戏引擎开发** 课程（参见 [`myuwbclasses.github.io/CSS452/`](https://myuwbclasses.github.io/CSS452/)）的学生们，他们提出了最严厉、要求最高，却又最具支持性的反馈。通过众多游戏与 API 扩展项目（参见 [`html5gameenginegroup.github.io/GTCS-Engine-Student-Projects/`](https://html5gameenginegroup.github.io/GTCS-Engine-Student-Projects/)），我们明确认识到必须更新 JavaScript 与 WebGL (Web Graphics Library) 版本、底层同步机制，最重要的是物理引擎的覆盖范围。

Fernando Arnez（首版合著者）教会了我们 JavaScript。Yaniv Schwartz 指引我们了解 JavaScript async/await 与 Promise。与 Huaming Chen 和 Michael Tanaya 的讨论与合作，直接促成了游戏引擎物理章节的完成。Akilas Mebrahtom 和 Donald Hawkins 构建了第 9 章末尾的额外示例，演示了常见物理材质的潜在预设。音量控制由 Kyla NeSmith 首先研究并集成。Nicholas Carpenetti 和 Kyla NeSmith 为初始游戏引擎开发了用户界面 API，遗憾的是未收录于此版。这些及无数其他反馈共同提升了本书内容的质量与改进。

贯穿全书示例项目的主角 **Dye** 及众多视听素材，基于教学对象与面向对象层级概念而设计的 **Dye Hard** 游戏。Dye Hard 原始开发团队成员包括：Matthew Kipps、Rodelle Ladia、Chuan Wang、Brian Hecox、Charles Chiou、John Louie、Emmett Scout、Daniel Ly、Elliott White、Christina Jugovic、Rachel Harris、Nathan Evers、Kasey Quevedo、Kaylin Norman-Slack、David Madden、Kyle Kraus、Suzi Zuber、Aina Braxton、Kelvin Sung、Jason Pace、Rob Nash。Kyle Kraus 创作了第 4 章 **音频支持** 项目中使用的背景音乐，原为教授循环而设计的 **Linx** 游戏所作。第 12 章游戏的背景音频由 David Madden 作曲、Aina Braxton 编排。感谢 Clover Wai 提供的图表与插图。

我们还要感谢 Spandana Chatterjee 对我们理念的信任、耐心，以及持续高效有力的支持。衷心感谢 Mark Powers 的勤奋与闪电般的邮件回复。Mark 应该学会并考虑偶尔睡一会儿觉。Nirmal Selvaraj 统筹一切，确保项目有序推进。

最后，感谢 Yusuf Pisan 对技术审阅给出的深刻、高效、且极快的反馈。

本著作中的所有观点、发现、结论与建议仅代表作者个人，不一定反映赞助方的观点。

# 作者简介

Kelvin Sung





# 关于作者

## Kelvin Sung

是华盛顿大学博塞尔分校（UWB）计算与软件系统系的教授。他获得了伊利诺伊大学厄巴纳-香槟分校的计算机科学博士学位。Kelvin 的背景涉及计算机图形学、硬件和机器架构。他从 Alias|Wavefront（现为 Autodesk 的一部分）来到 UWB，在那里他在设计和实现 Maya Renderer（一款获得奥斯卡奖的图像生成系统）中发挥了关键作用。在 UWB，在微软研究院和国家科学基金会的资助下，Kelvin 的工作专注于视频游戏机制、现实世界问题的解决方案以及远程协作支持的交叉领域。他与他的学生和同事合著了五本书：一本关于计算机图形学（*Essentials of Interactive Computer Graphics: Concepts and Implementation*，A.K. Peters，2008），其余关于 2D 游戏引擎（*Learn 2D Game Development with C#*，Apress，2013；《Build Your Own 2D Game Engine and Create Great Web Games》，Apress，2015 年 10 月；《Building a 2D Game Physics Engine》，Apress，2016；《Basic Math for Game Development with Unity 3D》，Apress，2019）。

## Jebediah Pavleas

![](img/334805_2_En_BookFrontmatter_Figb_HTML.jpg) 于 2016 年获得华盛顿大学博塞尔分校（UWB）计算机科学与软件工程理学硕士学位。他还于 2012 年获得 UWB 的理学学士学位，并获得了他所在届次的校长奖章。在研究生阶段，Jebediah 在微软研究院的 Enable 团队实习，为他们的 Eye-Gaze Wheelchair 项目（一种仅用眼睛控制的轮椅，供 ALS 患者使用）做出了贡献。他合著了三本关于 2D 游戏和游戏引擎的书（*Learn 2D Game Development with C#*，Apress，2013；《Build Your Own 2D Game Engine and Create Great Web Games》，Apress，2015 年 10 月；《Building a 2D Game Physics Engine》，Apress，2016）。在 UWB 期间，他的项目包括一个利用微软 Kinect 传感器教授代数的交互式数学应用程序，名为 Kinect Math。与此及其他项目相关，他在 *IEEE Computer* 和 *The Journal of Computing Sciences in Colleges* (CCSC) 上合著了出版物。Jebediah 喜欢设计、构建和玩各种游戏，以及为自己和他人改进技术的无障碍性。

## Matthew Munson

是华盛顿大学博塞尔分校计算机科学与软件工程专业的研究生。他于 2020 年在华盛顿大学博塞尔分校获得了计算机科学与软件工程以及机械工程的本科学位。Matthew 对操作系统开发、网络和嵌入式系统感兴趣。作为一名研究助理，Matthew 利用云计算分析了由俄勒冈海岸水听器记录的多年音频数据。这些数据被用于研究气候变化和航运噪音对海洋哺乳动物的影响。目前，Matthew 正在开发一个网络增强现实库，专注于允许用户从不同视角查看同一虚拟场景。

## Jason Pace

在互动娱乐行业拥有 15 年经验，作为制作人、设计师和创意总监参与了各类游戏的开发，从移动端的超休闲益智游戏到 Xbox 上的《光环》。作为设计师，Jason 构建游戏机制和系统，起源于一组简单而深思熟虑的交互（称为核心游戏循环），逐步引入多样性和复杂性，创造引人入胜且令人愉悦的互动体验，同时专注于使每款电子游戏独具趣味的要素。

# 关于技术审稿人

## Yusuf Pisan

![](img/334805_2_En_BookFrontmatter_Figc_HTML.jpg) 是华盛顿大学博塞尔分校计算与软件系统系的副教学教授。此前，他在悉尼科技大学工作，曾担任哈维·穆德学院、南加州大学、伍斯特理工学院（WPI）和哥本哈根 IT 大学（ITU）的访问教授。

他的研究兴趣包括计算机游戏的赋能技术、支持协作工作的虚拟环境设计以及计算机科学教育。他创立了澳大利亚互动娱乐会议系列，并帮助培育了澳大利亚游戏社区。他的出版物列表可在 Google Scholar 找到。

Yusuf 拥有西北大学人工智能博士学位。在 2017 年搬到西雅图之前，Yusuf 在芝加哥地区生活了 10 年，在悉尼生活了 20 年。

更多信息请见 <https://pisanorg.github.io/yusuf/>。

## Yogendra Sharma

![](img/334805_2_En_BookFrontmatter_Figd_HTML.jpg) 是一名开发人员，在可扩展和分布式应用程序的架构、设计和开发方面拥有经验，核心兴趣在于微服务和 Spring。他目前在 Intelizign Engineering Services Pvt. Ltd.，Pune 担任物联网和云架构师。

他还拥有 AWS、IoT、Python、J2SE、J2EE、NodeJS、VueJs、Angular、MongoDB 和 Docker 等技术的实践经验。

他不断探索技术新事物，思想开放，渴望了解新技术和框架。他审阅了 Packt 出版的多本书籍和视频课程。
